# **Finding Lane Lines on the Road** 

---
## Shuo Feng

**Finding Lane Lines on the Road**

In this project, the goal is to identify lane lines on the road, first in images, and later in a video streams.

[//]: # (Image References)
[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/edges/solidYellowCurve.jpg "Edges"
[image3]: ./test_images_output/rawline/solidYellowCurve.jpg "Raw Lines"
[image4]: ./test_images_output/solidYellowCurve.jpg "Calcualted Lines"
[image5]: ./test_images_output/All_solidYellowCurve.jpg "Final"
[image6]: ./test_images_output/All_solidWhiteCurve.jpg "Final"
[image7]: ./test_images_output/All_challenge1.jpg "Final"

---

### Reflection

### 1. Pipeline

**The pipeline of identifying lanes include the following steps:**

1) Convert color image/video frame into gray scale image for further edge detection. 

* The Blue channel of the color image is set to zero first as the lanes are in the color of yellow or white, and this will keep the lane information while removing other noise to certain extent.

2) Edge Detection

* The canny function from opencv is used to detect the strong edges in the grayscale image.

![alt text][image2]

3) Define Region of Interest

* The lane is assumed to show up in the middle bottom section of each image and a trapezoid shaped region is predefined as the region of interest (ROI). This ROI is used as a mask to mask the detected edges from step 2) in order to filter out most of the edges that do not are the edges of lanes.

4) Find Lines from Edges

* Hough transform is used to find lines that are long enough and gaps small enough etc.

5) Extract Lane Information from Lines

* Ideally, the lines are only from one of the two major lanes in the ROI. However, there're many cases that the lines are from other things other than the lane, such as the strong tree shades within ROI, dirts from the road, and other cars. 

* A) All lines are checked and only those with a slope larger than 0.5 (30 degree from horizontal) are kept. This is based on the assumption that the car is in lane and the lane lines are in the vertical directions. So any horizontal lines should not be considered.

* B) The left over lines are splitted into two groups based on the sign of the slope, with the intuition that one of the two lanes will have  positive slope, and the other will have negative slope. The slope k and the intercept b are kepted for each line. In the image below, the positive group is shown in green and negative in blue.

![alt text][image3]

* C) The average slope and intercept is calculated for each of the two groups and used to draw a line. The drawing is done by pre-defining the vertical coordinate of the two end points of the line. For example, after pre-defining y1 and y2, the x1 and x2 can be calculated with the k and b from step B.

![alt text][image4]


**The final results with both the raw grouped lines will be like these:**

![alt text][image5]

![alt text][image6]

![alt text][image7]

<video width=<!-- "960" height="540" controls>
        <source src="./test_videos_output/solidWhiteRight.mp4" type="video/mp4" markdown="1" >
</video>

<video width=<!-- "960" height="540" controls>
        <source src="./test_videos_output/solidYellowLeft.mp4" type="video/mp4" markdown="1" >
</video>

<video width=<!-- "960" height="540" controls>
        <source src="./test_videos_output/challenge.mp4" type="video/mp4" markdown="1" >
</video>

### 2. Potential shortcomings with your current pipeline
A) Noise handling. The edges within the ROI in vertical direction are propergated and affects the final computed line.
B) Lane edges are assumed to be straight lines which does not work well for curve cases.

### 3. Suggest possible improvements to your pipeline
A) 
B) Instead of using averages, use lenght-weighted averages in small region. 

