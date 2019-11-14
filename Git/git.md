# git使用小结  

## 1.安装  
[官网](https://git-scm.com/)下载

## 2.创建版本库、链接远程仓库

```git
git init   // 初始化版本库
```

```git
git remote add origin https://github.com/MercurialX/Study-Notes.git // 关联远程库
git push -u origin master                                           // 第一次推送-u
```

```git
git clone https://github.com/MercurialX/Study-Notes.git // 从远程库克隆
```

## 3.完整流程

```git
git clone git@github.com/MercurialX/Study-Notes.git // 从远程库克隆
git checkout -b dev-x                               // 创建并切换到分支dev-x
git add .                                           // 修改添加到缓存区
git commit -a                                       // 缓存区修改提交到版本库
git checkout master                                 // 切回主分支
git pull                                            // 获取远程库最新代码
git checkout dev-x                                  // 切换到工作分支
git rebase master                                   // 更新分支
----------------
git add .
git rebase --continue                               // 假如有冲突解决冲突
git rebase --skip
----------------
git checkout master                                 // 切回主分支
git merge dev-x                                     // 合并dev-x分支
git push (-u) origin master                         // 提交到远程库 初次-u
git branch -d dev-x                                 // 删除本地dev-x分支
```

## 4.其他

```git
git log                 // 查看log
git status              // 查看当前状态
git remote -v           // 查看远程库详细信息
git reset –-hard 版本号 // 强制回退到版本号
git checkout —- file    // 丢弃file的修改
git branch              // 查看所有分支
mkdir                   //
psd                     // 显示当前目录的路径
cat XX                  // 查看XX文件内容
git rm XX               // 删除XX文件
```
