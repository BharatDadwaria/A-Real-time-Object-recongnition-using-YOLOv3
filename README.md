# A-Real-time-Object-recognition-using-YOLOv3
YOLOv3 (2018) is the one of the path breaking research followed by YOLOv4 (2020) in the field of Computer Vision and Artificial Intelligence. This model can be helpful and useful in many research or industrial fields to make a change in the society.

![alt text](https://raw.githubusercontent.com/BharatDadwaria/A-Real-time-Object-recongnition-using-YOLOv3/master/result.jpeg)

Before starting the recognition system lets focus on the research paper of YOLOv3 which i have tried to understand and presenting in my words.

# YOLOv3 : An imprrovemental approach

## Introduction 
YOLO stands for “You Only look once” is a real time object detection architecture. This is the one of the revolutionary Deep Convolution based Object Detection and Classifier firstly published in 2016 as YOLOv1 by a bunch of researchers which is further awarded as the one of the path-breaking research in the year. The further extension goes to YOLO9000 model which is kind of the 2nd version of YOLO series, which was also awarded as the one of the path-breaking research in the field of Computer Vision and Deep Learning. In 2018 the 3rd version of the YOLO series was published which is also claimed to be faster, stronger and efficient then YOLO9000 (v2) although it may not  be the most accurate but it is the primary choice for real time object detection application such as autonomous vehicles. The one of the major advancement of the YOLO family then the other classifier such as R-CNN, Fast R-CNN and etc is its architecture. The other classifier uses the approach of first generate potential bounding boxes and then run classifier on these bounding images which may takes a lot of time to optimize each of its component. This is the one of the major reason behind the success of the YOLO family that is the architecture of the YOLO is designed in such a way that it does scan whole image only once and predicts the multiple boundary boxes and their class probability simultaneously.



## Architecture 
The YOLOv3 contains total of 106 layer fully convolutional architecture. YOLOv3 makes prediction into three scales which is achieved by downsampling the input images by 32,16,8 respectively and upsampling by 2*. The author used a feature map from earlier in the network and concatenate it with the upsampled features so that the size may be increased Then the author added some few more convolutional layers to predict a twice sized tensor. And the author performs the same design one more time to predict boxes  for the final scale. For feature extraction part YOLOv3 uses completely new network which is kind of hybrid approch of YOLOv2 (24 Convolutions) and Darknet-19 (19 Convolutions) network combining to 53 convolutional layer architecture thats why this networkis also called as Darknet-53. All the layers uses convolution of 3*3 and 1*1 successively. With training from ImageNet Dataset the author finds it more powerful then Darknet-19 and much efficient then ResNet-101 and ResNet-152.
![alt text](https://raw.githubusercontent.com/BharatDadwaria/A-Real-time-Object-recongnition-using-YOLOv3/master/YOLOv3%20Architecture3.png)

## Bounding box Prediction
As in YOLO9000 (YOLOv2), the architecture of the YOLOv3  predicts the bounding boxes same as the YOLO9000. The model predicts the four coordinates for each of the bounding boxes tx, ty, tz, th . At the time of training, YOLOv3 uses the same loss function as the YOLO9000 (YOLOv2) that is the Sum of Squared error loss. YOLOv3 used to predict the object class score by logistic regression for each of the boundary boxes. The new concept of anchor boxes was introduced in this paper which is the same boundary boxes predefined by some width and size.YOLOv3 predicts the boundary boxes in 3 different scales which is being extracted by using similar concept as Feature pyramid networks. 

## Class Prediction 
Each boundary boxes predicts the classes with the help of the multilabel  Classification. Rather using the softmax the author used the logistic classifier with the binary cross entropy loss at the training time for the class prediction for good performance. While using the softmax function the major disadvantage is the overlapping labels between multiple class (I.e : Woman or Person) for the same boundary box because the softmax function predicts only one class for the each of the boundary boxes which is usually notthe case while handling the complex datasets like Open Image dataset. And the multilabel approach is the best approach while handling the real time object detection and classification. 

Prediction is happened in 3 different  scales as we have already discussed in boundary boxes part. Since each image is divided into multiple grid and Each grid makes the 3 prediction using the 3 anchor boxes which lead to the total of 9 predictions. The predefined sized 9 anchor boxes are chosen using k-means clustering by selecting 9 most common output shapes which will be known as the 9 anchor boxes. A cell is selected if the center of an object falls  in the receptive field of that cell. The output of the network will consist of an equation for output dimension at each cell : 
(number of prediction per cell)*(5 +number of objects to be detected)  
Here 5 are the different coordinates that is the center coordinates of the cell(x,y), height, width, and detection confidence. The first four coordinates, we have already discussed in the boundary boxes part and the fifth one is detection confidence which is refers to the probability that the cell contain a particular object. In this research paper author uses the COCO dataset for experiment where they predicts 3 boxes (I.e number of prediction per cell) at each scale with 80 class prediction (I.e number of objects to be detected). The class prediction (80) contains the probability of that object being any object through one hot encoding. If the confidence interval has the value less then a particular threshold then we will consider that there is no objects in the bounding box so that we don't need to look at those 80 probabilities. So the output at each scale is now 
3*(5+80)*(#number of cells)
where the number of cells refers to the grid dimension scale size of that particular image. For example for grid size of 16*16 grid distribution the output at each scale will be 3*85*16*16 This means that we need these 768*85 (reduced the above 4d to 2d) times we have to look for making the bounding boxes. Prediction feature map at different scales. We used to predict 3 boundary box at each cell. So there will be 3 boundary boxes for each cell So we concatenate them all together to make prediction. For multiple detection the researchers used Non-max suppression algorithm which suppresses all the other detection but output the only appropriate one to get rid of multiple detection of same object based on some threshold. And then draw the remaining bounding boxes using OpenCV.

![alt text](https://raw.githubusercontent.com/BharatDadwaria/A-Real-time-Object-recongnition-using-YOLOv3/master/YOLOv3%20Architecture2.png)

The first detection (first scale) is done at 82nd layer, where the first 81th layer dowsampled the image with 32 stride making the image of 416*416 to a 13*13 feature map using 1*1 detection kernel outputs the 13*13*255 detection feature map dimension. Then the feature map from layer 79 to subjected to few convolutional layer before by upsampling the 2* to output the 26*26 which is further concatenated with the feature map from layer 61. And then the combined feature again subjected to some few 1*1 convolutional layers to fuse the features from the earlier layer 61. And the 2nd detection is done at 96th layer making it the 26*26*255 detection feature map dimensional and the same process continue of subjecting few convolution before from layer 91 and being concatenated from layer 36 and goes to few 1*1 convolutional layers follow to fuse the feature maps from layer 36, and at layer 106th the final detection part by output the 54*54*255. The YOLOv3 predicts total of 10647 boundary boxes for a 416*416 image, 10times more when compare to YOLOv2 which was 845 (13*13*5). And this is the power of YOLOv3 and making it faster, efficient and powerful then rest of the real time object detection algorithms.



