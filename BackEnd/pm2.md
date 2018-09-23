# Intall

```shell
npm install -g pm2
```

# Directory 

- `$HOME/.pm2` will contain all PM2 related files
- `$HOME/.pm2/logs` will contain all applications logs
- `$HOME/.pm2/pids` will contain all applications pids
- `$HOME/.pm2/pm2.log` PM2 logs
- `$HOME/.pm2/pm2.pid` PM2 pid
- `$HOME/.pm2/rpc.sock` Socket file for remote commands
- `$HOME/.pm2/pub.sock` Socket file for publishable events
- `$HOME/.pm2/conf.js` PM2 Configuration

# 常用命令

### 启动

参数说明：

- `--watch`：监听应用目录的变化，一旦发生变化，自动重启。如果要精确监听、不见听的目录，最好通过配置文件。
- `-i --instances`：启用多少个实例，可用于负载均衡。如果`-i 0`或者`-i max`，则根据当前机器核数确定实例数目。
- `--ignore-watch`：排除监听的目录/文件，可以是特定的文件名，也可以是正则。比如`--ignore-watch="test node_modules "some scripts""`
- `-n --name`：应用的名称。查看应用信息的时候可以用到。
- `-o --output <path>`：标准输出日志文件的路径。
- `-e --error <path>`：错误输出日志文件的路径。
- `--interpreter <interpreter>`：the interpreter pm2 should use for executing app (bash, python...)。比如你用的coffee script来编写应用。

完整命令行参数列表：[地址](http://pm2.keymetrics.io/docs/usage/quick-start/#options)

```
pm2 start app.js --watch -i 2
```

### 重启

```
pm2 restart app.js
```

### 停止

停止特定的应用。可以先通过`pm2 list`获取应用的名字（--name指定的）或者进程id。

```
pm2 stop app_name|app_id
```

如果要停止所有应用，可以

```
pm2 stop all
```

### 删除

类似`pm2 stop`，如下

```
pm2 stop app_name|app_id
pm2 stop all
```

### 查看进程状态

```
pm2 list
```

### 查看某个进程的信息

```
[root@iZ94wb7tioqZ pids]# pm2 describe 0
Describing process with id 0 - name oc-server
┌───────────────────┬──────────────────────────────────────────────────────────────┐
│ status            │ online                                                       │
│ name              │ oc-server                                                    │
│ id                │ 0                                                            │
│ path              │ /data/file/qiquan/over_the_counter/server/bin/www            │
│ args              │                                                              │
│ exec cwd          │ /data/file/qiquan/over_the_counter/server                    │
│ error log path    │ /data/file/qiquan/over_the_counter/server/logs/app-err-0.log │
│ out log path      │ /data/file/qiquan/over_the_counter/server/logs/app-out-0.log │
│ pid path          │ /root/.pm2/pids/oc-server-0.pid                              │
│ mode              │ fork_mode                                                    │
│ node v8 arguments │                                                              │
│ watch & reload    │                                                             │
│ interpreter       │ node                                                         │
│ restarts          │ 293                                                          │
│ unstable restarts │ 0                                                            │
│ uptime            │ 87m                                                          │
│ created at        │ 2016-08-26T08:13:43.705Z                                     │
└───────────────────┴──────────────────────────────────────────────────────────────┘
```

# 配置文件

### 简单说明

- 配置文件里的设置项，跟命令行参数基本是一一对应的。
- 可以选择`yaml`或者`json`文件，就看个人洗好了。
- `json`格式的配置文件，pm2当作普通的js文件来处理，所以可以在里面添加注释或者编写代码，这对于动态调整配置很有好处。
- 如果启动的时候指定了配置文件，那么命令行参数会被忽略。（个别参数除外，比如--env）

### 例子

举个简单例子，完整配置说明请参考[官方文档](http://pm2.keymetrics.io/docs/usage/pm2-doc-single-page/)。

```
{
  "name"        : "fis-receiver",  // 应用名称
  "script"      : "./bin/www",  // 实际启动脚本
  "cwd"         : "./",  // 当前工作路径
  "watch": [  // 监控变化的目录，一旦变化，自动重启
    "bin",
    "routers"
  ],
  "ignore_watch" : [  // 从监控目录中排除
    "node_modules", 
    "logs",
    "public"
  ],
  "watch_options": {
    "followSymlinks": false
  },
  "error_file" : "./logs/app-err.log",  // 错误日志路径
  "out_file"   : "./logs/app-out.log",  // 普通日志路径
  "env": {
      "NODE_ENV": "production"  // 环境参数，当前指定为生产环境
  }
}
```

# 环境切换

在实际项目开发中，我们的应用经常需要在多个环境下部署，比如开发环境、测试环境、生产环境等。在不同环境下，有时候配置项会有差异，比如链接的数据库地址不同等。

对于这种场景，pm2也是可以很好支持的。首先通过在配置文件中通过`env_xx`来声明不同环境的配置，然后在启动应用时，通过`--env`参数指定运行的环境。

### 环境配置声明

首先，在配置文件中，通过`env`选项声明多个环境配置。简单说明下：

- `env`为默认的环境配置（生产环境），`env_dev`、`env_test`则分别是开发、测试环境。可以看到，不同环境下的`NODE_ENV`、`REMOTE_ADDR`字段的值是不同的。
- 在应用中，可以通过`process.env.REMOTE_ADDR`等来读取配置中生命的变量。

```
  "env": {
    "NODE_ENV": "production",
    "REMOTE_ADDR": "http://www.example.com/"
  },
  "env_dev": {
    "NODE_ENV": "development",
    "REMOTE_ADDR": "http://wdev.example.com/"
  },
  "env_test": {
    "NODE_ENV": "test",
    "REMOTE_ADDR": "http://wtest.example.com/"
  }
```

### 启动指明环境

假设通过下面启动脚本（开发环境），那么，此时`process.env.REMOTE_ADDR`的值就是相应的 <http://wdev.example.com/> 

```
pm2 start app.js --env dev
```