ubuntu的应用程序快捷启动设置,都在/usr/share/applications/路径下.  可以看到很多*.desktop
下面就建立我们的studio
```
sudo vim /usr/share/applications/Studio.desktop
```
``` 
[Desktop Entry]
Name = Studio
comment= android studio
Exec=/home/yyb/tools/android/android-studio/bin/studio.sh
Icon=/home/yyb/tools/android/android-studio/bin/idea.png
Terminal=false
Type=Application
```
解释:Exec 和  Icon 就是上面准备工作,查到的执行命令路径,和图片的路径.

注意: 每行后面不能有空格,否则无效

## 在linux里如何建立一个快捷方式，连接到另一个目录 

用软链接
ln -s 源目录 目标快捷方式
