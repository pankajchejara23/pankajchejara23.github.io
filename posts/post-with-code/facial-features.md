---
title: "Facial feature extraction using OpenFace"

author: "Pankaj Chejara"

date: "2020-03-28"

categories: [python, openface, facial features]

image: "./p1-openface.jpg"

code-block-background: true
toc: true
---

In this post, I will discuss the work I have been doing recently. I needed to extract facial features from the recorded video and for this task, I decided to use OpenFace, an open-source face recognition library. In this post, I am sharing the installation process and tutorial on detecting facial landmarks.

<base target="_blank">

### Installation

I tried to install OpenFace on Mac OS but couldn't succeed. There were a lot of errors and compatibility issues that I couldn't get through. Therefore, I decided to install it on Ubuntu. For that, I installed a Virtual box on Mac and installed Ubuntu 18.04.

To install OpenFace, I followed the steps given [here](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Unix-Installation)

```bash
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get install g++-8

sudo apt-get install cmake

sudo apt-get install git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev

wget https://github.com/opencv/opencv/archive/4.1.0.zip

sudo unzip 4.1.0.zip
cd opencv-4.1.0
mkdir build
cd build

cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_TIFF=ON -D WITH_TBB=ON ..
make -j2
sudo make install

wget http://dlib.net/files/dlib-19.13.tar.bz2
tar xf dlib-19.13.tar.bz2
cd dlib-19.13
mkdir build
cd build
cmake ..
cmake --build . --config Release
sudo make install
sudo ldconfig
cd ../..

sudo apt-get install libboost-all-dev

git clone https://github.com/TadasBaltrusaitis/OpenFace.git

cd OpenFace
mkdir build
cd build

cmake -D CMAKE_CXX_COMPILER=g++-8 -D CMAKE_C_COMPILER=gcc-8 -D CMAKE_BUILD_TYPE=RELEASE ..
make
```

At this point, we have installed OpenFace. Now we need to download models. You can either download it manually or use a script provided in the OpenFace library. 

Manual download links.

* [scale 0.25](https://drive.google.com/uc?export=download&id=1TM_L_qNgd513z5i_T4CuXOF1Vl5DDu1l)

* [scale 0.35](https://onedrive.live.com/download?cid=2E2ADA578BFF6E6E&resid=2E2ADA578BFF6E6E%2153079&authkey=ANpDR1n3ckL_0gs)

* [scale 0.50](https://onedrive.live.com/download?cid=2E2ADA578BFF6E6E&resid=2E2ADA578BFF6E6E%2153074&authkey=AGi-e30AfRc_zvs)

* [scale 1.00](https://drive.google.com/uc?export=download&id=1b8semX96A2yNe194PvKh_rkU1frcI4jr)

I ran the following command to ran the script to download the model.

```bash
cd ..
sh ./download_models.sh
```

The script will download models in the directory `OpenFace/lib/local/LandmarkDetector/model/patch_experts`.

After completing this process, I ran the demo program by running the following command.

```bash
./bin/FaceLandmarkVid -f ""../samples/changeLighting.wmv"" -f ""../samples/2015-10-15-15-14.avi
```

::: {.callout-important}
## Execution error
I got an error `CEN patch expert` not found. The command was searching the models in the `OpenFace/build/bin/model/patch_experts`.
:::


:::{.callout-tip}
## Solution
I copied the files (`cen_patches_0.25_of.dat`,`cen_patches_0.35_of.dat`,`cen_patches_0.50_of.dat`,`cen_patches_1.00_of.dat`) in `OpenFace/build/bin/model/patch_experts` directory.
:::

### Running Demo

If you have a video with a single face, you can use `FaceLandmarkVid` or in case of multiple faces, you can use `FaceLandmarkVidMulti`


::: {.callout-note}
## Note
These files will be available in `OpenFace/build/bin` directory. Either you can specify the full path to facial landmark detector or cd to the bin directory and run the following command.
:::


```shell
FaceLandmarkVid -f file_name
```
Following is the demonstration of OpenFace on a video clip with single face.


[![](https://img.youtube.com/vi/osk0ezhaO8I/0.jpg)](https://www.youtube.com/watch?v=osk0ezhaO8I)
