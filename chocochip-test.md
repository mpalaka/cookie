# Chocolate Chip Count

In this code, we used a pure Image Processing approach to find the number of chocolate chips in the given cookie image.

#### Environment : OpenCV in Python3

Chocolate chips are small, ususally approximately circular objects which can be seen on the surface of the cookie. Moreover, they are significantly darker than the plain cookie. 

Assuming we can isolate these small darker figures, our task can be simply seen as counting the number of these smaller figures. 

The first step is to isolate the chips by making the background and the cookie (non-chocolate chip parts) a much ligter color so that the chocolate chips can stand out even more.
This can be done by simply thresholding. We are assuming that the background color is light, so that thresholding can be done effectively.

We convert the image to greyscale, as we do not require any color to identify the chocolate chips. We then perform smoothing in the form of gaussian blur using a 3x3 kernel
to smooth out the edges of the chocolate chips, and to remove any texture in the background. We then perform thresholding of the entire image by converting all pixels that have
a value of more than 85 to 255, and all other pixels to 0 (0 is black; 255 is white). This way, the darker pixels which represent the chocolate chips become black objects on a 
completely white background.

Now we must simply count the number of black objects. For this subtask, we use [Blob Detection](https://www.learnopencv.com/blob-detection-using-opencv-python-c/). A blob can be 
described as a group of connected pixels. Using the OpenCV class SimpleBlobDetector, we can detect blobs of certain properties in our image. For our purposes, we define a blob
such that it need not be perfectly circular, may be concave(some dough might be covering the chocolate chip), and might be slightly elongated or oval shaped. 

Upon performing blob detection with fine tuned parameters on our image of only chocolate chips, a list of keypoints is returned. This list contains the centers of each of the blobs that are detected. Hence, we return the length of this list to get the number of chocolate chips.
