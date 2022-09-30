# Game Math & Algorithm

- [1. Vector](#1-vector)
  - [1.1. Dot](#11-dot)
  - [1.2. Cross](#12-cross)
  - [1.3. Lagrange's Identities](#13-lagranges-identities)
- [2. Matrix](#2-matrix)
  - [2.1. Identity](#21-identity)
  - [2.2. Transpose](#22-transpose)
  - [2.3. Inverse](#23-inverse)
- [3. Curves](#3-curves)
  - [3.1. Interpolation](#31-interpolation)
  - [3.2. Bézier curve](#32-bézier-curve)
    - [3.2.1. Higher-order](#321-higher-order)
    - [3.2.2. Two-order](#322-two-order)
    - [3.2.3. Three-order](#323-three-order)
- [4. Transform](#4-transform)
  - [4.1. Translation](#41-translation)
  - [4.2. Scale](#42-scale)
  - [4.3. Euler rotation](#43-euler-rotation)
    - [4.3.1. Rotate around x axis](#431-rotate-around-x-axis)
    - [4.3.2. Rotate around y axis](#432-rotate-around-y-axis)
    - [4.3.3. Rotate around z axis](#433-rotate-around-z-axis)
  - [4.4. Quaternion](#44-quaternion)
    - [4.4.1. Rotate around any axis](#441-rotate-around-any-axis)
    - [4.4.2. Multiply](#442-multiply)
    - [4.4.3. Dot](#443-dot)
    - [4.4.4. Interpolation](#444-interpolation)
    - [4.4.5. Matrix](#445-matrix)
  - [4.5. Euler to Quaternion](#45-euler-to-quaternion)
  - [4.6. Look at](#46-look-at)
  - [4.7. Polar Coordinates](#47-polar-coordinates)
    - [4.7.1. In 2D](#471-in-2d)
    - [4.7.2. In 3D](#472-in-3d)
- [5. Collision detection](#5-collision-detection)
  - [5.1. Line segment](#51-line-segment)
  - [5.2. Plane](#52-plane)
  - [5.3. Triangle](#53-triangle)
  - [5.4. Tetrahedron](#54-tetrahedron)
  - [5.5. Box](#55-box)
  - [5.6. Sphere](#56-sphere)
  - [5.7. Capsule](#57-capsule)
  - [5.8. Two Line segment](#58-two-line-segment)
  - [5.9. Plane and Box](#59-plane-and-box)
  - [5.10. Plane and Sphere](#510-plane-and-sphere)
  - [5.11. Plane and Capsule](#511-plane-and-capsule)
  - [5.12. Box and Box](#512-box-and-box)
  - [5.13. Box and Sphere](#513-box-and-sphere)
  - [5.14. Box and Capsule](#514-box-and-capsule)
  - [5.15. Sphere and Sphere](#515-sphere-and-sphere)
  - [5.16. Sphere and Capsule](#516-sphere-and-capsule)
  - [5.17. Capsule and Capsule](#517-capsule-and-capsule)
- [6. Render pipeline](#6-render-pipeline)
  - [6.1. Camera](#61-camera)
  - [6.2. Object space to World space](#62-object-space-to-world-space)
  - [6.3. World space to View space](#63-world-space-to-view-space)
  - [6.4. View space to Clip space (Projection)](#64-view-space-to-clip-space-projection)
    - [6.4.1. Orthographic](#641-orthographic)
      - [6.4.1.1. OpenGL](#6411-opengl)
      - [6.4.1.2. DirectX](#6412-directx)
    - [6.4.2. Perspective](#642-perspective)
      - [6.4.2.1. OpenGL](#6421-opengl)
      - [6.4.2.2. DirectX](#6422-directx)
  - [6.5. Clip position to Normalized-Device-Coordinates (NDC)](#65-clip-position-to-normalized-device-coordinates-ndc)
  - [6.6. Vertex in shader](#66-vertex-in-shader)
    - [6.6.1. Vertex shader output](#661-vertex-shader-output)
    - [6.6.2. Fragment shader input](#662-fragment-shader-input)
  - [6.7. Depth](#67-depth)
    - [6.7.1. Write into depth texture](#671-write-into-depth-texture)
    - [6.7.2. Read from depth texture](#672-read-from-depth-texture)

## 1. Vector

$$
    \vec{v} = (x, y, z, w) =
    \begin{bmatrix}
        x \\
        y \\
        z \\
        w \\
    \end{bmatrix}
$$

### 1.1. Dot

$$
    \vec{a} = (x_a, y_a, z_a, 1) 
$$

$$
    \vec{b} = (x_b, y_b, z_b, 1)
$$

$$
    \vec{a} \cdot \vec{b} = |\vec{a}| |\vec{b}| \cos\theta =
    x_a x_b + y_a y_b + z_a z_b
$$

$$
    \vec{a} \cdot \vec{b} = \vec{b} \cdot \vec{a}
$$

$$
    \vec{a} \cdot (\vec{b} + \vec{c}) =
    \vec{a} \cdot \vec{b} + \vec{a} \cdot \vec{c}
$$

$$
    (\lambda\vec{a}) \cdot \vec{b} = \lambda(\vec{a} \cdot \vec{b}) =
    \vec{a} \cdot (\lambda\vec{b})
$$

### 1.2. Cross

$$
    \begin{array}{c}
        \vec{a} \times \vec{b} = (|\vec{a}| |\vec{b}| \sin\theta) \vec{n} \\
        = (y_a z_b - z_a y_b,\quad z_a x_b - x_a z_b,\quad x_a y_b - y_a x_b)
    \end{array}
$$

$$
    \vec{a} \times (\vec{b} + \vec{c}) =
    \vec{a} \times \vec{b} + \vec{a} \times \vec{c}
$$

$$
    (\vec{a} + \vec{b}) \times \vec{c} =
    \vec{a} \times \vec{c} + \vec{b} \times \vec{c}
$$

$$
    (\lambda\vec{a}) \times \vec{b} =
    \lambda(\vec{a} \times \vec{b}) = \vec{a} \times (\lambda\vec{b})
$$

### 1.3. Lagrange's Identities

$$
    (\vec{a} \times \vec{b}) \cdot (\vec{c} \times \vec{d}) = (\vec{a} \cdot \vec{c})(\vec{b} \cdot \vec{d}) - (\vec{a} \cdot \vec{d})(\vec{b} \cdot \vec{c})
$$

## 2. Matrix

$$
    M =
    \begin{bmatrix}
        c_{00} & c_{10} & c_{20} & c_{30} \\
        c_{01} & c_{11} & c_{21} & c_{31} \\
        c_{02} & c_{12} & c_{22} & c_{32} \\
        c_{03} & c_{13} & c_{23} & c_{33} \\
    \end{bmatrix}
$$

$$
    \vec{v} = M\vec{v_0} \rightarrow
    \begin{bmatrix}
        \vec{v}.x \\
        \vec{v}.y \\
        \vec{v}.z \\
        \vec{v}.w \\
    \end{bmatrix} = M
    \begin{bmatrix}
        \vec{v_0}.x \\
        \vec{v_0}.y \\
        \vec{v_0}.z \\
        \vec{v_0}.w \\
    \end{bmatrix}
    \And
    \begin{cases}
        \vec{v_0}.w=0 & \vec{v_0} \text{ as direction} \\
        & \text{ or } \\
        \vec{v_0}.w=1 & \vec{v_0} \text{ as point} 
    \end{cases}
$$

### 2.1. Identity

$$
    M_1 =
    \begin{bmatrix}
        1 & 0 & 0 & 0 \\
        0 & 1 & 0 & 0 \\
        0 & 0 & 1 & 0 \\
        0 & 0 & 0 & 1 \\
    \end{bmatrix}
$$

### 2.2. Transpose

$$
    M^T =
    \begin{bmatrix}
        c_{00} & c_{01} & c_{02} & c_{03} \\
        c_{10} & c_{11} & c_{12} & c_{13} \\
        c_{20} & c_{21} & c_{22} & c_{23} \\
        c_{30} & c_{31} & c_{32} & c_{33} \\
    \end{bmatrix}
$$

### 2.3. Inverse

$$
    M M^{-1} = M^{-1} M = M_1
$$

$$
    (M^T)^{-1} = (M^{-1})^T
$$

$$
    (M_A M_B)^{-1} = M_B^{-1} M_A^{-1}
$$

## 3. Curves

### 3.1. Interpolation

$$
    {\rm Lerp} (\vec{a}, \vec{b}, t) =
    (1 - t) \vec{a} + t \vec{b}
    \qquad t \in [0, 1]
$$

$$
    {\rm NLerp} (\vec{a}, \vec{b}, t) =
    {\rm normalize} ({\rm Lerp}(\vec{a}, \vec{b}, t))
    \qquad t \in [0, 1]
$$

$$
    {\rm SLerp} (\vec{a}, \vec{b}, t) =
    \frac{\sin((1-t)\theta)}{\sin \theta} \vec{a} +
    \frac{\sin t\theta}{\sin \theta} \vec{b}
    \qquad t \in [0, 1]
$$

### 3.2. Bézier curve

#### 3.2.1. Higher-order

$$
    P(t) = \sum_{i=0}^n{P_i B_{i,n}(t)} \qquad t \in [0, 1]
$$

$$
    B_{i,n}(t) = C_n^i t^i (1-t)^{n-i} = \frac{n!}{i!(n-i)!} t^i (1 - t)^{n-i} \qquad
    i \in \lbrace 0,1, \cdots , n \rbrace
$$

#### 3.2.2. Two-order

$$
    P(t) = (1-t)^2 P_0 + 2t(1-t) P_1 + t^2 P_2 \qquad t \in [0, 1]
$$


#### 3.2.3. Three-order

$$
    P(t) = (1-t)^3 P_0 + 3t(1-t)^2 P_1 + 3t^2(1-t)P_2 + t^3 P_3 \qquad t \in [0, 1]
$$

## 4. Transform

    Unity use left hand coordinate system.

$$
    \begin{array}{c}
        M_{TRS} = M_T M_R M_S \\
        =
        \begin{bmatrix}
            M_R.c_{00} \times M_S.c_{00} & M_R.c_{10} \times M_S.c_{11} & M_R.c_{20} \times M_S.c_{22} & M_T.c_{30} \\
            M_R.c_{01} \times M_S.c_{00} & M_R.c_{11} \times M_S.c_{11} & M_R.c_{21} \times M_S.c_{22} & M_T.c_{31} \\
            M_R.c_{02} \times M_S.c_{00} & M_R.c_{12} \times M_S.c_{11} & M_R.c_{22} \times M_S.c_{22} & M_T.c_{32} \\
            0                            & 0                            & 0                            & 1          \\
        \end{bmatrix}
    \end{array}
$$

$$
    M_R^{-1} = M_R^T
$$

$$
    M_{TRS}^{-1} = M_S^{-1} M_R^{-1} M_T^{-1}
$$

### 4.1. Translation

$$
    M_T =
    \begin{bmatrix}
        1 & 0 & 0 & x \\
        0 & 1 & 0 & y \\
        0 & 0 & 1 & z \\
        0 & 0 & 0 & 1 \\
    \end{bmatrix}
$$

$$
    M_T^{-1} =
    \begin{bmatrix}
        1 & 0 & 0 & -x \\
        0 & 1 & 0 & -y \\
        0 & 0 & 1 & -z \\
        0 & 0 & 0 & 1 \\
    \end{bmatrix}
$$

### 4.2. Scale

$$
    M_S =
    \begin{bmatrix}
        x & 0 & 0 & 0 \\
        0 & y & 0 & 0 \\
        0 & 0 & z & 0 \\
        0 & 0 & 0 & 1 \\
    \end{bmatrix}
$$

$$
    M_S^{-1} =
    \begin{bmatrix}
        \frac{1}{x} & 0           & 0           & 0 \\
        0           & \frac{1}{y} & 0           & 0 \\
        0           & 0           & \frac{1}{z} & 0 \\
        0           & 0           & 0           & 1 \\
    \end{bmatrix}
$$

### 4.3. Euler rotation

$$
    \begin{array}{c}
        M_R = M_{Ra} M_{Rb} M_{Rc} \\
        (a, b, c) \in \lbrace (x,y,z), (x,z,y), (y,x,z), (y,z,x), (z,x,y), (z,y,x) \rbrace
    \end{array}
$$

#### 4.3.1. Rotate around x axis

$$
    M_{Rx} =
    \begin{bmatrix}
        1 & 0           & 0            & 0 \\
        0 & \cos \theta & -\sin \theta & 0 \\
        0 & \sin \theta & \cos \theta  & 0 \\
        0 & 0           & 0            & 1 \\
    \end{bmatrix}
$$

#### 4.3.2. Rotate around y axis

$$
    M_{Ry} =
    \begin{bmatrix}
        \cos \theta  & 0 & \sin \theta & 0 \\
        0            & 1 & 0           & 0 \\
        -\sin \theta & 0 & \cos \theta & 0 \\
        0            & 0 & 0           & 1 \\
    \end{bmatrix}
$$

#### 4.3.3. Rotate around z axis

$$
    M_{Rz} =
    \begin{bmatrix}
        \cos \theta & -\sin \theta & 0 & 0 \\
        \sin \theta & \cos \theta  & 0 & 0 \\
        0           & 0            & 1 & 0 \\
        0           & 0            & 0 & 1 \\
    \end{bmatrix}
$$

### 4.4. Quaternion

$$
    i^2=j^2=k^2=ijk=-1
$$

$$
    ij = k  \qquad
    ji = -k \qquad
    jk = i  \qquad
    kj = -i \qquad
    ki = j  \qquad
    ik = -j
$$

#### 4.4.1. Rotate around any axis

$$
    \begin{split}
        \text{Axis: } \quad & \vec{a}
        = (x_a, y_a, z_a) = x_a i + y_a j + z_a k \quad |\vec{a}| = 1 \\
        \text{Quaternion: } \quad & Q(\vec{a}, \theta)
        = \vec{a} \sin\left(\frac{\theta}{2}\right) + \cos\left(\frac{\theta}{2}\right) \\
        &= (\vec{u}, w) 
        = (x i + y j + z k) + w \\
        & |Q(\vec{a}, \theta)| = 1 \quad q = Q(\vec{a}, \theta) \quad q^{-1} = Q(\vec{a}, -\theta) \\
        \text{Original vector: } \quad & \vec{v_0} = x_0 i + y_0 j + z_0 k \\
        \text{Rotated vector: } \quad & \vec{v} = q \vec{v_0} (q^{-1})
    \end{split}
$$

#### 4.4.2. Multiply

$$
    q_1 = (\vec{u_1}, w_1) = (x_1 i + y_1 j + z_1 k) + w_1
$$

$$
    q_2 = (\vec{u_2}, w_2) = (x_2 i + y_2 j + z_2 k) + w_2
$$

$$
    \begin{array}{c}
        q_1 q_2 & = (w_2 \vec{u_1} + w_1 \vec{u_2} + \vec{u_1} \times \vec{u_2},
        \quad w_1 w_2 - \vec{u_1} \cdot{} \vec{u_2}) \\
        & =
        \begin{bmatrix}
            w_1 & -z_1 & y_1 & x_1 \\
            z_1 & w_1 & -x_1 & y_1 \\
            -y_1 & x_1 & w_1 & z_1 \\
            -x_1 & -y_1 & -z_1 & w_1 \\
        \end{bmatrix}
        \begin{bmatrix}
            x_2 \\
            y_2 \\
            z_2 \\
            w_2 \\
        \end{bmatrix}
    \end{array}
$$

#### 4.4.3. Dot

$$
    q_1 \cdot q_2 = |q_1| |q_2| \cos \theta = \cos \theta =
    x_1 x_2 + y_1 y_2 + z_1 z_2 + w_1 w_2
$$

#### 4.4.4. Interpolation

$$
    \Delta q = q_2 (q_1^{-1})
$$

$$
    q_t =
    \left.
    \begin{cases}
        {\rm NLerp}(q_1, q_2, t) & \text{if } \Delta q \text{ is small} \\
                                 & \text{ or }                          \\
        {\rm SLerp}(q_1, q_2, t) & \text{if } \Delta q \text{ is big}   \\
    \end{cases}
    \right.
    t \in [0, 1]
$$

#### 4.4.5. Matrix

$$
    M_R =
    \begin{bmatrix}
        1-2y^2-2z^2 & 2xy+2zw       & 2xz-2yw       &0 \\
        2xy-2zw     & 1-2x^2-2z^2   & 2yz+2xw       &0 \\
        2xz+2yw     & 2yz-2xw       & 1-2x^2-2y^2   &0 \\
        0           & 0             & 0             &1 \\
    \end{bmatrix}
$$

$$
    q
    \begin{cases}
        x = & \frac{c_{21}-c_{12}}{4w} \\ 
        y = & \frac{c_{20}-c_{02}}{4w} \\ 
        z = & \frac{c_{10}-c_{01}}{4w} \\
        w = & \frac{1}{2} \sqrt{1+c_{00}+c_{11}+c_{22}}\\
    \end{cases}
$$

### 4.5. Euler to Quaternion

$$
    \begin{cases}
        s_x = \sin{\frac{\theta_x}{2}} \quad
        s_y = \sin{\frac{\theta_y}{2}} \quad
        s_z = \sin{\frac{\theta_z}{2}} \\
        c_x = \cos{\frac{\theta_x}{2}} \quad
        c_y = \cos{\frac{\theta_y}{2}} \quad
        c_z = \cos{\frac{\theta_z}{2}} \\
        q_x = s_x i + c_x \qquad \text{Rotate around x axis} \\
        q_y = s_y j + c_y \qquad \text{Rotate around y axis} \\
        q_z = s_z k + c_z \qquad \text{Rotate around z axis} \\
    \end{cases}
$$

$$
    \implies
    \begin{cases}
        q_a q_b q_c =
        \begin{array}{c}
            &((s_x c_y c_z + sign_1 s_y s_z c_x) i \\ 
            &+(s_y c_x c_z + sign_2 s_x s_z c_y) j \\ 
            &+(s_z c_x c_y + sign_3 s_x s_y c_z) k \\ 
            &+(c_x c_y c_z + sign_4 s_x s_y s_z))
        \end{array} \\
        (a, b, c, sign_1, sign_2, sign_3, sign_4) \in
        \begin{cases}
            (x, y, z, -1,  1, -1,  1) \\
            (x, z, y,  1,  1, -1, -1) \\
            (y, x, z, -1,  1,  1, -1) \\
            (y, z, x, -1, -1,  1,  1) \\
            (z, x, y,  1, -1, -1,  1) \\
            (z, y, x,  1, -1,  1, -1) \\
        \end{cases}
    \end{cases}
$$

### 4.6. Look at

$$
    \begin{array}{c}
        \vec{nr} = & {\rm normalize}(\vec{right})   \\
        \vec{nu} = & {\rm normalize}(\vec{up})      \\
        \vec{nf} = & {\rm normalize}(\vec{forward}) \\
    \end{array}
$$

$$
    M_R =
    \begin{bmatrix}
        \vec{nr}.x & \vec{nu}.x & \vec{nf}.x & 0 \\
        \vec{nr}.y & \vec{nu}.y & \vec{nf}.y & 0 \\
        \vec{nr}.z & \vec{nu}.z & \vec{nf}.z & 0 \\
        0          & 0          & 0          & 1 \\
    \end{bmatrix}
$$

### 4.7. Polar Coordinates

$$
    {\rm atan2}(y,x) =
    \begin{cases}
        \arctan{\frac{y}{x}}       & \text{if  } x > 0            \\
        \arctan{\frac{y}{x}} + \pi & \text{if  } x < 0, y \geq 0  \\
        \arctan{\frac{y}{x}} - \pi & \text{if  } x < 0, y < 0     \\
        \frac{\pi}{2}              & \text{if  } x = 0, y > 0     \\
        -\frac{\pi}{2}             & \text{if  } x = 0, y < 0     \\
        0                          & \text{if  } x = 0, y = 0     \\
    \end{cases}
$$

#### 4.7.1. In 2D

$$
    (x, y)
    \begin{cases}
        x = r \cos{\theta} \\
        y = r \sin{\theta} \\
    \end{cases}
    \iff
    (r, \theta)
    \begin{cases}
        r = \sqrt{x^2 + y^2} \\
        \theta = {\rm atan2}(y,x)
    \end{cases}
$$

#### 4.7.2. In 3D

$$
    (x, y, z)
    \begin{cases}
        x = r \sin{\theta} \cos{\phi} \\
        y = r \sin{\theta} \sin{\phi} \\
        z = r \cos{\theta}            \\
    \end{cases}
    \iff
    (r, \theta, \phi)
    \begin{cases}
        r = \sqrt{x^2 + y^2 + z^2}                \\
        \theta = {\rm atan2}(\sqrt{x^2 + y^2}, z) \\
        \phi = {\rm atan2}(y, x)                  \\
    \end{cases}
$$

## 5. Collision detection

$$
    \begin{split}
        \text{Any point : } & \vec{v} = (x, y, z) \\
        \text{Center : } & \vec{o} = (x_0, y_0, z_0) \\
        \text{Extends : } & \vec{e} = (x_e, y_e, z_e) \quad \vec{e} \geq 0 \\
        \text{Radius : } & r \quad r > 0 \\
        \text{Normal of plane : } & \vec{n} = (x_n, y_n, z_n) \\
        \text{Ray : } &
        \begin{cases}
            \vec{o_{ra}} = (x_{ra0}, y_{ra0}, z_{ra0}) & \text{original of ray} \\
            \vec{v_{ra}} = (x_{vra}, y_{vra}, z_{vra}) & \text{vector of ray} \\
        \end{cases} \\
        \text{Signed distance function : } & {\rm distance}(\Phi_1, \Phi_2) =
        \begin{cases}
            d & \text{if} & d > 0 & \text{outside the geometry}  \\
            d & \text{if} & d = 0 & \text{on the geometry}       \\
            d & \text{if} & d < 0 & \text{inside the geometry}   \\
        \end{cases} \\
        \text{Closest Point : } & {\rm closest}(\Phi_1, \Phi_2) \\
    \end{split}
$$

### 5.1. Line segment

$$
    \begin{split}
        \text{Line : } & \Phi = (\vec{o}, \vec{e}) \\
        \text{From } \Phi \text{ to } \vec{v} \text{ : } &
        \begin{cases}
            \vec{c} & = \vec{o} + \vec{e} \times {\rm clamp}(\frac{\vec{ov} \cdot \vec{e}}{\vec{e} \cdot \vec{e}}, 0, 1) \\
            {\rm distance}(\Phi, \vec{v}) & = |\vec{cv}| \\
            {\rm closest}(\Phi, \vec{v}) & = \vec{c} \\
        \end{cases} \\
    \end{split}
$$

### 5.2. Plane

$$
    \begin{split}
        \text{Surface : } & \Phi = (\vec{o}, \vec{n}) \\
        \text{From } \Phi \text{ to } \vec{v} \text{ : } & 
        \begin{cases}
            distance(\Phi, \vec{v}) &= \vec{n} \cdot (\vec{v} - \vec{o}) \\
            &= x_n(x - x_0) + y_n(y - y_0) + z_n(z - z_0) \\
            &= Ax + By + Cz + D \\
            {\rm closest}(\Phi, \vec{v}) &= \vec{v} - distance(\Phi, \vec{v}) \times \vec{n} \\
        \end{cases}
    \end{split}
$$

### 5.3. Triangle

$$
    \begin{split}
        \text{Surface : } & \Phi = (\vec{a}, \vec{b}, \vec{c}) \\
        \text{From } \Phi \text{ to } \vec{v} \text{ : } &
        \begin{cases}
            \vec{n} = {\rm normalize}(\vec{ab} \times \vec{ac}) \\
            d_{plane} = {\rm distance}(\Phi_{plane}, \vec{v}) \\
            \vec{p_{plane}} = {\rm closest}(\Phi_{plane}, \vec{v}) \\
            s = {\rm all}({\rm sign}(\vec{ab} \times \vec{ap_{plane}}) \equiv {\rm sign}(\vec{bc} \times \vec{bp_{plane}}) \equiv {\rm sign}(\vec{ca} \times \vec{cp_{plane}})) \\
            \vec{p_{ab}} = {\rm closest}(\Phi_{ab}, \vec{v}) \quad d_{ab} = |\vec{p_{ab} v}| \\
            \vec{p_{bc}} = {\rm closest}(\Phi_{bc}, \vec{v}) \quad d_{bc} = |\vec{p_{bc} v}| \\
            \vec{p_{ca}} = {\rm closest}(\Phi_{ca}, \vec{v}) \quad d_{ca} = |\vec{p_{ca} v}| \\
            {\rm distance}(\Phi, \vec{v}) = s \times d_{plane} + (1 - s) \times {\rm sign}(d_{plane}) \times \min(d_{ab}, d_{bc}, d_{ca}) \\
            {\rm closest}(\Phi, \vec{v}) = s \times \vec{p_{plane}} + (1-s) \times {\rm closest}(\lbrace \vec{p_{ab}}, \vec{p_{bc}}, \vec{p_{ca}} \rbrace, \vec{v}) \\
        \end{cases} \\
    \end{split}
$$

### 5.4. Tetrahedron

    Collision with 4 triangles.

### 5.5. Box

$$
    \begin{split}
        \text{Surface : } & \Phi = (\vec{o}, \vec{e}) \\
        \text{From } \Phi \text{ to } \vec{v} \text{ : } &
        \begin{cases}
            \vec{c} &= {\rm abs}(\vec{ov}) - \vec{e} \\
            {\rm distance}(\Phi, \vec{v}) &= |\max(\vec{c}, 0)| + \min(\max(\max(\vec{c}.x, \vec{c}.y), \vec{c}.z), 0) \\
            {\rm closest}(\Phi, \vec{v}) &= \vec{o} + {\rm clamp}(\vec{ov}, -\vec{e}, \vec{e}) \\
        \end{cases} \\
    \end{split}
$$

### 5.6. Sphere

$$
    \begin{split}
        \text{Surface : } & \Phi = (\vec{o}, r) \\
        \text{From } \Phi \text{ to } \vec{v} \text{ : } &
        \begin{cases}
            {\rm distance}(\Phi, \vec{v}) &= |\vec{ov}| - r \\
            {\rm closest}(\Phi, \vec{v}) &= \vec{o} + {\rm normalize}(\vec{ov}) \times r \\
        \end{cases}
    \end{split}
$$

### 5.7. Capsule

$$
    \begin{split}
        \text{Surface : } & \Phi = (\vec{o}, \vec{e}, r) \\
        \text{From } \Phi \text{ to } \vec{v} \text{ : } &
        \begin{cases}
            \vec{c} & = \vec{o} + \vec{e} \times {\rm clamp}(\frac{\vec{ov} \cdot \vec{e}}{\vec{e} \cdot \vec{e}}, -1, 1) \\
            {\rm distance}(\Phi, \vec{v}) & = |\vec{cv}| - r \\
            {\rm closest}(\Phi, \vec{v}) &= \vec{c} + {\rm normalize}(\vec{cv}) \times r \\
        \end{cases} \\
    \end{split}
$$

### 5.8. Two Line segment

$$
    \begin{split}
        \text{From } \Phi \text{ to } \Phi \text{ : } &
        \begin{cases}
            L_1(t_1) = o_1 + t_1 \times e_1 \quad (0 \leq t_1 \leq 1) \\
            L_2(t_2) = o_2 + t_2 \times e_2 \quad (0 \leq t_2 \leq 1) \\
            L_d = L_1(t_1) - L_2(t_2) \\
            L_d \cdot e_1 = 0 \quad L_d \cdot e_2 = 0 \\
            t_1 = \frac{bt_2 - c}{a} \quad t_2 = \frac{bt_1 + f}{e} \\
            t_1 = \frac{bf - ce}{d} \quad t_2 = \frac{af - bc}{d} \\
            a = e_1 \cdot e_1 \quad b = e_1 \cdot e_2 \quad c = e_1 \cdot o_2o_1 \\
            d = ae - b^2 \quad e = e_2 \cdot e_2 \quad f = e_2 \cdot o_2o_1 \\
            d \rArr (|e_1| |e_2| \sin(\theta))^2 \geq 0 \\
            t_1 = \begin{cases}
                0 & \text{if } d = 0 \text{ parallel} \\
                {\rm clamp}(\frac{bf-ce}{d}, 0, 1) & \text{if } d > 0 \text{ not parallel} \\
            \end{cases} \\
            t_2 = {\rm clamp}(\frac{bt_1+f}{e}) \\
            t_1 = {\rm clamp}(\frac{bt_2-c}{a}) \\
            {\rm distance}(\Phi, \Phi) = |L_d| \\
            {\rm closest}(\Phi, \Phi)_{L1} = L_1(t_1) \\
            {\rm closest}(\Phi, \Phi)_{L2} = L_2(t_2) \\
        \end{cases}
    \end{split}
$$

### 5.9. Plane and Box

$$
    \begin{split}
        \text{From } \Phi \text{ to } \Phi \text{ : } &
        \begin{cases}
            \vec{b_{min}} &= \vec{o_{box}} - \vec{e_{box}} \\
            \vec{b_{max}} &= \vec{o_{box}} + \vec{e_{box}} \\
            s &= \vec{n_{plane}} \geq 0 \\
            \vec{v_{back}} &= s \times \vec{b_{min}} + (1 - s) \times \vec{b_{max}} \\
            \vec{v_{front}} &= s \times \vec{b_{max}} + (1 - s) \times \vec{b_{min}} \\
            d_{min} &= d(\Phi_{plane}, \vec{v_{back}}) \\
            d_{max} &= d(\Phi_{plane}, \vec{v_{front}}) \\
            {\rm distance}(\Phi, \Phi) &=
            \begin{cases}
                d_{min} & \text{if} & d_{min} > 0 & \text{disjoint} \\
                d_{min} & \text{if} & d_{min} \leq 0 \And d_{max} \geq 0 & \text{intersect} \\
                d_{max} & \text{if} & d_{max} < 0 & \text{contains} \\
            \end{cases} \\
            {\rm closest}(\Phi, \Phi)_{box} &= \vec{v_{back}} \\
            {\rm closest}(\Phi, \Phi)_{plane} &= \vec{v_{back}} - d_{min} \times \vec{n_{plane}} \\
        \end{cases} \\
    \end{split}
$$

### 5.10. Plane and Sphere

$$
    \begin{split}
        \text{From } \Phi \text{ to } \Phi \text{ : } &
        \begin{cases}
            d_o &= {\rm distance}(\Phi_{plane}, \vec{o_{sphere}}) \\
            d_{min} &= d_o - r_{sphere} \\
            d_{max} &= d_o + r_{sphere} \\
            {\rm distance}(\Phi, \Phi) &=
            \begin{cases}
                d_{min} & \text{if} & d_{min} > 0 & \text{disjoint} \\
                d_{min} & \text{if} & d_{min} \leq 0 \And d_{max} \geq 0 & \text{intersect} \\
                d_{max} & \text{if} & d_{max} < 0 & \text{contains} \\
            \end{cases} \\
            {\rm closest}(\Phi, \Phi)_{sphere} &= \vec{o_{sphere}} - r_{sphere} \times \vec{n_{plane}} \\
            {\rm closest}(\Phi, \Phi)_{plane} &= \vec{o_{sphere}} - {\rm distance}(\Phi_{plane}, \vec{o_{sphere}}) \times \vec{n_{plane}} \\
        \end{cases} \\
    \end{split}
$$

### 5.11. Plane and Capsule

$$
    \begin{split}
        \text{From } \Phi \text{ to } \Phi \text{ : } &
        \begin{cases}
            \vec{a} &= \vec{o_{capsule}} + \vec{e_{capsule}} \\
            \vec{b} &= \vec{o_{capsule}} - \vec{e_{capsule}} \\
            d_{a} &= {\rm distance}(\Phi_{plane}, \vec{a}) \\
            d_{b} &= {\rm distance}(\Phi_{plane}, \vec{b}) \\
            s &= d_{a} \geq d_{b} \\
            \vec{v_a} &= s \times \vec{b} + (1 - s) \times \vec{a} \\
            \vec{v_b} &= s \times \vec{a} + (1 - s) \times \vec{b} \\
            \vec{v_{back}} &= \vec{v_a} - \vec{n_{plane}} \times r_{capsule} \\
            \vec{v_{front}} &= \vec{v_b} + \vec{n_{plane}} \times r_{capsule} \\
            d_{min} &= s \times d_{b} + (1 - s) \times d_{a} - r_{capsule} \\
            d_{max} &= s \times d_{a} + (1 - s) \times d_{b} + r_{capsule} \\
            {\rm distance}(\Phi, \Phi) &=
            \begin{cases}
                d_{min} & \text{if} & d_{min} > 0 & \text{disjoint} \\
                d_{min} & \text{if} & d_{min} \leq 0 \And d_{max} \geq 0 & \text{intersect} \\
                d_{max} & \text{if} & d_{max} < 0 & \text{contains} \\
            \end{cases} \\
            {\rm closest}(\Phi, \Phi)_{capsule} &= \vec{v_{back}} \\
            {\rm closest}(\Phi, \Phi)_{plane} &= \vec{v_{back}} - d_{min} \times \vec{n_{plane}} \\
        \end{cases}\\
    \end{split}
$$

### 5.12. Box and Box

$$
    \text{AABB}
    \begin{cases}
        \vec{a_{max}} = \vec{o_a} + \vec{e_a} \\
        \vec{a_{min}} = \vec{o_a} - \vec{e_a} \\
        \vec{b_{max}} = \vec{o_b} + \vec{e_b} \\
        \vec{b_{min}} = \vec{o_b} - \vec{e_b} \\
        \text{intersect} = {\rm not}({\rm any}(\vec{a_{min}} > \vec{b_{max}} \text{  or  } \vec{b_{min}} > \vec{a_{max}})) \\
    \end{cases}
$$

$$
    \text{AABB and OBB}
    \begin{cases}
    \end{cases}
$$

### 5.13. Box and Sphere

$$
    \begin{split}
        \text{From } \Phi \text{ to } \Phi \text{ : } &
        \begin{cases}
            \vec{c} &= {\rm abs}(\vec{o_{box}o_{sphere}}) - \vec{e_{box}} \\
            d_0 &= |\max(\vec{c}, 0)| + \min(\max(\max(\vec{c}.x, \vec{c}.y), \vec{c}.z), 0) \\
            {\rm distance}(\Phi, \Phi) &= d_0 - r_{sphere} \\
            {\rm closest}(\Phi, \Phi)_{box} &= \vec{o_{box}} + {\rm clamp}(\vec{o_{box}o_{sphere}}, -\vec{e_{box}}, \vec{e_{box}}) \\
            {\rm closest}(\Phi, \Phi)_{sphere} &= {\rm normalize}(\vec{o_{sphere}p_{box}}) \times r_{sphere}
        \end{cases} \\
    \end{split}
$$

### 5.14. Box and Capsule

### 5.15. Sphere and Sphere

$$
    \begin{split}
        \text{From } \Phi \text{ to } \Phi \text{ : } &
        \begin{cases}
            {\rm distance}(\Phi, \Phi) &= |\vec{o_a o_b}| - r_a - r_b \\
            {\rm closest}(\Phi, \Phi)_a &= \vec{o_a} + {\rm normalize}(\vec{o_a o_b}) \times r_a \\
            {\rm closest}(\Phi, \Phi)_b &= \vec{o_b} + {\rm normalize}(\vec{o_b o_a}) \times r_b \\
        \end{cases} \\
    \end{split}
$$

### 5.16. Sphere and Capsule

$$
    \begin{split}
        \text{From } \Phi \text{ to } \Phi \text{ : } &
        \begin{cases}
            \vec{c} & = \vec{o_{c}} + \vec{e_{c}} \times {\rm clamp}(\frac{\vec{o_{c}o_{s}} \cdot \vec{e_{c}}}{\vec{e_{c}} \cdot \vec{e_{c}}}, -1, 1) \\
            {\rm distance}(\Phi, \Phi) & = |\vec{co_s}| - r_c - r_s \\
            {\rm closest}(\Phi, \Phi)_{sphere} &= \vec{o_s} + {\rm normalize}(\vec{o_s c}) \times r_s \\
            {\rm closest}(\Phi, \Phi)_{capsule} &= \vec{c} + {\rm normalize}(\vec{c o_s}) \times r_c \\
        \end{cases} \\
    \end{split}
$$

### 5.17. Capsule and Capsule

    Same as two line segment.

## 6. Render pipeline

### 6.1. Camera

$$
    \begin{cases}
        n  &\text{Near clip plane distance value from camera}                                    \\
        f  &\text{Far clip plane distance value from camera}                                     \\
        l  &\text{Min x coordinate value at near plane}                                          \\
        r  &\text{Max x coordinate value at near plane}                                          \\
        b  &\text{Min y coordinate value at near plane}                                          \\
        t  &\text{Max y coordinate value at near plane}                                          \\
        a  & = \frac{r-l}{t-b} \And \lbrace t=-b, r=-l \rbrace \qquad \text{Aspect ratio}                  \\
        \theta  & = 2 \arctan{\frac{t-b}{2n}} \And \lbrace t=-b, r=-l \rbrace \qquad \text{Field of view} \\
    \end{cases}
$$

### 6.2. Object space to World space

$$
    M_M = {M_{TRS}}_{object} \quad \text{which is object's transform matrix}
$$

$$
    \begin{bmatrix}
        x_{world} \\
        y_{world} \\
        z_{world} \\
        1 \\
    \end{bmatrix}
    = M_M
    \begin{bmatrix}
        x_{object} \\
        y_{object} \\
        z_{object} \\
        1 \\
    \end{bmatrix}
$$

### 6.3. World space to View space

$$
    M_V =
    \begin{bmatrix}
        1 & 0 & 0  & 0 \\
        0 & 1 & 0  & 0 \\
        0 & 0 & -1 & 0 \\
        0 & 0 & 0  & 1 \\
    \end{bmatrix}
    {M_R^{-1}}_{camera} {M_T^{-1}}_{camera}
    \quad \text{which is camera's } M_R^{-1} \text{ and } M_T^{-1}
$$

$$
    \begin{bmatrix}
        x_{view} \\
        y_{view} \\
        z_{view} \\
        1 \\
    \end{bmatrix}
    \And \lbrace -n \geq z_{view} \geq -f \rbrace
    = M_V
    \begin{bmatrix}
        x_{world} \\
        y_{world} \\
        z_{world} \\
        1 \\
    \end{bmatrix}
$$

### 6.4. View space to Clip space (Projection)

    In DirectX platform,Unity remap OpenGL z and reverse z depth into DirectX z.

$$
    M_{GL2DX} =
    \begin{bmatrix}
        1 & 0 & 0            & 0           \\
        0 & 1 & 0            & 0           \\
        0 & 0 & -\frac{1}{2} & \frac{1}{2} \\
        0 & 0 & 0            & 1           \\
    \end{bmatrix}
$$

#### 6.4.1. Orthographic

##### 6.4.1.1. OpenGL

$$
    M_P = {M_{ortho}}_{GL} =
    \begin{bmatrix}
        \frac{2}{r-l} & 0             & 0              & -\frac{r+l}{r-l} \\
        0             & \frac{2}{t-b} & 0              & -\frac{t+b}{t-b} \\
        0             & 0             & -\frac{2}{f-n} & -\frac{f+n}{f-n} \\
        0             & 0             & 0              & 1                \\
    \end{bmatrix}
$$

$$
    \begin{bmatrix}
        x_{clip}                     \\
        y_{clip}                     \\
        z_{clip}                     \\
        w_{clip} = 1                 \\
    \end{bmatrix}
    \And
    \begin{cases}
        -1 & \leq x_{clip} & \leq 1  \\
        -1 & \leq y_{clip} & \leq 1  \\
        -1 & \leq z_{clip} & \leq 1  \\
    \end{cases}
    = M_P
    \begin{bmatrix}
        x_{view}                     \\
        y_{view}                     \\
        z_{view}                     \\
        1                            \\
    \end{bmatrix}
$$

##### 6.4.1.2. DirectX

$$
    \text{Original } {M_{ortho}}_{DX} =
    \begin{bmatrix}
        \frac{2}{r-l} & 0             & 0              & -\frac{r+l}{r-l} \\
        0             & \frac{2}{t-b} & 0              & -\frac{t+b}{t-b} \\
        0             & 0             & \frac{1}{f-n} & -\frac{n}{f-n}    \\
        0             & 0             & 0              & 1                \\
    \end{bmatrix}
$$

$$
    \text{But in Unity } M_P = M_{GL2DX} {M_{ortho}}_{GL} =
    \begin{bmatrix}
        \frac{2}{r-l} & 0             & 0              & -\frac{r+l}{r-l} \\
        0             & \frac{2}{t-b} & 0              & -\frac{t+b}{t-b} \\
        0             & 0             & \frac{1}{f-n}  & \frac{f}{f-n}    \\
        0             & 0             & 0              & 1                \\
    \end{bmatrix}
$$

$$
    \begin{bmatrix}
        x_{clip}                     \\
        y_{clip}                     \\
        z_{clip}                     \\
        w_{clip} = 1                 \\
    \end{bmatrix}
    \And
    \begin{cases}
        -1 & \leq x_{clip} & \leq 1  \\
        -1 & \leq y_{clip} & \leq 1  \\
        1  & \geq z_{clip} & \geq 0  \\
    \end{cases}
    = M_P
    \begin{bmatrix}
        x_{view}                     \\
        y_{view}                     \\
        z_{view}                     \\
        1                            \\
    \end{bmatrix}
$$

#### 6.4.2. Perspective

##### 6.4.2.1. OpenGL

$$
    M_P = {M_{persp}}_{GL} =
    \begin{bmatrix}
        \frac{2n}{r-l} & 0              & \frac{r+l}{r-l}  & 0                   \\
        0              & \frac{2n}{t-b} & \frac{t+b}{t-b}  & 0                   \\
        0              & 0              & -\frac{f+n}{f-n} & -\frac{2fn}{f-n}    \\
        0              & 0              & -1               & 0                   \\
    \end{bmatrix}
$$

$$
    \begin{bmatrix}
        x_{clip}                          \\
        y_{clip}                          \\
        z_{clip}                          \\
        w_{clip} = -z_{view} = z_{camera} \\
    \end{bmatrix}
    \And
    \begin{cases}
        -n & \leq x_{clip} & \leq f       \\
        -n & \leq y_{clip} & \leq f       \\
        -n & \leq z_{clip} & \leq f       \\
    \end{cases}
    = M_P
    \begin{bmatrix}
        x_{view}                          \\
        y_{view}                          \\
        z_{view}                          \\
        1                                 \\
    \end{bmatrix}
$$

##### 6.4.2.2. DirectX

$$
    \text{Original } {M_{persp}}_{DX} =
    \begin{bmatrix}
        \frac{2n}{r-l} & 0              & -\frac{r+l}{r-l}  & 0                   \\
        0              & \frac{2n}{t-b} & -\frac{t+b}{t-b}  & 0                   \\
        0              & 0              & \frac{f}{f-n}     & -\frac{fn}{f-n}     \\
        0              & 0              & 1                 & 0                   \\
    \end{bmatrix}
$$

$$
    \text{But in Unity } M_P = M_{GL2DX} {M_{persp}}_{GL} =
    \begin{bmatrix}
        \frac{2n}{r-l} & 0              & \frac{r+l}{r-l}  & 0                   \\
        0              & \frac{2n}{t-b} & \frac{t+b}{t-b}  & 0                   \\
        0              & 0              & \frac{n}{f-n}    & \frac{fn}{f-n}      \\
        0              & 0              & -1               & 0                   \\
    \end{bmatrix}
$$

$$
    \begin{bmatrix}
        x_{clip}                          \\
        y_{clip}                          \\
        z_{clip}                          \\
        w_{clip} = -z_{view} = z_{camera} \\
    \end{bmatrix}
    \And
    \begin{cases}
        -n & \leq x_{clip} & \leq f       \\
        -n & \leq y_{clip} & \leq f       \\
        n  & \geq z_{clip} & \geq 0       \\
    \end{cases}
    = M_P
    \begin{bmatrix}
        x_{view}                          \\
        y_{view}                          \\
        z_{view}                          \\
        1                                 \\
    \end{bmatrix}
$$

### 6.5. Clip position to Normalized-Device-Coordinates (NDC)

$$
    \begin{bmatrix}
        x_{NDC} = \frac{x_{clip}}{w_{clip}} \\
        y_{NDC} = \frac{y_{clip}}{w_{clip}} \\
        z_{NDC} = \frac{z_{clip}}{w_{clip}} \\
        1 \\
    \end{bmatrix}
    \And
    \begin{cases}
        \text{OpenGL}  &
        \begin{cases}
            -1 & \leq x_{NDC} & \leq 1 \\
            -1 & \leq y_{NDC} & \leq 1 \\
            -1 & \leq z_{NDC} & \leq 1 \\
        \end{cases}                    \\
        \text{ or }                    \\
        \text{DirectX} & 
        \begin{cases}
            -1 & \leq x_{NDC} & \leq 1 \\
            -1 & \leq y_{NDC} & \leq 1 \\
            1  & \geq z_{NDC} & \geq 0 \\
        \end{cases}                    \\
    \end{cases}
$$

### 6.6. Vertex in shader

#### 6.6.1. Vertex shader output

$$
    SV\underline{\quad}POSITION =
    \begin{bmatrix}
        x_{clip}    \\
        y_{clip}    \\
        z_{clip}    \\
        w_{clip}    \\
    \end{bmatrix}
    = M_P M_V M_M
    \begin{bmatrix}
        x_{object}  \\
        y_{object}  \\
        z_{object}  \\
        1           \\
    \end{bmatrix}
$$

#### 6.6.2. Fragment shader input

$$
    SV\underline{\quad}POSITION =
    \begin{bmatrix}
        (\frac{x_{NDC}}{2} + \frac{1}{2}) \times [\text{output texture width}] + \frac{1}{2}  \\
        (\frac{y_{NDC}}{2} + \frac{1}{2}) \times [\text{output texture height}] + \frac{1}{2} \\
        z_{NDC}                                                                               \\
        w_{clip}                                                                              \\
    \end{bmatrix}
$$

### 6.7. Depth

#### 6.7.1. Write into depth texture

$$
    texture.r =
    \begin{cases}
        \text{OpenGL}  : & \frac{z_{NDC}}{2} + \frac{1}{2}  \\
        \text{ or }                                         \\
        \text{DirectX} : & z_{NDC}                          \\
    \end{cases}
$$

#### 6.7.2. Read from depth texture

$$
    P =
    \begin{cases}
        \text{OpenGL} &
        \begin{cases}
            x = 1 - \frac{f}{n}            \\
            y = \frac{f}{n}                \\
            z = \frac{1}{f} - \frac{1}{n}  \\
            w = \frac{1}{n}                \\
        \end{cases}                        \\
        \text{ or }                        \\
        \text{DirectX} &
        \begin{cases}
            x = -1 + \frac{f}{n}           \\
            y = 1                          \\
            z = -\frac{1}{f} + \frac{1}{n} \\
            w = \frac{1}{f}                \\
        \end{cases}                        \\
    \end{cases}
$$

$$
    {\rm RevZ}(r) =
    \begin{cases}
        \text{OpenGL}  : & r     \\
        \text{ or }              \\
        \text{DirectX} : & 1 - r \\
    \end{cases}
$$

$$
    depth =
    \begin{cases}
        \text{Orthographic} &
        \begin{cases}
            01 &
            \begin{cases}
                \text{from 0} : & \frac{(f-n) \times {\rm RevZ}(r) + n}{f}  \\
                \text{from n} : & {\rm RevZ}(r)                             \\
            \end{cases}                                                     \\
            eye &
            \begin{cases}
                \text{from 0} : & (f-n) \times {\rm RevZ}(r) + n            \\
                \text{from n} : & (f-n) \times {\rm RevZ}(r)                \\
            \end{cases}                                                     \\
        \end{cases}                                                         \\
        \text{Perspective} &
        \begin{cases}
            01 &
            \begin{cases}
                \text{from 0} : & \frac{1}{P.x \times r + P.y}            \\
                \text{from n} : & \frac{1}{P.x + \frac{P.y}{r}}           \\
            \end{cases}                                                     \\
            eye &
            \begin{cases}
                \text{from 0} : & \frac{1}{P.z \times r + P.w}            \\
                \text{from n} : & \frac{1}{P.z \times r + P.w} - n        \\
            \end{cases}                                                     \\
        \end{cases}                                                         \\
    \end{cases}
$$
