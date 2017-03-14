tracker和storage分别运行
```
docker run -dit --restart=always --network=host -p 181:80 -p 22122:22122 -v ~/fastdfs/tracker_data:/fastdfs/tracker/data -v ~/fdfs:/etc/fdfs/ --name tracker 21jhf/fastdfs-tracker
docker run -dit --restart=always --network=host -p 281:80 -p 23000:23000 -p 11411:11411 -e TRACKER_SERVER=192.168.1.185:22122 -v ~/fastdfs/storage_data:/fastdfs/storage/data -v ~/fdfs:/etc/fdfs/ -v ~/fdht:/etc/fdht --name u_group1_storage1 21jhf/fastdfs-storage-dht
```

配置文件放置在宿主机~/fdfs 和~/fdht 目录，数据存储在~/fastdfs/storage/data目录
