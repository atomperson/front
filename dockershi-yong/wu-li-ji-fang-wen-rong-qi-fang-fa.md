# 物理机访问容器方式

    1. **通过宿主机docker-machine的端口映射**
        
       在run的时候通过-p指定宿主机映射容器对应哪个端口。
       
           docker run -itd --name test -p 80:8080 centos bash
              
       也可以一次绑定多个端口对应关系。
       
           docker run -itd --name test -p 80:80 -p 8080:8080 centos bash // 多个-p同时使用
           
       还可以随机分配端口映射 -P 参数配置。
           
           docker run -itd --name test -P centos bash 
           
       然后外部物理机器访问容器（container）就可以利用容器宿主机（虚拟机）的ip进行访问。
       
    2. **物理机改变路由(route)去访问容器**
    
        windows使用cmd(管理员权限)对route操作。
        
            1.route add 172.17.0.0 mask 255.255.255.0 192.168.99.100   // 第一个ip是目标网段 子网掩码 最后一个ip是网关（暂可理解为物理之间的通道即虚拟机）
            2.route delete 172.17.0.0  // 删除对应网段的记录
            3.route change 172.17.0.0 mask 255.255.255.0 192.168.99.110 // 修改对应网段的映射关系
            4.route print 192.168.95.22 // 打印当前物理机route表
            
        route 有 print、add、change、delete，四个方法对路由操作。
        
        此方法对多容器在一个宿主机上有效! 多虚拟机下的容器都会以172.17.0.0网段为主。
        
    3. **物理机的局域网内其他机器访问容器（扩展）**
    
        若要在局域网内所有机器可访问docker内的容器，需要在docker所在机器上进行端口映射。
        
        用法：
        
           netsh interface portproxy add v4tov4 [listenport=]<integer>|<servicename>
                                          [connectaddress=]<IPv4 address>|<hostname>       
                                          [[connectport=]<integer>|<servicename>]
                                          [[listenaddress=]<IPv4 address>|<hostname>]
                                          [[protocol=]tcp]
        
        参数：
        
                        标记                        值
                    listenport                - IPv4 侦听端口
                  connectaddress              - IPv4 连接地址
                   connectport