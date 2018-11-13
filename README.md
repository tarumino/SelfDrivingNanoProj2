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
[video1]: ./project_video.mp4 "Video"

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  If you want to extract more test images from the videos, you can simply use an image writing method like `cv2.imwrite()`, i.e., you can read the video in frame by frame as usual, and for frames you want to save for later you can write to an image file.  

To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `output_images`, and include a description in your writeup for the project of what each image shows.    The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

If you're feeling ambitious (again, totally optional though), don't stop there!  We encourage you to go out and take video of your own, calibrate your camera and show us how you would implement this project from scratch!


## Writeup / README
---

Right here!

## Camera Calibration
---
### Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

To calibrate the camera the openCV's calibrateCamera function has been used. Instead of the 8x6 example, the chessboard given were 9x6 so the parameters has been modified to nx = 9 and ny = 6 to match the pattern. All of the calibration chessboards are loaded, grey-scaled, and then the corners are found. From these corners, the distortions are calculated by taking the real vs image point difference to obtain camera matrix. Using the camera matrix, the image can be undistorted as shown below. We can see that left image is warped whereas undistorted chessboard on right has straight lines.

![alt text][image1]


