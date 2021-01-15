# Image-Restoration

Project by Damian Wojtowicz<br>

### Description
The goal of this project is to restore damaged images. There are two sample images that need to be restored;<br>
Faded.jpg and Damaged.jpg, and the algorithm should work for any images of the same type.<br>
<br>
<img src="https://i.imgur.com/8kJtFGr.png" width="100%" height="100%"><br>
<br>
I have developed two algorithms for restoring each image type because I was simply unable to find enough similary between the two images. Both are very different in my personal opinion. Below I outline the differences:<br>
<br>
**Image 1 (Faded Image):**<br>
1\. Has straight defined edges<br>
2\. The method of restoration involves reducing the intensity by a fixed value<br>
<br>
**Image 2 (Yellow Damage Image):**<br>
1\. Has no well defined edges.<br>
2\. The damage is not equal across the damaged region of interest.<br>
   Some parts of the image are more damaged than others.<br>
   The first image deals with brightness/intensity. The second image<br>
   involves restoring the ratio of RGB and denoising.<br>
<br>
The process of segmenting the damage and restoring the two images is different and it is treated as such. <br>
This program has the capability to distinguish between the two different types of images automatically and so all the <br>
user has to do is provide the image.<br>
<br>
After the program is finished the image is displayed on screen and also written to a file (restoredImage.jpg)<br>
Matplot lib is used to display the "progress" images of the algorithm which can be viewed at the end.<br>

### Method For Restoring Image 1 (Faded Image)
1\. A grayscale image of the original is created. This will be useful in the algorithm in many ways.<br>
2\. Canny edge detection [2] is used to get the contours of the image<br>
3\. Contours are redrawn ticker with the drawContours() method so that it is easier to extract horizontal and vertical lines<br>
4\. Horizontal and Vertical lines are extracted using morphological transformations<br>
5\. Intersection points of the horizontal and vertical lines are obtained using the bitwise_and() method.<br>
   These intersection points define where the faded region is.<br>
6\. These intersection points are filtered to obtain two points that can be used to form a rectangle that defines the faded region.<br>
   These two points are used for cropping the image<br>
7\. The faded region is cropped<br>
8\. The minimum intensity value of the faded region and the original image is obtained. The difference between these two values<br>
   is the "shift" value. This value will be subtracted from the faded region to restore it.<br>
9\. The cropped restored region is put back on top of the original<br>
10\. The border between the previously faded region and the undamaged region that remained even after the restoration is<br>
    hidden using the inpaint() method from the opencv library.<br>
11\. The restored image is displayed to the user on screen.<br>
12\. The restored image is written to a file.

### Method For Restoring Image 2 (Damaged Image)
1\. The image is converted to the RGB colour space<br>
2\. The mean values for red, green, and blue in the image are obtained<br>
3\. The highest and lowest of the three values are picked out<br>
4\. Another image is created, it is the original image converted to the HSV colour space<br>
5\. The damaged pixels have their saturation reduced to 0, this is done by iterating over the pixels and <br>
    if the ratio of the lowest mean colour to the highest mean colour is past a certain value then we know there <br>
    is colour damage and that pixel's saturation is turned into 0.<br>
6\. The image with reduced saturation no longer has the dark patches that the original grayscale image had.<br>
    The reduced saturation image is then passed to a denoise function from opencv where the damage is denoised/blended<br>
7\. The restored image is displayed to the user on screen.<br>
8\. The restored image is written to a file.


### Alternative methods researched
I have researched the use of Hough Line Transform in order to extend out the lines in an attempt to better identify<br>
the border between the faded region and the undamaged region to extract the faded region. This has not turned out well <br>
because too many intersections between the lines were created in some instances and that affected the algorithm in a negative way.<br>
<br>
I have also researched changing the contract and brightness of the entire image in an attempt to better define the damage.<br>
After several rounds of experimentation I have found that altering the image before Canny Edge Detection [2] had the opposite effect<br>
or no positive effect at least. Increasing the brightness has blended the faded region<br>
to the white undamage pixels and increasing the contrast can blend the faded region to the black undamaged black pixels.<br>
As a result the countours became less defined and less accurate.<br>
<br>
I have also experimented with kernels that blur, sharpen, define vertical edges and horizontal edges. None of these have yeileded<br>
the desired results and only further decreased the accuracy of the algorithm.<br>
<br>
I have experimented with colour spaces such as RGB, HSV, LAB, XYZ and YUV. As well as their combinations. I have found that different colour spaces<br>
do not help better identify the damage in the Faded.jpg image. However, converting to RGB has been helpful in identifying the damage in Damaged.jpg image.<br>
<br>
I have experimented with histogram equalization of colored images in an attempt to restore Damaged.jpg.<br>
https://towardsdatascience.com/histogram-equalization-a-simple-way-to-improve-the-contrast-of-your-image-bcd66596d815
This has not helped as much as I had hoped because the damage is stronger in some regions than it is in others.<br>
<br>
I have also researched thresholding in an attempt to isolate the damage in the Faded.jpg image and this has not helped. This is mainly due to<br>
the fact that a simple threshold will not identify the damage. Adaptive thresholding has not been much more successful over my current method.

### Testing
The code has been tested with 4 other images (Faded2.jpg, Faded3.jpg, Faded4.jpg, Faded5.jpg) that are of similar nature to Faded.jpg<br>
All three images have been restored successfully.<br>

The code has also been tested on 2 other images (Damaged2.jpg, Damaged3.jpg) that are of similar nature to Damaged.jpg<br>
It is actually the same image where the colour of the damage is a different hue and the algorithm was performed performed well and the<br>
program was still able to distingish between the Damaged and Faded images.<br>
I have not been able to find similar images to Damaged.jpg where there has been colour damage so I simply recoloured Damaged.jpg.<br>

### Conclusion
The restoration for the faded images works very well in terms of both performance and speed.<br>
The restoration of the yellow damaged image, or any colour for that matter has also been successful in terms of both performance and speed.<br>
There is a slight "glow" as a result of the denoising in the Damaged.jpg image, however it can still be considered<br>
a very successful restoration.<br>

### References
1\. OpenCV: Image Inpainting [Internet]. Opencv.org.<br>&nbsp;&nbsp;&nbsp; 
    2021 [cited 2021 Jan 15].<br>&nbsp;&nbsp;&nbsp; 
    Available from: https://docs.opencv.org/3.4/df/d3d/tutorial_py_inpainting.html<br>&nbsp;&nbsp;&nbsp; 
    <br>
2\. Canny Edge Detection — OpenCV-Python Tutorials 1 documentation [Internet]. Readthedocs.io. 2013 [cited 2020 Nov 22].<br>&nbsp;&nbsp;&nbsp; 
    Available from: https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_canny/py_canny.html<br>
    <br>
3\. OpenCV: Morphological Transformations [Internet]. Opencv.org. 2020 [cited 2020 Nov 22].<br>&nbsp;&nbsp;&nbsp; 
    Available from: https://docs.opencv.org/master/d9/d61/tutorial_py_morphological_ops.html<br>
    <br>
