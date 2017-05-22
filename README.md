# Vehicle Detection
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


[image2]: ./output_images/bin.png "Before undistort image based on camera calibration"
[image3]: ./output_images/hist.png "Before flipping"
[image4]: ./output_images/hog.png "Before flipping"
[image5]: ./output_images/windows.png "Before flipping"
[image6]: ./output_images/heatmap.png "Shadowed image"
[image7]: ./output_images/predicted.png "Shadowed image"
[image8]: ./output_images/final_video.png "Shadowed image"
[image9]: ./output_images/augment_img.png "Shadowed image"

In this project I created vehicle detection pipeline using Linear Support Vector Machine model. 
To implement This pipeline I have to apply features extracting techniques to extract features for classification model and sliding windows techinque to capture one vehicle in the whole image. 


The Project
---

The goals / steps of this project are the following:
* 1. Extract Features with following techniques. 
     * (1) Using raw pixel value by reducing their size which is useful to detect image that are not varing their appearance.  
    * (2) Using histogram of colors to detect color difference between car & Non-car values. 
    * (3) HOG features to capture direction of the gradient. This impose strong value to the parts of image which show great variation in shape while impose small value to an parts of image where show small variation  by noise.  
* 2 Fit the features to Linear Support Vector Machine model. 
* 3 Make a sliding windows to apply Machine Learning model for vehicle and non-vehicle classification.
* 4 Heatmap thresholding to detect False Positive. 
* 5 Define class to save recent detected vehicles on video.  

* The data used for model fitting is stored in vehicles and non-vehicles folder.
* You can skip the model fitting part by loading .sav and .npy files 

Extract Features
---

### Using raw pixel
* Using raw pixel value of reduced size image. 
![alt text][image2]

### Using Color Histogram of the Image. 
* By comparing histogram of various color space we can verify that pixel value vary most in the hsv color space. 
![alt text][image3]

### Get histogram of oriented gradients
* By capturing the intensity of the gradient in each region of image we can use it as features for classification model. 
![alt text][image4]


### Combining features 
* I choosed hsv as a color space which show greatest variation in histogram. 
* Choosed 9 orientations on HOG features. 8 pixel per cell and 2 cell per blocks as hog parameters. 
* Used 16 * 16 as a size for spatial features. 

Fit the features to Linear Support Vector Machine model. 
---
* Considering the model implementation time and avoid local minimum problem I choosed SVM machine rather than NN. 
* Didn't try to implement non-linear kernel since model show high accuracy on test data set. (98.81%)

Transform Image by sliding windows. 
---
* To apply fitted SVC model from previous steps we need to follow these steps. 
    * (1) Get hog features of road image with center camera on the vehicle. 
    * (2) Slide image with pre-defined window size append spatials bins and color bins as featues with hog features from step1. 
    * (3) We can adjust window size. We need to use bigger windows on closer car and smaller windows on car with great distances. 
    * (4) From the features of each window predict if each window image is car or not. If predicted value of window is car return window as a point
![alt text][image5]

Heatmap thresholding to detect False Positive. 
---
* To handle False Positive detect heat map of positive windows and choose positive windows that meat certain threshold.
![alt text][image6]
![alt text][image7]

Define class to save recent detected vehicles on video.  
---
* I have defined hot window classes to update constantly for more robust detection of image. 
* By using recent 7 detected windows we can detect vehicles from video that show great signs. 

Final result
---
[![alt text][image8]](https://youtu.be/sGH4k4GTAPk)

Discussion.
---
* I have tried to implement PCA for dimensional reduction for more robust model implementaion however it impose much time on features implementation so I decided not to use on this project. 
* I have spent so much time because of scailing difference between jpg and png format. 
* I could use deep learning model to make better detection.