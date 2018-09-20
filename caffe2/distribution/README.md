> caffe2分布式提供了redis和共享存储两种方式来实现节点间的通信，下面的例子基于redis方式实现节点间的通信与同步。
### 安装配置Redis server
```
Ubuntu:
sudo apt-get -y install redis-server redis-tools

sudo vim /etc/redis/redis.conf
找到 bind 127.0.0.1 这一行
修改为 bind 0.0.0.0
这样表示允许远程连接访问

重新启动redis-server，并将其设置为开机启动
sudo systemctl restart redis-server
sudo systemctl enable redi-server
```
### 编译安装caffe2
```
apt-get install -y --no-install-recommends \
      build-essential \
      cmake \
      git \
      libgoogle-glog-dev \
      libgtest-dev \
      libiomp-dev \
      libleveldb-dev \
      liblmdb-dev \
      libopenmpi-dev \
      libsnappy-dev \
      libprotobuf-dev \
      openmpi-bin \
      openmpi-doc \
      protobuf-compiler 
```

#### Python2.7安装
```
apt-get install -y --no-install-recommends python-dev python-pip libopencv-dev \
apt-get install python-setuptools
pip install --user \
      future \
      numpy \
      protobuf \
      networkx \
      enum
apt-get install -y --no-install-recommends libgflags-dev

查看python的软连接，看python是否对应当前的版本
Python2.7： ln -s /usr/bin/python2.7 /usr/bin/python
```
#### 安装hiredis依赖  
```
由于要运行基于redis同步节点的分布式程序，所以需要此依赖
apt-get install libhiredis-dev
```
#### 下载源码并编译
```
cd /usr/local
git clone --recursive https://github.com/pytorch/pytorch.git && cd pytorch
git submodule update --init
mkdir build && cd build
cmake -DUSE_REDIS=ON ..
make install -j 12
```

###执行分布式程序
#### 节点0执行：
    python /usr/local/lib/python2.7/dist-packages/caffe2/python/examples/resnet50_trainer.py \
    --train_data /mnt/mnist_train_lmdb/ \
    --test_data /mnt/mnist_test_lmdb/ \
    --db_type lmdb \
    --gpus 0 \
    --num_gpus 1 \
    --num_shards 2 \
    --shard_id 0 \
    --run_id 0000 \
    --redis_host 192.168.100.11 \
    --redis_port 6379
    
#### 节点1执行：
    python /usr/local/lib/python2.7/dist-packages/caffe2/python/examples/resnet50_trainer.py \
    --train_data /mnt/mnist_train_lmdb/ \
    --test_data /mnt/mnist_test_lmdb/ \
    --db_type lmdb \
    --gpus 0 \
    --num_gpus 1 \
    --num_shards 2 \
    --shard_id 1 \
    --run_id 0000 \
    --redis_host 192.168.100.11 \
    --redis_port 6379
> 注：每次运行分布式程序时需要更换run_id,因为运行完后，redis会留有run_id信息,可以在redis服务器上使用``redis-cli``命令连接上redis数据库，使用``keys * `` 查询数据库中的key
