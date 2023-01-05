---
layout: post
title: OpenCV quick intro
date: 2023-01-05 
description: openCV
tags: python consult
---

# Getting started with OpenCV
Install the latest versions of OpenCV and imutils (used to simplify code) depending on your OS and IDE.

These codes are based on the fantastic beginner tutorials provided by [pyimagesearch](pyimagesearch.com).

We will get straight into coding, explaining in each line its function.


```python
# load_display_save_image.py
import imutils
import cv2
import argparse

# parsing command line
parser = argparse.ArgumentParser(description="Load image, show shape, display it, save it as .png.",
    epilog="The end.")
parser.add_argument("-i","--image", required=True, help="path of the image") 
args = vars(parser.parse_args())

image = cv2.imread(args["image"])
(h, w, d) = image.shape # represented as numpy array
# in any array 3D you have the rows (height), columns (width) and depth(number of channels, if color=3)
# that is how images are represented 
print(f"height = {h} px, width = {w} px, depth = {d} channels")

cv2.imshow("Loaded image", image)
# show image in new window
cv2.waitKey(0)
# waiting for clicking the image and push any key to continue execution (0)
# cv2.destroyAllWindows(); cv2.waitKey(1) # use it to show image and when clicked destroy it

cv2.imwrite("createdimage.png", image)
```

### Array slicing

In Computer Vision (CV) the images are represented as arrays, with the number of rows the height and the number of columns the width, 400x600=240000 px. Each pixel of an image has a value corresponding to its brightness in a grayscale bar, going from 0 to 255 (considering a 8bits resolution). In color images, we have 3 channels if we use the RGB space, which for historical reasons is used as BGR in openCV. It is represented as a 3-tuple with 256^3=16777216 combinations of colors. Also as an array the coordinate system origin is in the left upper corner (0,0). This is the reference we will use for indexing the array.

The idea is to obtain a Region of Interest (ROI) for further processing.


```python
# slicing_image_bgr.py
import imutils
import cv2
import argparse

# parsing command line
parser = argparse.ArgumentParser(description="Load image, get BGR, change color, display it.",
    epilog="The end.")
parser.add_argument("-i","--image", required=True, help="path of the image") 
args = vars(parser.parse_args())

image = cv2.imread(args["image"])
cv2.imshow("Original", image)
# get color channels in one pixel
(B,G,R) = image[40,200] # row 40, column 200
print(f"R = {R}, G ={ G}, B = {B}")
image[50,50] = (0,0,255) # setting red color

# select a ROI 
roi = image[0:100, 0:100]
# slicing the rows and the columns (ystart:yend, xstart:xend)
cv2.imshow("ROI", roi)
image[0:100, 0:100] = (0,255,0) # green
cv2.imshow("Changed image", image)

cv2.waitKey(0)
```

### Drawing

It's important to point out features in our images, select ROIs that contain something that the algorithm detected. Any drawing operation is performed in-place (the original image is altered), therefore it is important to remember to create a copy and operate on this image `copied=image.copy()`.


```python
import numpy as np
import cv2
%matplotlib inline

# canvas with 200x200 px with 3 channels
canvas = np.zeros((300, 300, 3), dtype = "uint8")
# black, important declare 8 bit array

# line from left to right
green = (0, 255, 0)
cv2.line(canvas, (0, 0), (200, 200), green, 3)
# starting to ending, then color, then thickness in px
cv2.imshow("Canvas line", canvas)

# draw a square (empty) giving top left corner and bottom right corner, 2 px thickness
cv2.rectangle(canvas, (10, 10), (60, 60), green, 2)
cv2.imshow("Canvas square", canvas)

# blue filled rectangle
blue = (255, 0, 0)
cv2.rectangle(canvas, (70, 70), (100, 100), blue, -1)
cv2.imshow("Canvas filled square", canvas)
cv2.waitKey(0)
cv2.destroyAllWindows()
# seems in jupyter we need this line

# circle at the center with increasing radius
canvas = np.zeros((200, 200, 3), dtype = "uint8")
(centerX, centerY) = (canvas.shape[1] // 2, canvas.shape[0] // 2)
# integer division for placing the center
white = (255, 255, 255)
for r in range(0, 175, 25):
	cv2.circle(canvas, (centerX, centerY), r, white)
cv2.imshow("Canvas circles", canvas)
cv2.waitKey(0)
cv2.destroyAllWindows()

# draw 5 random circles
for i in range(0, 5):
	# randomly generate
	radius = np.random.randint(5, high = 100) # radius from 5 to 100
	color = np.random.randint(0, high = 256, size = (3,)).tolist()
	pt = np.random.randint(0, high = 150, size = (2,)) # center random

	# draw our random circle
	cv2.circle(canvas, tuple(pt), radius, color, -1)

# Show our masterpiece
cv2.imshow("Canvas random", canvas)
cv2.waitKey(0)
cv2.destroyAllWindows()

canvas = np.zeros((200, 200, 3), dtype = "uint8")
cv2.putText(canvas, "Hey!", (100, 100), 
	cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 255, 100), 2)
# starting position,font used (generally sans-serif), scale, color and letter thickness
cv2.imshow("Text in canvas", canvas)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### Image transformation
Translation, rotation and resizing. Formally, we will need to calculate several parameters like the point of rotation, create the translation matrix, apply an affine operator and so on. The library `imutils` save us tedious work. Here we see how to associated arguments with the flags `nargs` and also consider the type of variable introduced for the function.


```python
# transformation_image.py
import imutils
import cv2
import argparse

# parsing command line
parser = argparse.ArgumentParser(description="Translate, rotate or resize image")
parser.add_argument("-i","--image", required=True, help="path of the image") 
parser.add_argument("-T","--Translate", nargs='+', help="translate image, 0: trans x, 1: trans y", type=float) 
parser.add_argument("-R","--Rotate", nargs='+', help="rotate image, 0: angle", type=float) 
parser.add_argument("-rn","--RotateNoCrop", nargs='*', help="rotate no cropping image, 0: angle", type=float) 
parser.add_argument("-r","--resize", nargs='*', help="resize image keeping the aspect ratio, use width", type=int) 
args = vars(parser.parse_args())

image = cv2.imread(args["image"])
if args['Translate']:
    trax = args['Translate'][0] # x dimension, lateral
    tray = args['Translate'][1] # y dimension, vertical
    translated = imutils.translate(image,trax,tray)
    cv2.imshow("Translated image", translated)
    cv2.waitKey(0)

if args['Rotate']:
    rot = args['Rotate'][0] # clockwise is negative
    rotated = imutils.rotate(image, rot) 
    cv2.imshow("Rotated image", rotated)
    cv2.waitKey(0)

if args['RotateNoCrop']:
    # the image is cropped due to opencv not caring about what we actually did to the image
    # this function preserves the whole size
    rot = args['RotateNoCrop'][0] # clockwise is negative
    rotated = imutils.rotate_bound(image, rot)
    cv2.imshow("Rotated image", rotated)
    cv2.waitKey(0)

if args['resize']:
    # to maintain aspect ratio is neccesary to compute the proportion and resize accordingly
    # with imutils a wrapper function does the job
    ww = args['resize'][0]
    resized = imutils.resize(image, width=ww) # provide width or height
    cv2.imshow(" Resize image", resized)
    cv2.waitKey(0)
```

### Smoothing
Remove the high frequency content of images (like sharp edges) to avoid confounding information that the algorithms might take as useful. Behind the scenes a kernel is applied to the image.

A noisy image is full of specks (a sort of outlier), pixels that have different intensity values than their surroinding pixels. Spatial filtering is a neighborhood operation where is pixel value is changed based on neighboring values. A filter is a matrix that acts like a mask. When placed over a region of an image it creates a window around a pixel, and depending on the operation differing results are obtained. For instance, linear filter performs weighted average, multiplying the value of the filter and the image and summing them. These are sometimes called sliding window operations. You can increase the light sensitivity of a digital camera sensor to improve the brightness of a picture taken in low light. Many modern digital cameras (including mobile phone cameras) automatically increase the ISO (camera setting) in dim light. However, this increase in sensitivity amplifies noise picked up by the sensor, leaving the image grainy. This noise can interfere with text identification by polluting regions in the binarized image.


```python
# blurring_gauss.py
import cv2
import argparse

# parsing command line
parser = argparse.ArgumentParser(description="Blurred image through gaussian filter")
parser.add_argument("-i","--image", required=True, help="path of the image") 
args = vars(parser.parse_args())

image = cv2.imread(args["image"])
blurred = cv2.GaussianBlur(image, (11, 11), 0)
# gaussian with 11x11 kernel with 0 standard deviation in both x-y
cv2.imshow("Blurred", blurred)
cv2.waitKey(0)
```

### Little project for image processing

We are going to develop a little script for counting the shapes in an image. For that we need to follow several steps.

+ Gray image. When loaded into memory, a grayscale image occupies a third of the space required for an RGB image, requiring less computational power to process and can reduce computation time. Also, since grayscale images are conceptually simpler than RGB images, the development of an image processing algorithm can be more straightforward. On the other hand, if we wanted to classify objects by color, the color RGB would be essential. *Luminance* is a photometric measure of the luminous intensity per unit area of light travelling in a given direction. It describes the amount of light that passes through, is emitted from, or is reflected from a particular area. *Brightness* is the term for the subjective impression of the objective luminance measurement standard. *Contrast* is the difference in luminance or color that makes an object distinguishable. It is determined by the difference in the color and brightness of the object and other objects within the same field of view. The maximum contrast of an image is the contrast ratio or dynamic range.

+ Separating an image into distinct parts is called segmenting (differentiation) an image. This performed by edge detection, using texture differences, shapes and sizes, by color (green screens behind), etc. The idea is that you want a region of interest ROI, and remove/ignore the rest. This is where a mask comes in; a binary mask is a logical array that indicates the ROI, with 1s the pixels you want to keep and 0s the one to ignore/remove. You can create a binary black and white image from a grayscale image by thresholding its intensity values. Values below the cutoff are assigned the value 0, while those above are assigned the value 1.


```python
import argparse
import imutils
import cv2

ap = argparse.ArgumentParser()
ap.add_argument("-i", "--image", required=True,
	help="specify path to input image, relative or global")
args = vars(ap.parse_args())

image = cv2.imread(args["image"]) # args["image"] = args.image
(h, w, d) = image.shape
cv2.imshow('Original', image)

### Converting to grayscale for edge detection and thresholding
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
cv2.imshow('Gray', gray)
cv2.waitKey(0)

### Edge detecion with Canny algorithm
edged = cv2.Canny(gray, 30, 150)
# min threshold, max threshold, and sobel kernel size (default 3)
# sobel filter: discrete differentiation operator, computing an 
# approximation of the gradient of the image intensity function, enhancing high freq variations
cv2.imshow("Edged", edged)
cv2.waitKey(0)

### Thresholding
# used to remove dark or light areas and highlight some regions
# binary inverse threshold: in the image all pixel values less than 225
# to 255 (white; foreground) and all pixel values >= 225 to 255
# (black; background), thereby segmenting the image, critical step for finding contours
thresh = cv2.threshold(gray, 225, 255, cv2.THRESH_BINARY_INV)[1]
cv2.imshow("Thresh", thresh)
cv2.waitKey(0)

### Detecting contours
# using the thresholded image find the outlining
cnts = cv2.findContours(thresh.copy(), cv2.RETR_EXTERNAL,
	cv2.CHAIN_APPROX_SIMPLE)
# simply finding white px in a copy of thresh
cnts = imutils.grab_contours(cnts)
# compatibility wrapper for previous versions
output = image.copy()
# make copy when drawing over images
for c in cnts:
	# draw each contour on the output image with a 3px thick purple
	# outline, then display the output contours one at a time
	cv2.drawContours(output, [c], -1, (240, 0, 159), 3)
#     cv2.imshow("Contours", output)
# 	cv2.waitKey(0)
    
text = f"{len(cnts)} objects found!"
cv2.putText(output, text, (10, 25),  cv2.FONT_HERSHEY_SIMPLEX, 0.7,
	(240, 0, 159), 2)
cv2.imshow("Contours", output)
cv2.waitKey(0)

### Erosions and dilations
# To reduce noise in thresholded binary images
# we apply erosions to reduce the size of foreground objects (eroding pixels)
# useful for removing some blobs or undesired objects
mask = thresh.copy()
mask = cv2.erode(mask, None, iterations=5)
# 5 iterations for reducing size
cv2.imshow("Eroded", mask)
# cv2.waitKey(0)

# Similarly dilations enlarge the ground and can combine foreground objects (nearby contours) when interested
mask = thresh.copy()
mask = cv2.dilate(mask, None, iterations=5)
cv2.imshow("Dilated", mask)
cv2.waitKey(0)

### Masking
# Mask out or hide regions we are not interested in, maybe to enchance some feature or the image.
mask = thresh.copy()
output = cv2.bitwise_and(image, image, mask=mask)
cv2.imshow("Output", output)
cv2.waitKey(0)
```
