---
title: Compiling OpenCV from source
date: "2020-05-14"
draft: true
---

If you plan to work on anything related to visual computing or image understanding in python, chances are the first library you'll probably want to reach for is OpenCV. OpenCV has a ton of clever algorithms for manipulating images: it has almost every 'old school' image manipulation technique (these are techniques that don't require neural networks) and is used widely alongside machine learning libraries like Tensorflow, PyTorch and Caffe to implement some more modern techniques.

When it comes to setting up OpenCV. You have two main options. You can either:

1. Install a pre-built binary using your favourite package manager (pip, apt-get, brew, nix, yay, etc.), or
2. Download the source code and compile it yourself

When I was first getting started with OpenCV, I was surprised by how different the pre-built binaries can be depending on where you were to install it from and the version of python that you are using it with. By default, `pip install opencv` lacks some non-free features such as SHIFT detectors that are provided by the homebrew version.

Last week, while I was setting up my Jetson Nano, I compiled OpenCV from scratch and discovered a whole slew of flags that let you to tweak how OpenCV behaves. In my particular case, compiling OpenCV from scratch allows me to utilize the Jetson Nano's onboard GPU.

## Actually compiling the library

Compiling OpenCV from scratch involved first generating a make file by running CMAKE with a whole slew of flags and then running make.

```bash
# I prefer aria2c, but wget works just as well!
$ aria2c https://github.com/opencv/opencv/archive/4.1.2.zip
$ mv 4.1.2.zip opencv.zip
$ aria2c https://github.com/opencv/opencv_contrib/archive/4.1.2.zip
$ mv 4.1.2.zip opencv_contrib.zip
$ unzip opencv.zip
$ unzip opencv_contrib.zip
$ mkdir -p opencv/build && cd opencv/build

$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D WITH_CUDA=ON \
    -D CUDA_ARCH_PTX="" \
    -D CUDA_ARCH_BIN="5.3,6.2,7.2" \
    -D WITH_CUBLAS=ON \
    -D WITH_LIBV4L=ON \
    -D BUILD_opencv_python3=ON \
    -D BUILD_opencv_python2=OFF \
    -D BUILD_opencv_java=OFF \
    -D WITH_GSTREAMER=ON \
    -D WITH_GTK=ON \
    -D BUILD_TESTS=OFF \
    -D BUILD_PERF_TESTS=OFF \
    -D BUILD_EXAMPLES=OFF \
    -D OPENCV_ENABLE_NONFREE=ON \
    -D OPENCV_EXTRA_MODULES_PATH=/home/`whoami`/opencv_contrib/modules ..

# 4 because that's the number of cores on the jetson nano
$ make -j4
```

It was a long process. Compiling OpenCV took well over **2 hours**. But in the end, I can be sure that my installation of OpenCV has all the features that I want and no extra bloat alongside it.

## A learning experience

After you install OpenCV, you can symlink the binary it produces into the `lib/python/site-packages` folder of any other python installations or virtual environments you might have.

To find out where OpenCV has been installed to, you need to start python and run the following two lines:

```python
>>> import cv2
>>> cv2.__file__
'/path/to/cv2.so'
```

Once you have this path, you can symlink your other installations of python to use the same binary.

After building OpenCV, I had some trouble locating the binary. I thought I had screwed up in compiling the binary and decided to recompile the code. I ended up spending a day or so compiling OpenCV over and over again (3 or 4 times). At the end I discovered, that the binary was being installed to a location that was different from where I had in mind

Lesson learned: Always double check the locations of the packages that you install. Especially if you are compiling them from source.
