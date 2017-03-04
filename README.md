
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
[calib_image]: ./output_images/calibration4_undist.jpg "Calibration"
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


Camera calibration was done using the get_calib_objects function to get a set of object and image points, then these are passed to cv2 calibrateCamera. Used chessboard images are 9x6. Below is an example of a calibration image that has been undistorted.
![alt text][calib_image]

###Pipeline (single images)

####1. Image is undistorted using the results of the camera calibration

- starting image-
![alt text][original]

- undistorted image-
![alt text][undist_img]

####2. Thresholds
I experimented with a number of different binary threshold functions, finally settling on a combination of the x gradient, y gradient, directional gradient and the S channel of the HLS colourspace.
In my experiments, the directional gradient was quite noisy but was useful as a final filter nonetheless.
- gradx threshold -
![alt text][gradx]
- grady threshold-
![alt text][grady]
- hls thresholded -
![alt text][hls]
- directional threshold - 
![alt text][dir]


####3. Perspective transformation
I found it most useful to transform all of the gradients to "top down" perspective before combining them. After that, I added some masking to remove noise from areas well outside the expected lane areas.
"source" and "destination" points were selected based on turning one of the sample "straight lane" images into vertical lines in the top down perspective.

- final threshold, in top down perspective - 
![alt text][binary]

####4. Finding lane lines
Based on the above threshold images, I used two functions to get lane lines. The first assumes nothing (get_fit_window), and looks for lane lines at the bottom of the image and moves up in windows, tracking the lines. 
The second function (get_fit_no_window) assumes you already have a previous lane line and just fine tunes it based on current threshold values.



####5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I used get_curve function to get the curve radius, based on the function given in the class. Position of the vehicle was calculated in the image_pipeline function by comparing the predictions of the base of the left/right lanes with the image midpoint.

####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

Function merge_results takes the output of the threshold functions, and the calculated lines and merges them back on the final image, including warping the perspective back to the same as the original image

![alt text][final]

---

###Pipeline (video)


Here's a [link to my video result](./output.mp4). The lines are a little wobbly as the car transitions through some shadowy areas, but still stay reasonably close to the actual lane lines.

---

###Discussion


From trying the challenge video, my current pipeline is still very sensitive to shadows and objects that are close to the lane lines. I think that would be improved by tighter masking, as well as refining my choices for thresholding functions and limits used.

Other improvements would be to use the data from previous frames more. Currently I am using a moving average over 20 frames to produce a less jittery output. I'm also doing a very basic evaluation of the fitted lines to accept or reject the prediction for a given frame. That evaluation could be made more robust by comparing the calculated values with the previous accepted values to make sure they don't differ too much.

Also note that my solution re-uses a lot of code from the lectures for this project, and previous lectures.
