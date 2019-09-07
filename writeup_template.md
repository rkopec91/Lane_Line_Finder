# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)


[image1]: ./test_output/original.jpg "Original"
[image2]: ./test_output/gray.jpg "Gray"
[image3]: ./test_output/gaussian.jpg "Gaussian"
[image4]: ./test_output/canny.jpg "Canny"
[image5]: ./test_output/region.jpg "Region"
[image6]: ./test_output/hough.jpg "Hough"
[image7]: ./test_output/final.jpg "Final"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.   First, I converted then image to grayscale and then used a gaussian blur to remove any imperfections that may show up in the canny image.  I used a small kernal of 3 so it doesn't blur the image too much.  After that I used the provided function, canny, to get a canny image with the lower threshold of 50 and higher threshold of 150.  Once the canny image was found, I computed the vertices for the region of interest.  From the image shape, I found the height and width of the actual image and computed the region of interest from there.  The region I schose was (130,height),(450, int(height*0.6)), (500, int(height*0.6)), (900,height).  This was then an input, along with the canny image, to the region of interest provided function.  Once that was found, the hough lines were computed using the provided function, hough lines.  I chose the following inputs to this function as rho=2, theta=np.py/180, threshold=60, min line len=150, and the max line gap=150. Finally, I return the output of the weighted_img function.

This gave a pretty good answer but wasn't great considering the lines weren't always long enough.  At some points in the videos, the right line was too short.  I needed to change the draw_lines function. For each line found, I would find the slope and intercept using (y2-y1)/(x2-x1) and the y=mx+b function.  Then, if the slope found was less then 0.0, it belonged to the left lane, and all others belonged to the right lane.  These intercepts and slopes were separated into a left and right line group.  The minimum y (the y that was closest to the horizon between the previous found minimum y, y1, and y2) was also kept for later calculations.  For each of the lines (left and right) I found the mean slope and mean intercept.  Then using the y=mx+b function, I found the minimum and maximum x values.  From this I was able to draw lines utilizing the cv2.line function.  The inputs to this were the (xmin,ymin) (xmax,height).  This allowed for the lines to be extended towards the horizon.  The thickness also had to be changed for each of the lines.  It started out with a thickness of 2 but I increased it to 12 so it would show a little better. 

### Original:
![alt text][image1]

### Gray:
![alt text][image2]

### Gaussian:
![alt text][image3]

### Canny:
![alt text][image4]

### Region:
![alt text][image5]

### Hough:
![alt text][image6]

### Final:
![alt text][image7]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be when there are sharp turns.  This works on highway driving but what happens if we were driving on urban streets?  Lanes aren't as straight.  There could be winding roads like lombard street in San Francisco.  This will cause a big issue.  Also, weather could be another factor.  If there is snow on the ground, there is no way you can detect theses lines. Also, what about streets that don't have lines on them?  In my neighborhood, there are no lines that separate you from oncoming traffic.


### 3. Suggest possible improvements to your pipeline

There are a few things I would like to look into.  Maybe using deep learning to detect lines on the street?  It could also help with detecting lanes when there are no lines, or lines that are covered in snow.  It could also help us navigate in urban areas, not just highways.  I do know there are also other hough transforms that could be applied to this.  Maybe the circle hough transform could help detect curves in the lines.  Also, doing a transformation on the image could help me get a good top down view on the lanes.
