树莓派的kali系统上来是需要去登陆的，这样造成了很多不便，比如我需要网络来ssh，但没登陆它就没网，而你又不想每次都整个键盘怎么办，别急，kali可以自动登陆的。
kali系统用的登陆器应该是lightDM，你只需要这样配置:
cd /etc/lightdm/
vi lightdm.conf
编辑lightdm.conf这个文件，这个文件中有很多设置，有空了可以研究下，我们这次只弄自动登陆。
网上有很多自动登陆，说的不是很清楚，在下就被坑了一把，因为ligntdm.conf这个文件里上边部分有大量的说明，有一个字段是这样写的
# autologin-user = User to log in with by default (overrides autologin-guest)
# autologin-user-timeout = Number of seconds to wait before loading default user
我看网上说把这两个改一下就好，结果发现并不行，这里有个大坑，再往下找有个[Seat:*]这样的字段，你会看到这个字段下边还有一个这样的注释
# autologin-user=
# autologin-user-timeout=0
对了，修改这里才可以，必须修改[Seat:*]这个字段下边的autologin-user、autologin-user-timeout才管用。
把这两个值修改成这样:
autologin-user=root
autologin-user-timeout=0
如果需要自动登陆其他用户，那把root部分换成其他用户就可以，这样就可以自动登陆了。
