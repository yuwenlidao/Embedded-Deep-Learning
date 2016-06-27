<b>Commands to install caffe</b>

```
$ sudo add-apt-repository universe
$ sudo apt-get update –y
$ sudo apt-get install cmake –y

# General Dependencies
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler –y
$ sudo apt-get install --no-install-recommends libboost-all-dev –y

# BLAS
$ sudo apt-get install libatlas-base-dev –y

# Remaining Dependencies
$ sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev –y

# Change Directory
$ cd Programs/caffe-caffe-0.14-fp16

# Copy Makefile.config.example
$ cp Makefile.config.example Makefile.config

# Use only 3 cores on L4T 23.1 install
# 4 cores hangs system
$ make -j3 all

# Run the tests to make sure everything works
$ make -j3 runtest
```

<b>Change Frequency Settings</b>

> For Linux PC, please add sudo in front of each command. 

```
#1 Change Directory
   $ cd /home/ubuntu/Programs/caffe-experimental-fp16/01-tx1-settting/
   
#2 Replace tegra-analysis.sh with paper-tegra-analysis-690M.sh
   Src: https://github.com/charlyng/Embedded-Deep-Learning/tree/fcc8b7e13dcfc5767da277700caf6a67e19a91b0/tx1-freq-settting

#3 Change the frequency in tegra-analysis.sh *
   $ ./paper-tegra-analysis-690M.sh 

* Frequency will be reset after session ended. <-- mean after reboot, freq back to default value
```
<b>Run tegrastats</b>
```
#4 Run tegrastats
   $ ./tegrastats
```
<b>Run prototxt file</b>
```
#5 Change Directory
   $ cd /home/ubuntu/Programs/caffe-experimental-fp16/
  
FP32

#6 Run the prototxt file for AlexNet, GoogLeNet and Vgg16 *
   AlexNet
   $ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib; ./build/tools/caffe time -model=models/bvlc_alexnet/deploy1.prototxt -gpu 0 --iterations 100
   
   GoogLeNet
   $ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib; ./build/tools/caffe time -model=models/bvlc_googlenet/deploy1.prototxt -gpu 0 --iterations 100
   
   Vgg16
   $ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib; ./build/tools/caffe time -model=models/vgg16/deploy.1.prototxt -gpu 0 --iterations 100
  
FP16

#7 Run the prototxt file for AlexNet, GoogLeNet and Vgg16 *
   AlexNet
   $ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib; ./build/tools/caffe_fp16 time -model=models/bvlc_alexnet/deploy1.prototxt -gpu 0 --iterations 100
   
   GoogLeNet
   $ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib; ./build/tools/caffe_fp16 time -model=models/bvlc_googlenet/deploy1.prototxt -gpu 0 --iterations 100
   
   Vgg16
   $ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib; ./build/tools/caffe_fp16 time -model=models/vgg16/deploy.1.prototxt -gpu 0 --iterations 100

   * Make the necessary changes for different batch size to the above commands when reproducing benchmark results
   Note: deployx where x refers the batch no.  
         --> For AlexNet & GoogLeNet, deployx
         --> For Vgg16, deploy.x
```