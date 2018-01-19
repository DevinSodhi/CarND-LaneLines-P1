# **Finding Lane Lines on the Road** 

## Writeup


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline had a few steps in it.

First I had to define some hard coded values to determine the vertices I was going to work in. This isn't the ideal way to do this, but its the way i know for now. 

After defining a general area, I used the following steps:

1. grayscale the input image.
2. blue the image using a guassianfilter and a window of 5
3. use the canny algorithm with a lower threshold of 100, and an upper of 200.
4. use the vertices on the canny image to figure out my area of interest. This wasn't so clearcut for the challenge image
5. define the hough parameters rho, theta, threshold, min_line_length and ma_line_gap
6. return the weighted image.
    inside this function, is drawlines, which we needed to modify as well.
    The slope portion was easy enough, but i realized with some research that i could improve on getting all the slope/averages data by actually doing that part on the fly. I already had two sets of hough line segments (positive and negative slope), and all i had to do was to pick the correct region, and get a line fit on all the segments. If the segments are good, then we can extract a good "lane line".





### 2. Identify potential shortcomings with your current pipeline

1. the second test, around the 13 second mark shivers a little. I m not sure what the correct way to dispell this would be. The hough liines are correct (tested with hough_tester function), but the drawn line keels out a bit. One thing that improved this was a min_line_length to 60 pixels. Still, I was looking around online and someone suggested filtering out any large differences between consecutive frames for slopes. Another idea online was to give up on very horizontal line segments that come out of the hough transform, and with a little experimentation that seemed to work, still its not perfect.

2. my polygon is more or less hard coded. this polygon is the vertices range that I care about. I wish there was a way to make this dynamic and dependent on the scene. I have an idea or two, but haven't had the chance to test anything

3. my dealing with the first frames of my movies is essentially a hard coded hack in response to failures to build in jupyter. i need to figure out a better way to deal with it.



### 3. Suggest possible improvements to your pipeline

The challenge basically doesn't work.

There are a few things that need to be taken care of to make the algorithm more robust:
1. isolating a polygon that ignores most of the road between the lane lines. (the hood glare, the grass in the background, all contribute to the poor results). I checked thae hough lines specifically using a tester function, and they are all over the place.
2. being able to tell when there is a shadow on the scene, maybe some sort of normalization? but that might break the whole algorithm. (shadows because of the stark difference in the brightness of the pixels , especiall when shadows are sharp lines)

### 4. Resources:

 I originally had a simpler pipeline for the averages of my lane lines. I was going through all the iterations of the possible value. I was also not accounting for small errors in my second video because of horizontal hough lines


Then I saw suggestions for improvements at two different locations:

1. https://peteris.rocks/blog/extrapolate-lines-with-numpy-polyfit/
    I moved away from polyfit to fitline functions later, but this article was crucial is making me actually think about using more cv2 functions rather than hard coding things.
2. A further improvement on this method was by Ottonello over at: http://ottonello.gitlab.io/selfdriving/nanodegree/lane/lines/2016/12/15/lane-lines-submitted.html who took this idea a step further with polyfit and moving averages. This was a tough one to work with because once I saw his ideas, it was hard to unsee them. I'll have to use less clues in the future.


