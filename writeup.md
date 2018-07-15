# **Finding Lane Lines on the Road** 

---

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidYellowCurve2.jpg "solidYellowCurve2"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I apply a gaussian blur with a kernel size 5 and the a canny edge filter.
I chose the region of interest so that the top is a little bit below the horizon of the scene. The reason is because the horizon is the furthest sight of the camera.
After that hough lines is applied.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by separating the line based on the slope. If the lines are visualized
in a planar coordinate system, the lines with negative slope must lie on the quadrant I or III (top right and bottom left) whereas lines with positive slope lie on quadrant II and IV (top left and bottom right),
for this reason the lines with negative slope are classified as a left line whereas the lines with positive slope are classified as a right line. After right and left lane classification, a line fitting algorithm is used to take the average
line equation of the lines. At this project I used RANSAC algorithm to fit the line. One of the benefit of RANSAC is that it can filter outliers, means that in our case, all lines that are not belong to the lane line
(e.g. horizontal line) will be automatically removed. After getting the equation, the x coordinate of the 2 points are calculated by using image height - 1 and the top of our ROI as y values.

The following is an example of the final result: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the car drives in an extreme curved line. In this situation a straight line model is not applicable anymore.

Another shortcoming could be a line identification failure under extreme sunlight causes by lack of contrast.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use a curved line model instead of a straight line model for the fitting algorithm.

Another potential improvement could be to identify not only the contrast of the color but also the color itself. The reason is that lane has fix colors which are yellow or white.
