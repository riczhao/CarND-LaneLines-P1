#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I implemented two different piplines. The top parts are same. The bottom parts are different, by using HoughLines and HoughLinesP.

Comon part:
* convert image to grayscale
* gaussian blur, removing noise
* canny edge detection, tune parameters of the previous step and this step
* create a mask of regsion of interest, tune mask corners

HoughLines:
* get hough lines in polar coordinates, in order of intersection count desc.
* select top valid lines, ignoring lines that are with angle less than 10 degree. left/right lines are seperated by angle pi/2.
* draw left and right lines in mask region.

HoughLinesP:
* HoughLinesP only output small line segments, collect lines first.
* left/right lines can be seperate by middle x coordinate of mask. lines across left and right are invalid.
* get average of m and b for left and right line separately.
* draive left and right lines in mask region.

###2. Identify potential shortcomings with your current pipeline

It's still like a toy, with a lot of restriction and shortcomings.
* mask (region of interest) is fixed. It won't work if camera position moved or care is not going straightly.
  Solution: possiblly need to detect road ground area.
* engine cover may appear in the image
  Solution: calculate difference with the prev image can get covered region of the image
* curve lanes
  Solution: 1) fix it using HoughLinesP. The mask area can be divided into small bars top to bottom. Apply m/b calculation onto the small bars only. 2) use polynomial to mimic the curv.
* different resolution of image cause fixed mask failure
  Solution: quick fix is to use percentage to define mask


###3. Suggest possible improvements to your pipeline
Included in previous section
