
## Introduction

This project is to recognise images coming from the webcam on a PC, 
using Lubuntu running in a Virtual Machine guest (VM). 
This example refers to Virtual Box options.

## Pass through your webcam

* Start your VM
* In Virtual Box Guest Menu (toolbar) / Devices / Webcams / _\<choose your webcam\>_
* Inside the actual VM's Start Menu / Sounds and Video / guvcview

This will test whether you have successfully passed your webcam into the VM

If guvcviev failed:
* Open terminal
```
sudo apt install -y cheese
```
* Then run
```
cheese
```

## Clone scripts to use

We found https://github.com/gudovskiy/yoloNCS a little complicated, 
so we wanted yo use https://github.com/jincongho/NCS_TinyYolo for the script to 
take the webcam feed and pass it to YOLO. However that was only compatible with 
NCSDK1, so we forked it to make it compatible with NCSDK2, 
in https://github.com/artmg/NCSDK2_TinyYolo

```
mkdir -p ~/CamRecog
cd ~/CamRecog
git clone https://github.com/artmg/NCSDK2_TinyYolo.git
cd NCSDK2_TinyYolo
make
```

Here is the sample output from the make command

```
making prototxt
Prototxt file already exists

making profile
mvNCProfile tiny-yolo-v1.prototxt -s 12
mvNCProfile v02.00, Copyright @ Intel Corporation 2017

****** WARNING: using empty weights ******
/usr/local/bin/ncsdk/Controllers/FileIO.py:65: UserWarning: You are using a large type. Consider reducing your data sizes for best performance
Blob generated
USB: Transferring Data...
Time to Execute :  151.82  ms
USB: Myriad Execution Finished
Time to Execute :  132.19  ms
USB: Myriad Execution Finished
USB: Myriad Connection Closing.
E: [         0] dispatcherEventReceive:198	dispatcherEventReceive() Read failed -1

USB: Myriad Connection Closed.
Network Summary

Detailed Per Layer Profile
                                                               Bandwidth   time
#    Name                                                MFLOPs  (MB/s)    (ms)
===============================================================================
0    data                                                   0.0222998.7   0.005
1    scale1                                               173.4  1183.0   8.738
2    pool1                                                  3.2   796.8   7.687
3    scale2                                               462.4   867.1  15.908
4    pool2                                                  1.6   935.3   3.275
5    scale3                                               462.4   653.2  10.606
6    pool3                                                  0.8   965.5   1.586
7    scale4                                               462.4   411.2   8.723
8    pool4                                                  0.4   959.1   0.798
9    scale5                                               462.4   203.9  11.210
10   pool5                                                  0.2   937.4   0.408
11   scale6                                               462.4   281.2  11.066
12   pool6                                                  0.1   873.4   0.219
13   scale7                                               462.4   726.8  12.976
14   scale8                                               231.2   654.9   8.187
15   fc9                                                    0.0  2172.4  16.201
-------------------------------------------------------------------------------
                                   Total inference time                  117.59
-------------------------------------------------------------------------------
Generating Profile Report 'output_report.html'...

making caffemodel
Downloading caffemodel file
--2018-05-25 19:29:36--  http://ncs-forum-uploads.s3.amazonaws.com/ncappzoo/tiny_yolo/tiny-yolo-v1_53000.caffemodel
Resolving ncs-forum-uploads.s3.amazonaws.com (ncs-forum-uploads.s3.amazonaws.com)... 52.219.20.116
Connecting to ncs-forum-uploads.s3.amazonaws.com (ncs-forum-uploads.s3.amazonaws.com)|52.219.20.116|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 108402374 (103M) [application/octet-stream]
Saving to: ‘./tiny-yolo-v1_53000.caffemodel’

tiny-yolo-v1_53000. 100%[===================>] 103.38M  4.29MB/s    in 3m 17s  

2018-05-25 19:32:55 (536 KB/s) - ‘./tiny-yolo-v1_53000.caffemodel’ saved [108402374/108402374]


making compile
mvNCCompile -o graph -w tiny-yolo-v1_53000.caffemodel -s 12 tiny-yolo-v1.prototxt
mvNCCompile v02.00, Copyright @ Intel Corporation 2017

/usr/local/bin/ncsdk/Controllers/FileIO.py:65: UserWarning: You are using a large type. Consider reducing your data sizes for best performance
Blob generated

```

## Improvements

* add this to the main NCAppZoo repo
  - fork https://github.com/movidius/ncappzoo
  - go into your equivalent of https://github.com/movidius/ncappzoo/tree/ncsdk2/caffe/TinyYolo
  - add in the stream code and submit a pull request
   - maybe add our code into V2 and jincongho into V1
   


