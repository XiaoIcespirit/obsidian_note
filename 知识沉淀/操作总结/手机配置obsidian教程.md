1. 手机要下载F-Droid，这是个软件商店。在这上面下载Termux，这是个linux运行终端。下这个：
![](assets/手机配置obsidian教程/file-20260221132637787.png)
2. 有两个方法：第一个是利用obsidian的git插件。在终端里面运行相关命令，具体命令如下：
‘’‘
//这个是进入相关目录
 cd /storage/emulated/0/Documents/obsidian_note/知识沉淀  
 //这个是向系统说明这个目录是安全目录
 git config --global --add safe.directory /storage/emulated/0/Documents/obsidian_note
 //然后运行基础git命令
 git init
 git branch -M main
 git remote add origin https://github.com/XiaoIcespirit/obsidian_note.git
 git config --global --add safe.directory /storage/emulated/0/Documents/obsidian_note
 git pull origin
’‘’
  第二个方法是下载GitSync软件，在这个软件里面连接github，然后选一个文件夹把github项目clone下来，最后用obsidian打开这个项目，也可以达到上传改动文件到github的效果