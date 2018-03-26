
How to install Tensorflow 1.5.0 using official pip package
Easiest method to install official pre-built tensorflow pip packages for both CPU and GPU versions on Ubuntu and Windows.

2018-01-23 Arun Mandal 36

Hello everyone. This is going to be a tutorial on how to install tensorflow using official pre-built pip packages. To install tensorflow with pip packages is easier as compared to building using CMake or Bazel. Pre-built pip package are fully tested officially. However, since they are configured in such a way that they can support legacy hardware too, using pip package may not use full capability on your new and powerful hardware. Building pip package the solution to fully optimize tensorflow to use full capability of your hardware. However, building is a time-consuming process and generally recommended for advanced users only. If you are looking to build tensorflow instead, you can check out our other blog posts:

    For Ubuntu 16.04
    For Windows 10

In this tutorial, we will look at how to install tensorflow CPU and GPU both for Ubuntu as well as Windows OS. For our purpose, we will look at installing the latest version tensorflow, tensorflow 1.5.0, at the time this blog is published. To install tensorflow in any OS, I highly recommended using virtual environment setup (conda, virtualenv etc.). Currently only 64-bit python is supported by Tensorflow.

We have also performed speed comparison on the tensorflow 1.5.0 with CUDA 9 and cuDNN 7.5 support with tensorflow 1.4.1 with CUDA 8 and cuDNN 6 to calculate just how faster the new version of tensorflow is in comparison. You can click on the link here to check that out.

There must be 64-bit python installed tensorflow does not work on 32-bit python installation.
For Ubuntu 16.04 64bit OS:
Install CPU Version of Tensorflow:

CPU version of tensorflow is recommended for new users of tensorflow. Unless you are handling large datasets, CPU version of tensorflow works just fine. Also, this is the simplest method to install tensorflow.

Step1: Download whl file

Goto https://pypi.python.org/pypi/tensorflow and download whl pacakage related to your python version and os.

For eg. If your tensorflow version is 1.5.0, your python version is 3.5, and OS is linux then select tensorflow-1.5.0-cp35-cp35m-manylinux1_x86_64.whl

Step 2: Install whl file

Create a new virtual environment and activate it then install the whl file using the command. If you are having trouble setting up a virtual environment, you can refer to our other article here.

for python 2:

pip2 install [whl file path]

for python 3:

pip3 install [whl file path]

Step 3: Verify tensorflow installation

Verify tensorflow using following commands:

$ python3

>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> sess.run(hello)
'Hello, TensorFlow!'
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> sess.run(a + b)
42
>>> sess.close()

This is all you need to do to install tensorflow CPU version on Ubuntu 16.04.
Install GPU Version of Tensorflow:

Using GPU version of tensorflow will greatly speed up training dataset time. Once you are working with large datasets, it is impractical to rely only on CPU for deep learning. Tensorflow GPU is recommended for intermediate to advanced users and anyone who works with handling large dataset. Advanced users who want to build pip package to get optimum performance can follow the link at the top of the article to build tensorflow gpu for ubuntu.

Step 1: Update and Upgrade your system:

sudo apt-get update 
sudo apt-get upgrade

Step 2: Verify You Have a CUDA-Capable GPU:

lspci | grep -i nvidia

Note GPU model. eg. GeForce 840M

If you do not see any settings, update the PCI hardware database that Linux maintains by entering update-pciids (generally found in /sbin) at the command line and rerun the previous lspci command.

If your graphics card is from NVIDIA then go to http://developer.nvidia.com/cuda-gpus and verify if listed in CUDA enabled GPU list.

Step 3: Verify You Have a Supported Version of Linux:

To determine which distribution and release number you’re running, type the following at the command line:

uname -m && cat /etc/*release

The x86_64 line indicates you are running on a 64-bit system which is supported by Cuda 9.0

Step 4: Install Dependencies:

sudo apt-get install build-essential

sudo apt-get install cmake git unzip zip

sudo apt-get install python2.7-dev python3.5-dev pylint

Step 5: Install linux kernel header:

Goto terminal and type:

uname -r

You can get like “4.10.0-42-generic”. Note down linux kernel version.

To install linux header supported by your linux kernel type the following command:

sudo apt-get install linux-headers-$(uname -r)

Step 6: Download the NVIDIA CUDA Toolkit:

Go to this link and download Installer for Linux > x86_64 > Ubuntu > 16.04 > deb[network]

I highly recommend network installer to get updated gpu driver supported by your linux kernel.

For, direct download

wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.0.176-1_amd64.deb

If you have Cuda 9.1 or other version installed then this tensorflow prebuilt package will not work. Remove nvidia cuda related files (drivers, Cuda Toolkit, etc)

sudo apt-get purge nvidia*
sudo apt-get auto-remove

Installation Instructions:

sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
sudo dpkg -i cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda-9.0

Step 7: Reboot the system to load the NVIDIA drivers.

Step 8: Go to terminal and type:

nano ~/.bashrc

In the end of the file, add:

export PATH=/usr/local/cuda-9.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

ctrl+x then y to save and exit

source ~/.bashrc
sudo ldconfig
nvidia-smi

Check driver version.

(not likely) If you got nvidia-smi is not found then you have unsupported linux kernel installed. Comment your linux kernel version noted in step 5.

Step 9: Install cuDNN 7.0.5:

Goto this link and download the required files. (Membership required)

After login

Download the following:

cuDNN v7.0.5 Runtime Library for Ubuntu16.04 (Deb)

cuDNN v7.0.5 Developer Library for Ubuntu16.04 (Deb)

cuDNN v7.0.5 Code Samples and User Guide for Ubuntu16.04 (Deb)

Goto downloaded folder and in terminal perform following:

sudo dpkg -i libcudnn7_7.0.5.15-1+cuda9.0_amd64.deb
sudo dpkg -i libcudnn7-dev_7.0.5.15-1+cuda9.0_amd64.deb
sudo dpkg -i libcudnn7-doc_7.0.5.15-1+cuda9.0_amd64.deb

Verifying cuDNN installation:

cp -r /usr/src/cudnn_samples_v7/ $HOME
cd  $HOME/cudnn_samples_v7/mnistCUDNN
make clean && make
./mnistCUDNN

If cuDNN is properly installed and running on your Linux system, you will see a message similar to the following:

Test passed!

Step 10: Install Dependencies

libcupti (required)

sudo apt-get install libcupti-dev
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
sudo ldconfig

Step11: Download whl file

Goto https://pypi.python.org/pypi/tensorflow-gpu and download whl package related to your python version and os.

For eg., if your python version is 3.5 and os is linux then select

tensorflow_gpu-1.5.0-cp35-cp35m-manylinux1_x86_64.whl

Step 12: Install whl file

Create a new virtual environment and activate it then install the whl file using the command.

for python 2:

pip2 install [whl file path]

for python 3:

pip3 install [whl file path]

Step 13: Verify Tensorflow installation

Verify tensorflow using following commands:

$ python

>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> sess.run(hello)
'Hello, TensorFlow!'
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> sess.run(a + b)
42
>>> sess.close()

Note:- It may take some time after tf.Session() for first time.

This is all you need to do to install tensorflow GPU version on Ubuntu 16.04.
For Windows-64bit OS:
Install CPU Version of Tensorflow:

I have already mentioned the condition in which CPU version of tensorflow is preferable to the GPU version earlier in the post. Let’s get right to how to install tensorflow CPU version for windows OS.

Step1: Download whl file

Goto https://pypi.python.org/pypi/tensorflow and download whl package related to your python version and os.

For eg., if your python version is 3.5 and os is Windows then select

tensorflow-1.5.0-cp35-cp35m-win_amd64.whl

Step 2: Install whl file

Create a new virtual environment and activate it then install the whl file using the command.

for python 2:

pip2 install [whl file path]

for python 3:

pip3 install [whl file path]

Step 3: Verify tensorflow installation

Verify tensorflow is properly installed using the following commands:

$ python

>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> sess.run(hello)
'Hello, TensorFlow!'
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> sess.run(a + b)
42
>>> sess.close()

This is all you need to do to install tensorflow CPU version on Windows OS.
Install GPU Version of Tensorflow:

GPU tensorflow is recommended for intermediate to advanced users. For users who work with large dataset, the GPU version is almost a necessity as it can greatly speed up training time. Advanced users who want to build pip package to get optimum performance can follow the link at the top of the article to build tensorflow gpu for Windows.

Step 1: Verify you have a CUDA-Capable GPU:

Before doing anything else, you need to verify that you have a CUDA-Capable GPU in order to install tensorflow GPU. You can verify that you have a CUDA-capable GPU through the Display Adapters section in the Windows Device Manager. Here you will find the vendor name and model of your graphics card(s).

The Windows Device Manager can be opened via the following steps:

Open a run window from the Start Menu or (Win+R)

Run:

control /name Microsoft.DeviceManager

If your graphics card is from NVIDIA then go to http://developer.nvidia.com/cuda-gpus and verify if listed in CUDA enabled GPU list.

Step 2: Download the NVIDIA CUDA Toolkit:

Go to this link and download Installer for Windows > x86_64 > 10 > exe[network]

Uninstall Nvidia and Cuda related programs from Control Panel > Program and Features.

If a reboot is required, then reboot.

Install it in default location with default settings. It will update your GPU driver if required.

Step 3: Reboot the system to load the NVIDIA drivers.

Step 4: Install cuDNN 7.0.5:

Goto this link and download the necessary files. (Membership required)

After login, download the following:

cuDNN v7.0.5 Library for Windows [your version]. For me, it’s Windows 10

Goto downloaded folder and extract cudnn-9.0-windows[your version]-x64-v7.zip

Go inside extracted folder and copy all files and folder from cuda folder (eg. Bin, include, lib) and paste to “C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0”.

Step 5: Download whl file

Goto https://pypi.python.org/pypi/tensorflow-gpu and download whl pacakage related to your python version and os.

For eg., if your python version is 3.5 and os is Windows then select

tensorflow_gpu-1.5.0-cp35-cp35m-win_amd64.whl

Step 6: Install whl file

Create a new virtual environment and activate it then install the whl file using the command.

for python 2:

pip2 install [whl file path]

for python 3:

pip3 install [whl file path]

Step 7: Verify tensorflow installation

Verify tensorflow using following commands:

$ python

>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> sess.run(hello)
'Hello, TensorFlow!'
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> sess.run(a + b)
42
>>> sess.close()

Note:- It may take some time after tf.Session() for first time.

This is all you need to do to install tensorflow GPU version on Windows OS.

If you follow the steps mentioned above carefully, you will be able to install tensorflow both CPU and GPU version on Ubuntu as well as Windows OS. If you encounter any problem during the process, do let us know in the comments and we will help you.
