# autoSubmit
[![Go_weekdays](https://github.com/DeleteMemoryyy/autoSubmit/actions/workflows/go_weekdays.yml/badge.svg)](https://github.com/DeleteMemoryyy/autoSubmit/actions/workflows/go_weekdays.yml) [![Go_weekends](https://github.com/DeleteMemoryyy/autoSubmit/actions/workflows/go_weekends.yml/badge.svg)](https://github.com/DeleteMemoryyy/autoSubmit/actions/workflows/go_weekends.yml)

本项目通过读取环境变量中的学号和密码，依次执行:
- 登录portal获取`portalToken`
- 通过token登录portal获取cookie
- 带着cookie访问出入校报备，获取`simsoToken`
- 通过`simsoToken`登录simso系统，获取`sid`
- 带着`sid`访问出入校报备小程序并填报

## usage

**强烈建议使用自动执行**


### github actions 自动执行

配置文件在[go.yml](.github/workflows/go.yml)，目前配置为每天北京时间8点执行一次。

#### github actions 配置

- fork本项目
- 在自己的repo下Settings/Secrets中设置USERNAME和PASSWORD，分别为学号和密码
- fork的项目会默认关闭actions，需手动点击repo页的actions以enable


### local run

#### build

建议使用`go >= 1.13`

```shell script
git clone https://github.com/yzs981130/autoSubmit.git
cd autoSubmit
go build
```

#### run

##### 环境变量方法设置参数
- 环境变量`USERNAME`：学号
- 环境变量`PASSWORD`：密码

```shell script
USERNAME=xxx PASSWORD=xxx ./autosubmit
```

##### 命令行传参设置
```shell script
$ ./autosubmit -h
Usage of ./autosubmit:
  -password string
    	portal密码
  -reason string
    	出入校事由 (default "西市买鞍鞯")
  -track string
    	出校行动轨迹 (default "北大西门-畅春园-北大西门")
  -username string
    	学号
```

```shell script
./autosubmit -username=1900012345 -password=dashabi -reason "玩" -track "康博斯-CBD-公主楼"
```

注意：环境变量只能传学号和密码。

如果成功，会显示如下log：
```shell script
portal登录成功
simso登录成功
提交成功
提交成功
```
两个`提交成功`分别为出校和入校备案成功，如果日志不同则可能失败，请在issue中反馈；后续也可能会添加debug信息和错误处理

