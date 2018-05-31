# **Finding Lane Lines on the Road**

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following 5 steps.
1. Convert the image to a grayscale
2. Apply Gaussian blur filter to remove noise in edge detection
3. Perform Canny edge detection
4. Apply "region of interest" (ROI) mask to the edge image to exclude the parts of image that are potentially not related to the lane finding.
5. Apply Hough transform to find line segments in the edge detected ROI masked image. Post process Hough transform output line segments to remove potential outliers and extrapolate a smooth lane markings.


In the above pipeline there were many parameter and design choices that are more or less clear from the code. One thing that particularly needs to be explained a bit more is the "draw_lines" function that fuses the result of the Hough transform to generate single line for each side of the estimated lane marking.

I did the following sub-pipeline to fuse the Hough transform generated line segments.
1. Separate the line segments into two groups based on the sign of the slope. For instance, if the slope of a line segment is negative then it belongs to the left lane and otherwise.
2. If a line segment has slope close to zero (less than 20 degrees), i.e., almost horizontal, compared to the direction of travel. Remove them as outliers as they are more likely to be not a part of lane markings.
3. Remove further outliers that have slope deviating more than 10% from the median slope of all the line segments.
4. Now we have refined line segments that we believe are truly part of the lane markings. We fuse them together by computing average gradient 'm' and average intercept 'b'. We use these mean values of slope and intercept to compute the co-ordinates of extrapolated lane markings. We use the end points as the ones that have fixed 'y' axis value. One end is controlled by the margin from top of image, e.g., ROI, and other end is limited by the y_max.   


### 2. Identify potential shortcomings with your current pipeline
There are multiple shortcomings as listed below
1. Canny edge detection: I used fixed thresholds, which clearly seem to struggle with the "challenger.mp4" frames (after 4 sec) that move from shade to bright sun-light. Auto-thresholding based on the image properties such as lighting effects would be helpful.

### 3. Suggest possible improvements to your pipeline

Following possible improvements can be done,
1. Adapt Canny edge detection thresholds
2. Fine tune Hough transform parameters
3. Currently using 10% deviation from median slope and less than 20 degree of angle as thresholds for detecting outlier line segments. These parameters can be further fine tuned, as well as, new methods of eliminating outlier line segments can be thought. 
