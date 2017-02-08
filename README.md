
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

[undist_img]: ./output_images/undist.jpg "Undistorted"
[original]: ./output_images/original.jpg "Original Image"
[final]: ./output_images/final.jpg "Final Example"
[binary]: ./output_images/combined_masked_binary.jpg "Binary Example"
[gradx]: ./output_images/gradx.jpg "GradX"
[grady]: ./output_images/grady.jpg "GradY"
[hls]: ./output_images/hls_binary.jpg "HLS"
[s_image]: ./output_images/s_image.jpg "S"
[dir]: ./output_images/dir_binary.jpg "Dir"
[video1]: ./output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

---

###Camera Calibration

####1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

Camera calibration was done using the get_calib_objects function to get a set of object and image points, then these are passed to cv2 calibrateCamera. Used chessboard images are 9x6


###Pipeline (single images)

####1. Image is undistorted using the results of the camera calibration
![alt text][original]
- starting image-
To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
- undistorted image-
![alt text][undist_img]
####2. Thresholds
I experimented with a number of different binary threshold functions, finally settling on a combination of the x gradient, y gradient, directional gradient and the S channel of the HLS colourspace.
In my experiments, the directional gradient was quite noisy but was useful as a final filter nonetheless.
- gradient image-
![alt text][gradx]
![alt text][grady]
![alt text][hls]
![alt text][dir]
![alt text][binary]


####3. Perspective transformation
I found it most useful to transform all of the gradients to "top down" perspective before combining them. After that, I added some masking to remove noise from areas well outside the expected lane areas.
"source" and "destination" points were selected based on turning one of the sample "straight lane" images into vertical lines in the top down perspective.

- perspective images - 

####4. Finding lane lines
Based on the above threshold images, I used two functions to get lane lines. The first assumes nothing, and looks for lane lines at the bottom of the image and moves up in windows, tracking the lines. 
The second function assumes you already have a previous lane line and just fine tunes it based on current threshold values.



####5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][final]

---

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./output.mp4)

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

