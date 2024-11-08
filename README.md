 
# Server-Code-execc
## Table of Contents
- [Connect to the Server](#connect-to-the-server)
- [General Python Cleanup](#general-python-cleanup)
- [PyTorch Memory Management](#pytorch-memory-management)
- [TensorFlow Memory Management](#tensorflow-memory-management)
- [JAX Memory Management](#jax-memory-management)
- [Monitoring and Verifying Memory Release](#monitoring-and-verifying-memory-release)
- [Example Script](#example-script)
- [Kubernetes YAML File](#kubernetes-yaml-file)
- [Run Code with Kubernetes Node](#run-code-with-kubernetes-node)
- [Resources Managment](#resources-managment)


---


### Connect to the Server

``` console
ssh username@hostname
```
Or with putty <br/><br/>
![putty](https://github.com/roumpakis/Server-Code-exec/blob/master/images/Capture.JPG)
 
 <br/>

---
## General Python Cleanup

At the end of your code, delete large objects and call garbage collection to help free memory:

```python
import gc

# Delete large objects
del large_object

# Force garbage collection
gc.collect()
```


---
## PyTorch Memory Management

PyTorch provides utilities to manage and release GPU memory manually.

### 1. Clear GPU Cache

PyTorch caches memory on the GPU, which needs to be cleared explicitly:

```python
import torch

# Release memory of tensors
del tensor

# Clear the PyTorch cache
torch.cuda.empty_cache()
torch.cuda.ipc_collect()
```
### 2. Use Context Managers

Context managers automatically release memory once the code block exits, which is particularly useful to free memory after computations are completed:

```python
with torch.no_grad():
    # Your computation here

```



![running-pods](https://github.com/roumpakis/Server-Code-exec/blob/master/images/running-pods.JPG)

---
### Run code with kubernetes node  
With ```#``` the commands will run directly on ```my-pod``` kubernetes node. <br/>
In ```manifest.yaml``` we have mount ```path``` with ```hostPath``` and for this example it means that 
```/mnt/files/shared``` path in server is now visible at ```/app/scripts``` path in kubernetes node.


```console
cd /app/scripts
ls
python transparency.py
```
![running-code](https://github.com/roumpakis/Server-Code-exec/blob/master/images/code-exec.JPG)

---

### Resources Managment
GPU time-slicing enables workloads that are scheduled on oversubscribed GPUs to interleave with one another. 
There is no memory or fault-isolation between replicas. Internally, GPU time-slicing is used to multiplex workloads from replicas of the same underlying GPU.

For demanding tasks and in consultation with the server administrators a specific physical unit can be used.



![running-code](https://github.com/roumpakis/Server-Code-exec/blob/master/images/specific.JPG)








