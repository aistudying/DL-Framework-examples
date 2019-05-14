Environment
```
CUDA9.0 
cuDNN7 
nccl2
pytorch >= 0.4.0
```
### MNIST Example  
node01
```
$ python main.py --init-method tcp://node01:23456 --rank 0 --world-size 3
```
node02
```
$ python main.py --init-method tcp://node01:23456 --rank 1 --world-size 3
```
node03
```
$ python main.py --init-method tcp://node01:23456 --rank 2 --world-size 3
```
