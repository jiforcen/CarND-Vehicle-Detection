
## **Vehicle Detection Project**

---

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/HOG.png
[image2]: ./output_images/Results.png
[image3]: ./output_images/Window1.png
[image4]: ./output_images/Window2.png
[image5]: ./output_images/Window3.png
[image6]: ./output_images/Window4.png
[image7]: ./output_images/Hot_Areas.png
[image8]: ./output_images/Video_vis.png
[image9]: ./output_images/Final_Image.png

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README
Writeup that includes all the rubric points is here. [writeup](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup.md).

All the code included in this project is in the IPython notebook called [Code.ipynb](https://github.com/jiforcen/CarND-Vehicle-Detection/blob/master/Code.ipynb)

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

Function "get_hog_features" in cell 2 contains the functions necesaries to obtain HOG features from a image. Also in this cell are included diferent functions used in the project.

I started by reading in all the `vehicle` and `non-vehicle` images in cell 3, 8792 car images and 8962 not car images are readed, so we can considered it a well balanced dataset.

In cell 4 an exampled of hog extracted hog features are shown in one of the `vehicle` and `non-vehicle` classes images:
Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

![alt text][image1]

####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters looking which produces best results with the classifier. The combination presented give us 99.35% of accuracy so is considered correct.

Using `YCrCb` color space the parameteres used are:

* HOG: (`orient` = 9, `pix_per_cell = 8`, `cell_per_block = 2`, `hog_channel = "ALL"`)
* Spatial features: (`spatial_size = (32, 32)`)
* Histogram features: (`hist_bins=32`)

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

In cell 5 a linear SVM is trained using the features explained before extracted from the `vehicle` and `non-vehicle` dataset.
Data is splited into randomized training and test sets, 20 percent of data is used for test. Spatial and histogram features are added because improves the accuracy result of the classifier.
After the trainning proccess an accuracy of 99.35% is obtained.

![alt text][image2]

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

As we can see in cell 11 and in the next images, sliding windows are used to detect cars on the road at different sizes. Far cars appears in the middle of the image, whilst near cars apears in the bottom, thats the reason why windows increase their size when are in the bottom of the image.

Scales are decided depending on the size that cars appears in image. Windows bigger than cars produce detection of cars bigger than in reality.

Overlap were obtained experimentally, detecting correctly cars in the whole video.

![alt text][image3]
![alt text][image4]
![alt text][image6]
![alt text][image5]

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Functions created for car finding are in cells 9 and 10 of the IPhyton notebook. Four different scales are used from 1.0 to 2.5 with the features and classifier explained before.
It was important to scale correctly the image, because depending on the format image values vary from 0 to 255 or from 0 to 1.

###Filter false positives an overllapping bounding boxes.

####1. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

As we can see in pipeline detection (Cell 16), boxes detected in each frame of the video are recorded using class called `Hist_Boxes` (Cell 17). With the last n frames detection a heatmap is created and thresholded to identify vehicle positions.

I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap and the bounding boxes for two test images, for more we can see cell 18.
![alt text][image7]

---
### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

In cells 19 to 27 videos are generated.
For `test_video.mp4` and `project_video.mp4` are generated videos which include the final result, all the windows detected in this frame and the hot areas. These videos are the next [link to test_video_vis.mp4](./test_video_vis.mp4), [link to project_video_vis.mp4](./project_video_vis.mp4).
In the next image is showed a frame:
![alt text][image8]

Also the `project_video.mp4` lanes are detected using the functions of project 4 (Cell 25), so we can see in the same image lanes and car detected simultaneously [link to my final video result](./project_video_final.mp4)
![alt text][image9]

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

With the method used when two cars are closer are considered the same but is not correct. So separate it could be intereseting.

If I were going to pursue this project further I will try using Deep learning because is a great tool that can be used to anotate the whole image.

Also can be interesting detect what cars are viewed from front or from behind, to detect the direction of the detected car to ours.

