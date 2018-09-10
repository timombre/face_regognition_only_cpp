# Face_regognition_only_cpp

Inspired by https://github.com/vinayakkailas/Face_Recognition in python
(for a working update, check my fork: https://github.com/timombre/Face_Recognition)

Face recognition using Tensorflow C++ API and OpenCV

## Requirements:

	1. Tensorflow built from sources :
	Follow official guidelines
	https://www.tensorflow.org/install/install_sources
	
	Generate shared library: (bazel build //tensorflow:libtensorflow_cc.so)
	
	2. OpenCV and contribs installed:
	
	git clone https://github.com/opencv/opencv.git
	cd opencv 
	git checkout 3.3.1 
	cd ..
	git clone https://github.com/opencv/opencv_contrib.git
	cd opencv_contrib
	git checkout 3.3.1
	cd ..
	cd opencv
	mkdir build
	cd build
	cmake -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=/usr/local \
      -D INSTALL_C_EXAMPLES=ON \
      -D INSTALL_PYTHON_EXAMPLES=ON \
      -D WITH_TBB=ON \
      -D WITH_V4L=ON \
      -D WITH_QT=ON \
      -D WITH_OPENGL=ON \
      -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
      -D BUILD_JPEG:BOOL=ON -DWITH_JPEG:BOOL=ON\
      -D BUILD_EXAMPLES=ON ..
	make
	sudo make install
	sudo sh -c 'echo "/usr/local/lib" >> /etc/ld.so.conf.d/opencv.conf'
	sudo ldconfig
	

Works on Unix systems (Not tested on Mac though)

## Using:

In Makefile, modify TF_BUILDDIR ?= with your Tensorflow directory.

make

Download Tensorflow weightfile, Landmarks and OpenCV Haarcascade:

wget https://github.com/arunmandal53/facematch/raw/master/20180408-102900/20180408-102900.pb

wget https://raw.githubusercontent.com/opencv/opencv/master/data/haarcascades/haarcascade_frontalface_alt2.xml

wget https://raw.githubusercontent.com/kurnianggoro/GSOC2017/master/data/lbfmodel.yaml


## Create a dataset of faces for each person 

sudo apt install ffmpeg 

ffmpeg -i /dev/video0 /PATH-FOR-VID/LABEL.mkv #modify dev/video0 if needed and choose your path

ffmpeg -i /PATH-FOR-VID/LABEL.mkv -vf fps=2 /PATH-FOR-IMAGES/LABEL_%04d.jpg -hide_banner

Arrange your dataset in below order

```
root folder  
│
└───Person 1
│   │───IMG1
│   │───IMG2
│   │   ....
└───Person 2
|   │───IMG1
|   │───IMG2
|   |   ....
```

cd prepdatabase

./prepdatabase.sh PATH_TO_YOUR_DATA_DIRECTORY

cd ..

./face_recognition face_embeddings_database.txt


## WIP

-The accuracy is still low, needs better alignment implementation (in progress)



