# Neural Network CUDA Example
## Environments
* NVIDIA Driver: 418.116.00
* CUDA: 11.0
* Python: 3.7.3
* PyTorch: 1.7.0+cu110
* CMake: 3.16.3

*Cannot ensure successful running in other environments.*

## Code structure
```shell
├── include
│   └── add2.h # header file of add2 cuda kernel
├── kernel
│   └── add2_kernel.cu # add2 cuda kernel
├── pytorch
│   ├── add2_ops.cpp # torch wrapper of add2 cuda kernel
│   ├── time.py # time comparison of cuda kernel and torch
│   ├── train.py # training using custom cuda kernel
│   ├── setup.py
│   └── CMakeLists.txt
├── LICENSE
└── README.md
```

## PyTorch
### Compile cpp and cuda

**Setuptools**  
```shell
python3 pytorch/setup.py install
```

**CMake**  
```shell
mkdir build
cd build
cmake ../pytorch
```
打开vs2019，选择release模式，点击生成

### Run python
**Compare kernel running time**  
```shell
python3 pytorch/time.py --compiler jit
python3 pytorch/time.py --compiler setup
python3 pytorch/time.py --compiler cmake
```

**Train model**  
```shell
python3 pytorch/train.py --compiler jit
python3 pytorch/train.py --compiler setup
python3 pytorch/train.py --compiler cmake
```


## Implementation details (in Chinese)
[PyTorch自定义CUDA算子教程与运行时间分析](https://godweiyang.com/2021/03/18/torch-cpp-cuda)  
[详解PyTorch编译并调用自定义CUDA算子的三种方式](https://godweiyang.com/2021/03/21/torch-cpp-cuda-2)  
[三分钟教你如何PyTorch自定义反向传播](https://godweiyang.com/2021/03/24/torch-cpp-cuda-3)

## F.A.Q
> **Q.** ImportError: libc10.so: cannot open shared object file: No such file or directory  
**A.** You must do `import torch` before `import add2`.

> **Q.** tensorflow.python.framework.errors_impl.NotFoundError: build/libadd2.so: undefined symbol: _ZTIN10tensorflow8OpKernelE  
**A.** Check if `${TF_LFLAGS}` in `CmakeLists.txt` is correct.