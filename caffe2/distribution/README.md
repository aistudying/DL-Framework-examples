需要安装hiredis依赖  
```
apt-getinstall libhiredis-dev
```
编译时添加redis支持  
```
cmake -DUSE_REDIS=ON ..
```
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


