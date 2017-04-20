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

Here's an example of a binary thesholded image
![alt_text][image10]


####3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.



```
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])

```
This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt_text][image12]

![alt text][image4]

####4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt_text][image13]

![alt text][image5]

####5. Describe how (and identify where in your code) you calculated the radius of curvature of the 
and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt_text][image14]
![alt text][image6]

---

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./p4.mp4)

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

