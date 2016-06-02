解决ubuntu中vi插入模式中键盘不对应问题
 
在终端下输入 `sudo gedit /etc/vim/virmc.tiny`
 
1、在弹出的文本编辑器中找到 `set compatible`
 
将其改为`set nocompatible`
 
2、在`set nocompatible`下一行输入`set backspace=2`
 
3、点击保存，退出。