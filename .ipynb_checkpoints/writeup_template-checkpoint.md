# **Finding Lane Lines on the Road** 

## Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline I implemented consisted of 8 steps:
1. Thresholding in RGB space to find lane lines
2. Turning into grayscale
3. Make the image binary
4. Apply a Gaussian filter
5. Apply the Canny Edge Detector
6. Select a region of interest
7. Get Hough lines
8. Merge the Hough lines with the original image

To select the parameters for each step, I tested out different values using `ipywidgets`, which provides convenient sliders to modify the values of these parameters.

At first I thought using another color space would be necessary to identify the lane lines properly. However, I found a combination of parameters in RGB color space that gets close enough for both cases.
- Red: 224
- Green: 136
- Blue: 0

Keeping a 1:3 ratio (low:high) for the Canny Edge Detector thresholds, I decided on 50:150 since it provided good results on most test images.

To make the pipeline more robust, the region of interest had to be defined relative to the size of the image. These parameters are defined percentually and they form a polygon that encloses the lane.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function to follow this logic:
1. Separate lines into corresponding lanes by slope.
2. Calculate all slopes, intercepts and lengths for the lines in each of the groups.
3. Calculate a weighted average of the slopes and intercepts using the lengths as the weights.
4. Print the new lines on the image

The image below illustrates the steps I previously mentioned:

<img src="images/image.png" width="480" alt="Steps" />


### 2. Identify potential shortcomings with your current pipeline

There are many potential shortcomings in this pipeline:
- The lanes are currently approximated to a first degree polynomial. This makes fitting the drawn line to a curve harder.
- Thresholding in RGB space may work in these test cases, but it isn't assured to work in all possible cases. 


### 3. Suggest possible improvements to your pipeline

Many improvements could be made on this pipeline. 
- Lane lines could be approximated to a higher degree polynomial to accomodate curves in the lane better. 
- Another color space could be used for better thresholding. 