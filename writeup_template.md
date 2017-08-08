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
[combined_binary]: ./output_images/combined_binary "Combined Binary"
[Output Images]: ./output_images/final_result.png "Final Result"
[fit_output]: ./output_images/fit_output.png "fit warp"
[video1]: ./project_video.mp4 "Video"
[Project video output]: ./project_video_output.mp4 "Project video Output"
[Challenger video Output]: ./challenger_video_output.mp4 "Challenger video Output"
[Harder Challenger video Output]: ./harder_video_output.mp4 "Harder Challenger video Output"


## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

### Camera Calibration

For camera calibration, I used all the chess board images provided with the project. 

Populated objectPoints (#3d points in real world space) and imgPoints (corresponding 2D points in image plane) using the function findChessboardCorners of cv2 library. 

Once these image points and object points were populated used calibratedCamera to calibrate the camera. 

It is present in the following section of Advanced-Lane-Lines notebook. 

"Compute the camera calibration matrix and distortion coefficients given a set of chessboard images" 

There after a wrote a function in the section "Test distortion correction" to convert all the test images to undistored form.  

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./examples/example.ipynb" (or in lines # through # of the file called `some_file.py`).  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][combined_binary]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image 
It is present in section """Use color transforms, gradients, etc., to create a thresholded binary image"""

Here's an example of my output for this step. 

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform is a function called perspective_transform() defined in the section ### perspective transform ### .

I used the following numbers for src and detination points. 

    src = np.float32([[575,464],[710,464], [1093,714], [218,714]])
    img_size = (img.shape[1], img.shape[0])
    dst = np.float32([[300,0],[950,0], [950,img_size[1]], [300,img_size[1]]])
   
I tested my perspective transform by drawing the src and dst points onto a test image and its corresponding warped  to verify that the lines appear parallel in the warped image.

The sample image is present in """ Perspective Transform """ section. 

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I used the sliding window technique introduced in our class. It is present under def fit_binary_warped under ### Perspective Transformation ## 

[fit_output]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I used the sample implementation introduced in our class of the mathetical formula for calculation of radius of curvature under the section "Measuring Curvature".   

The radius of curvature is present in """ Curvature Calculation """ section under def curvature.
The position of the vehicle with respect to the center is present in """ Distance from the Center """" section under def distance_from_cente. Both these values are displayed in the final video. 


#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in # Finding the lines # section of the code in my code. Here is an example of my result on a test image. This is also present in the notebook under the same section.

![alt text][Output Images]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Please find the link to my video's output. Each video is subclip of 10 seconds.

[Project video output]

[Challenger video Output]  

[Harder Challenger video Output]

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Exercise present within the tutorial helped me to understand the concept though I admit that had to go through each section multiple times. I put more emphasis to sturcutre my program with more functions rather than writing it as a proecedural program. Selecting the threshold values gradients and  channels combination for color and sobel gradient was challenging and though it worked perfectly for project video, it didn't worked for challenging and higher challengers video. I will need to revisit my solution so that it works for challenging videos too.
