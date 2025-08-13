# Camera Calibration
Geometric camera calibration, also referred to as camera resectioning, estimates the parameters of a lens and image sensor of an image or video camera.
Camera calibration is the process of figuring out the exact settings and characteristics of a camera so it can accurately represent the 3D world in a 2D image. It involves calculating important camera parameters like:

- **Intrinsic parameters**: These describe the camera’s internal features, such as focal length (how zoomed in the camera is), the principal point (image center), and lens distortion (corrections for fisheye or bent images).
- **Extrinsic parameters**: These describe the camera's position and orientation relative to the world (where the camera is located and which way it’s pointing).

***

#### Need for Camera Calibration?

- **To correct distortions** caused by the camera lens so that straight lines in the real world appear straight in images.
- **To accurately measure sizes and distances** of objects seen by the camera.
- **To relate real-world 3D coordinates to 2D image points**, which is critical for applications like 3D reconstruction, navigation, robotics, and your Optical Landing System project.
- **To determine the camera's location and angle** in a scene, enabling precise interpretation of what the camera “sees.”
- Without calibration, images can be distorted and measurements wrong, which will hurt any computer vision tasks relying on the camera.

***
## Camera Calibration Parameters
Camera calibration parameters are the values that define how a camera captures a 3D scene and projects it onto a 2D image. These parameters can be split into two main types: intrinsic and extrinsic.
<p align="center">
<img width="657" height="154" alt="image" src="https://github.com/user-attachments/assets/d1318c5f-bfdc-4e9e-bbdc-f1922aa6d029" />
  
</p>
<p align="center">
<img width="584" height="321" alt="image" src="https://github.com/user-attachments/assets/198d5c91-63c1-4ba0-af85-571a3386ba73" />
</p>

***

### 1. Intrinsic Parameters

These describe the internal characteristics of the camera itself, essentially how the camera forms an image of the 3D world onto the 2D sensor.

- **Focal length (fx, fy):** Specifies how strongly the camera focuses light, affecting the zoom level. It's usually expressed in pixels.
- **Principal point (cx, cy):** The point in the image that corresponds to the optical center where the camera's axis intersects the image plane (usually near the image center).
- **Skew coefficient (γ):** Accounts for the skewness between the x and y pixel axes (ideally zero if axes are perpendicular).
- **Distortion coefficients:** Correct lens distortions like radial (barrel or pincushion) and tangential distortions caused by the camera lens imperfections.

**intrinsic matrix (K)**:

$$
K =
\begin{bmatrix}
f_x & \gamma & c_x \\
0 & f_y & c_y \\
0 & 0 & 1
\end{bmatrix}
$$

This matrix maps 3D camera coordinates onto 2D image pixel coordinates.

<img width="579" height="416" alt="image" src="https://github.com/user-attachments/assets/e37ede5a-c36b-4a6d-bfa1-08a08cd4ee8a" />

***

### 2. Extrinsic Parameters

These describe the camera's position and orientation in the world, relating the world coordinate system to the camera coordinate system.

- **Rotation matrix (R):** A 3×3 matrix that describes how the world is rotated to align with the camera.
- **Translation vector (t):** A 3×1 vector that describes the camera's position shift in the world coordinate system.

<p align="center">
<img width="326" height="121" alt="image" src="https://github.com/user-attachments/assets/c2f1980e-b346-4a42-82b2-eb7e9cd84ca7" />
</p>

Together, these form the **extrinsic matrix [R | t]**, a 3×4 matrix used to convert 3D points from world coordinates into the camera coordinate system.

***

## The Full Camera Projection

The overall camera projection matrix combines intrinsic and extrinsic parameters:

$$
P = K [R | t]
$$

<p align="center">
<img width="680" height="120" alt="image" src="https://github.com/user-attachments/assets/a5d5b021-48a4-4328-be66-400605e3d25a" />
</p>

The world points are transformed to camera coordinates using the extrinsic parameters. The camera coordinates are mapped into the image plane using the intrinsics parameters.


***
## Distortion in Camera Calibration
The camera matrix does not account for lens distortion because an ideal pinhole camera does not have a lens. To accurately represent a real camera, the camera model includes the radial and tangential lens distortion.
- Linear distortion refers to image transformations where straight lines in the real world remain straight in the image, and the transformation can be modeled using only scaling, rotation, translation, and shear.
- Non-linear distortion refers to image distortions that bend straight lines into curves in the image, usually as a result of lens imperfections.
  - The most common types are:
      ### Radial Distortion
      Radial distortion occurs when light rays bend more near the edges of a lens than they do at its optical center. The smaller the lens, the greater the distortion.
      <p align="center">
      <img width="648" height="125" alt="image" src="https://github.com/user-attachments/assets/6238b406-ce3f-4e34-98b5-4b9f56e74afd" />
      </p>

      ### Tangential Distortion
      Tangential distortion occurs when the lens and the image plane are not parallel. The tangential distortion coefficients model this type of distortion.
      <p align="center">
      <img width="536" height="269" alt="image" src="https://github.com/user-attachments/assets/127a4449-f8bd-4fa8-8034-68bbcabf70b6" />
      </p>

***
## References

1. MathWorks, *What Is Camera Calibration?* MATLAB & Simulink Documentation. [https://in.mathworks.com/help/vision/ug/camera-calibration.html](https://in.mathworks.com/help/vision/ug/camera-calibration.html)  
2. OpenCV, *Camera Calibration and 3D Reconstruction Tutorial (Python)*. [https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html](https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html)  
3. MathWorks, *Camera Calibration* Documentation. [https://in.mathworks.com/help/vision/camera-calibration.html](https://in.mathworks.com/help/vision/camera-calibration.html)  
4. Zhang, Z., *A Flexible New Technique for Camera Calibration*, IEEE Transactions on Pattern Analysis and Machine Intelligence, 2000.  
5. Hartley, R., & Zisserman, A., *Multiple View Geometry in Computer Vision*, Cambridge University Press, 2004.  
6. Shree K. Nayar, *Camera Calibration*, Columbia University. [YouTube](https://youtube.com/playlist?list=PL2zRqk16wsdoCCLpou-dGo7QQNks1Ppzo&si=oaDtYB6_8ZIb8CVd)
