##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

[image7]: ./output_images/chess_original.png "chess original"
[image8]: ./output_images/chess-undistorted.png "chess undistorted"
[image9]: ./output_images/undistorted.png "undistorted"
[image10]: ./output_images/threshdolded-s-xsobel.png "thresholded"
[image11]: ./output_images/threshold-transform.png "warped"
[image12]: ./output_images/transform.png "transformed"
[image13]: ./output_images/lane_line.png "identify lane lines"
[image14]: ./output_images/example_output.jpg "Output"
[video2]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!
###Camera Calibration

####1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

Camera calibration is done on line 6 of https://github.com/angelovila/CarND-P4-advanced-lane-finding/blob/master/p4.ipynb.

Created a function called findchess that accepts a list of calibration images (image with chessboard) and using cv2.findchessboardCorners to find the position of each corners storing the positions in a list. Then using the points to  cv2.Calibrate camera.

Resulting values from cv2.Calibrate camera can then be used to cv2.undistort which takes an image and returns an undistorted image based on the values gathered from cv2.Calibrate Camera

Original chessboard image

![alt_text][image7]

Undistorted chessboard image

![alt_text][image8]


###Pipeline (single images)

####1. Provide an example of a distortion-corrected image.

Here's an example of an distortion-corrected image
-https://github.com/angelovila/CarND-P4-advanced-lane-finding/blob/master/p4.ipynb (line 10)

![alt text][image9]


####2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

Created multiple functions for different types of thresholded binary images.
-https://github.com/angelovila/CarND-P4-advanced-lane-finding/blob/master/p4.ipynb (from line 12 to line 20)

Here's an example of a binary thesholded image.

![alt_text][image10]


####3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

I first undistort the image and then apply a cv2.getPerspectiveTransform. 
-https://github.com/angelovila/CarND-P4-advanced-lane-finding/blob/master/p4.ipynb (line 21)

The following are used for the source and destination points
```
src = np.float32([
                  [240,670],
                  [560,470],
                  [720,470],
                  [1040,670],
                 ])

dst = np.float32([
                  [215,670],
                  [215,0],
                  [1065,0],
                  [1065,670],
])
```

Resulting transformed image is below

![alt_text][image12]


####4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?
-https://github.com/angelovila/CarND-P4-advanced-lane-finding/blob/master/p4.ipynb (line 27-28)

I created a function called lane_lines that accepts a transformed binary image. Then returns an output image with the lane lines identified with a plotted lines. Also returns the left and right polynomial values.

It assumes a middle dividing line and between the binary-transfomed image and then assigns a value on the red channel, and then assigns a vlue on the blue color channel for the right lane line

Then give a specific window dimension, it scans the image window by window counting the window with the most number of pixels to identify where is the lane line.

Then using the identified windows for the left lane and right lane, the position of the window is then used to plot a polynomial


![alt_text][image13]


####5. Describe how (and identify where in your code) you calculated the radius of curvature of the 
and the position of the vehicle with respect to center.

Using the left and right polynomial values generated from lane_lines function, curvature is calculated using the equation
curve=∣2A∣(1+(2Ay+B)**2)**3/2
-https://github.com/angelovila/CarND-P4-advanced-lane-finding/blob/master/p4.ipynb (line 30)



####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.



![alt_text][image14]


---

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./p4.mp4)

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

There issues on the thresholding where lane lines are not correctly detected on certain road conditions. Potentially the way to fix this is to have a specific location threshold inputs in the pipeline.
