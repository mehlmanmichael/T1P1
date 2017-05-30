[//]: # (Image References)

# **Finding Lane Lines on the Road: Writeup**

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of a number of steps. In order:
* Import image
* Convert to grayscale
* Gaussian blur
* Perform edge detection (Canny)
* Set up a trapezoidal mask based on image size parameters
* Use a Hough transform to find lines (lots of tweaking of parameters)
* Find single lines as described below.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by the following method:
* Fit all detected lines linearly (slope and intercept)
* Discard line segments that had "inappropriate" slopes (ie way too vertical or way too horizontal)
* Bin line segments according to slope in either left or right line array
* Average left and right line arrays individually (in slope and intercept)
* Draw left and right lane lines by calculating x values given y values (bottom of image and adjustable middle of image) as well as slope and intercept.

### 2. Identify potential shortcomings with your current pipeline


There are several potential shortcomings:
* Lanes curve sharply (linear fit no longer appropriate)
* Lane lines not clearly visible or obscured by debris (my pipeline does not quit gracefully... throws errors if lanes are not in appropriate location or slope)
* Cars or other features (such as painted markings in the lane indicating, eg, turns or highway assignments) within trapezoidal ROI may affect offset.  Since slope of line features must be within certain values, this is not a significant danger for affecting perceived line direction (off values thrown out), but if a marking has a line with appropriate slope but in the middle of the lane, it will skew the lane position (not angle) in my code.


### 3. Suggest possible improvements to your pipeline

Possible improvements include:
* Require that the left lane be on the left side (according to intercept) and the right lane be on the right side (according to intercept) rather than just requiring correct slope range
* Fit lane points with a higher degree polynomial (2nd order probably sufficient) to account for curving lanes
* Perform image detection on cars / debris / etc. so that it can be omitted from lane detection (ie if you see a car, ignore stuff in that ROI)
