# ORBSLAM2_with_pointcloud_map-in-Ubuntu-16.04
make some changes to gaoxiang12's ORBSLAM2_with_pointcloud_map and can successfully compile and run without crash in the end



This file is the instruction for compile and run gaoxiang's ORBSLAM2_with_pointcloud_map succfully in Ubuntu 16.04.
(https://github.com/gaoxiang12/ORBSLAM2_with_pointcloud_map). 


# steps and some possible problems

## 1.Unzip and merge files
You can get two folders named "ORB_SLAM2_modified" and "g2o_with_orbslam2" respectively.

## 2.Install some prerequisites

attention: you msut make sure that you have uninstalled g2o of other version if you have it .
Firstly, install cmake 3 and g++

```
sudo apt-get install cmake 3(press tab)
-----
sudo apt-get install g++
---
```
Then, install Pangolin, eigen3 and opencv(2 or 3).
You can finish them by following it. https://github.com/raulmur/ORB_SLAM2.

Pangolin: (https://github.com/stevenlovegrove/Pangolin)

```
sudo apt-get install libglew-dev
---
sudo apt-get install libboost-dev libboost-thread-dev libboost-filesystem-dev
---
git clone https://github.com/stevenlovegrove/Pangolin Pangolin
--
cd Pangolin
mkdir build
cd build
cmake ..
make 
sudo make install
---
```
Eigen3: http://eigen.tuxfamily.org/index.php?title=Main_Page

```
sudo apt-get install libeigen3-dev
```
OpenCV: https://opencv.org/
You can use OpenCV2 or OpenCV3. But you need to change CMkeLists.txt corresponding to its version.
I installed OpenCV3. 
Firstly, install dependencies.
```
sudo apt-get install build-essential libgtk2.0-dev libvtk5-dev libjpeg-dev libtiff4-dev libjasper-dev libopenexr-dev libtbb-dev
---
```
You should press "Tab" when you input these words to install existing version in you operating system. Do not copy them directly.
Then, download OpenCV3.4.2 (maybe other version about 3) sources in https://opencv.org/releases.html and unzip it.
```
cd path-to-OpenCV 3.4.2
mkdir build
cd build
cmake ..
make
sudo make install
---
```
It might be slow in your computer when "make". You can use "make -j4" to accelerate.

PCL: http://pointclouds.org/

```
sudo apt-get install libpcl-dev pcl-tools
---
```
## 3.Install g2o_with_orbslam2
annotate some code in CMakeLists.txt in g2o_with_orbslam2 folder

```
# shall we build the examples
#SET(G2O_BUILD_EXAMPLES ON CACHE BOOL "Build g2o examples")
#IF(G2O_BUILD_EXAMPLES)
 # MESSAGE(STATUS "Compiling g2o examples")
#ENDIF(G2O_BUILD_EXAMPLES) 
```
then annotate some codes in g2o folder in g2o_with_orbslam2

```
# Examples
#IF(G2O_BUILD_EXAMPLES)
 # ADD_SUBDIRECTORY(examples)
#ENDIF(G2O_BUILD_EXAMPLES)  
```
After, I compile g2o_with_orbslam2, but encountering two errors. However, I forget to take some pictures for you....
You can just compile it without changeing codes and try to find if you would meet these problems. Think about how to understand these errors.

### Problems:
1). error: no matching function for call to'g2o::SE2:setRotation(Eigen::Rotation2D<double>::Scaler)'
t.setRotation(t.rotation().angle()+_measurement);

open g2o/types/slam2d/edge_se2_pointxy_bearing.cpp
change "t.setRotation(t.rotation().angle()+_measurement);" to "t.setRotation((Eigen::Rotation2Dd)(t.rotation().angle()+_measurement));"

2). error: static assertion failed: YOU_MIXED_DIFFERENT_NUMERIC_TYPES__YOU_NEED_TO_USE_THE_CAST_METHOD_OF_MATRIXBASE_TO_CAST_NUMERIC_TYPES_EXPLICITLY

open g2o/solvers/eigen/linear_solver_eigen.h
change "typedef Eigen::PermutationMatrix<Eigen::Dynamic, Eigen::Dynamic, SparseMatrix::Index> PermutationMatrix;" to "typedef Eigen::PermutationMatrix<Eigen::Dynamic, Eigen::Dynamic, SparseMatrix::StorageIndex> PermutationMatrix;
"




