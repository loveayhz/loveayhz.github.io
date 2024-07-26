树莓派到手装完系统后的操作记录：

1.安装vim，[修改vim无法复制的问题](https://ayhz.art/article/1213)

```
sudo apt-get install vim
pi@raspberrypi2:~ $ whereis vim
vim: /usr/bin/vim /etc/vim /usr/share/vim /usr/share/man/man1/vim.1.gz
pi@raspberrypi2:~ $ cd /usr/share/vim
pi@raspberrypi2:/usr/share/vim $ ls
addons  registry  vim90
pi@raspberrypi2:/usr/share/vim $ cd vim90/
pi@raspberrypi2:/usr/share/vim/vim90 $ ls
autoload       compiler      delmenu.vim  filetype.vim  ftplugin.vim        import      indoff.vim  macros     optwin.vim  print        synmenu.vim  vimrc_example.vim
bugreport.vim  debian.vim    doc          ftoff.vim     ftplugof.vim        indent      keymap      menu.vim   pack        scripts.vim  syntax
colors         defaults.vim  evim.vim     ftplugin      gvimrc_example.vim  indent.vim  lang        mswin.vim  plugin      spell        tutor
pi@raspberrypi2:/usr/share/vim/vim90 $ cd ~
pi@raspberrypi2:~ $ sudo vim /usr/share/vim/vim90/defaults.vim
```

2.修改源，[改为清华源]()

```
pi@raspberrypi2:~ $ sudo vim /etc/apt/sources.list
pi@raspberrypi2:~ $ sudo vim /etc/apt/sources.list.d/raspi.list
pi@raspberrypi2:~ $ sudo apt update -y
pi@raspberrypi2:~ $ sudo apt upgrade -y
pi@raspberrypi2:~ $ systemctl --user daemon-reload && \
    systemctl --user restart rpi-connect.service
```

3.[修改root用户密码](https://blog.zydyh.net/?p=423)：

```
pi@raspberrypi2:/etc/apt/sources.list.d $ sudo passwd root
New password: 
Retype new password: 
passwd: password updated successfully
pi@raspberrypi2:/etc/apt/sources.list.d $ sudo passwd --unlock root
Usage: passwd [options] [LOGIN]

Options:
  -a, --all                     report password status on all accounts
  -d, --delete                  delete the password for the named account
  -e, --expire                  force expire the password for the named account
  -h, --help                    display this help message and exit
  -k, --keep-tokens             change password only if expired
  -i, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -l, --lock                    lock the password of the named account
  -n, --mindays MIN_DAYS        set minimum number of days before password
                                change to MIN_DAYS
  -q, --quiet                   quiet mode
  -r, --repository REPOSITORY   change password in REPOSITORY repository
  -R, --root CHROOT_DIR         directory to chroot into
  -S, --status                  report password status on the named account
  -u, --unlock                  unlock the password of the named account
  -w, --warndays WARN_DAYS      set expiration warning days to WARN_DAYS
  -x, --maxdays MAX_DAYS        set maximum number of days before password
                                change to MAX_DAYS
```

4.安装docker：https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/

常规安装 `sudo curl -sSL https://get.docker.com | sh`失败，

```
pi@raspberrypi2:~ $ sudo curl -sSL https://get.docker.com | sh
curl: (28) Failed to connect to get.docker.com port 443 after 134152 ms: Couldn't connect to server
```

问题还是在于网络，于是设置了HTTPS_PROXY和HTTP_PROXY,还是不行

```
pi@raspberrypi2:~ $ export HTTP_PROXY=http://192.168.1.7:10809
pi@raspberrypi2:~ $ export HTTPS_PROXY=http://192.168.1.7:10809
pi@raspberrypi2:~ $ sudo curl -sSL https://get.docker.com | sh
# Executing docker install script, commit: 6d9743e9656cc56f699a64800b098d5ea5a60020
+ sudo -E sh -c apt-get update -qq >/dev/null
+ sudo -E sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
+ sudo -E sh -c install -m 0755 -d /etc/apt/keyrings
+ sudo -E sh -c curl -fsSL "https://download.docker.com/linux/debian/gpg" -o /etc/apt/keyrings/docker.asc
+ sudo -E sh -c chmod a+r /etc/apt/keyrings/docker.asc
+ sudo -E sh -c echo "deb [arch=arm64 signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian bookworm stable" > /etc/apt/sources.list.d/docker.list
+ sudo -E sh -c apt-get update -qq >/dev/null
W: Failed to fetch https://download.docker.com/linux/debian/dists/bookworm/InRelease  Could not connect to download.docker.com:443 (2a03:2880:f102:183:face:b00c:0:25de), connection timed out Could not connect to download.docker.com:443 (185.60.216.169), connection timed out
W: Some index files failed to download. They have been ignored, or old ones used instead.
+ sudo -E sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-ce-rootless-extras docker-buildx-plugin >/dev/null
E: Package 'docker-ce' has no installation candidate
E: Package 'docker-ce-cli' has no installation candidate
E: Unable to locate package containerd.io
E: Couldn't find any package by glob 'containerd.io'
E: Couldn't find any package by regex 'containerd.io'
E: Unable to locate package docker-compose-plugin
E: Package 'docker-ce-rootless-extras' has no installation candidate
E: Unable to locate package docker-buildx-plugin
pi@raspberrypi2:~ $ sudo curl -sSL https://get.docker.com | sh
# Executing docker install script, commit: 6d9743e9656cc56f699a64800b098d5ea5a60020
+ sudo -E sh -c apt-get update -qq >/dev/null
W: Failed to fetch https://download.docker.com/linux/debian/dists/bookworm/InRelease  Could not connect to download.docker.com:443 (2a03:2880:f102:183:face:b00c:0:25de), connection timed out Could not connect to download.docker.com:443 (185.60.216.169), connection timed out
W: Some index files failed to download. They have been ignored, or old ones used instead.
+ sudo -E sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
+ sudo -E sh -c install -m 0755 -d /etc/apt/keyrings
+ sudo -E sh -c curl -fsSL "https://download.docker.com/linux/debian/gpg" -o /etc/apt/keyrings/docker.asc
+ sudo -E sh -c chmod a+r /etc/apt/keyrings/docker.asc
+ sudo -E sh -c echo "deb [arch=arm64 signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian bookworm stable" > /etc/apt/sources.list.d/docker.list
+ sudo -E sh -c apt-get update -qq >/dev/null
W: Failed to fetch https://download.docker.com/linux/debian/dists/bookworm/InRelease  Could not connect to download.docker.com:443 (2a03:2880:f102:183:face:b00c:0:25de), connection timed out Could not connect to download.docker.com:443 (185.60.216.169), connection timed out
W: Some index files failed to download. They have been ignored, or old ones used instead.
+ sudo -E sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-ce-rootless-extras docker-buildx-plugin >/dev/null
E: Package 'docker-ce' has no installation candidate
E: Package 'docker-ce-cli' has no installation candidate
E: Unable to locate package containerd.io
E: Couldn't find any package by glob 'containerd.io'
E: Couldn't find any package by regex 'containerd.io'
E: Unable to locate package docker-compose-plugin
E: Package 'docker-ce-rootless-extras' has no installation candidate
E: Unable to locate package docker-buildx-plugin
```

找到清华大学镜像站docker的安装教程：https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/

教程使用注意：比较人性化的提供了是否使用sudo，ipv4还是v6,系统发行版本等选项可以进行更改，

我们使用树莓派的官方系统，所以参考 `Debian/Ubuntu/Raspbian 用户`

如果我们没有选择sudo，则需要切换至root用户安装，刚安装的树莓派系统只有pi用户，使用pi用户登录后

`sudo passwd root`更改root用户密码，

`sudo passwd --unlock root`解锁root用户，

`sudo -i`切换root用户

5.设置静态ip

dhcp的方式参考：https://blog.csdn.net/m0_47673868/article/details/132410559，如下，我使用最新版的树莓派系统，似乎没有启用dhcpd服务，所以并没有用

```
sudo vim /etc/dhcpcd.conf
interface wlan0
static ip_address=192.168.1.42/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1

interface eth0
static ip_address=192.168.1.142/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```

使用nmcli的方式：[Linux 中 nmcli 命令使用详解_linux shell_脚本之家](https://www.jb51.net/jiaoben/318584xui.htm)

```Linux
pi@raspberrypi2:~ $ systemctl status NetworkManager
● NetworkManager.service - Network Manager
     Loaded: loaded (/lib/systemd/system/NetworkManager.service; enabled; preset: enabled)
     Active: active (running) since Thu 2024-07-18 20:03:24 CST; 9min ago
       Docs: man:NetworkManager(8)
   Main PID: 677 (NetworkManager)
      Tasks: 3 (limit: 3910)
        CPU: 881ms
     CGroup: /system.slice/NetworkManager.service
             └─677 /usr/sbin/NetworkManager --no-daemon

Jul 18 20:03:37 raspberrypi2 NetworkManager[677]: <info>  [1721304217.3022] device (docker0): state change: unavailable -> disconnected (reason 'connection-assumed', sys-iface-state: 'extern>
Jul 18 20:03:37 raspberrypi2 NetworkManager[677]: <info>  [1721304217.3045] device (docker0): Activation: starting connection 'docker0' (62dc214c-0abe-45d8-9356-b7d5cbcfc558)
Jul 18 20:03:37 raspberrypi2 NetworkManager[677]: <info>  [1721304217.3048] device (docker0): state change: disconnected -> prepare (reason 'none', sys-iface-state: 'external')
Jul 18 20:03:37 raspberrypi2 NetworkManager[677]: <info>  [1721304217.3054] device (docker0): state change: prepare -> config (reason 'none', sys-iface-state: 'external')
Jul 18 20:03:37 raspberrypi2 NetworkManager[677]: <info>  [1721304217.3059] device (docker0): state change: config -> ip-config (reason 'none', sys-iface-state: 'external')
Jul 18 20:03:37 raspberrypi2 NetworkManager[677]: <info>  [1721304217.3065] device (docker0): state change: ip-config -> ip-check (reason 'none', sys-iface-state: 'external')
Jul 18 20:03:37 raspberrypi2 NetworkManager[677]: <info>  [1721304217.3115] device (docker0): state change: ip-check -> secondaries (reason 'none', sys-iface-state: 'external')
Jul 18 20:03:37 raspberrypi2 NetworkManager[677]: <info>  [1721304217.3120] device (docker0): state change: secondaries -> activated (reason 'none', sys-iface-state: 'external')
Jul 18 20:03:37 raspberrypi2 NetworkManager[677]: <info>  [1721304217.3133] device (docker0): Activation: successful, device activated.
Jul 18 20:03:38 raspberrypi2 NetworkManager[677]: <info>  [1721304218.1935] agent-manager: agent[0bc0c5cb61420b66,:1.37/org.freedesktop.nm-applet/1000]: agent registered
pi@raspberrypi2:~ $ sudo nmcli -p connection show
======================================
  NetworkManager connection profiles
======================================
NAME                UUID                                  TYPE      DEVICE  
-------------------------------------------------------------------------------------------------------------------
Wired connection 1  634f57f4-a4d9-30e1-8956-ba928c97993e  ethernet  end0  
preconfigured       0c10eaa9-8ee6-4d24-8d22-3816213d80dd  wifi      wlan0   
lo                  bbba6acb-90bc-4b4c-8e83-162fadd39ba9  loopback  lo  
docker0             1cec0f18-62b3-4610-afea-56ec33d18822  bridge    docker0 
pi@raspberrypi2:~ $ hostname -I
192.168.1.8 192.168.1.3 172.17.0.1 2409:8a55:51a1:5150:b536:a16b:a3b:600b 2409:8a55:51a1:5150:4213:8ea:ed34:ded7
# IP地址/24之前的
sudo nmcli c mod 'Wired connection 1' ipv4.addresses 192.168.2.2/24 ipv4.method manual
# 网关
sudo nmcli con mod 'Wired connection 1' ipv4.gateway 192.168.0.1
# DNS服务器
sudo nmcli con mod 'Wired connection 1' ipv4.dns "192.168.0.1"
# 更新有线网络，需要在插网线模式下才能更新成功
sudo nmcli c down 'Wired connection 1' && sudo nmcli c up 'Wired connection 1'
```

```
pi@raspberrypi2:~ $ sudo nmcli -p connection show
======================================
  NetworkManager connection profiles
======================================
NAME                UUID                                  TYPE      DEVICE  
-------------------------------------------------------------------------------------------------------------------
Wired connection 1  634f57f4-a4d9-30e1-8956-ba928c97993e  ethernet  end0  
preconfigured       0c10eaa9-8ee6-4d24-8d22-3816213d80dd  wifi      wlan0   
lo                  ca554f79-c94a-44fd-bf2f-95e7b1cd5784  loopback  lo  
docker0             62dc214c-0abe-45d8-9356-b7d5cbcfc558  bridge    docker0 
pi@raspberrypi2:~ $ sudo nmcli connection modify "preconfigured" ipv4.addresses 192.168.1.42/24 ipv4.method manual
pi@raspberrypi2:~ $ ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:c2:f8:ee:5a  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

end0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.8  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::1645:eed0:2d18:6916  prefixlen 64  scopeid 0x20<link>
        inet6 2409:8a55:51a1:5150:b536:a16b:a3b:600b  prefixlen 64  scopeid 0x0<global>
        ether dc:a6:32:5c:74:0a  txqueuelen 1000  (Ethernet)
        RX packets 195037  bytes 12078246 (11.5 MiB)
        RX errors 0  dropped 184128  overruns 0  frame 0
        TX packets 3808  bytes 293965 (287.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 34  bytes 3358 (3.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 34  bytes 3358 (3.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.3  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::afad:4a8f:7e05:e97e  prefixlen 64  scopeid 0x20<link>
        inet6 2409:8a55:51a1:5150:4213:8ea:ed34:ded7  prefixlen 64  scopeid 0x0<global>
        ether dc:a6:32:5c:74:0b  txqueuelen 1000  (Ethernet)
        RX packets 1436  bytes 188822 (184.3 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 855  bytes 76272 (74.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

pi@raspberrypi2:~ $ sudo nmcli c down 'preconfigured' && sudo nmcli c up 'preconfigured'
Connection 'preconfigured' successfully deactivated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/2)

Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/5)
pi@raspberrypi2:~ $ ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:c2:f8:ee:5a  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

end0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.8  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 1234::1645:eed0:2d18:6916  prefixlen 64  scopeid 0x20<link>
        inet6 1234:8a55:51a1:5150:b536:a16b:a3b:600b  prefixlen 64  scopeid 0x0<global>
        ether ab:a6:32:5c:74:0a  txqueuelen 1000  (Ethernet)
        RX packets 195856  bytes 12141695 (11.5 MiB)
        RX errors 0  dropped 184494  overruns 0  frame 0
        TX packets 4339  bytes 335441 (327.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 34  bytes 3358 (3.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 34  bytes 3358 (3.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.42  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 1234::afad:4a8f:7e05:e97e  prefixlen 64  scopeid 0x20<link>
        inet6 1234:8a55:51a1:5150:4213:8ea:ed34:ded7  prefixlen 64  scopeid 0x0<global>
        ether ab:cd:32:5c:74:0b  txqueuelen 1000  (Ethernet)
        RX packets 1439  bytes 189138 (184.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 900  bytes 82046 (80.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

pi@raspberrypi2:~ $
sudo nmcli connection modify 'Wired connection 1' ipv4.addresses 192.168.1.142/24 ipv4.method manual
sudo nmcli c down 'Wired connection 1' && sudo nmcli c up 'Wired connection 1'
```

这样使用dhcp不行的时候，使用nmcli的方式修改有线和wifi的静态ip，这样三个修改完之后，同时再修改笔记本的ip为静态的。方便我们给树莓派开代理，否则动态的ip每次都要修改，尤其docker的代理设置更不方便，每次需要重启docker.

为raspberry也设置静态服务时发现，它既没有启用dhcpd也没有启用NetworkManager

关于这点：[如何使用树莓派Bookworm系统中配置网络的新方法NetworkManager](https://blog.csdn.net/qq_33919450/article/details/134258422) 说的很清楚，即Bookworm 版本系统中，将使用多年的 dhcpcd 换成了 NetworkManager（以前是在rasp-config中可选）

所以我查看系统发行版本，果然是bullseye而非Bookworm，于是使用raspi-config启用NetworkManager服务，然后再使用nmcli启用静态ip，另外，使用nmtui也可以通过NetworkManager进行配置

不过，尴尬的是raspberry通过raspi-config配置使用NetworkManager后就连接不上ssh了，ip地址也找不到

```
pi@raspberrypi:~ $ systemctl status NetworkManager
● NetworkManager.service - Network Manager
     Loaded: loaded (/lib/systemd/system/NetworkManager.service; disabled; vendor preset: enabled)
    Drop-In: /usr/lib/systemd/system/NetworkManager.service.d
             └─10-dhcpcd.conf
     Active: inactive (dead)
       Docs: man:NetworkManager(8)
pi@raspberrypi:~ $ systemctl status dhcpd
Unit dhcpd.service could not be found.
pi@raspberrypi:~ $ nmtui
NetworkManager is not running.
pi@raspberrypi:~ $ arch
aarch64
pi@raspberrypi:~ $ cat /proc/device-tree/model
Raspberry Pi 4 Model B Rev 1.2pi@raspberrypi:~ $ lsb_release -a
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux 11 (bullseye)
Release:        11
Codename:       bullseye
pi@raspberrypi:~ $ sudo raspi-config
```

连接网线之后重新连接ssh，ifconfig发现wlan0没有ipv4,而且sudo nmcli connection show没有wlan0

```
pi@raspberrypi:~ $ ifconfig
br-ca6290c2c50a: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.19.0.1  netmask 255.255.0.0  broadcast 172.19.255.255
        ether 12:34:58:54:a3:3c  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 abcd::42:faff:fe02:47b1  prefixlen 64  scopeid 0x20<link>
        ether 12:34:fa:02:47:b1  txqueuelen 0  (Ethernet)
        RX packets 17  bytes 1568 (1.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 48  bytes 10022 (9.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

docker_gwbridge: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.18.0.1  netmask 255.255.0.0  broadcast 172.18.255.255
        ether 02:42:49:13:54:b9  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.6  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 abcd::e292:bd12:ad8d:7a3c  prefixlen 64  scopeid 0x20<link>
        inet6 abcd:8a55:51a1:5150:29c0:bb72:d3e9:fcb7  prefixlen 64  scopeid 0x0<global>
        ether 12:a6:32:74:3c:77  txqueuelen 1000  (Ethernet)
        RX packets 304  bytes 32842 (32.0 KiB)
        RX errors 0  dropped 151  overruns 0  frame 0
        TX packets 163  bytes 20877 (20.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 122  bytes 14550 (14.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 122  bytes 14550 (14.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth8adfb82: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 abcd::78b7:ecff:fe3e:dff7  prefixlen 64  scopeid 0x20<link>
        ether 12:b7:ec:3e:df:f7  txqueuelen 0  (Ethernet)
        RX packets 17  bytes 1806 (1.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 58  bytes 11476 (11.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethfd6fb43: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 1234::9c79:b3ff:fe83:bef0  prefixlen 64  scopeid 0x20<link>
        ether 12:79:b3:83:be:f0  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 43  bytes 5834 (5.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 12:bf:83:a9:2e:eb  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0   
pi@raspberrypi:~ $ sudo networkctl list
WARNING: systemd-networkd is not running, output will be incomplete.

IDX LINK            TYPE     OPERATIONAL SETUP
  1 lo              loopback n/a         unmanaged
  2 eth0            ether    n/a         unmanaged
  3 wlan0           wlan     n/a         unmanaged
  4 docker_gwbridge bridge   n/a         unmanaged
  5 br-ca6290c2c50a bridge   n/a         unmanaged
  6 docker0         bridge   n/a         unmanaged
  8 vethfd6fb43     ether    n/a         unmanaged
 10 veth8adfb82     ether    n/a         unmanaged

8 links listed.
pi@raspberrypi:~ $ sudo nmcli connection show
NAME                UUID                                  TYPE      DEVICE  
Wired connection 1  865e1185-89a7-3539-af55-bf06065b89d9  ethernet  eth0  
docker0             b4234078-9016-4c21-962d-2ce79b66bf57  bridge    docker0   
br-ca6290c2c50a     ac576ffc-6ca3-4387-929b-005d74b62537  bridge    br-ca6290c2c50a 
docker_gwbridge     8a62ae80-766f-483f-b675-a297ed127d78  bridge    docker_gwbridge 
sudo nmcli connection modify 'Wired connection 1' ipv4.addresses 192.168.1.141/24 ipv4.method manual
sudo nmcli c down 'Wired connection 1' && sudo nmcli c up 'Wired connection 1'
```

使用 `sudo ifconfig wlan0 up`尝试启用，还是不起作用，于是我试着再次 `sudo raspi-config`配置无线SSID,配置后就可以使用了，不知道原因.可能是bug?

```
pi@raspberrypi:~ $ sudo nmcli connection show
NAME                UUID                                  TYPE      DEVICE  
Wired connection 1  865e1185-89a7-3539-af55-bf06065b89d9  ethernet  eth0  
CMCC-A1-1102        d81636d3-cddd-48a1-9a1b-ea7d084d4284  wifi      wlan0   
docker0             b4234078-9016-4c21-962d-2ce79b66bf57  bridge    docker0   
br-ca6290c2c50a     ac576ffc-6ca3-4387-929b-005d74b62537  bridge    br-ca6290c2c50a 
docker_gwbridge     8a62ae80-766f-483f-b675-a297ed127d78  bridge    docker_gwbridge
sudo nmcli connection modify 'Wired connection 1' ipv4.addresses 192.168.1.141/24 ipv4.method manual
sudo nmcli c down 'Wired connection 1' && sudo nmcli c up 'Wired connection 1'
sudo nmcli connection modify 'CMCC-A1-1102' ipv4.addresses 192.168.1.41/24 ipv4.method manual
sudo nmcli c down 'CMCC-A1-1102' && sudo nmcli c up 'CMCC-A1-1102'
```

这样就可以了。

但是注意：在 `输入密码的时候一定要注意键盘的大小写`，刚才因为键盘不知道什么时候切换了大写连输了好多次密码都错误，最后打字的时候才发现是大写了，真的应该给capsLock也加一个指示灯或者输入法应该有标识大小写。

这样我们的三块树莓派分别设置了静态ip为41,42和43，对应如果接了有线的话就是141,142和143.

接下来给笔记本设置静态ip,同时尝试树莓派docker安装v2rayN或者clash，以及git代理

6.docker配置本地docker registry和代理解决docker镜像无法访问的问题:

(1).配置docker registry的insecure-registries(默认docker pull是https会报错):

```
pi@raspberrypi3:~ $ sudo vim /etc/docker/daemon.json
pi@raspberrypi3:~ $ cat /etc/docker/daemon.json
{
  "registry-mirrors": ["https://xiangpeach123456.mirror.aliyuncs.com"],
  "insecure-registries": ["192.168.1.43:5000"]
}
pi@raspberrypi3:~ $ systemctl daemon-reload
pi@raspberrypi3:~ $ systemctl restart docker
pi@raspberrypi3:~ $ docker pull 192.168.1.43:5000/minio/mc:RELEASE.2021-04-22T17-40-00Z
```

(2)配置docker代理，这里局域网内的笔记本开了v2rayN代理，在参数设置中允许来自局域网的连接√，并且笔记本设置ipv4手动固定为192.168.1.40，默认socks5代理端口号是10808，则http代理端口号为10808+1=10809，如下设置之后且192.168.1.40正常开启着代理，才可正常访问docker registry

```
sudo mkdir -p /etc/systemd/system/docker.service.d
pi@raspberrypi3:~ $ sudo vim /etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=http://192.168.1.40:10809"
Environment="HTTPS_PROXY=http://192.168.1.40:10809"
Environment="NO_PROXY=localhost,127.0.0.1,192.168.1.41,192.168.1.42,192.168.1.43,.ayhz.art,.tuna.tsinghua.edu.cn"

pi@raspberrypi3:~ $ sudo systemctl daemon-reload
pi@raspberrypi3:~ $ sudo systemctl restart docker
pi@raspberrypi3:~ $ docker pull shellspec/openwrt
```

6.docker安装配置frpc因为更改了树莓派的ip,可能导致之前的镜像有一些问题，打了tag之后还是不行。

另外代理也必须开全局才行，而且速度有点慢

```
pi@raspberrypi2:~ $ sudo docker pull 192.168.1.43:5000/snowdreamtech/frpc:0.52.1
Error response from daemon: manifest for 192.168.1.43:5000/snowdreamtech/frpc:0.52.1 not found: manifest unknown: manifest unknown
pi@raspberrypi2:~ $ sudo docker pull snowdreamtech/frpc:0.52.1
Error response from daemon: Get "https://registry-1.docker.io/v2/": net/http: TLS handshake timeout
pi@raspberrypi2:~ $ sudo docker pull snowdreamtech/frpc:0.52.1
0.52.1: Pulling from snowdreamtech/frpc
579b34f0a95b: Pull complete 
114d621e9ab5: Pull complete 
Digest: sha256:7dd470795c91ae3f8ae3a18ea58e40b4ded1b4981b749f29f904a6976bea2960
Status: Downloaded newer image for snowdreamtech/frpc:0.52.1
docker.io/snowdreamtech/frpc:0.52.1
```

一通操作之后，发现raspberrypi和raspberrypi1反而无法连接到frps了，不知道什么原因，是因为docker代理吗？

7.树莓派之间开启密钥免密登录。
