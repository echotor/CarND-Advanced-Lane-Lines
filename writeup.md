## Advanced Lane Finding

### This is forth project of Self-Driving Car Engineer Nanodegree Program.

---

**Advanced Lane Finding Project**

The steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Defined color transforms and gradients function, and find a combined method function to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Finally, Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/undistorted_calibration2.jpg "Chess Image"
[image2]: ./output_images/undistorted_output.png "Undistorted Image"
[image3]: ./output_images/undistorted_test2.jpg "Undistorted Test"
[image4]: ./output_images/combined_binary.jpg "Combined Binary"
[image5]: ./output_images/source_and_des_point.png "Source Des"
[image6]: ./output_images/warp_result.png "Warp result"
[image7]: ./output_images/2d_polynomial.png "2d Polynomial"
[image8]: ./output_images/bad_line_find.jpg "Bad Line Find"
[image9]: ./output_images/good_line_find.jpg "Good Line Find"
[image10]: ./output_images/final_result.jpg "Final Result"
[video1]: ./project_video.mp4 "Video"


### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the third code cell of the IPython notebook located in "./SolutionImplement.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection. Like below:

![alt text][image1]

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image2]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image3]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I define the related function about transform in seventh code cell of the IPython notebook located in "./SolutionImplement.ipynb". They imply the transform on image like program.

Then at firstly I used a combination of color and sobel x gradient to generate a binary image(in eleventh code cell of the IPython notebook located in "./SolutionImplement.ipynb"). I tested in the test images. Here's an example of my output for this step.

![alt text][image4]

#### 3. Describe how you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp_image()`, which appears in in eithth code cell of the IPython notebook located in "./SolutionImplement.ipynb".  The `warp_image()` function takes as inputs an image (`img`), as well as source (`src_point`) and destination (`dst_point`) points.  I chose the hardcode the source and destination points in the following manner:

```
src_point = np.float32([
        [580, 460],
        [700, 460],
        [1040, 680],
        [260, 680],
    ])

dst_point = np.float32([
        [260, 0],
        [1040, 0],
        [1040, 720],
        [260, 720],
    ])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 580, 460      | 260, 0        | 
| 700, 460      | 1040, 0      |
| 1040, 680     | 1040, 720      |
| 260, 680      | 260, 720        |

The area of image like below:

![alt text][image5]

I verified that my perspective transform was working as expected by drawing the `src_point` and `dst_point` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image6]

#### 4. Describe how you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image7]

I defined the `find_lane_lines` function in "./SolutionImplement.ipynb".

#### 5. Describe how you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in function `get_curvature` of the IPython notebook located in "./SolutionImplement.ipynb".

But I found the effect of this was not good. Like below:

![alt text][image8]

So I change the combined function which in eleventh code cell of the IPython notebook located in "./SolutionImplement.ipynb". And I combined the HLS L-channel and LAB B-channel color transform together. The effect of this combined transform is more better than above. Like below:

![alt text][image9]

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in seventeenth code cell of the IPython notebook located in "./SolutionImplement.ipynb".  Here is an example of my result on a test image:

![alt text][image10]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./output_images/project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

* At firstly, I used a combination of color and sobel x gradient to generate a binary image, but find the effect on image is not good. So we should try one more combined transforms. I combined the sobel x、sobel y、magnitude of the gradient、direction and color transform together. The effect of this combined transform is more better than above.
* When I apply in challenge video, the effect is not good. I think the reason is in combination function. For more better effect, we should find a better combination to improve the effect of detect line.
* I find the effect of combined method is good on the normal video image. Maybe I should add other function together.
