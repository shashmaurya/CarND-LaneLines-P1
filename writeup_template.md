# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

#[image1]: ./examples/grayscale.jpg "Grayscale"
[image1]: ./test_images_output/solidWhiteCurve_hough.png "Line Segments - Hough"
[image1]: ./test_images_output/solidWhiteCurve_finalLines.png "Final Lane Markings"
---

### Reflection

### 1. Pipeline Description


Starting with the provided functions, I used the following steps to reach my end goal

* Converted the image to grayscale using the function 'grayscale'
* Applied blur to grayscale image by calling the 'gaussian_blur' function
* Using the output from last step, applied canny transform to it using function 'canny'
* Defined the vertices for region of interest and called the function 'region_of_interest' to apply it
* By applying hough transform, generated lines from the region of interest; called function 'get_hough_lines'
    * The function itself calls the draw_lines function that uses hough lines to arrive at the two final lines
    * More details on the draw_lines function below
* Called the weighted_img function to combine the image with the lines
* All the above steps are performed in a function 'get_final_image' which calls the other function in order
    * Most of the tuning parameters are defined inside the get_final_image function
    * The function returns the final image as output
    
The result after applying the above with original draw_lines function is below

![alt text][image1]

Description of updated draw_lines function
* The updated function still uses the same arguments, with higher default thickness modified
* Iterates through the hough lines and classifies left and right lane lines based on line slope
* Only lines longer than a defined threshold are used in the next steps
* Stores the indices of start and end-points of each line segment, based on the left or right classification
* Using the x and y indices of the line segments part of the left lane marking, fits a line
* Repeats the above process for line segments of right lane marking
* Both final lines -left and right - are plotted on the image

Final result with updated draw_lines function: 

![alt text][image2]


### 2. Potential shortcomings with your current pipeline


There are certain problems associated with the approach defined
The correctness depends greatly on the region of interest selected
If the region in not accurate, there will be hough lines generated for object other than lane markings
These lines can lead the algorithm astray and draw incorrect markings

Since the classification is entirely based on the slope, there is possiblity of the fitted line
picking up some strange lines outside of the lane markings. This can be solved by using a method to 
remove outliers.



### 3. Possible Improvements To Existing Pipeline

For the problem mentioned above, there can be multiple ways to mitigate

**Filter the lines**
Ignore very short lines
Compare average slope of all line segments to the final line. If there the difference is beyond
a threshold, adjust the filter on the dynamically for line length

**Temporal averaging**
For a video, use lines from last few images (can start with lst 3-5 frames) and use their average to 
arrive at a line that is more stable

