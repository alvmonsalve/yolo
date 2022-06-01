# Yolo Vehicle Counter and Speed Estimator

## Overview
This is a demonstration project that uses the YOLO real-time object detection architecture, please refer to the [YOLO paper] (https://pjreddie.com/media/files/papers/YOLOv3.pdf). The project is based on the code from (https://github.com/guptavasu1213/Yolo-Vehicle-Counter/). Other vehicle detection systems were considered, such as (https://www.hindawi.com/journals/mpe/2021/1577614/), however, the first one was chosen due to code availability.

The demonstration aims to count every vehicle (motorcycle, bus, car) detected in two recorded videos using YOLOv3 object-detection algorithm. Addtionally, the vehicle speed is estimated.

## Output Videos
The output videos can be found in the following links:

Video1: https://drive.google.com/file/d/1TvrZ82my_kzBsrAroQT1OAR9v3BKhfQH/view?usp=sharing

Video2: https://drive.google.com/file/d/1AHG5RUGIm_oC50hVxfrJaqMn6SJzuPb-/view?usp=sharing


Detection is very effective for both videos. Vehicle speed is estimated for cars in 3 incoming lanes. This can be easily extended to all lanes. Although the system is not calibrated, we can find the speed to be consistent for the traffic conditions. Speed estimation degrades as vehicles move accross the pedestrian crossing. This is due to the adopted approximation method.

## Speed Estimation
An instantaneous vehicle speed is calculated between video frames, and stored in a cyclic buffer from where the average interval speed is obtained. The vertical vehicle displacement is calculated using the center of the vehicle detection. A convertion factor from pixels to miles per hours were derived for 5 areas of the road. Those 5 areas are depicted by the picture below.
<p align="center">
  <img src="https://raw.githubusercontent.com/alvmonsalve/yolo/main/SpeedEstimationContours.jpg">
</p>

The imlementation described above is a very approximate method. We suggest to find the camera location and use triangle geometry to estimate distances.

To do: Calculate speed between frames that are not consecutives, or frames with timestamps.

## To do
YOLO is a very successul method for vehicle detection. However, it is computational expensive. For this reason, a review of the network layers is mandatory.

This code was an excellent and available choice for a good demo implementation. However, code needs to be re-written to address the specific structure needs of a speed estimator.

Write a manual for the implemented python function.


## Prerequisites
* Linux distro or MacOS (Tested on Ubuntu 18.04)
* A street video file to run the vehicle counting 
* The pre-trained yolov3 weight file should be downloaded by following these steps:
```
cd yolo-coco
wget https://pjreddie.com/media/files/yolov3.weights
``` 

## Dependencies for using CPU for computations
* Python3 (Tested on Python 3.6.9)
```
sudo apt-get upgrade python3
```
* Pip3
```
sudo apt-get install python3-pip
```
* OpenCV 3.4 or above(Tested on OpenCV 3.4.2.17)
```
pip3 install opencv-python==3.4.2.17
```
* Imutils 
```
pip3 install imutils
```
* Scipy
```
pip3 install scipy
```

## Dependencies for using GPU for computations
* Installing GPU appropriate drivers by following Step #2 in the following post:
https://www.pyimagesearch.com/2019/12/09/how-to-install-tensorflow-2-0-on-ubuntu/
* Installing OpenCV for GPU computations:
Pip installable OpenCV does not support GPU computations for `dnn` module. Therefore, this post walks through installing OpenCV which can leverage the power of a GPU-
https://www.pyimagesearch.com/2020/02/03/how-to-use-opencvs-dnn-module-with-nvidia-gpus-cuda-and-cudnn/
## Usage
* `--input` or `-i` argument requires the path to the input video
* `--output` or `-o` argument requires the path to the output video
* `--yolo` or `-y` argument requires the path to the folder where the configuration file, weights and the coco.names file is stored
* `--confidence` or `-c` is an optional argument which requires a float number between 0 to 1 denoting the minimum confidence of detections. By default, the confidence is 0.5 (50%).
* `--threshold` or `-t` is an optional argument which requires a float number between 0 to 1 denoting the threshold when applying non-maxima suppression. By default, the threshold is 0.3 (30%).
* `--use-gpu` or `-u` is an optional argument which requires 0 or 1 denoting the use of GPU. By default, the CPU is used for computations
```
python3 yolo_video.py --input <input video path> --output <output video path> --yolo yolo-coco [--confidence <float number between 0 and 1>] [--threshold <float number between 0 and 1>] [--use-gpu 1]
```
Examples: 
* Running with defaults
```
python3 yolo_video.py --input inputVideos/video01.mp4 --output outputVideos/video01_Out.avi --yolo yolo-coco 
```
* Using GPU
```
python3 yolo_video.py --input inputVideos/video01.mp4 --output outputVideos/video01_Out.avi --yolo yolo-coco --use-gpu 1
```
