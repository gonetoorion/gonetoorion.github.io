### ubuntu 19.10 recovery模式下连接无线网络(加密类型wap1)

##### 一. 进入root shell
    1. 进入recovery菜单
    2. 选择network Enable networking
    3. 选择root Drop to root shell prompt
    4. 输入用名密码登录系统

##### 二. 连接
    1. 运行iwconfig　命令确认无线网卡是哪个(以下以wlan0为例)
    2. 运行ip link set wlan0 up 启动无线网卡
    3. 运行iw dev wlan0 scan|grep SSID 扫描附近的无线网络，注意SSID并记住(以下SSID以wireless111为例)
    4. 查看这个文件是否存在/etc/wpa_supplicant/wpa_supplicant.conf，如果不存在则运行echo "ctrl_interface=/var/run/wpa_supplicant">/etc/wpa_supplicant/wpa_supplicant.conf
    5.运行  wpa_supplicant -Dwext -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf& 如果出错，请运行ps aux|grep wpa命令来确认是否有这样的进程早已启动，如果启动了可以运行killall wpa_supplicant命令结束该进程，并再次运行wpa_supplicant -Dwext -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf&
    6. 运行wpa_cli -i wlan0
          1. 运行add_network　正常返回数字比如0 (以下均已0为例)
          2. 运行set_network 0 ssid "wlan0" 成功返回ok
          3. 运行set_network 0 psk "密码" 成功返回ok
          4. 运行enable_network 0
          5. 输入q, enter键退出wap_cli
    7. 运行dhclient wlan0　分配ip (dhclient -r可以释放)
    8. 运行ifconfig 查看wlan0是否分配了ip
    9. ping 一个域名来看看网络是否连上了，ping不通则需要看看/etc/resolv.conf文件中的DNS配置，可以设置成8.8.8.8保存退出后，再ping下试试
