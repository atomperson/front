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
    
        windows使用cmd(管理员权限)