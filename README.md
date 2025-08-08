# Camera Calibration using OpenCV

This repository contains code for performing camera calibration using images of a checkerboard pattern. Camera calibration is the process of estimating the intrinsic and extrinsic parameters of the camera to correct lens distortions and relate 3D object points to their 2D image projections.

The code calibrates a camera by detecting corners on checkerboard calibration images and computing camera parameters using those detected points.

***

## Overview

Camera calibration is essential in computer vision and robotics applications like 3D reconstruction, object measurement, and navigation. This process finds the camera's internal characteristics (intrinsic parameters like focal length and optical center) and its position relative to the scene (extrinsic parameters).

This project uses a set of checkerboard images and OpenCV functions to automatically detect checkerboard corners and calculate the camera calibration matrices.

For detailed theoretical background and explanations of camera calibration concepts, refer:  
[Camera Calibration - MATLAB Documentation](https://in.mathworks.com/help/vision/ug/camera-calibration.html)
[Camera Calibration - OpenCV Documentation](https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html)
***

## How the Code Works (main.py)

1. **Checkerboard Setup**  
   The checkerboard pattern size is defined as 6 rows and 9 columns of internal corners:
   ```python
   CHECKERBOARD = (6, 9)
   ```
   This tells the program how many inner corners to expect per calibration image.

2. **3D Object Points Initialization**  
   World coordinates for the checkerboard corners are generated assuming the board lies flat on the XY plane with Z=0:
   ```python
   objp = np.zeros((1, CHECKERBOARD[0] * CHECKERBOARD[1], 3), np.float32)
   objp[0,:,:2] = np.mgrid[0:CHECKERBOARD[0], 0:CHECKERBOARD[1]].T.reshape(-1, 2)
   ```
   These 3D points are fixed for all images because the physical checkerboard size doesn’t change.

3. **Image Loading and Corner Detection**  
   The script reads all `.jpg` images from the `./images` folder and converts each to grayscale. For each image, it tries to find checkerboard corners using:
   ```python
   cv2.findChessboardCorners()
   ```
   If corners are found, their locations are refined to subpixel accuracy with:
   ```python
   cv2.cornerSubPix()
   ```
   Detected corners are drawn on the image for visual confirmation.

4. **Storing Correspondences**  
   For each successful image, the known 3D points (`objpoints`) and detected 2D image corner points (`imgpoints`) are appended to their respective lists.

5. **Camera Calibration**  
   After processing all images, the intrinsic and extrinsic camera parameters are calculated using:
   ```python
   cv2.calibrateCamera(objpoints, imgpoints, gray.shape[::-1], None, None)
   ```
   This returns:
   - **Camera Matrix (mtx):** Intrinsic parameters describing focal length, skew, and principal point.
   - **Distortion Coefficients (dist):** Parameters modeling lens distortion.
   - **Rotation Vectors (rvecs):** Rotation of the checkerboard pattern relative to the camera.
   - **Translation Vectors (tvecs):** Position of the checkerboard relative to the camera.

6. **Output**  
   The script prints the calibration results to the console.

***

## Usage

1. Place your checkerboard images in the `./images` directory.
2. Run the script:
   ```bash
   python main.py
   ```
3. The script will show each checkerboard image with detected corners. Press any key to move to the next image.
4. After processing all images, camera calibration matrices and distortion coefficients are printed.

***

## Requirements

- Python 3.x
- OpenCV (`opencv-python`)
- NumPy
- glob (standard Python library)

Install dependencies with:
```bash
pip install opencv-python numpy
```


