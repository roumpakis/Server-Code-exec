 
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


## TensorFlow Memory Management

TensorFlow offers options to release memory at the end of the session, which is especially helpful in interactive environments.

### 1. Clear GPU Memory

To reset and release memory in TensorFlow, use a session cleanup method to clear the backend session:

```python
import tensorflow as tf

# Reset TensorFlow session
tf.keras.backend.clear_session()
```
---

### 2. Use Limited GPU Memory Growth

By default, TensorFlow may attemp to allocate all avaliable GPU memory. To prevent this and only allocate memory as needed, enable memory growth for each GPU: 



```python
# List all GPU devices
gpu_devices = tf.config.experimental.list_physical_devices('GPU')
for device in gpu_devices:
    tf.config.experimental.set_memory_growth(device, True)

```


---

### Resources Managment
GPU time-slicing enables workloads that are scheduled on oversubscribed GPUs to interleave with one another. 
There is no memory or fault-isolation between replicas. Internally, GPU time-slicing is used to multiplex workloads from replicas of the same underlying GPU.

For demanding tasks and in consultation with the server administrators a specific physical unit can be used.



![running-code](https://github.com/roumpakis/Server-Code-exec/blob/master/images/specific.JPG)








