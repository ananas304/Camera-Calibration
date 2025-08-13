# Camera Calibration
Geometric camera calibration, also referred to as camera resectioning, estimates the parameters of a lens and image sensor of an image or video camera.
Camera calibration is the process of figuring out the exact settings and characteristics of a camera so it can accurately represent the 3D world in a 2D image. It involves calculating important camera parameters like:

- **Intrinsic parameters**: These describe the camera’s internal features, such as focal length (how zoomed in the camera is), the principal point (image center), and lens distortion (corrections for fisheye or bent images).
- **Extrinsic parameters**: These describe the camera's position and orientation relative to the world (where the camera is located and which way it’s pointing).

***

#### Why Do You Need Camera Calibration?

- **To correct distortions** caused by the camera lens so that straight lines in the real world appear straight in images.
- **To accurately measure sizes and distances** of objects seen by the camera.
- **To relate real-world 3D coordinates to 2D image points**, which is critical for applications like 3D reconstruction, navigation, robotics, and your Optical Landing System project.
- **To determine the camera's location and angle** in a scene, enabling precise interpretation of what the camera “sees.”
- Without calibration, images can be distorted and measurements wrong, which will hurt any computer vision tasks relying on the camera.

***
## Camera Calibration Parameters
Camera calibration parameters are the values that define how a camera captures a 3D scene and projects it onto a 2D image. These parameters can be split into two main types: intrinsic and extrinsic.
<img width="657" height="154" alt="image" src="https://github.com/user-attachments/assets/d1318c5f-bfdc-4e9e-bbdc-f1922aa6d029" />

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

Together, these form the **extrinsic matrix [R | t]**, a 3×4 matrix used to convert 3D points from world coordinates into the camera coordinate system.

***

## The Full Camera Projection

The overall camera projection matrix combines intrinsic and extrinsic parameters:

$$
P = K [R | t]
$$

<img width="680" height="120" alt="image" src="https://github.com/user-attachments/assets/a5d5b021-48a4-4328-be66-400605e3d25a" />

The world points are transformed to camera coordinates using the extrinsic parameters. The camera coordinates are mapped into the image plane using the intrinsics parameters.


***


