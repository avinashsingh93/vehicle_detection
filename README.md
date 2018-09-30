## Writeup


**Vehicle Detection Project**



[//]: # (Image References)
![Car Hog features](/output_images/car_hog_feature.png)
![Non Car Hog features](/output_images/non_car_hog_feature.png)
![heatmap](/output_images/heatmap.png)
![output_image 1](/output_images/test1_outputimage.png)
![output_image 2](/output_images/test2_outputimage.png)
![output_image 3](/output_images/test3_output_image.png)
![output_image 4](/output_images/test4_output_image.png)
![output_image 5](/output_images/test5_output_image.png)
![output_image 6](/output_images/test_image6.png)

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.
I started by reading in all the `vehicle` and `non-vehicle` images.  Here are the images of one of each of the `vehicle` and `non-vehicle` classes:

![Car Hog features](/output_images/car_hog_feature.png)

I then took a random image and perfomed extracted some of the features of the image.  Below are the hog feature of the one 'car' object and 'non-car object'.:

![Non Car Hog features](/output_images/non_car_hog_feature.png)

Also I used `YCrCb` color space and HOG parameters of `orientations=16`, `pixels_per_cell=16 and ` `cells_per_block=2`, `hog_channel=0`, `spatial_size = (16, 16)` and `hist_bins = 16`:



#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of orientations, color_space and hog_channel to check which combination suits well on my code. Some of the combinations were :

1) Colorspace = 'YCrCb' orientation = 11 , hog_channel='ALL'
2) Colorspace = 'RGB' orientation = 11 , hog_channel='ALL'
3) Colorspace = 'HUV' orientation = 15 , hog_channel='ALL'
4) Colorspace = 'YCrCb' orientation = 16 , hog_channel='ALL'

But finally i got maximum accuracy with Colorspace `YCrCb` , `orientation = 16` and `hog_channel=0. This seem to be perfect combination as per me for my code.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

Earlier i was training classifier using linear model. But the results were not as per my expectations as the code was failing to detect cars ar some of the places. Then i tried using rbf model to train the classifier. ALthough it took a bit more time, but the results were satisfactorily.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I decided to take 3 windows with sizezs (64, 64) , (96, 96) and (128, 128) to perform sliding windows search.Overlapping which i used was 70%. Also i am running my windows in lower half of the image i.e. starting from 400 to 656. I used heat map to reject false images. For this i have taken threshold value of 2. 



#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Below are some of the examples of test images for which my pipeline is working fine:

![output_image 1](/output_images/test1_outputimage.png)
![output_image 2](/output_images/test2_outputimage.png)
![output_image 3](/output_images/test3_output_image.png)
![output_image 4](/output_images/test4_output_image.png)
![output_image 5](/output_images/test5_output_image.png)
![output_image 6](/output_images/test_image6.png)

---


Earlier i was not getting a good accuracy when using linear model to train the classifier. Also I was getting false positives which shouldn't be there. Using rbf model helped me to improve my accuracy. Also using heat map helped me to overcome false positives issue. 


### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a link to my project video and test video:

![project_output_video](/output_videos/project_video_output.mp4)
![test_video_output](/output_videos/test_output_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Below is one of the image of heat map after the required function was added in the code:


![heatmap](/output_images/heatmap.png)



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

In this project i had to try various combinations of my parameter in order to find which parameters are best suited for my classifier. For this i tried combinations of orient between 11 and 21 with only 1 window in it. But one window was not able to detect all the cars in it as size of car differs depending on position of car from the camera. So i had to use several windows to capture all sizes of car images in it. Using 3 windows of size (64,64), (96,96) and (128,128) minimizes the errors which i was getting while detecting the cars. But still the code was detecting some false positives in the image.Then i added heatmap in my code and set threshold=2 and changed the kernel from linear to rbf. By doing this, my accuracy increased to more than 99%. But still my code was detecing some false positives at video time = 23 seconds. 
Trying the same combination with hog_channel=0 instead of 'ALL' helped me to overcome this issue. The code was running fine after this.

I still feel that my model might not work in different lightning conditions and thus cannot be used in real time. I will try to use other techniques such as YOLO(real-time object detection system) to make the code more robust.
