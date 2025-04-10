---
title: Shell 脚本 | 部署 Java 应用
author: since2014
date: 2022-09-23 15:11:00 +0800
categories: [Technology, Linux]
tags: [shell, deploy, script, java]
render_with_liquid: false
---

## 查找进程并杀掉进程

```shell
# 查找并杀死旧的Java进程
PID=$(ps -ef | grep $APP_NAME | grep -v grep | awk '{ print $2 }')
if [ -z "$PID" ]
then
    echo "Application is already stopped."
else
    echo "Killing $PID"
    kill $PID
fi

# 查找并杀死旧的Java进程(更好的方式)
# 获取Java进程的PID
pid=$(pgrep -f "java -jar $JAR_NAME")

# 判断PID是否存在
if [ -z "$pid" ]; then
  echo "Java进程不存在"
else
  # 发送SIGTERM信号给Java进程
  kill -15 $pid

  # 判断Java进程是否关闭
  if [ $? -eq 0 ]; then
    echo "Java进程已成功关闭"
  else
    echo "无法关闭Java进程"
  fi
fi
```

## 测试环境示例

```shell
#!/bin/bash
#定义变量
REPO_DIR="/servers/since-web3/git_reposit/app-services"  # 代码仓库目录
BRANCH_NAME="develop" #代码分支
JAR_NAME="app-admin-1.0.0.jar"  # jar包名称
BASE_DIR="/servers/since-web3/app-services-admin" #应用目录
BACKUP_DIR="/servers/since-web3/bak"  # 备份目录
LOG_NAME="airdrop.log" #日志名称
ENV="test" #环境

# 备份原有jar包
cd $BASE_DIR 
date=$(date +%Y%m%d)
if [ -f $JAR_NAME ]; then
	    echo "Backing up original jar..."
	    cp $JAR_NAME $BACKUP_DIR/${date}_$JAR_NAME
	    echo "${date}_$JAR_NAME"
	    cp $LOG_NAME $BACKUP_DIR/${date}_$LOG_NAME
fi

# 进入代码仓库目录
cd $REPO_DIR

branch=$(git rev-parse --abbrev-ref HEAD)

# 如果当前分支不是test分支，切换分支
if [ "$branch" != "$BRANCH_NAME" ]; then
	    echo "Current branch $branch !!!"
	        echo "Checkout to $BRANCH_NAME"
		    git checkout $BRANCH_NAME
fi

# 更新代码
echo "Pulling latest code..."
git pull

# 打包应用
echo "Packing application..."
mvn clean package

cp app-admin/target/$JAR_NAME $BASE_DIR

#进入应用主目录
cd $BASE_DIR 

# 找到并杀掉当前的java进程
echo "Killing current java process..."
# 获取Java进程的PID
pid=$(pgrep -f "java -jar $JAR_NAME")

# 判断PID是否存在
if [ -z "$pid" ]; then
  echo "Java进程不存在"
else
  # 发送SIGTERM信号给Java进程
  kill -15 $pid

  # 判断Java进程是否关闭
  if [ $? -eq 0 ]; then
    echo "Java进程已成功关闭"
  else
    echo "无法关闭Java进程"
  fi
fi

# 等待一段时间让java进程关闭
sleep 20

# 启动新的应用
echo "Starting new application..."
nohup java -jar $JAR_NAME --spring.profiles.active=$ENV > $LOG_NAME 2>&1 &

echo "Deployment is complete."
```

## 正式环境使用示例

```shell
#!/bin/bash
#定义变量
REPO_DIR="/servers/git_reposit/app-services"  # 代码仓库目录
BRANCH_NAME="main" #代码分支
JAR_NAME="app-admin-1.0.0.jar"  # jar包名称
BASE_DIR="/servers/services/since-web3/app-admin" #应用目录
BACKUP_DIR="/servers/services/since-web3/app-admin/bak"  # 备份目录
LOG_NAME="app-admin.log" #日志名称
ENV="prd" #环境

# 备份原有jar包
cd $BASE_DIR 
# %Y%m%d不能加双引号""
date=$(date +%Y%m%d)
if [ -f $JAR_NAME ]; then
	    echo "Backing up original jar..."
	    cp $JAR_NAME $BACKUP_DIR/$date_$JAR_NAME
		  cp $LOG_NAME $BACKUP_DIR/$date_$LOG_NAME
fi

# 进入代码仓库目录
cd $REPO_DIR

branch=$(git rev-parse --abbrev-ref HEAD)

# 如果当前分支不是test分支，切换分支
if [ "$branch" != "$BRANCH_NAME" ]; then
	    echo "Current branch $branch !!!"
	        echo "Checkout to $BRANCH_NAME"
		    git checkout $BRANCH_NAME
fi

# 更新代码
echo "Pulling latest code..."
git pull

# 打包应用
echo "Packing application..."
mvn clean package

cp app-admin/target/$JAR_NAME $BASE_DIR

#进入应用主目录
cd $BASE_DIR 

# 找到并杀掉当前的java进程
echo "Killing current java process..."
# 获取Java进程的PID
pid=$(pgrep -f "java -jar $JAR_NAME")

# 判断PID是否存在
if [ -z "$pid" ]; then
  echo "Java进程不存在"
else
  # 发送SIGTERM信号给Java进程
  kill -15 $pid

  # 判断Java进程是否关闭
  if [ $? -eq 0 ]; then
    echo "Java进程已成功关闭"
  else
    echo "无法关闭Java进程"
  fi
fi

# 等待一段时间让java进程关闭
sleep 20

# 启动新的应用
echo "Starting new application..."
nohup java -jar $JAR_NAME --spring.profiles.active=$ENV > $LOG_NAME 2>&1 &

echo "Deployment is complete."
```

```shell
#!/bin/bash
#定义变量
REPO_DIR="/servers/git_reposit/app-services"  # 代码仓库目录
BRANCH_NAME="main" #代码分支
JAR_NAME="app-task-1.0.0.jar"  # jar包名称
BASE_DIR="/servers/services/since-web3/app-task" #应用目录
BACKUP_DIR="/servers/services/since-web3/app-task/bak"  # 备份目录
LOG_NAME="app-task.log" #日志名称
ENV="prd" #环境

# 备份原有jar包
cd $BASE_DIR 
# %Y%m%d不能加双引号""
date=$(date +%Y%m%d)
if [ -f $JAR_NAME ]; then
	    echo "Backing up original jar..."
	    cp $JAR_NAME $BACKUP_DIR/$date_$JAR_NAME
		  cp $LOG_NAME $BACKUP_DIR/$date_$LOG_NAME
fi

# 进入代码仓库目录
cd $REPO_DIR

branch=$(git rev-parse --abbrev-ref HEAD)

# 如果当前分支不是test分支，切换分支
if [ "$branch" != "$BRANCH_NAME" ]; then
	    echo "Current branch $branch !!!"
	        echo "Checkout to $BRANCH_NAME"
		    git checkout $BRANCH_NAME
fi

# 更新代码
echo "Pulling latest code..."
git pull

# 打包应用
echo "Packing application..."
mvn clean package

cp app-task/target/$JAR_NAME $BASE_DIR

#进入应用主目录
cd $BASE_DIR 

# 找到并杀掉当前的java进程
echo "Killing current java process..."
# 获取Java进程的PID
pid=$(pgrep -f "java -jar $JAR_NAME")

# 判断PID是否存在
if [ -z "$pid" ]; then
  echo "Java进程不存在"
else
  # 发送SIGTERM信号给Java进程
  kill -15 $pid

  # 判断Java进程是否关闭
  if [ $? -eq 0 ]; then
    echo "Java进程已成功关闭"
  else
    echo "无法关闭Java进程"
  fi
fi

# 等待一段时间让java进程关闭
sleep 20

# 启动新的应用
echo "Starting new application..."
nohup java -jar $JAR_NAME --spring.profiles.active=$ENV > $LOG_NAME 2>&1 &

echo "Deployment is complete."
```