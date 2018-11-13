## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

This is the writeup of the Car Nano Degree Project 2 Advabced Lane Lines


The Project
---

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

[image1]: ./output_images/calibration_result.jpg "Undistorted"
[image2]: ./output_images/vehicle_calibration_result.jpg "Vehicle Undistorted"
[image3]: ./output_images/output_images/color_thresh_out.jpg "Color Threshold"
[image4]: ./output_images/grad_thresh_out.jpg "Grad Threshold"
[image5]: ./output_images/color_and_grad_thresh.jpg "Color and Grad Threshold"
[image6]: ./output_images/perspective_transformed.jpg "Perspective Transformed"
[image7]: ./output_images/polyfit.jpg "polyfit"
[image8]: ./output_images/process_result.jpg "result"
[video1]: ./project_video_out.mp4 "Video"

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  If you want to extract more test images from the videos, you can simply use an image writing method like `cv2.imwrite()`, i.e., you can read the video in frame by frame as usual, and for frames you want to save for later you can write to an image file.  

To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `output_images`, and include a description in your writeup for the project of what each image shows.    The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

If you're feeling ambitious (again, totally optional though), don't stop there!  We encourage you to go out and take video of your own, calibrate your camera and show us how you would implement this project from scratch!


## Writeup / README
---
#### 1.) Provide a Writeup / README that includes all the rubric points and how you addressed each one.

Right here!

## Camera Calibration
---
#### 1.) Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

To calibrate the camera the openCV's calibrateCamera function has been used. Instead of the 8x6 example, the chessboard given were 9x6 so the parameters has been modified to nx = 9 and ny = 6 to match the pattern. All of the calibration chessboards are loaded, grey-scaled, and then the corners are found. From these corners, the distortions are calculated by taking the real vs image point difference to obtain camera matrix. Using the camera matrix, the image can be undistorted as shown below. We can see that left image is warped whereas undistorted chessboard on right has straight lines.
![alt text][image1]

### 2.) 

## Pipeline (test images)
---
#### 1.)  Provide an example of a distortion-corrected image.

We can apply the exact same concept as above to undistort the image taken on vehicle, the result is shown below.
![alt text][image2]

#### 2.) Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image. Provide an example of a binary image result.

The color transformation, gradient filters were used in the section "Color transformation", "Gradient filter", and "Combining color and gradient threshold" which does exactly that. For the color section, the hls was used at it was best at amplifying the color effects which is varied by light and shades. The binary output filtered with threshold to gain colors that are yellow or white which are the color of lane markers.
![alt text][image3]

The gradient transform aimed to filter by Sobel filter which would extract stable lines which fits a certain criteria such as length, stability and general angle, which is further filtered with combination of amplitude and directional gradient filer to filter out lines that are clearly irrelevant, i.e. lines that are completely horizontal which can't possibly be lane lines on highways.
![alt text][image4]

Finally the two are combined to give output that gets points that were filtered by color and gradient filters.
![alt text][image5]

#### 3.) Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The image is perspective transformed to give a top down birds eye view. This is shown in section "Perspective transform". The transform is done by first getting and estimate from straight line to draw a trapezoid like shape to gain the target points that will map real view to brids eye view. Then the targets points are refined to gain as possible of a straight line as possible to gain image shown as below.
![alt text][image6]

#### 4.) Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

The poly-fit is done in the portion of the code "Filter, perspective transform, finding lanes". The image is first filtered then perspective transformed to be ready to be processed to find the line fits. The lines are found by first finding the rough estimate of starting position from hystogram, then a moving filter traces the line and create a polynomial fit of general region found.
![alt text][image7]

#### 5.) Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

After the poly fit lines are obtained, we can estimate the radius of curvature by plugging in the polynomial curvature into the radius of curvature equation, almost like taking a tangent to a curve, the result is shown in the image in the next section.

#### 6.) Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

Finally, all of these results are combined together create a polynomial line and fill onto the warped imaged. This image is then warped back to the original image to show us the lane borders found in the image, along with the lane curvature and offset from the center of lane as shown below.
![alt text][image8]

## Pipeline (video)
---
#### Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!)

By feeding the above algorithm into the pipeline that feeds in the video image, we can get the processed result as shown below.
[link to video result](./project_video_out.mp4)

## Discussion
---
####Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?

The biggest problem faced in this project was making it all fit together. Although individual components worked fairly well, it was rather cumbersome to put them all together in a working fashion though not too bad. Another is that there is so much tuning of parameters. Although each parameters were tuned component per component, it is impossible to tune them all in a way as to tune in a way that best benefits the entire pipeline... unless... this optimization can be automated... perhaps by machine learning by itself...?! The pipeline will most definitely fail in broken line with sharp curves or any obstacles obstructing the lines. To make it more robust, we could try predicting line in short term, to predict line curvature from what we saw in previous few seconds as well as long term where a memory or conecpt of lane lines considering database of lane lines and curvature to guess what is feasible, likely, unlikely to estimate the lines.