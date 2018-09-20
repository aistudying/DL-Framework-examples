#### Environment  
CUDA9.0   
cuDNN7  
nccl2  
Tensorflow-gpu 1.8.0  
#### 使用方法：
首先编辑example.py代码中第22行开始的集群描述信息，把所有节点写到描述信息中，主机名或ip地址均可，使用主机名需要在所有节点上配置/etc/hosts文件
然后依次在各节点执行以下命令
```
ps server:
python example.py --job_name="ps" --task_index=0
worker0:
python example.py --job_name="worker" --task_index=0
worker1:
python example.py --job_name="worker" --task_index=1
worker2:
python example.py --job_name="worker" --task_index=2
```
#### 注意：
tensorflow 1.8中的mnist默认下载地址为谷歌的地址，国内网络不可达，需要将/usr/local/lib/python2.7/dist-packages/tensorflow/contrib/learn/python/learn/datasets/mnist.py文件第38行的网址修改为lecun的网址：http://yann.lecun.com/exdb/mnist/
