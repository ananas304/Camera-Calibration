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
      Radial distortion occurs when light rays bend more near the edges of a lens than they do at its optical center. The smaller the lens, the greater the distortion.  Modeled with coefficients k1, k2, k3.

      <p align="center">
      <img width="648" height="125" alt="image" src="https://github.com/user-attachments/assets/6238b406-ce3f-4e34-98b5-4b9f56e74afd" />
      </p>

      ### Tangential Distortion
      Tangential distortion occurs when the lens and the image plane are not parallel. The tangential distortion coefficients model this type of distortion. Modeled with coefficients p1, p2.
      <p align="center">
      <img width="536" height="269" alt="image" src="https://github.com/user-attachments/assets/127a4449-f8bd-4fa8-8034-68bbcabf70b6" />
      </p>

### Distortion Coefficient

**Distortion coefficients** are numerical parameters that model and quantify how a real camera lens deforms or “distorts” an image compared to the ideal pinhole camera model.

 <p align="center">
<img width="707" height="307" alt="image" src="https://github.com/user-attachments/assets/20545213-5c80-485f-8eb9-ffc530c987de" />
 </p>
 
During calibration, the solver finds the values of k1, k2, k3, p1, p2 that best fit the observed distortion in sample images.

***
## Stereo Camera Calibration
A stereo camera is a camera system that uses two or more camera lenses (and image sensors) placed a fixed distance apart to capture images of the same scene from slightly different viewpoints—mimicking human binocular vision. Stereo camera calibration is the process of determining both the individual (intrinsic) parameters for each camera in a stereo setup and the precise position and orientation (extrinsic parameters) of the cameras relative to each other.

The process typically follows these steps:

1. **Individual Camera Calibration**  
   Each camera in the stereo pair is calibrated independently to estimate its intrinsic parameters, including focal length, principal point, and lens distortion coefficients. This step ensures that each camera’s internal characteristics are well understood.
   
    Fig: Detection of chessboard corners with color-coded correspondences in left and right stereo calibration images.
     <img width="950" height="359" alt="image" src="https://github.com/user-attachments/assets/b7b3ca61-895e-45ea-8ac3-05ae2daf1d15" />

    Fig: intrinsic matrix consisting of focal lengths and principle points of left and right cameras.
    <p align="center">
      <img width="347" height="155" alt="image" src="https://github.com/user-attachments/assets/11be7451-ff22-4143-8c79-630574693a19" />
    </p>


2. **Stereo Pair Calibration (Extrinsic Parameters)**  
   Once individual calibrations are done, the next step finds the rotation and translation (extrinsic parameters) that describe the relative position and orientation between the two cameras. This defines how the cameras are arranged spatially with respect to each other.
   
   fig: Rotation and Translation matrices between the cameras.
   <p align="center">
    <img width="406" height="161" alt="image" src="https://github.com/user-attachments/assets/219adb72-91b4-4eed-94ce-920c83140cb7" />
  </p>


3. **Image Capture of Calibration Pattern**  
   Multiple synchronized images of a known calibration pattern (such as a checkerboard) are taken simultaneously by both cameras from different angles and positions. The known pattern points provide correspondences between the two cameras.

4. **Feature Extraction and Matching**  
   The calibration software detects feature points (like checkerboard corners) in both images and establishes correspondences between them.

5. **Estimation of Stereo Parameters**  
   Using the matched points and known calibration pattern geometry, the calibration algorithm estimates the stereo parameters that best align the cameras’ views. This includes refining intrinsics if necessary.

6. **Rectification and Validation**  
   The calibration results are used to rectify (align) the stereo images so that corresponding points lie on the same image rows, simplifying depth computation. The calibration quality is validated by measuring reprojection errors to ensure accuracy.

   <img width="1182" height="438" alt="image" src="https://github.com/user-attachments/assets/132edbe0-54b4-4066-a8e4-100f313f6e33" />

7. **Application of Calibration**  
   The final calibrated stereo model enables accurate triangulation of points, allowing 3D reconstruction and depth estimation for downstream applications like robotic vision or optical landing systems.

Check out [Camera-Calibration/stereo camera calibration](https://github.com/ananas304/Camera-Calibration/tree/main/stereo%20camera%20calibration)

***

## Pixel to Millimeter conversion
Converting pixels to millimeters (or vice versa) is a common task in camera calibration and measurement applications. The math behind it depends on knowing the **physical size of the image sensor** and the **resolution of the image**.

### **1. Pixel Size Calculation**

Suppose you know:
- The **physical width or height of your camera's sensor** (e.g., 6.4mm wide).
- The **image resolution** (e.g., 1920 pixels wide).

<img width="500" height="200" alt="image" src="https://github.com/user-attachments/assets/258e1645-f257-4bc1-9f76-87122fb254de" />


### **2. Direct Pixel-to-Millimeter Conversion**
<img width="500" height="160" alt="image" src="https://github.com/user-attachments/assets/dc3e92b7-d865-4610-8d7a-f6ec25e68632" />

### **3. Using Calibration Patterns (Practical Camera Calibration)**

If you use a **calibration object** (like a chessboard) with known square size (say, 20mm between corners), and you measure the distance between those corners in pixels in your image, you get a real-world conversion factor:
<img width="400" height="70" alt="image" src="https://github.com/user-attachments/assets/b8d28d21-c74f-48d2-ba82-ea2b18e3b035" />


***

## References

1. MathWorks, *What Is Camera Calibration?* MATLAB & Simulink Documentation. [https://in.mathworks.com/help/vision/ug/camera-calibration.html](https://in.mathworks.com/help/vision/ug/camera-calibration.html)  
2. OpenCV, *Camera Calibration and 3D Reconstruction Tutorial (Python)*. [https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html](https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html)  
3. MathWorks, *Camera Calibration* Documentation. [https://in.mathworks.com/help/vision/camera-calibration.html](https://in.mathworks.com/help/vision/camera-calibration.html)  
4. Zhang, Z., *A Flexible New Technique for Camera Calibration*, IEEE Transactions on Pattern Analysis and Machine Intelligence, 2000.  
5. Shree K. Nayar, *Camera Calibration*, Columbia University. [YouTube](https://youtube.com/playlist?list=PL2zRqk16wsdoCCLpou-dGo7QQNks1Ppzo&si=oaDtYB6_8ZIb8CVd)
6. Stereo Calibrate two cameras GitHub repo [TemugeB / python_stereo_camera_calibrate](https://github.com/TemugeB/python_stereo_camera_calibrate?tab=readme-ov-file)
