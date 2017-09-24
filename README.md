# Tensorflow_installation
record my installation on Ubuntu 17+

回忆记录一下在ubuntu 17.04 上踩坑的经历

1. 安装anaconda，为了方便预装一下包，推荐在[清华开源镜像站](https://mirrors.tuna.tsinghua.edu.cn)安装，不然太慢了
2. 安装Ubuntu系统后是没有装Nvidia显卡驱动的，在关于计算机里面没有提到Nvidia显卡型号。所以通过lspci查看：
```
lspci | grep VGA
```

  2.2. 注意在安装一些包的时候可能会提示错误，通过终端提示的fix的命令能解决

3. 看到显卡之后去Nvidia官网，也可以查看到是否支持cuda的显卡http://developer.nvidia.com/cuda-gpus

  3.1. 注意安装显卡驱动的时候需要通过tty模式，crt+alt+F1
  
  
  3.2. 可以通过crt+alt+F7 返回到图形界面模式
  
  
  3.3. 需要关闭lightdm, `sudo lightdm stop` 结束之后 `sudo lightdm start` 启动就恢复了
 
4. 下载驱动之后／我下载的是runfile(.run)，通过`sudo sh <filename>.run`来安装，前面已经装过显卡了，就不用再装了。还有这里会提示

5. 后来又知道了直接通过 `sudo apt-get install cuda`安装cuda-8.0，所以前面也可以忽略。

6. 使用`nvidia-smi`来检查是否安装好了

7. 然后安装cuDNN 6.0版本的，在[这里](https://developer.nvidia.com/rdp/cudnn-archive)找到对应版本

8. 注意一下路径，apt安装后的路径一般都是 /usr/local/cuda-8.0/

9. 这时候cuDNN就要将里面的文件复制到/usr/local/cuda-8.0/文件夹里面 使用`sudo cp <path> </usr/local/cuda-8.0/> `就可以了

10. 注意一下有时候可能会提示没有存在这个路径，使用sudo mkdir创建 lib64和include就可以了

10.1. 添加环境变量，在home路径下的.bashrc里面，添加路径，格式就是 `export CUDA_HOME=/usr/local/cuda-8.0` 还有`export LD_LIBRARY_PATH=${CUDA_HOME}/lib64/` and `export PATH=${CUDA_HOME}/bin:${PATH}`

11. 安装tensorflow-gpu 这个就还是通过conda安装吧，tensorflow 官网有教程，conda比较快

12. 对于安装完后出现的错误补充：
```
ImportError: libcudnn.so.6: cannot open shared object file: No such file or directory
#or
ImportError: libcudnn.so.5: cannot open shared object file: No such file or directory
#之类的，检查在cuda-8.0文件夹下面有没有lib64，环境变量，还有lib64里面有没有libcudnn这个文件
#如果有，就可以通过一个ln指向libcudnn.so.6，大概还是不兼容的原因。


#在/usr/local/cuda-8.0/lib64 下面使用
sudo ln -s libcudnn.so.7.* libcudnn.so.6
#后面的就是被指向的文件
```
然后再`import tensorflow as tf`就ImportError了


##检查

```
cd anaconda3/lib/python3.6/sit-packages/tensorflow/examples/tutorials/mnist/
python mnist.py
```
