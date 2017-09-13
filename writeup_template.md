# **Finding Lane Lines on the Road**

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[input_curve2]: ./test_images/curve2.jpg "YellowLine"
---

### Reflection

### Pipeline

My pipeline consisted of 5 steps.
### 1 - Grayscale
Initially just used a simple conversion to grayscale. This worked well until I tried the final challenge.
This is an example of a yellow line on a light gray road surface.
![Yellow curved line hard to see in grayscale][input_curve2]

  - so first I extracted yellow from the image as a mask
  - then changed the yellow to white this improved edge detection in grayscale significantly
  - then convert to grayscale

### 2 - Blur
Remove noise in the image.

    blur = gaussian_blur(gray, gaussian_blur_kernel_size)
    debug_imgs.append(("blur", blur))

###3 - Edge detection

###4 Mask region of interest

###5 Extract hough lines
I modified the code significantly here. Instead of just extracting the lines
and immediately drawing them, I extracted them as lines then processed them
with various approaches so I could visually compare the results.

I provide 3 versions of draw_lines.

1) Raw - just draw the lines as detected by hough
2) Averaged - separate by left and right by position in the image and slope of line. Then
   extrapolate based on an average of slope and y-intercept and draw from min_y to max_y

3) Fitted line. Again separeate the lines by slope and position. Draw all points on lines
   into an array of points then use polyfit to do a linear regression on the points to find
   the fitted line.


If you'd like to include images to show how the pipeline works, here is how to include an image:



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ...

1 Changing lanes the slopes and positions will change quite a bit and my current
solution will filter out and not detect the lines in that case.




Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...
Noticing that the lanes lines are similar from image to image, using some of the detected lines
from the previous frames could lead to a much more stable detection algorithm

Another potential improvement could be to ...
