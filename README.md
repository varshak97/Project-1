**Title:**

BaggageAI Computer Vision Hiring Assignment

**Description:**

Two sets of images are given:- background and threat objects. Background images are the

background x-ray images of baggage that gets generated after passing through a X-ray machine at

airport. Threat images are the x-ray images of threats that are prohibited at airport while travelling.

• Task is to cut the threat objects, scale it down, rotate with 45 degree and paste it

into the background images using image processing techniques in python.

• Threat objects should be translucent, means it should not look like that it is cut pasted. It

should look like that the threat was already there in the background images. Translucent

means the threat objects should have shades of background where it is pasted.

• Threat should not go outside the boundary of the baggage. \*\*difficult\*\*

• If there is any background of threat objects, then it should not be cut pasted into the

background images, which means while cutting the threat objects, the boundary of a threat

object should be tight bound.

**Requirements:**

Python 3.6.9

**Libraries used** : OpenCV, Numpy, os, Pillow.

**Folder contains:**

Background image folder

Threat image folder

Sample outputs folder

Varsha&#39;s outputs folder

File - BaggageAI\_solution.ipynb

Functions in the file:

1. detect\_contours(image):

This function detects contours of the input image given to it as arguement.

Argument - image read from threat image path or background image path as required.

Explanation - retval, dst = cv2.threshold(src, thresh, maxval, type[, dst])

cv2.threshold is used to convert grayscale image into binary image with threshold here set to 233 because histograms of the threat images show pixel value of 232.5 as lightest intensity value after white (255). type is set as cv2.THRESH\_BINARY\_INV, since detection of object is more convenient when object pixels are white while background is black.

cv2.findContours is used to find contours in binary image, which is why thresholding was done. This method requires 3 arguments. First argument is binary image whose contours is to be found. Second argument &#39;mode&#39; is set to cv2.RETR\_EXTERNAL to retrieve only the extreme outer contours and third argument &#39;method&#39; is set to cv2.CHAIN\_APPROX\_SIMPLE.

2. get\_ROI\_coordinates(contours):

This function is used to get the rectangular co-ordinates of region of interest i.e. threat object from the threat image.

Argument - Contours of the image found from the detect contours function is given as argument for this function

Explanation - cv2.boundingRect function returns co-ordinates x,y,w,h where (x,y) are the top-left coordinates of the rectangle and (w,h) are its width and height.

3. rotate\_bound(image, angle):

This function rotates images such that the entire image is included and none of the image is cut off while rotating.

Credits: Adrian Rosebrock&#39;s blog post (link: https://www.pyimagesearch.com/2017/01/02/rotate-images-correctly-with-opencv-and-python/)

Argument - Image that needs to be rotated at a specific angle (in degrees)

cv2.getRotationMatrix2D (center, angle, scale) is used to find matrix with centre co-ordinates, negative angle for clockwise rotation. Sine and cosine values (rotation components) of this matrix is calculated for computing the new bounding dimensions of the image.

cv2.warpAffine(image, M, (nW, nH), borderValues) is used to perform rotation with borderValues set as (225,225,225) for white background throughout.

4. preprocess(background\_image, rotated\_ROI):

This function is used for preprocessing threat and background images such as scaling down threat image when background image is small and setting mid-points of background image to paste the threat image within the boundary of the baggage.

Argument - background image and rotated region of interest

cv2.resize is used to resize colour image by defining scaling value &#39;imgScale&#39; to find new width and height which will ultimately be the new size after resizing.

Other functions used:

To paste the threat object in the background image, bitwise operations and addition operation is performed by first computing masks (normal and inverted) and then cv2.bitwise\_and and cv2.add operation.

Pillow Image function blend is also used Image.blend(image1, image2, alpha). This function is used to blend image1 and image2 to get the translucent effect. Alpha is an interpolation factor value between 0 and 1.

cv2.imwrite function is used to write the new formed images into the destination location.

**Credits:**

1. OpenCV documentation (https://docs.opencv.org/master/)

2. Adrian Rosebrock&#39;s blog post on bounded rotation of images (https://www.pyimagesearch.com/2017/01/02/rotate-images-correctly-with-opencv-and-python/)
