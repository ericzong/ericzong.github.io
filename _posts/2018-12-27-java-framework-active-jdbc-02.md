---
layout: post
title: "ActiveJDBC笔记：2 基本使用"
categories: Java
tags: Java Java框架
excerpt: ActiveJDBC基本使用说明。
author: "Eric Zong"
---

* content
{:toc}

# 编写模型

ActiveJDBC 的模型类必须派生自 org.javalite.activejdbc.Model。

## 模型与表的映射

### 映射推断

模型类可以不指定映射的表，ActiveJDBC 会根据英文单复数的变形来推断映射关系。

简单说来就是，单数形式的模型名映射到对应复数形式的表，比如：Computer => computers。

但这种变形并不仅限于在单数形式后加“s”转变为复数形式的情况，也包括一些特殊的变形方式。详细可参考官方对于 [English inflections](http://javalite.io/english_inflections) 的说明。

### 覆盖映射

自动的映射推断很方便，但限制了表与模型的命名，某些情况下，这可能不是我们想要的。

针对这种情况，ActiveJDBC 提供了 @Table 注解来覆盖指定模型映射的表。

## 模型与数据库关联

默认情况下，模型与“default”数据库关联，但可使用 @DbName 注释覆盖。

## 缓存

使用 @Cached 注解指定缓存模型。

### 配置

放置于 classpath 根目录：

```properties
#inside file: activejdbc.properties
#or EHCache:
cache.manager=org.javalite.activejdbc.cache.EHCacheManager
#cache.manager=org.javalite.activejdbc.cache.OSCacheManager
```

详细缓存提供器配置参考：http://javalite.io/caching

### 清除

破坏性操作（插入、更新、删除）后系统将清除与此表相关（本表及关联表）的缓存。

但是，如果不想清除关联表缓存，可以在本模型上注解 @UnrelatedTo(RelatedModel.class)。

也可以手动清除缓存，可以基于表或 Model 进行：

```java
org.javalite.activejdbc.cache.QueryCache.instance().purgeTableCache("table");
Model.purgeCache();
```

### 缓存暴露

检索缓存模型实例时，对于同一查询后续调用返回完全相同的实例。换句话说，对于两次相同查询，如果第一次查询后修改了模型实例，即使未保存，第二次查询返回的仍是这个被修改的实例。

### 乐观锁

默认版本列为 version_record，可使用 @VersionColumn 覆盖。

## setter/getter

ActiveJDBC 不通过属性映射表字段，因此，也不需要编写 setter/getter 方法，而是使用 Model.get(fieldName) 和 Model.set(fieldName, value) 替代它们，并且 Model 还提供了许多重载或是绑定不同数据类型的便捷方法。

> 关于 Clob
>
> set() 方法可以将字符串写入 Clob 字段中。
> get() 方法可以从 Clob 字段中读取数据为字符串或强转为 Clob 类型进行处理，而 getString() 方法可将 Clob 字段读取为字符串。
>
> 对于缓存的模型而言，Clob 字段将被事先读取入字符串中，即使连接断开，事后亦可访问。但可能会有较大的内存占用。

get() 方法甚至会将 CLOB 字段自动转换为字符串。

## 主键

### 复合键

使用 @CompositePK 注解复合键，并使用 Model.findByCompositeKeys() 查询（确保参数顺序与注解一致）。

### 覆盖主键

使用 @IdName 注解覆盖主键。

### 主键生成器

使用 @IdGenerator 注解主键生成器。

## 关系

### 一对多

一对多关系默认通过表名和关系字段（可能是外键字段，也可能并未定义外键约束）自动推断，关系字段形式通常是：单数表名_id。

如果关系字段命名不符合约定，可使用 @BelongsTo 注解覆盖：

```java
@BelongsTo(parent = Parent.class, foreignKeyName = "parent_id")
```

从属多个父模型如下配置：

```
@BelongsToParents({
	@BelongsTo(parent=Parent1.class, foreignKeyName="parent1_id"),
	@BelongsTo(parent=Parent2.class, foreignKeyName="parent2_id")
})
```

### 多对多

多对多关系与一对多类似，只是多了一个中间关系表，默认情况下，关系通过两个有关系的表与中间关系表关系字段（可能是外键字段，也可能并未定义外键约束）自动进行推断。

如果命名不符合约定，可使用 @Many2Many 注解覆盖：

```java
@Many2Many(sourceFKName = This.class, 
	other = Other.class, targetFKName = "other_id",
	join = "中间表名")
```

注意：@Many2Many 是单侧的，即其提供了足够信息给框架，不需要在另一端再添加。

### 多态关联

多态关联，简单来说是一种特殊的一对多关系，特殊之处在于，多个父类型关联的子类型结构相似，因此，这些子类型被存储在同一表中，该表不仅有父表 ID，还有父类型字段。

```java
@BelongsToPolymorphic(parents = {Parent1.class, Parent2.class})
```

默认情况下，父类型字段保存的是父模型的全限定名，不过，可以使用覆盖：

```java
@BelongsToPolymorphic(
parents     = { Parent1.class, Parent2.class}, 
typeLabels  = {"Parent1",     "Parent2"} )
```

## 管理时间

如果表中存在 created_at 和 updated_at 列，并且其类型对应 java.sql.Timestamp，那么，框架将自动管理这些时间列。除非使用 model.manageTime(false); 禁用。

## 验证

### 验证方法

通常，我们在静态初始化块中进行属性存在验证，使用 Model.validatePresenceOf() 方法：

```java
public class XXX extends Model {
    static {
    	validatePresenceOf("xxx", "yyy");
    }
}
```

当然，Model 中还包含其他一些 validate 方法。甚至，你可以自定义一个验证器（Validator/ValidatorAdapter）。

```java
public class CustomValidator extends ValidatorAdapter {
   void validate(Model m){
       boolean valid = true;
      // perform whatever validation logic, then add errors to model if validation did not pass:
      if(!valid)
         m.addValidator(this, "custom_error");
    }
}
```

验证方法一般均是静态方法，因此，你也可以在模型类外部调用。

### 附加验证

Model 中的验证方法可能会返回 ValidationBuilder 子类对象，其上声明了一些附加验证方法。

### 触发验证

以下操作会触发验证：

* model.validate()
* model.save()
* model.saveIt()
* Model.createIt()

### 获取验证消息

执行验证后，可通过如下方法获取错误消息：

```java
Map<String, String> errors = model.errors();
String xxxError = errors.get("xxx");
```

### 客制化消息

Model 中的验证方法会返回一个 ValidationBuilder 对象，其 message() 方法可定制验证失败时的消息。

上文已知验证方法可以在模型类外进行调用，因此，客制化消息也可以在外部添加。

**静态参数**

message() 方法不仅可以直接传入消息文本，还可以传入 activejdbc_messages 资源绑定（resource bundle）中的键值。在资源文件中可以使用占位符 {0}、{1} ……它们顺序引用验证器中除第一个属性参数外的第 2、3……参数。

如官网例子中，activejdbc_messages.properties 条目如下：

```properties
temperature.outside.limits = Temperature is outside acceptable limits, while it needs to be between {0} and {1}
```

则如下验证：

```java
validateRange("temp", 10, 2000).message("temperature.outside.limits");
```

获取到的失败消息会是：Temperature is outside acceptable limits, while it needs to be between 10 and 2000。

**动态参数**

当然，占位符可以超出验证器参数数量，超出的占位符需要在获取消息时提供值，条目如下：

```properties
temperature.outside.limits = Temperature is outside acceptable limits, while it needs to be between {0} and {1} for user: {2}
```

使用如上相同的验证，则显然缺少 {2} 的值，因此，需要在获取消息时提供，如下：

```java
String message = temp.errors().get("temp", user.getName());
```

**国际化**

由于可以使用资源文件，所以很容易进行国际化，如需要给 errors() 传入一个 Locale 对象即可获取国际化消息：

```java
String message = temp.errors(new Locale("de", "DE")).get(temp);
```

## 转换器

转换器跟验证器用法类似，通常也放置于模型类的静态初始化块中，常用的有如下一些：

dateFormat()、timestampFormat()、blankToNull()、zeroToNull()……

注意，如果设置的类型错误，将得到一个数据库底层的异常。



# 数据库管理

数据库连接通过 Base 和 DB 类附加到一个本地线程 Map 变量中，Base 总是操作名为“default”的数据库连接。

DB 可以使用 TWR 自动关闭。

不能直接使用连接池，只能通过 DB 打开一个数据源来使用。

## 属性文件配置

属性文件名为 database.properties，放置于 classpath 根目录。

## 环境变量

配置属性前缀用于 ACTIVE_ENV 环境变量中，以指定无参数时，open() 方法所使用的数据库参数。如果该环境变量未配置，默认值为 development。

## 环境变量覆盖

```
ACTIVEJDBC.URL
ACTIVEJDBC.USER
ACTIVEJDBC.PASSWORD
ACTIVEJDBC.DRIVER

ACTIVEJDBC.JNDI
```

注：覆盖属性文件配置。

## 系统变量覆盖

```
activejdbc.url
activejdbc.user
activejdbc.password
activejdbc.driver

activejdbc.jndi
```

注：覆盖属性文件和环境变量配置。

## 指定 schema

```
-Dactivejdbc.default.schema=myschema
```

# 模型常用 API

```java
model.set(name, value)
model.saveIt();
model.findFirst(where, params);
model.where(where, params);
model.delete();
Model.deleteAll();
Model.findAll();
Model.count();
Model.count(query, params);
Model.findBySQL(query, params);
```

## 创建保存

### save() vs. saveIt()

save() 和 saveIt() 在保存之前都会进行校验，区别在于校验失败时，后者抛出异常，而前者不会抛出异常，而校验失败的信息可通过 errors() 方法获取。

### create() vs. createIt()

跟 save() 和 saveIt() 类似。

## 查询

```java
Model.where("columnName = 'value'")
```

### 参数化

```java
Model.where("columnName = ?", "value")
```

### 处理大结果集

```java
Model.findWith(final ModelListener listener, String query, Object ... params)
```

### Dump

```java
Model.findAll().dump();
```

### 限制和排序

```java
lazyList.limit(limit).offset(offset).orderBy(orderBy);
```

### 分页

```java
new Paginator(modelClass, pageSize, query, params).orderBy(orderBy);
paginator.getPage(pageNumber);
paginator.getCurrentPage();
paginator.pageCount();
```

### 延迟加载

默认 ActiveJDBC 查询是延迟加载的。

可急切加载一对多、多对一、多对多的上下级关系：

```java
lazyList.include(ParentOrChildClass);
lazyList.toMaps();
```

## 级联删除

```
// 深层删除
model.deleteCascade();

// 跳过指定关联
associations = Model.getMetaModel().getAssociationForTarget("xxx");
model.deleteCascadeExcept(associations);

// 浅层删除
// 一对多、多态关联，删除当前记录和直接子记录
// 多对多，删除当前记录和关系表记录
model.deleteCascadeShallow();
```



## 批处理

```java
Model.updateAll("columnName = ?", params);
Model.update("columnName = ?", "columnName2 = 'value'", params);
Model.deleteAll();
Model.delete(query, params);

PreparedStatement ps = Base.startBatch(stm);
Base.addBatch(ps, params);
...
Base.executeBatch(ps);
ps.close();
```

## 生命周期

### 注册外部监听

需要实现 CallbackListener 接口或继承 CallbackAdapter，并注册其对象：

```java
Model.callbackWith(callback);
```

### 重写 Model 回调

Model 继承自抽象类 CallbackSupport，与 CallbackAdapter 类似，重写需要的回调方法即可。

从源代码上看，Model 本身的回调是先于外部回调触发的。

## 事务管理

```java
Base.openTransaction();
Base.commitTransaction();
Base.rollbackTransaction();

Base.connection().setAutocommit(false);
```

## 转换成其他文本格式

### XML

model.toXml(pretty, declaration, attributeNames)

当然，调用 include() 方法后，还可以生成依赖对象节点。lazyList 也可以生成 XML 文档。

### JSON

model.toJson(pretty, attributeNames)

类似 XML 转换。