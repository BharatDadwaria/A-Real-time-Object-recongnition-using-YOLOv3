# A-Real-time-Object-recongnition-using-YOLOv3
YOLOv3 (2018) is the one of the path breaking research followed by YOLOv4 (2020) in the field of Computer Vision and Artificial Intelligence. This model can be helpful and useful in many research or industrial fields to make a change in the society.

Before starting the recognition system lets focus on the research paper of YOLOv3 which i have tried to understand and presenting in my words.

# YOLOv3 : An imprrovemental approach

## Introduction 
YOLO stands for “You Only look once” is a real time object detection architecture. This is the one of the revolutionary Deep Convolution based Object Detection and Classifier firstly published in 2016 as YOLOv1 by a bunch of researchers which is further awarded as the one of the path-breaking research in the year. The further extension goes to YOLO9000 model which is kind of the 2nd version of YOLO series, which was also awarded as the one of the path-breaking research in the field of Computer Vision and Deep Learning. In 2018 the 3rd version of the YOLO series was published which is also claimed to be faster, stronger and efficient then YOLO9000 (v2) although it may not  be the most accurate but it is the primary choice for real time object detection application such as autonomous vehicles. The one of the major advancement of the YOLO family then the other classifier such as R-CNN, Fast R-CNN and etc is its architecture. The other classifier uses the approach of first generate potential bounding boxes and then run classifier on these bounding images which may takes a lot of time to optimize each of its component. This is the one of the major reason behind the success of the YOLO family that is the architecture of the YOLO is designed in such a way that it does scan whole image only once and predicts the multiple boundary boxes and their class probability simultaneously.

![alt text](http://url/to/img.png)

## Architecture 
The YOLOv3 contains total of 106 layer fully convolutional architecture. YOLOv3 makes prediction into three scales which is achieved by downsampling the input images by 32,16,8 respectively and upsampling by 2*. The author used a feature map from earlier in the network and concatenate it with the upsampled features so that the size may be increased Then the author added some few more convolutional layers to predict a twice sized tensor. And the author performs the same design one more time to predict boxes  for the final scale. For feature extraction part YOLOv3 uses completely new network which is kind of hybrid approch of YOLOv2 (24 Convolutions) and Darknet-19 (19 Convolutions) network combining to 53 convolutional layer architecture thats why this networkis also called as Darknet-53. All the layers uses convolution of 3*3 and 1*1 successively. With training from ImageNet Dataset the author finds it more powerful then Darknet-19 and much efficient then ResNet-101 and ResNet-152.

