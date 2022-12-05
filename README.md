# Image-Mosaics

## Description

In this assignment, I've implemented an image stitcher that uses image warping
and homographies to automatically create an image mosaic. I will focus on the case where
we have two input images that should form the mosaic, where we warp one image into the plane
of the second image and display the combined views. original input.

## Steps

### Stitching Two Images

* I’ve loaded the images provided in the assignment in BGR and in grey mode. The grey images are used when getting the correspondences to calculate the homography matrix. The BGR images are used in the final step when stitching the images to retain the original images' colours. Please note that I’ve scaled the images to half of its original size to visualise them side by side so that the result is clear (I can just remove the resizing by commenting the resize instructions and everything will run as it is).

![image](https://user-images.githubusercontent.com/61145262/205770245-61b66505-9a66-4d72-b1b9-b7b1de57412d.png)

* I used the SIFT descriptor to get the best two correspondences for each which corresponds betIen the two images. I applied the ratio check of these two correspondences and if the ratio is below a certain threshold then I admit this point as a matching point. Please note that this threshold is tuned so that the returned points are exactly 50 points. And finally I visualise these correspondences to get the following:

![image](https://user-images.githubusercontent.com/61145262/205770075-c1f7834a-e59a-4e93-9435-7fad9b1a87e6.png)

* I calculated the homography matrix using the direct linear transform (DLT) from the matched points returned from the SIFT descriptor. The calculate_homography() function takes a list of matched points and returns the homography matrix using SVD. Finally I scaled the matrix so that the last element is 1 (the matrix would be up to scale). And finally I checked whether the homography matrix is correct or not by comparing it with the homography returned from the opencv2 library and you can see that they are very close to each other:

![image](https://user-images.githubusercontent.com/61145262/205770364-648894bb-1ce0-42bb-9d5f-478678cacd50.png)

* Finally, I warp the two images together using forward warping (rounding was used) to get the new boundaries (I limit the image height so it doesn’t exceed the original image height) of the source image when the homography matrix is applied to the original boundaries. Then I fill the area bounded by the new boundaries using inverse warping (bilinear interpolation was used). Here is the warped image:

![image](https://user-images.githubusercontent.com/61145262/205770455-6f675104-2888-4b0e-9334-03010189cd96.png)

* Finally I stitch the two images together to get the final output as following:

![image](https://user-images.githubusercontent.com/61145262/205770488-879efe34-ea97-4adb-9ec0-75164d93063f.png)

* Here is another example:

![image](https://user-images.githubusercontent.com/61145262/205770529-6ce53359-d8ab-4e74-abd8-f5a232ba235a.png)
![image](https://user-images.githubusercontent.com/61145262/205770750-386e09b3-9be8-4bd3-ade6-c0fe1ff3a30a.png)
![image](https://user-images.githubusercontent.com/61145262/205770599-fc7056d2-712d-4203-bda6-1a1b1e3ff70a.png)
![image](https://user-images.githubusercontent.com/61145262/205770622-d1e56061-713f-4e78-a029-65e486b3a6bd.png)

### Stitching Three Images

* The same steps are held in the previous part except for some small details:

* The images are loaded in grey only since there are no colored photos provided.

![image](https://user-images.githubusercontent.com/61145262/205770855-2e1a4991-cd25-49fc-9a3e-b11bc066e277.png)

* I have stitched the first and the third images together as a first step since they can be aligned horizontally.
Here are the correspondences between the two images:

![image](https://user-images.githubusercontent.com/61145262/205770925-8ef585c9-4d7d-4643-a4c7-71088c7b4e8d.png)

* I calculate the homography matrix and I warp the image and I get the following:

![image](https://user-images.githubusercontent.com/61145262/205770959-0a40eb1a-2ccc-494f-8fc6-9cce3982dd09.png)

* I stitch the two images together to get the following:

![image](https://user-images.githubusercontent.com/61145262/205771024-9398b164-f90e-4f1d-ab9d-1ead90bdeed2.png)

* Then I wrap the second image with the previous stitched image. First I get the following correspondences:

![image](https://user-images.githubusercontent.com/61145262/205771056-a83aabdb-8e59-419d-9fa0-0ecdae593350.png)

* I calculate the homography matrix and I wrap the two images (I won’t limit the height here so I can see the whole image unlike the previous cases) and I get the following warped image:

![image](https://user-images.githubusercontent.com/61145262/205771095-23983584-6558-4f0c-b387-d93485efd53e.png)

* Finally I stitch this one was the previously stitched images so I get the following final output:

![image](https://user-images.githubusercontent.com/61145262/205771119-b4605175-a7e4-4c3f-8335-0e3bd3a2c636.png)
