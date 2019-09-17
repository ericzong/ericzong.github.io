---
layout: page
title: npm与yarn命令比较
category: Node.js
date: 2019-03-01
author: Eric Zong
---

| No.  | npm(v5)                                                      | Yarn                              |
| ---- | ------------------------------------------------------------ | --------------------------------- |
| 1    | npm init                                                     | yarn init                         |
| 2    | npm link                                                     | yarn link                         |
| 3    | npm outdated                                                 | yarn outdated                     |
| 4    | npm publish                                                  | yarn publish                      |
| 5    | npm run                                                      | yarn run                          |
| 6    | npm cache clean                                              | yarn cache clean                  |
| 7    | npm login                                                    | yarn login                        |
| 8    | npm test                                                     | yarn test                         |
| 9    | npm install                                                  | yarn [install]                    |
| 10   | npm install --no-package-lock                                | yarn install --no-lockfile        |
| 11   | npm install --production                                     | yarn --production                 |
| 12   | npm install *package* --save<br>npm install *package*@latest --save | yarn add *package*                |
| 13   | npm install *package* --save-dev                             | yarn add *package* --dev          |
| 14   | npm install *package* --save-optional                        | yarn add *package* --optional     |
| 15   | npm install *package* --save-exact                           | yarn add *package* --exact        |
| 16   | npm install *package* --global                               | yarn global add *package*         |
| 17   | npm uninstall *package* --save                               | yarn remove *package*             |
| 18   | npm update                                                   | yarn upgrade                      |
| 19   | npm update --global                                          | yarn global upgrade               |
| 20   | npm rebuild                                                  | yarn add --force                  |
| 21   | rm -rf node_modules && npm install                           | yarn upgrade                      |
| 22   | npm version major                                            | yarn version --major              |
| 23   | npm version minor                                            | yarn version --minor              |
| 24   | npm version patch                                            | yarn version --patch              |
| 25   |                                                              | yarn install --flat               |
| 26   |                                                              | yarn install --har                |
| 27   |                                                              | yarn install --pure-lockfile      |
| 28   |                                                              | yarn add *package* --peer         |
| 29   |                                                              | yarn add *package* --tilde        |
| 30   |                                                              | yarn licenses ls                  |
| 31   |                                                              | yarn licenses generate-disclaimer |
| 32   |                                                              | yarn why *package*                |
| 33   |                                                              | yarn upgrade-interactive          |
| 34   | npm xmas                                                     |                                   |
| 35   | npm visnup                                                   |                                   |

注：

`install` 是 `yarn` 的默认行为。

对于 `npm` 而言，如果 `npm config set save true` 则 `--save` 是默认的；而对 `yarn` 来说，修改 `package.json` 是默认行为。