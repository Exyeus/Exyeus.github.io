# ASC26 Bilibili PKU Lec1 Intro

### 不同之处

专用的体系结构
ECC内存：为了不产生错误，会有纠错机制，进而无法和家用内存通用。

通道主要给 PCLE 插槽，用来搞外设通道。家用电脑会给无线网卡、M.2 等用于外设。
### 由此产生的代价：

软硬件都产生了新的挑战。
pthread，cuda，openmp，openmpi
### 基本工具
#### ssh

- `~/.ssh/config` 修改以简化配置
- `ssh-keygen` 创建公钥私钥。非对称加密。
#### rsync

同步 **两处计算机** 的文件与目录。
```
rsync [options] src des
options: -r(recursive) 
```
 
### 开发工具

SISD，SIMD，SIMT。
- SISD：一次只能做一个任务。
- SIMD：用一个 instruction 去操作多个数据。数学上联系。Vectorized。
- SIMT：一个指令，但是用了不同的计算器。不像 SIMD 用一个很长的运算器。
#### 使用 SIMD：AVX、SSE

```cpp
__m256 vectorAdd(__m256 a, __m256 b, __m256 c) {
	return __mm256_add_ps(a, b);
}
```

速度提升，CPU复杂。
#### SIMT：GPU；CUDA、ROCM（AMD）

两个长得差不多。
```cpp
__global__ void addVector(float* a, float* b, float* c) {
	int tid = blockIdx.x;
	if (tid < 8) {
		c[tid] = a[tid] + b[tid];
	}
}
```

##### OPENCL：开放计算语言。异构平台编写程序的框架。

同样是基于 C 的语法。可以跨平台。但是CUDA有了更好的编译器，更多的预制库。社区发展得没有CUDA好。
#### MULTI-THREAD: OPENMP

可以利用多线程，用 CPU 进行加速。
可以很好地跨平台开发。加一条与编译指令就可以很好地实现并行。`# pragma`
### 性能调优
#### CPU：

cache。优化方法：
- 循环展开；比如一次做原来两次循环的工作量。通过更改循环体里面的代码。
- 内存预取；自己是可以预测自己的程序的，将某些内存地址区域的数据，指定存到更加高的cache层级去。
##### 流水线——出现分支——分支预测

估计每个 if 进去或者不进去各自的概率。
但是，开发者可以知道这些 if 的大致概率，比着写计算机算法做得更好。
CPP20提供了相关的语法！
#### GPU

线程束分化；理解GPU内存模型；并发。
##### 线程束分化

虽然并行，但是 if - else 之中存在等待的要求。
##### 内存模型

访存？布局？
局部性？十个线程与十个数组：`A[0]B[0]C[0]A[1]B[1]C[1]...`
##### 并发

还有转移到 CPU 的需求。
#### RDMA：Remote Direct Memory Access

绕过操作系统内核直接访问闪存之中数据的技术。
节省大量 CPU 资源，减少吞吐量，降低网络通信延迟。










