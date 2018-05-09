# Git常用命令

## 一：第一次使用git
### 1：首先给git上创建新的仓库Repository
### 2：复制新建仓库的https地址
https://github.com/liuyanliang2015/test.git
### 3：本地使用git命名克隆仓库
git clone https://github.com/liuyanliang2015/test.git
### 4：初始化
git init
### 5：添加README文件
touch README.md
### 5：上传项目
上传单个文件：git add README.md
上传所有文件：git add .
### 6：提交文件，备注提交说明
git commit -m "first commit"
![](https://github.com/liuyanliang2015/BertNote/blob/master/pics/git-commit.png)
### 7:关联远程仓库
$ git remote add origin https://github.com/liuyanliang2015/test.git
### 8：推送内容到远程仓库
git push -u origin master
![](https://github.com/liuyanliang2015/BertNote/blob/master/pics/git-push.png)

## 二：再次编辑
### 1:提交文件前，先更新本地仓库
git pull
### 2：上传本地文件
上传单个文件：git add README.md
上传所有文件：git add .
### 3：推送内容到远程仓库
git push -u origin master
![](https://github.com/liuyanliang2015/BertNote/blob/master/pics/git-push.png)





