# stitching-image
Membahas proses image stitching tanpa MPI

### Buat Sebuah Folder Sebuah folder yang berisi potongan-potongan gambar, codingan python, serta output gambar.
* Di Windows

letak direktori :
> C:\image-stitching-opencv Tugas Besar(1)\image-stitching-opencv Tugas Besar

![image1](https://user-images.githubusercontent.com/150003742/284070391-ff648eb1-920a-4aff-a668-967cb73fcddf.png)

### Gambar yang digunakan
![image2](https://user-images.githubusercontent.com/150003742/284070032-d86434a4-c777-47a4-a2b9-b4885a9f7e5c.png)
![image3](https://user-images.githubusercontent.com/150003742/284070042-4a7700e5-19b0-47c0-81e7-b1ab6b2da9dc.png)
### Program python yang di gunakan
```
# USAGE
# python image_stitching_simple.py --images images/scottsdale --output output.png

# import the necessary packages
from imutils import paths
import numpy as np
import argparse
import imutils
import cv2

# construct the argument parser and parse the arguments
ap = argparse.ArgumentParser()
ap.add_argument("-i", "--images", type=str, required=True,
	help="path to input directory of images to stitch")
ap.add_argument("-o", "--output", type=str, required=True,
	help="path to the output image")
args = vars(ap.parse_args())

# grab the paths to the input images and initialize our images list
print("[INFO] loading images...")
imagePaths = sorted(list(paths.list_images(args["images"])))
images = []

# loop over the image paths, load each one, and add them to our
# images to stich list
for imagePath in imagePaths:
	image = cv2.imread(imagePath)
	images.append(image)


# initialize OpenCV's image stitcher object and then perform the image
# stitching
print("[INFO] stitching images...")

# Create a Stitcher with a default ORB (feature-based) detector
stitcher = cv2.Stitcher_create(cv2.Stitcher_SCANS)

# Detect keypoints and set camera parameters manually
status, stitched = stitcher.stitch(images)
if status != cv2.Stitcher_OK:
    print("[INFO] Camera parameters adjustment failed. Retrying with manual adjustment...")
    
    # Manually set camera parameters
    stitcher.setWarper(cv2.detail_WaveCorrectKind_HORIZ)
    stitcher.setWaveCorrection(True)
    stitcher.setFeaturesFinder(cv2.Stitcher_createFeaturesFinder())
    
    # Retry stitching
    status, stitched = stitcher.stitch(images)

# print additional information
print("[INFO] Stitching Status:", status)

# if the status is '0', then OpenCV successfully performed image
# stitching
if status == cv2.Stitcher_OK:
    # write the output stitched image to disk
    cv2.imwrite(args["output"], stitched)

    # display the output stitched image to our screen
    cv2.imshow("Stitched", stitched)
    cv2.waitKey(0)

# otherwise, the stitching failed
else:
    print("[INFO] image stitching failed ({})".format(status))

    # print additional information
    if status == cv2.Stitcher_ERR_NEED_MORE_IMGS:
        print("[INFO] Need more images for stitching.")
    elif status == cv2.Stitcher_ERR_HOMOGRAPHY_EST_FAIL:
        print("[INFO] Homography estimation failed.")
    elif status == cv2.Stitcher_ERR_CAMERA_PARAMS_ADJUST_FAIL:
        print("[INFO] Camera parameters adjustment failed.")
    elif status == cv2.Stitcher_ERR_MATCH_CONFIDENCE_FAIL:
        print("[INFO] Match confidence test failed.")
    elif status == cv2.Stitcher_ERR_CAMERA_PARAMS_VERIFY_FAIL:
        print("[INFO] Camera parameters verification failed.")

# ... (existing code)

``` 
### Lakukan penginstallan berikut : 
```pip install imutils```dan ```pip install opencv-python```dan ```pip install numpy```

### Buka direktori image-stitching-opencv Tugas Besar pada cmd
gunakan command 
```cd /path direktori/```

contoh : cd C:\image-stitching-opencv Tugas Besar\image-stitching-opencv Tugas Besar

### jalankan program 
gunakan command 
```python image_stitching_simple.py --images images/scottsdale --output output.png```
![images4](https://user-images.githubusercontent.com/150003742/284118670-5dd70f94-cb23-4592-aa9b-5efc86a9243c.jpg)
### Output
![images5](https://user-images.githubusercontent.com/150003742/284118709-6a7ab100-c869-416b-b325-1815b117b175.jpg)
