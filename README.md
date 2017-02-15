# Project 4 - Advanced Lane Finding

<p align="center">
 <a href="https://www.youtube.com/watch?v=VY1K6rj9TUQ&feature=youtu.be"><img src="./view.gif" alt="Overview" width="50%" height="50%"></a>
 <br>Qualitative results. (click for full video)
</p>

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


Below is the list of techniques used to detect lanes.

- Camera Calibration: Transformation between 2D image points to 3D object points.
- Distortion Correction: Consistent representation of the geometrical shape of objects.
- Perpective Transform: Warping images to effectively view them from a different angle or direction.
- Edge Detection: Sobel Operator, Magnitude Gradient, Directional Gradient, and HLS Color Space with Color thresholding
- Sanity Check: Used Coefficients of Polynomial fittings for parrellel line 


### Camera Calibration


### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.
<p align="center">
 <img src="./img/camera.png" alt="Overview" width="100%" height="100%">
 <br>Qualitative results. (click for full video)
</p>

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.
<p align="center">
 <img src="./img/hls.png" alt="Overview" width="100%" height="100%">
 <br>HLS
</p>

<p align="center">
 <img src="./img/lab.png" alt="Overview" width="100%" height="100%">
 <br>LAB
</p>

<p align="center">
 <img src="./img/sobel.png" alt="Overview" width="100%" height="100%">
 <br>SOBEL X
</p>

<p align="center">
 <img src="./img/threshold.png" alt="Overview" width="100%" height="100%">
 <br> Combined
</p>

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.
<p align="center">
 <img src="./img/transform.png" alt="Overview" width="100%" height="100%">
 <br> Transform
</p>


#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?
I use the Udacity Histogram method for identifying peaks to find the laneline initially using the threadholded binary warped images using sliding window technque. Then us polyfit to find the coefficients.

For subsequences frame, as the lines are detected, the line are idenitified by searching from the previous detected line.

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

```
def find_curvature(yvals, fitx):
    ym_per_pix=30/720 # meters per pixel in y dimension
    xm_per_pix = 3.7/700 # meters per pixel in x dimension
    # Define y-value where we want radius of curvature
    y = np.max(yvals)*ym_per_pix
    radiusCurvature = ((1 + (2*fitx[0]*y + fitx[1])**2)**1.5)/np.absolute(2*fitx[0])
    return radiusCurvature
```

```
def find_position(left,right):
    position = image_shape[1]/2
    ym_per_pix=30/720 # meters per pixel in y dimension
    xm_per_pix = 3.7/700 # meters per pixel in x dimension
    
    left  = left[ (left[:,0] > 700)]
    right = right[ (right[:,0] > 700)]

    minLeft = np.min(left)
    minRight = np.max(right)
    
    laneCentre = (minLeft + minRight)/2
    return (position - laneCentre)*xm_per_pix
```


#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.
<p align="center">
 <img src="./img/resultImage.png" alt="Overview" width="100%" height="100%">
 <br> Result
</p>

###Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Most problems come from shadows and noises. This is solved using the averaging of last the 10 fit cofficients to make the land detection more stable. Another important technique used was sanity checking for frame that didnt make sense. This is checked by checking if right and left lane are parrellel within an error and if it is not then these frames are ignored, this solved most of my shadow problem.

There is a lot of rooms for improvement
