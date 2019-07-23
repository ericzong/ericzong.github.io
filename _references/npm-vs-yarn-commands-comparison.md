---
layout: page
title: npm与yarn命令比较
category: Node.js
date: 2019-03-01
author: Eric Zong
---

| No.  | npm(v5)                                                      | Yarn                              |
| ---- | ------------------------------------------------------------ | --------------------------------- |
|      | npm init                                                     | yarn init                         |
|      | npm link                                                     | yarn link                         |
|      | npm outdated                                                 | yarn outdated                     |
|      | npm publish                                                  | yarn publish                      |
|      | npm run                                                      | yarn run                          |
|      | npm cache clean                                              | yarn cache clean                  |
|      | npm login                                                    | yarn login                        |
|      | npm test                                                     | yarn test                         |
|      | npm install                                                  | yarn [install]                    |
|      | npm install --no-package-lock                                | yarn install --no-lockfile        |
|      | npm install --production                                     | yarn --production                 |
|      | npm install *package* --save<br>npm install *package*@latest --save | yarn add *package*                |
|      | npm install *package* --save-dev                             | yarn add *package* --dev          |
|      | npm install *package* --save-optional                        | yarn add *package* --optional     |
|      | npm install *package* --save-exact                           | yarn add *package* --exact        |
|      | npm install *package* --global                               | yarn global add *package*         |
|      | npm uninstall *package* --save                               | yarn remove *package*             |
|      | npm update                                                   | yarn upgrade                      |
|      | npm update --global                                          | yarn global upgrade               |
|      | npm rebuild                                                  | yarn add --force                  |
|      | rm -rf node_modules && npm install                           | yarn upgrade                      |
|      | npm version major                                            | yarn version --major              |
|      | npm version minor                                            | yarn version --minor              |
|      | npm version patch                                            | yarn version --patch              |
|      |                                                              | yarn install --flat               |
|      |                                                              | yarn install --har                |
|      |                                                              | yarn install --pure-lockfile      |
|      |                                                              | yarn add *package* --peer         |
|      |                                                              | yarn add *package* --tilde        |
|      |                                                              | yarn licenses ls                  |
|      |                                                              | yarn licenses generate-disclaimer |
|      |                                                              | yarn why *package*                |
|      |                                                              | yarn upgrade-interactive          |
|      | npm xmas                                                     |                                   |
|      | npm visnup                                                   |                                   |

注：

`install` 是 `yarn` 的默认行为。

对于 `npm` 而言，如果 `npm config set save true` 则 `--save` 是默认的；而对 `yarn` 来说，修改 `package.json` 是默认行为。