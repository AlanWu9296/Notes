# npm script 基础

## npm init 快速创建项目

如果要修改 package.json，可以直接用编辑器编辑，或者再次运行 npm init，npm 默认不会覆盖修改里面已经存在的信息

npm init -f（意指 --force，或者使用 --yes）告诉 npm 直接跳过参数问答环节，快速生成 package.json

```shell
# set default value
npm config set init.author.email "wangshijun2010@gmail.com" 
npm config set init.author.name "wangshijun" 
npm config set init.author.url "http://github.com/wangshijun" 
npm config set init.license "MIT" 
npm config set init.version "0.1.0"...

# skip the interactive to build package.json file
npm init -f 
```

## npm run 执行任意命令

```shell
# npm test && npm start  equal to npm run test/start
npm test
npm start
# npm run will list all available scripts
npm run
```

当我们运行 npm run xxx 时，基本步骤如下： 从 package.json 文件中读取 scripts 对象里面的全部配置； 以传给 npm run 的第一个参数作为键，本例中为 xxx，在 scripts 对象里面获取对应的值作为接下来要执行的命令，如果没找到直接报错； 在系统默认的 shell 中执行上述命令，系统默认 shell 通常是 bash，windows 环境下可能略有不同

# 运行多个 npm script

## npm script 串行

用 `&&` 符号把多条 npm script 按先后顺序串起来

```js
"test": "npm run lint:js && npm run lint:css && npm run lint:json && npm run lint:markdown && mocha tests/"
```

## npm script 并行

把连接多条命令的 `&&` 符号替换成 `&` 即可

```js
"test": "npm run lint:js & npm run lint:css & npm run lint:json & npm run lint:markdown & mocha tests/"
```

保证异步结果完成后触发，用 &wait

```shell
npm run lint:js & npm run lint:css & npm run lint:json & npm run lint:markdown & mocha tests/ & wait

```

## npm-run-all

```shell
# install
npm i npm-run-all -D
# run sequential
npm-run-all lint:js lint:css lint:json lint:markdown mocha
# run with symbols, this will run all lint:js, lint:css, lint:json ...
npm-run-all lint:* mocha
#run paralell and no need for &wait
npm-run-all --parallel lint:* mocha
```

# npm script 传递参数和添加注释

## npm script 传递参数

```json
// -- is to pass the parameters to the real command
"lint:js": "eslint *.js",
"lint:js:fix": "npm run lint:js -- --fix"
```

## npm script 添加注释

json 天然是不支持添加注释

直接在 script 声明中做手脚，因为命令的本质是 shell 命令（适用于 linux 平台），我们可以在命令前面加上注释

```json
"test": "# 运行所有代码检查和单元测试 \n    npm-run-all --parallel lint:* mocha"
```

# npm script 的钩子

为了方便开发者自定义，npm script 的设计者为命令的执行增加了类似生命周期的机制，具体来说就是 `pre` 和 `post` 钩子脚本

运行 npm run test 的时候，分 3 个阶段： 

1. 检查 scripts 对象中是否存在 pretest 命令，如果有，先执行该命令 
2. 检查是否有 test 命令，有的话运行 test 命令，没有的话报错
3. 检查是否存在 posttest 命令，如果有，执行 posttest 命令

```json
// pretest will automatically do in advance when triggering test
"pretest": "npm run lint",
"test": "mocha tests/",
```

#  npm script 中使用变量

npm 还提供了 `$PATH` 之外的更多的变量，比如当前正在执行的命令、包的名称和版本号、日志输出的级别

```shell
# get all variables
npm run dev

# to get all variables about npm_package
npm run env | grep npm_package | sort
```

```json
// example
"cover:cleanup": "rm -rf coverage && rm -rf .nyc_output",    
"cover:archive": "mkdir -p coverage_archive/$npm_package_version && cp -r coverage/* coverage_archive/$npm_package_version",   
"postcover": "npm run cover:archive && npm run cover:cleanup && opn coverage_archive/$npm_package_version/index.html"
```

## 使用自定义变量

```json
// define the variable
"config": {
    "port": 3000
},
// access the variable with "npm_package_config"
$npm_package_config_port
```

# 用 node.js 脚本替代复杂的 npm script

对于前端工程师来说，使用 Node.js 来编写复杂的 npm script 具有明显的 2 个优势：首先，编写简单的工具脚本对前端工程师来说额外的学习成本很低甚至可以忽略不计，其次，因为 Node.js 本身是跨平台的，用它编写的脚本出现跨平台兼容问题的概率很小

```shell
#install shelljs for using shell
npm i shelljs -D
# npm install shelljs --save-dev
# yarn add shelljs -D
```

shelljs 为我们提供了各种常见命令的跨平台支持，比如 cp、mkdir、rm、cd 等命令，此外，理论上你可以在 Node.js 脚本中使用任何 [npmjs.com](https://link.juejin.im?target=https%3A%2F%2Fwww.npmjs.com) 上能找到的包

```js
// an example
const { rm, cp, mkdir, exec, echo } = require('shelljs'); 
const chalk = require('chalk'); 
console.log(chalk.green('1. remove old coverage reports...')); 
rm('-rf', 'coverage'); 
rm('-rf', '.nyc_output'); 
console.log(chalk.green('2. run test and collect new coverage...')); 
exec('nyc --reporter=html npm run test'); 
console.log(chalk.green('3. archive coverage report by version...')); 
mkdir('-p', 'coverage_archive/$npm_package_version'); 
cp('-r', 'coverage/*', 'coverage_archive/$npm_package_version'); console.log(chalk.green('4. open coverage report for preview...')); 
exec('npm-run-all --parallel cover:serve cover:open');
```

1. 简单的文件系统操作，建议直接使用 shelljs 提供的 cp、rm 等替换； 
2. 部分稍复杂的命令，比如 nyc 可以使用 exec 来执行，也可以使用 istanbul 包来完成； 
3. 在 exec 中也可以大胆的使用 npm script 运行时的环境变量，比如 $npm_package_version；

```json
"scripts": { 
"test": "cross-env NODE_ENV=test mocha tests/",  
"cover": "scripty",     
"cover": "node scripts/cover.js", 
"cover:open": "scripty" 
}
```

在package.json中使其指向新的脚本