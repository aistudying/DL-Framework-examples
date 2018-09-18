#### Environment  
CUDA9.0   
cuDNN7  
nccl2  
Tensorflow-gpu 1.8.0  

#### run command:
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
