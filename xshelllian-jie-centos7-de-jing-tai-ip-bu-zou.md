# xshell连接centos7的静态IP步骤

因为VMware默认的设置为centos7自动分配IP地址，给开发带来不便，这里直接将IP地址设置成静态的步骤

---

* ![](/assets/import.png)
* ![](/assets/import2.png)
* 进入centos7，输入命令 vi  /etc/sysconfig/network-scripts/ifcfg-ens33，进入配置的界面
* 按i，进入编辑模式，有几个参数是需要注意配置的：                                                                                                               BOOTPROTO="static"
   设置静态
  IPADDR=192.168.175.128
     设置指定的IP地址，是xshell主要用到的连接IP
  NETMASK=255.255.255.0
    子网掩码，默认是这个
  GATEWAY=192.168.175.2
     网关IP，网段要一样，后面一位一般设置为2
  DNS1=116.116.116.116   DNS服务器，一般找你的主要网卡的DNS服务器IP,用于联网                                                              ONBOOT="yes" 用于设置开机启动                                                                                                                                                                                     设置完成之后，按esc，推出编辑模式，再按shift+:两个键，进入vi命令，按wq保存并退出
* 找到打开网络和共享中心，找到VMNet8的网络连接，设置IPV4协议：                                                                                                                     ![](/assets/import3.png)
* 在centos中输入 systemctl restart network  重置 IP的设置，在按入ip addr 找到刚刚自己所在设置的网卡看IP地址是否设置成功： ![](/assets/import4.png)
* 静态设置成功后，在xshell中添加一个新的标签，并在其中如图设置：                                                                                                                         ![](/assets/import5.png)                                                                           在用户身份验证中，输入连接centos的用户密码，然后就可以成功了：                                                                                                                      ![](/assets/import6.png)



