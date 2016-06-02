首先，确保服务器在安装过程中，选择安装openssh软件。

如没有进行安装，请使用以下命令进行安装：

`apt-get install -y openssh-server`

安装完成后，使用root登录系统：

编辑`vim /etc/ssh/sshd_config`文件，将`PermitRootLogin` 的值改为`yes`

因为为root账户设置了密码，所以还要更改`PermitEmptyPasswords`为 `no`。然后`：wq`保存文件，

然后重启ssh服务，如下：

`service ssh restart