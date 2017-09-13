# **Finding Lane Lines on the Road**

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)
[input_curve2]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/test_images/curve2.jpg "YellowLine"
[bad_gray_scale]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/writeup_images/curve2.jpg/gray_bad_yellow_contrast.jpg "Bad Gray"
[good_gray_scale]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/writeup_images/curve2.jpg/gray_good_yellow_contrast.jpg "Good Gray"
[blur]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/writeup_images/curve2.jpg/blur.jpg "Blurred"
[edges]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/writeup_images/curve2.jpg/edges.jpg "Edges"
[masked]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/writeup_images/curve2.jpg/masked.jpg "Masked"
[raw]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/writeup_images/curve2.jpg/raw.jpg "Raw"
[average]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/writeup_images/curve2.jpg/average.jpg "Average"
[fitted]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/writeup_images/curve2.jpg/interp.jpg "Fitted"
[final]: https://github.com/liamog/CarND-LaneLines-P1/blob/master/writeup_images/curve2.jpg/final.jpg "Final"

---

### Reflection

### Pipeline

My pipeline consisted of 5 steps.
###### 1 - Grayscale
Initially just used a simple conversion to grayscale. This worked well until I tried the final challenge.
This is an example of a yellow line on a light gray road surface.
![Yellow curved line easy to see in color][input_curve2]
![Yellow curved line hard to see in grayscale][bad_gray_scale]

- so first I extracted yellow from the image as a mask
- then changed the yellow to white this improved edge detection in grayscale significantly
- then convert to grayscale

![Yellow curved line easier to see in grayscale][good_gray_scale]

This did introduce more noise in the image though.
###### 2. - Blur
Remove noise in the image.
![Blurred][blur]


###### 3. - Edge detection
![Edges][edges]

###### 4. - Mask region of interest
In this case I just provided an easy mechanism to adjust this based on specifying
percentage range of the height of the image required and width percent ranges for the apex width.

So the image looks like this.
![Masked][masked]

###### 5. Extract hough lines

I modified the code significantly here. Instead of just extracting the lines
and immediately drawing them, I extracted them as lines then processed them
with various approaches so I could visually compare the results.

I provided 3 different implementations of draw_lines.

1) Raw - just draw the lines as detected by hough
![Raw][raw]

2) Averaged - separate by left and right by position in the image and slope of line. Then
   extrapolate based on an average of slope and y-intercept and draw from min_y to max_y
![Average][average]

3) Fitted line. Again separeate the lines by slope and position. Draw all points on lines
   into an array of points then use polyfit to do a linear regression on the points to find
   the fitted line.
![Fitted][fitted]

Final video is [here](https://github.com/liamog/CarND-LaneLines-P1/tree/master/test_videos_output)


### 2. Identify potential shortcomings with your current pipeline


1) Changing lanes the slopes and positions will change quite a bit and my current
solution will filter out and not detect the lines in that case.

2) Other lane markings on the road could skew both the average and fitting approaches.

3) Road edges in the region of interest, say with the dividing barrier, cause false positive
   line detection

4) Other cars closer in front may also play havoc with this approach.

### 3. Suggest possible improvements to your pipeline


1) Noticing that the lanes lines are similar from image to image, using some of the detected lines
from the previous frames could lead to a much more stable detection algorithm

2) Using other approaches to find the most dominant lines. I looked briefly at RANSAC as one
possible approach.

3) A more complex region of interest mask may help to reduce some false positives ,but could lose
   lane detection during lane changes.

4) Ideally the polyfit could be made to extract the curve rather than a linear approximation. I tried
using higher orders but it was very sensitive to outliers in the input lines.
