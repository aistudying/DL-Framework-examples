Environment
```
CUDA9.0 
cuDNN7 
nccl2
pytorch >= 0.4.0
```
### MNIST Example  
Rank 0
```
$ python main.py --init-method tcp://node01:23456 --rank 0 --world-size 3
```
Rank 2
```
$ python main.py --init-method tcp://node01:23456 --rank 1 --world-size 3
```
Rank 3
```
$ python main.py --init-method tcp://node01:23456 --rank 2 --world-size 3
```
