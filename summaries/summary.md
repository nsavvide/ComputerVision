# Computer vision 2023-2024

## Basics

### What is computer Vision?

Computer vision tries to understand RGB images, videos, point clouds, depth images etc.

It tries to find an answer on these questions:
- What is it? Can be done with Object detection/recognition
- Where is it? Can be done with 3D scene reconstruction
- Where does it go? Can be done with motion analysis

### Motion analysis

Motion analysis is trying to decide where the object is going.

#### Foreground/Background segmentation:

Here you **separate** the foreground from the background. A new image frame is compared to the background model based on previous frames. All pixels that do not correspond to the background model are labeled as foreground. You can do this using **background subtraction**, deep learning-based methods or a mixture of Gaussian models can be employed for this purpose. 
![alt text](assets/image.png)

**Challenging problems:**
- Image noise
- Illumination changes
- moving foreground (leaves), shadows, rain, snow, fog, headlights etc

#### Optical Flow Analysis: 

<div style="float:right;margin-left:20px;">
  <img src="assets/image-2.png" alt="Object Tracking Image" width="300"/>
</div>

This refers to the **pattern of apparent motion of objects**, surfaces, and edges in visual scene caused by the relative motion between an observer and the scene. Optical flow analysis **estimates the motion of objects by tracking the displacement of pixels** between consecutive frames in a video sequence. Flow vectors are then computed. This information is useful for understanding the dynamics of a scene, detecting moving objects, or estimating the camera's motion.

**Challenging problems:**
- Image noise
- Illumination changes
- ...

#### Odometry for Ego-Localization and Tracking:

Odometry is the use of data from **motion sensors** to estimate the position, orientation and distance of a moving vehicle (or robot) over time. In the context of ego-localization, odometry helps **determine the position and movement of the observer** (camera, robot, etc.) relative to its surroundings. This information is crucial for tasks like **navigation, mapping, and autonomous vehicle control.**
![alt text](assets/image-3.png)

#### Object Tracking based on FG/BG Segmentation:

<div style="float:right;margin-left:20px;">
  <img src="assets/image-1.png" alt="Object Tracking Image" width="400"/>
</div>

Once foreground objects are detected through segmentation, **object tracking algorithms** can be applied to follow these objects across frames. These algorithms often involve associating detected objects in the current frame with previously tracked objects, using techniques like **Kalman filters**, **particle filters**, or **deep learning-based trackers**. By continuously updating the position and characteristics of tracked objects, this process enables tasks such as **surveillance, activity recognition, and behavior analysis**.

### 3D modeling and scene reconstruction

#### Stereo Reconstruction

<div style="float:right;margin-left:20px;">
  <img src="assets/image-4.png" alt="Object Tracking Image" width="300"/>
</div>

Stereo reconstruction leverages the **information from two or more images** taken from **different viewpoints** to reconstruct the **3D structure of a scene**. By analyzing the disparities between corresponding points in these images, it's possible to compute **depth information and create a depth map**. Techniques such as stereo matching algorithms (e.g., block matching, semi-global matching) are often used to find corresponding points and compute disparities.

#### Visual Hulls

<div style="float:right;margin-left:20px;">
  <img src="assets/image-5.png" alt="Object Tracking Image" width="300"/>
</div>

Visual hulls represent the **shape of an object or scene** by **intersecting the viewing frustums of multiple cameras or viewpoints**. This technique is based on the **principle that the volume occupied by an object can be inferred by determining the regions that are visible from different viewpoints**. Visual hulls are often used for object reconstruction and shape estimation.

#### Image Stitching

Image stitching is the process of **combining multiple overlapping images** to create a panoramic or wide-angle view of a scene. This technique involves **finding corresponding points between images**, estimating the transformation (e.g., homography) between them, and blending the images seamlessly. Image stitching is commonly used in applications such as panoramic photography, **virtual tours**, and mapping. Common tools are SIFT or ORB feature detectors in combination with descriptors. The Ransac algorithm or affine/homographic transformations.

<img src="assets/image-6.png" alt="Object Tracking Image" width="500"/>

#### View Interpolation

<div style="float:right;margin-left:20px;">
  <img src="assets/image-7.png" alt="Object Tracking Image" width="300"/>
</div>

View interpolation involves **synthesizing new views of a scene from existing views or images**. This technique is useful for **generating intermediate views** between known viewpoints or for virtual camera navigation in 3D environments. View interpolation can be achieved through methods like **depth-based rendering, image-based rendering, or morphing**.

#### Pose Estimation

Pose estimation refers to the process of **determining the position and orientation (pose) of a camera relative to a scene or object.** This information is essential for aligning images in 3D reconstruction, tracking camera motion, or registering virtual objects into real-world scenes. Pose estimation techniques can be based on **feature matching, geometric constraints, or deep learning approaches**.

<img src="assets/image-8.png" alt="Object Tracking Image" width="400"/>

### Object and scene segmentation/detection/recognition

<div style="float:right;margin-left:20px;">
  <img src="assets/image-9.png" alt="Object Tracking Image" width="400"/>
</div>

**Image segmentation** is segmenting an image into **subparts** based on common characteristics like **texture, color, smoothness, etc**.

<img src="assets/image-10.png" alt="Object Tracking Image" width="300"/>

For the rest he shows a lot of pictures of hand tracking, object detection. 

## Image formation: Projective geometry

### Homogeneous coordinates

<div style="float:right;margin-left:20px;">
  <img src="assets/image-11.png" alt="Object Tracking Image" width="300"/>
</div>

When projecting a 3D scene onto a 2D plane, what happens with parallel lines? They don't seem that parallel anymore don't they... This is because of the projection. This introduces a new concept, **points at infinity**. 

The **real projective plane** is an **extension of the Euclidean plane**. It includes all the usual points of the Euclidean plane plus additional "points at infinity". These points at infinity are used **to treat parallel lines** as if they intersect **at a unique point at infinity**, thus unifying the treatment of parallel and intersecting lines.

3D scenes are often projected onto 2D planes (like a computer screen) for visualization. **Homogeneous coordinates** facilitate this process. A 3D point (X, Y, Z) in space can be projected onto a 2D surface. This involves a **mathematical transformation** (like a perspective projection), where **distant objects appear smaller than closer ones**, **mimicking** how the human eye perceives **depth**.

In projective geometry, **parallelism and concurrency** are indeed instances of the same concept. **Parallel lines** in Euclidean geometry meet at a **point at infinity** in projective geometry. Similarly, **concurrent lines (lines that all intersect at a single point)** in Euclidean geometry are just a special case where the point of intersection is not at infinity.

**Homogeneous coordinates** are particularly suited for describing projections for several reasons:

- **Ease of Mathematical Operations:** They simplify many geometric transformations, such as translation, rotation, and scaling, into matrix operations.
- **Handling Points at Infinity:** They provide a natural way to deal with points at infinity, crucial for understanding projections and perspective.
- **Unified Framework:** They offer a unified framework for dealing with both finite and infinite points, making algorithms more elegant and general.

**Some rules of the projective plane:**

<div style="float:right;margin-left:20px;">
  <img src="assets/image-12.png" alt="Object Tracking Image" width="300"/>
</div>

2D point is represented by triple (X, Y, Z), where X, Y, Z are not all zero. ($\lambda$X,$\lambda$Y,$\lambda$Z) represent the same point for all $\lambda \neq 0$. When Z $\neq 0$ (X, Y, Z) in the projective plane corresponds to $(\frac{X}{Z}, \frac{Y}{Z})$ in the Euclidean plane. The points (X, Y, 0) are points at infinity in the projective plane, and they lie at the line at infinity.

**Equation of line**

$$ ax + by + cz = 0 $$

Where a, b and z are **constants**, the equation is also **homogeneous** (this means that all the terms have the same degree.). The constants (a,b,c) and $\lambda$(a, b, c) all represent the same line. The line also passes through the point **(b, -a, 0) at infinity**. Each line (not at infinity) meets the **line at infinity** in exactly one point.

In the **Cartesian plane**, a line is defined like this:
$$ ax + by + c = 0 $$

> When (X, Y, Z) in the projective plane lies on a line with coefficients (a, b, c), then (X/Z, Y/Z) in the euclidean planes lies on a line defined by the same coefficients (a, b, c).

**Parallel lines in the Euclidean plane**

<div style="float:right;margin-left:20px;">
  <img src="assets/image-13.png" alt="Object Tracking Image" width="300"/>
</div>

$$ ax + by + c_1 = 0,\quad ax + by + c_2 = 0 $$

These lines correspond to lines in the projective plane that meet at a **common point (b, -a, 0) at infinity**. This means that **parallelism is replaced** by **concurrency at infinity**. (They meet each other in infinity)

**Lines and points**

The point (X, Y, Z) lies on the line defined by (a, b, c) if 
$$ aX + bY + cZ = 0 $$

In vector notation:

$$ 0 = \begin{bmatrix} a & b & c \end{bmatrix} \begin{bmatrix} X \\ Y \\ Z \end{bmatrix} = I^Tp $$

Thus a line can also be represented as a homogeneous 3-vector I.

A **line can be defined by two points** x and y (cross product of the vectors, think of the right hand rule.)
$$ I = x \times y $$
Proof: Let the line be constructed as follows
$$ I \triangleq x \times y $$
(The triangle on the = means that this is equal by definition.)

$$ I^Tx = x^TI = x^T (x \times y) = 0, \quad I^Ty = y^TI = y^T (x \times y) = 0 $$

Apparently, because the rank of the matrix spanned by the three vectors is lower than three, the scalar triple product is zero.

In the same way, a point can be defined as the intersection of two lines I and m:

$$ x = I \times m $$

**Geometric interpretation of line parameters**

<div style="float:right;margin-left:20px;">
  <img src="assets/image-14.png" alt="Object Tracking Image" width="300"/>
</div>

A line I is a homogeneous 3-vector, which is a ray (part of a line with a fixed starting point and no endpoint) in projective space, it is perpendicular to every point ray p on the line:
$$ I^Tp = 0 $$
As I is perpendicular to p1 and p2:
$$ I = p_1 \times p_2 $$
So I is the plane normal.

**Point and line duality**

**Duality principle:** To any theorem of 2-dimensional projective geometry, there is a dual theorem, which may be derived by interchanging the roles of points and lines in the original theorem.

Examples:

$$ p \leftrightarrow I $$
$$ p^TI = 0 \leftrightarrow I^T p = 0 $$
$$ p = I \times I' \leftrightarrow I = p \times p' $$

**Geometric interpretation of line intersection**

<div style="float:right;margin-left:20px;">
  <img src="assets/image-15.png" alt="Object Tracking Image" width="300"/>
</div>

Line parameters defined by two points:

As I is perpendicular to p1 and p2:
$$ I = p_1 \times p_2 $$

Point parameters by intersection of two lines:

As p is perpendicular to I1 and I2:

$$ p = I_1 \times I_2 $$

**Intersection of parallel lines**

Suppose that I and m are two parallel lines:

$$ I = \begin{bmatrix} a \\ b \\ c \end{bmatrix}, \quad m = \begin{bmatrix} a \\ b \\ d \end{bmatrix} $$

Intersection of I and m is given by:

$$ p = I \times m = \begin{bmatrix} a \\ b \\ c \end{bmatrix} \times \begin{bmatrix} a \\ b \\ d \end{bmatrix} = \begin{bmatrix} bd -bc \\ ac -ad \\ ab -ab \end{bmatrix} = (d-c) \begin{bmatrix} b \\ -a \\ 0 \end{bmatrix} $$

This is a point at infinity.

**Projective space**

Projective: (X, Y, Z, W) is equivalent to (X/W, Y/W, Z/W) in the Cartesian space. The point (X, Y, Z, 0) is a point at infinity. The projective space is the set of all points (X, Y, Z, W) in 4D space, where (X, Y, Z, W) and ($\lambda$X, $\lambda$Y, $\lambda$Z, $\lambda$W) represent the same point for all $\lambda \neq 0$.

### 2D transformations

![alt text](assets/image-18.png)

1. **Euclidean Transformation (Rigid Transformation):**<br>
![alt text](assets/image-16.png)
   - **Matrix:** It consists of a 2x2 rotation matrix  R  and a translation vector  t . The bottom row is  [0, 0, 1]  to maintain the homogeneity of the matrix.
   - **Degrees of Freedom (dof):** There are 3 degrees of freedom: 2 for translation (tx, ty) and 1 for rotation.
   - **Invariants:** Euclidean transformation preserves lengths, areas, and the angles between lines. It's a rigid transformation that doesn't change the shape or size of an object.

2. **Similarity Transformation:**<br>
![alt text](assets/image-19.png)
   - **Matrix:** Similar to the Euclidean, but includes a uniform scaling factor  S  along with the rotation matrix  R  and translation vector  t .
   $$ \begin{bmatrix} x' \\ y' \\ 1  \end{bmatrix} = \begin{bmatrix} s \cos \theta & -s \sin \theta & t_x \\ s \sin \theta & s \cos \theta & t_y \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} $$
   - **Degrees of Freedom (dof):** It has 4 degrees of freedom: 2 for translation (tx, ty), 1 for rotation (theta), and 1 for scaling (s).
   - **Invariants:** This transformation preserves the shape of objects. Angles remain unchanged, and all lengths are scaled by the same factor.

3. **Affine Transformation:**<br>
![alt text](assets/image-20.png)
   - **Matrix:** The matrix is a generalization of the Similarity matrix without the restriction of uniform scaling and rotation. The elements  
   $$ a_{11} ,  a_{12} ,  a_{21} ,  a_{22} $$
   $$ \begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} =  \begin{bmatrix} a & b & t_x \\ c & d & t_y \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} $$
   allow for scaling, rotation, and shearing transformations.
   - **Degrees of Freedom (dof):** There are 6 degrees of freedom: 2 for translation, 2 for rotation and scaling, and 2 for shearing.
   - **Invariants:** Affine transformations preserve parallelism of lines but not lengths and angles. Ratios of distances along parallel lines remain constant.

4. **Projective Transformation (Homography):**<br>
![alt text](assets/image-21.png)
   - **Matrix:** This is a 3x3 matrix that allows for projective transformations which include perspective distortions. The matrix  H  can represent complex transformations that can't be achieved with Affine transformations.
   $$ \begin{bmatrix} \lambda x' \\ \lambda y' \\ \lambda \end{bmatrix} =  \begin{bmatrix} h_{11} & h_{12} & h_{13} \\ h_{21} & h_{22} & h_{23} \\ h_{31} & h_{32} & h_{33} \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} $$
   - **Degrees of Freedom (dof):** It has 8 degrees of freedom since the last element  h_{33}  is typically set to 1 for practical reasons (and to avoid a trivial solution of a zero matrix) (arbitrary scaling), but all other elements can change.
   - **Invariants:** Projective transformations preserve collinearity and concurrency of points, meaning that points lying on the same line before transformation will still lie on a line after transformation.

### 3D transformations

![alt text](assets/image-22.png)

1. **Transformation**:
   - **Translation**: Moving an object without rotating or changing its size or shape.
   - **Rigid (Euclidean)**: A combination of translation and rotation that preserves distances and angles.
   - **Similarity**: A transformation that includes translation, rotation, and uniform scaling (enlargement or reduction).
   - **Affine**: A transformation that includes translation, rotation, scaling, and shearing. Parallel lines remain parallel.
   - **Projective**: A transformation that maps straight lines to straight lines but does not necessarily preserve parallelism. It includes perspective distortions.

2. **Matrix**:
   - **I**: The identity matrix.
   - **t**: A translation vector.
   - **R**: A rotation matrix.
   - **s**: A uniform scaling factor.
   - **A**: A matrix representing affine transformation.
   - **Ĥ**: A homography matrix representing projective transformation.

3. **# DoF (Degrees of Freedom)**:
   - **Translation**: 3 degrees of freedom (2 for position in the plane, 1 for depth).
   - **Rigid (Euclidean)**: 6 degrees of freedom (3 for position, 3 for orientation).
   - **Similarity**: 7 degrees of freedom (the 6 from rigid, plus 1 for scale).
   - **Affine**: 12 degrees of freedom (6 from rigid, 1 for scale, and 5 for shearing and non-uniform scaling).
   - **Projective**: 15 degrees of freedom (12 from affine, plus 3 more for perspective transformations).

4. **Preserves**:
   - **Translation**: Orientation.
   - **Rigid (Euclidean)**: Lengths.
   - **Similarity**: Angles.
   - **Affine**: Parallelism.
   - **Projective**: Straight lines (but not necessarily parallelism).

5. **Icon**:
   - **Translation**: A shifted square.
   - **Rigid (Euclidean)**: A rotated square.
   - **Similarity**: A square that is rotated and uniformly scaled.
   - **Affine**: A parallelogram showing that parallelism is preserved.
   - **Projective**: A quadrilateral showing how a square can be transformed, representing perspective distortion.

