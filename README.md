#**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report
---

## Reflection

### Description of the pipeline

Original Image:

[image1]: ./examples/1.png "Original"
![alt text][image1]

My pipeline consisted of 9 steps. 
Things that I noticed when I tried  to process the challenge video were that the video size was different and that part of the road was of a different tone. 
To fix this issues I resized the image and created a mask of the yellow line before converting to grayscale to preserve it since Canny had a hard time finding a meaningful contrast between the yellow line and the light grey road. 
Finally I added the edges detected by Canny and the yellow mask and continued processing. 
Another modification was the possibility of specifying if the pipeline should draw the left line, the right line or both. This depends on some modifications I made to the draw_lines function that I will cover after the description of the pipeline.

Step by Step:

###1.Resize the image to make sure that the challenge or any video uses the same dimensions.

###2.Mask the yellow line.

[image2]: ./examples/2.png "Yellow Line Mask"
![alt text][image2]

###3.Convert the images to grayscale.

[image3]: ./examples/3.png "Grayscale"
![alt text][image3]

###4.Blur the image.

[image4]: ./examples/4.png "Blur"
![alt text][image4]

###5. Apply Canny to the image.

[image5]: ./examples/5.png "Canny"
![alt text][image5]

###6. Consolidate Canny's result with Yellow mask image.

###7. Remove everything outside the area of interest.

[image6]: ./examples/6.png "Region of interest"
![alt text][image6]

###8. Draw the lines on a blank image

[image7]: ./examples/7.png "Lines on a blank image"
![alt text][image7]

###9. Finally I draw the lines on the image

[image8]: ./examples/8.png "Final Result"
![alt text][image8]


### Description of the draw_lines function
The objectives I based the modifications of this function were: 
1 - Detect if it is the right or left line
2 - Average the positions of the lines to draw a single line from top to bottom

To detect left or right I used:
left: x1 < img.shape[1]/2 and x2 < img.shape[1]/2 and (-0.6)*(x2-x1) > (y2-y1)
right: x1 > img.shape[1]/2 and x2 > img.shape[1]/2 and (0.6)*(x2-x1) < (y2-y1)

To average the position of the lines (independently for left or right) I created a global variable and I used the extend function.
Immediately I noticed that averaging the lines for the whole video was creating a fix line that didn't always matched the lines. To fix this I created a buffer of line aggregations and reseted every 20 frames so it adjust as the lines move.
To make it more obvious I draw the left line in red and the right one on blue.

Finally I added a parameter to draw the left, right or both lines. 

### How to draw the left line or the right line
Based on the modifications I made to draw_lines I was able to create some new functions to draw the left line, the right line or both.

```
def process_image(image):       
    result =  pipeline(image,"both")
    return result

def process_image_left(image):       
    result =  pipeline(image,"left")
    return result

def process_image_right(image):       
    result =  pipeline(image,"right")
    return result
```

### Potential improvements to my pipeline

A possible improvement would be to create a mask for both yellow and white lines to make sure we don't lose tracking when the contrast of the image is to low. 

Also, make both lines of the same height. 
