## Writeup

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

[undistchess]: ./writeup_images/undistortchess.png "Undistorted Chess board"
[undistlane]: ./writeup_images/undistortlane.png "Undistorted Lane"
[binary_thresh]: ./writeup_images/binary_threshold.jpg "Binary Thresholded Image"
[fitlaneline]: ./writeup_images/fitlines.png "Fitting lane Lines"
[contfitline]: ./writeup_images/continfitlines.png "Continours fitting lines"
[lanesdetected]: ./writeup_images/lanesdetected.png "Lanes detected"
[persp]: ./writeup_images/perspectivetransform1.png "Perspective Transform"
[meanbrightnesslow]: ./writeup_images/mean_brightness.png "Mean brightness low"
[meanbrightnesshigh]: ./writeup_images/mean_brightness2.png  "Mean brightness high"
[outputvideo]: ./avg-final-project_video_output.mp4 "Output video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the code cells 2-6 of the IPython notebook located in "./detect_lane.ipynb"

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][undistchess]

### Pipeline (single images)

#### 1. Distortion Correction

I apply the distortion correction using the function `cal_undistort()`. It takes in the arguments `img, objpoints, imgpoints` which are obtained from the calibration step above.
![alt text][undistlane]


#### 2. Binary Thresholding

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps defined in `threshold_pipeline()`).  

I performed a dynamic thresholding based on the mean brightness of the image. I calculate the mean brightness in a specific region of image by converting the image to HSV space and then taking the mean of the V channel to find the brightness of the image (steps defined in `mean_brightness()`)

For an image with lots of asphalt the mean brightness is low
![alt text][meanbrightnesslow]

For an image with lots of asphalt the mean brightness is high
![alt text][meanbrightnesshigh]

Based on the brightness I decide which combination of thresholding to apply to the image (details in `threshold_pipeline()`). 

All the different binary thresholds are visualized
![alt text][binary_thresh]

In the end I used the combination of the color thresholds and sobel. 
#### 3. Perspective Transform

The code for my perspective transform includes a function called `perspective_transform()`.  I chose to  hardcode the source and destination points in the following manner:

```python
src_pt1 = [585, 460]
dst_pt1 = [320, 0]
src_pt2 = [200, 720]
dst_pt2 = [320, 720]
src_pt3 = [1100, 720]
dst_pt3 = [960, 720]
src_pt4 = [705, 460]
dst_pt4 = [960, 0]

src = np.float32([src_pt1, src_pt2, src_pt3, src_pt4])
dst = np.float32([dst_pt1, dst_pt2, dst_pt3, dst_pt4])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 200, 720      | 320, 720      |
| 1100, 720     | 960, 720      |
| 705, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][persp]

#### 4. Identifying Lane lines and Fitting Line

Then I identified lane lines by using the sliding window method and fitting lane lines to them using 2nd order polynomials. This is defined in the function `fitlines()`. 

![alt text][fitlaneline]

Once the lines have been fit instead of using a sliding window, for the next frame I use a margin of 100 to detect lines in the next frame. 

![alt text][contfitline]


#### 5. Radius of Curvature and Center Offset

This is implemented in the function `find_curvature`

#### 6. Example output

The final output after passing an image through the pipeline outlined above is

![alt text][lanesdetected]

---

### Pipeline (video)

Here's the final video:
[![Advanced Lane Finding](https://img.youtube.com/vi/qa55edKIEco/0.jpg)](https://www.youtube.com/watch?v=qa55edKIEco)

---

### Discussion
The pipeline is likely to generalize poorly to roads with very white/concrete sections like in the challenge video. It is highly tuned to the current scenario. To obtain a generalizeable system it might be a good idea to train a Deep learning model. 
