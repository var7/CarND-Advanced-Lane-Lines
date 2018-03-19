## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


In this project, I write a software pipeline to identify the lane boundaries in a video using advanced Computer Vision Techniques.  Check out the [writeup](https://github.com/var7/CarND-Advanced-Lane-Lines/blob/master/writeup.md) 

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

The pipeline works well on the given project video but performs poorly on the challenge videos. 

For further information refer the [writeup](https://github.com/var7/CarND-Advanced-Lane-Lines/blob/master/writeup.md) and [notebook](https://github.com/var7/CarND-Advanced-Lane-Lines/blob/master/detect_line.ipynb). 

Here's the final video:  

[![Advanced Lane Finding](https://img.youtube.com/vi/qa55edKIEco/0.jpg)](https://www.youtube.com/watch?v=qa55edKIEco)

