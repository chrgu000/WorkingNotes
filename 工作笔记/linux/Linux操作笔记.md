# Linux操作笔记

## XShell上下载和上传文件

1. linux工具**lrzsz**，使用linux命令（yum -y install lrzsz）

2. **下载**    Linux命令行输入    sz `文件名`     回车

   打开一个文件选择框，确定保存路径即可

3. **上传**    Linux命令行输入    rz   回车   

   打开文件选择框，选择上传的文件，点击打开即可
   
   
   
   下载多个文件可将其进行**压缩**再下载。
   
   tar  -cvf    filename.tar     ./*
   
   zip  -r  filename.zip    ./*