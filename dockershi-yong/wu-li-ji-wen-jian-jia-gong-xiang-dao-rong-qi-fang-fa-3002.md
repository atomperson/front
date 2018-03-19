# 物理机文件共享到容器方法

1. **第一步：物理机文件夹共享至虚拟机（容器宿主机）**
    
   命令：
            
       docker-machine create --virtualbox-share-folder "D:\dockerFolder:www" testBox 
       
   在创建容器的时候，需要加入 --virtualbox-share-folder 命令指定本地文件共享至虚拟机的什么目录，即可完成物理机与虚拟机间的文件夹共享。
       
2. **第二步：虚拟机（宿主机）与容器共享文件夹**
    
   命令：
        
       docker run -itd --name test -v //www://www centos bash
            
   路径上使用//是windows下的情况,linux下使用/www:/www即可，-v就是docker用来数据卷共享。
        

            
    