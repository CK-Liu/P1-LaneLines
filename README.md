# **Finding Lane Lines on the Road**

## Writeup

### In this project we detected lane lines both in images and videos using Python and OpenCV. For a self-driving car, being able to detect the road is essential for the car to drive autonomously.

### Environment: Jupyter Notebook, Anaconda3, OpenCV, Numpy, moviepy

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/output_solidYellowCurve2.jpg
[image2]: ./examples/FailureFrame-challenge.jpg
[image3]: ./examples/FailureFrame2-challenge.JPG

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

**Step 1:** First, I copy the input image into a new image and perform Gaussian blur to the input RGB image

**Step 2:** Covert RGB domain into HLS domain and threshold on S channel to generate hls_binary image. 'Yellow' color is easier to be detected in HLS S channel

**Step 3:** Separate Red channel from input image and save it as a grayscale image. It is easier to detect 'white' color in 'red channel'

**Step 4:** Gaussian blur grayscale image

**Step 5:** Canny edge detection and generate canny_binary image

**Step 6:** Combine hls_binary image and canny_binary image together

**Step 7:** Custom select a region of interest and mask away the less likely region which may have lane lines

**Step 8:** Hough line detection in the region of interest

**Step 9:** draw_lines()

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by calculating the slopes and center points of detected Hough lines. If the slope is positive, we append center point and slope to the right line list. If the slope is negative, we append center point and slope to the left line list. By using the line equation 'y=kx+b', we can calculate the cross points with the middle and bottom of an input image. Then we draw a line to connect these two points together.

Here is a testing result image of finding lane lines:

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One shortcoming is when processing 'challenge.mp4'. The overall performance is good. But there are few frames in the video are not doing well. Here is an example:
![alt text][image2]

Another shortcoming is when the lane lines start to curve, our method failed to predict the curvature of the lane lines. Here is an example when lane lines have a curve:
![alt text][image3]


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to save the previous measured slope and calculate the difference between current slope and previous slope. If the difference is beyond a threshold value, we still use the previous calculation to draw lines.

Another potential improvement could be to find a method to measure curvature. Instead of fitting a line 'y=kx+b', we can fit a parabola 'y=ax^2 + bx + c'. But it will be a little bit difficult and I need more time.
