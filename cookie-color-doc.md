# Cookie Color

In this code, we used a pure Image Processing approach to find the top 3 hex codes of the given cookie image.

#### Environment : OpenCV in Python3

The approach we took for this problem was to to search and keep track of the colour codes that could be seen in the cookie, and then output those colors which occurred with the highest frequency.

The first step was to remove the background from the image by making it completely black(0,0,0) so that we would focus soley on the the color of the cookie. 
For this, we used the [GrabCut algorithm](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_grabcut/py_grabcut.html) which uses a ROI(region of interest) or bounding box 
around the cookie, which differenciates between the background and the foreground, then uses this information to predict which pixels are in the background and which are in the 
foreground within the ROI as well. 

The GrabCut algorithm creates a mask which can then be put over the original image, which gives us an image of only the foreground(the cookie) with the background blacked out.
Instead of putting the mask over the original image of the cookie, we put it over a smoothed image. The cookie itself has very small variations in the color, so simple blurring
 with a kernel size of 9x9 was used to average out the the colors on the cookie and make it less sharp. We do not need to worry about the color of the chocolate chips 
 interfering as their area is much smaller than the cookie itself.
 
 A list containing the hex codes of all the pixels in the foreground(cookie) was made, and then the count of each hex code was calculated. A list containing the top 3 hex codes 
 is then returned. 
