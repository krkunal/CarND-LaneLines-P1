# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[Image_1]: ./test_images_output/solidWhiteCurve.jpg "Test Image 1"
[Image_2]: ./test_images_output/solidYellowCurve.jpg "Test Image 2"
[Image_3]: ./test_images_output/solidYellowLeft.jpg "Test Image 3"
[Image_4]: ./test_images_output/solidWhiteRight.jpg "Test Image 4"
[Image_5]: ./test_images_output/solidYellowCurve2.jpg "Test Image 5"
[Image_6]: ./test_images_output/whiteCarLaneSwitch.jpg "Test Image 6"

---

## **Reflection**

## 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 9 steps. I've first masked the image based on lane colors; viz. yellow and white. This step helped identify the desired pixels to a great extent. Then I defined the region of interest rectangle consisting of the bottom 1/3rd of the image and masked the rest of the area above the horizon. Then I converted the image to Gray scale and then applied Gaussian blur with kernel size 5 to achieve smooth edges. Then the Canny transform was applied and the output of it was fed to the Hough transform to get the Hough lines. Almost all of the hyperparamter tuning was confined to the Gaussian blurring, Canny, and Hough stages. Then I applied median blurring with a kernel size of 11 to average out the marked pixels which in some cases were showing multiple lines for each of the lane line in the region of interest.    

In order to draw a single line on the left and right lanes, I defined a function extrapolate_hough which takes the median blurred Hough image as input and identifies the contours in a triangular region on interest with traingle's base at the bottom of the image and its apex at the center of the image. Once the contours are identified, lines were drawn within the region of interest to have continuous lane lines. Then the rectangular region of interest was again used to mask out any lines extending beyond the bottom 1/3rd of the image. Then this image with the continuous lane lines was overlaid on the original image. 

Following are some test images whem passed through this pipeline: 

![Test Image 1] [Image_1]

![Test Image 2][Image_2]

![Test Image 3][Image_3]

![Test Image 4][Image_4]

![Test Image 5][Image_5]

![Test Image 6][Image_6]

## 2. Identify potential shortcomings with your current pipeline


Some of the shortcomings are - 

1.) The Color masking is dependent on the assumption that the lane colors will be white & yellow. It might not work on faded lane markers. Also, it might lead to some issues when a white or yellow vehicle is ahead of the vehicle in the same lane. But, this step has helped me in getting a descent performance on the challenge video. Without this step, the model was performing poorly on the challenge video. 

2.) There is some issue in the lane line extrapolation that I did using the OpenCV FindContours & Fitline methods. Sometimes the extrapolated lane lines oscillate wildly this is because of the varying slope for various contour lines in the running video frame. Some implementation that can ignore small changes in the slope might help controlling the oscillations of the extrapolated lines.

3.) Besides, this solution might not work as well in case of images in adverse weather conditions; e.g. raining, snowing etc. Also, this solution has not been validated upon night time images, which might be difficult to process due to poor lighting conditions.

4.) The pipeline has also not be optimized from the perspective of response time i.e. the time taken to process the images.


## 3. Suggest possible improvements to your pipeline

1.) One improvement might be to help control the oscillations of the expolated lane lines. Some implementation that can ignore small changes in the slope might help controlling the oscillations of the extrapolated lines.

2.) Another improvement can be to remove the color mask and find some hyperparameter setting for Gaussian blurring, Canny, and Hough transform that can help getting better and stable performance in various scenarios.

3.) The solution needs to be improved further to handle different scenarios like rainy/snowy weather, night time images etc.

4.) The solution can be optimized to improve the overall response time. 