# Damaged Cookie Identification

In this code, we used a pure Image Processing approach to find any damages in the given cookie. Damages include : prominent breaks/cracks in the cookie, damage on the edges, and cookies
that are not complete(half a cookie, only a chunk of cookie etc.)

#### Environment : OpenCV in Python3

For this problem, we used 2 image processing techniques in conjunction:

1. Manually checking for cracks/breaks in the middle of the cookie
2. Using a blob detector of specific fine-tuned parameters to detect the proper shape of the cookie

Both methods require the same preprocessing of the image. First, the foreground of the image(just the cookie) is extracted using 
[GrabCut algorithm](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_grabcut/py_grabcut.html).

### Method 1 : Manual Checking

In this method, we use only the mask created by the GrabCut algorithm to detect any major cracks in the cookie. The mask is a 2d array of the same size as the picture
containing 0's and 1's, where 0 denotes that the pixel is the background, and 1 denotes that the pixel is part of the foreground. By tuning the parameters and the number of iterations
that the GrabCut function uses, we can get quite good results and the algorithm is able to detect cracks or breaks in the cookie and mark them as a background pixel.

We first perform smoothing to make sure the edges of the cookie are slightly less textured. Then, by looping through each row, we are able to find the pixels from where the 
cookie begins and ends in that row. We then iterate through the pixels where we know the cookie exists, (these are denoted by 1) and we search for a pixel with the value 0 
within that pixel range. If we find a pixel value 0, we know that it is a background pixel and is an indicator of a break in the cookie. 

This is a decent approach to find cracks in the middle of the cookie, but it is not good at finding cracked edges or incomplete cookies.

### Method 2 : Blob Detection

This method is better in finding edge damages. Here, we do not take only the mask created by the GrabCut algorithm, but the mask put onto the original image, so that we get a 
blacked out background, and the cookie. The image is then preprocessed further. We convert the image to greyscale, as we are not concerned about the color of the cookie, only the
shape. We then perform a Gaussian blur to smooth out the edges and remove texture on the cookie. The image is then subjected to thresholding to make the cookie white. This is 
easy, none of the colors on the cookie are pure black (0 value) like the background. So we are able to capture the values of the full cookie (including the chocolate chips) by putting
a sufficiently low threshold value(but not 0) and converting all those pixels to white.

Now we have the shape of the cookie in white against a black background. We then invert the image (make the blacks into white and the whites into black) as blob detection works 
best on white background. We also resize the image to make it smaller (150x150) as this also helps the blob detector. 

Finally, and the most important part is setting the parameters of the blob detector. Our ideal cookie is round, but we can accept some slight irregularity in the edges, as no cookie
has a perfectly round shape. We also allow an elongated shape, as the resizing may have made the cookie into more of an oval shape. We also set the maximum and minimum 
size parameters of the cookie. We can detect overly irregular, broken edges or cookies with chunks of it missing by setting the convexity parameter. Having the blob be more convex
means that the shape of the cookies is rounded more outwards. In contrast, a concave shape would have an inward curve, towards the center of the cookie, which is a clear sign of
edge deformity. It is these kinds of overly concave cookies which should be rejected and marked as damaged. 

After these parameters are set and fine-tuned, we can run the blob detector to check if the shape is an acceptable cookie or not. The blob detector will detect the shape only if all the
parameters are met, i.e, only if the cookie is in an undamaged state. Otherwise, it will not detect anything at all. The keypoints list contains the centers of all the blobs that
are detected in the image. If the length of the keypoints list is 1, then the cookie is undamaged. If the length of the list is 0, then the cookie is damaged.


Putting the code all together, we find results of the first method, returning true if the cookie is damaged, and false if it is not. We also perform the second method and return
similar values : true if damaged, false otherwise. To put the two results together, we say that the cookie is completely undamaged only if both tests return false. 
Hence, to get our final result, we perform the *or* operation and return the final result. 




