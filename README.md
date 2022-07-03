# Game Math & Algorithm

- [1. Vector](#1-vector)
  - [1.1. Dot](#11-dot)
  - [1.2. Cross](#12-cross)
  - [1.3. Interpolation](#13-interpolation)
- [2. Matrix](#2-matrix)
  - [2.1. Identity](#21-identity)
  - [2.2. Transpose](#22-transpose)
  - [2.3. Inverse](#23-inverse)
- [3. Transform](#3-transform)
  - [3.1. Translation](#31-translation)
  - [3.2. Scale](#32-scale)
  - [3.3. Euler rotation](#33-euler-rotation)
    - [3.3.1. Rotate around x axis](#331-rotate-around-x-axis)
    - [3.3.2. Rotate around y axis](#332-rotate-around-y-axis)
    - [3.3.3. Rotate around z axis](#333-rotate-around-z-axis)
  - [3.4. Quaternion](#34-quaternion)
    - [3.4.1. Rotate around any axis](#341-rotate-around-any-axis)
    - [3.4.2. Multiply](#342-multiply)
    - [3.4.3. Dot](#343-dot)
    - [3.4.4. Interpolation](#344-interpolation)
    - [3.4.5. Matrix](#345-matrix)
  - [3.5. Euler to Quaternion](#35-euler-to-quaternion)
  - [3.6. Look at](#36-look-at)
  - [3.7. Polar Coordinates](#37-polar-coordinates)
    - [3.7.1. In 2D](#371-in-2d)
    - [3.7.2. In 3D](#372-in-3d)
- [4. Render pipeline](#4-render-pipeline)
  - [4.1. Camera](#41-camera)
  - [4.2. Object space to World space](#42-object-space-to-world-space)
  - [4.3. World space to View space](#43-world-space-to-view-space)
  - [4.4. View space to Clip space (Projection)](#44-view-space-to-clip-space-projection)
    - [4.4.1. Orthographic](#441-orthographic)
      - [4.4.1.1. OpenGL](#4411-opengl)
      - [4.4.1.2. DirectX](#4412-directx)
    - [4.4.2. Perspective](#442-perspective)
      - [4.4.2.1. OpenGL](#4421-opengl)
      - [4.4.2.2. DirectX](#4422-directx)
  - [4.5. Clip position to Normalized-Device-Coordinates (NDC)](#45-clip-position-to-normalized-device-coordinates-ndc)
  - [4.6. Vertex in shader](#46-vertex-in-shader)
    - [4.6.1. Vertex shader output](#461-vertex-shader-output)
    - [4.6.2. Fragment shader input](#462-fragment-shader-input)
  - [4.7. Depth](#47-depth)
    - [4.7.1. Write into depth texture](#471-write-into-depth-texture)
    - [4.7.2. Read from depth texture](#472-read-from-depth-texture)

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

### 1.3. Interpolation

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

## 3. Transform

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

### 3.1. Translation

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

### 3.2. Scale

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

### 3.3. Euler rotation

$$
    \begin{array}{c}
        M_R = M_{Ra} M_{Rb} M_{Rc} \\
        (a, b, c) \in \lbrace (x,y,z), (x,z,y), (y,x,z), (y,z,x), (z,x,y), (z,y,x) \rbrace
    \end{array}
$$

#### 3.3.1. Rotate around x axis

$$
    M_{Rx} =
    \begin{bmatrix}
        1 & 0           & 0            & 0 \\
        0 & \cos \theta & -\sin \theta & 0 \\
        0 & \sin \theta & \cos \theta  & 0 \\
        0 & 0           & 0            & 1 \\
    \end{bmatrix}
$$

#### 3.3.2. Rotate around y axis

$$
    M_{Ry} =
    \begin{bmatrix}
        \cos \theta  & 0 & \sin \theta & 0 \\
        0            & 1 & 0           & 0 \\
        -\sin \theta & 0 & \cos \theta & 0 \\
        0            & 0 & 0           & 1 \\
    \end{bmatrix}
$$

#### 3.3.3. Rotate around z axis

$$
    M_{Rz} =
    \begin{bmatrix}
        \cos \theta & -\sin \theta & 0 & 0 \\
        \sin \theta & \cos \theta  & 0 & 0 \\
        0           & 0            & 1 & 0 \\
        0           & 0            & 0 & 1 \\
    \end{bmatrix}
$$

### 3.4. Quaternion

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

#### 3.4.1. Rotate around any axis

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

#### 3.4.2. Multiply

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

#### 3.4.3. Dot

$$
    q_1 \cdot q_2 = |q_1| |q_2| \cos \theta = \cos \theta =
    x_1 x_2 + y_1 y_2 + z_1 z_2 + w_1 w_2
$$

#### 3.4.4. Interpolation

$$
    \Delta q = q_2 (q_1^{-1})
$$

$$
    q_t =
    \left.
    \begin{cases}
        {\rm NLerp}(q_1, q_2, t) & \text{if } \Delta q \text{ is small} \\
                                 & \text{ or }                   \\
        {\rm SLerp}(q_1, q_2, t) & \text{if } \Delta q \text{ is big}   \\
    \end{cases}
    \right.
    t \in [0, 1]
$$

#### 3.4.5. Matrix

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

### 3.5. Euler to Quaternion

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

### 3.6. Look at

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

### 3.7. Polar Coordinates

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

#### 3.7.1. In 2D

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

#### 3.7.2. In 3D

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
        r = \sqrt{x^2 + y^2 + z^2} \\
        \theta = {\rm atan2}(\sqrt{x^2 + y^2}, z) \\
        \phi = {\rm atan2}(y, x)
    \end{cases}
$$

## 4. Render pipeline

### 4.1. Camera

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

### 4.2. Object space to World space

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

### 4.3. World space to View space

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

### 4.4. View space to Clip space (Projection)

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

#### 4.4.1. Orthographic

##### 4.4.1.1. OpenGL

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

##### 4.4.1.2. DirectX

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

#### 4.4.2. Perspective

##### 4.4.2.1. OpenGL

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

##### 4.4.2.2. DirectX

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

### 4.5. Clip position to Normalized-Device-Coordinates (NDC)

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

### 4.6. Vertex in shader

#### 4.6.1. Vertex shader output

$$
    SV\_ POSITION =
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

#### 4.6.2. Fragment shader input

$$
    SV\_ POSITION =
    \begin{bmatrix}
        (\frac{x_{NDC}}{2} + \frac{1}{2}) \times [\text{output texture width}] + \frac{1}{2}  \\
        (\frac{y_{NDC}}{2} + \frac{1}{2}) \times [\text{output texture height}] + \frac{1}{2} \\
        z_{NDC}                                                                               \\
        w_{clip}                                                                              \\
    \end{bmatrix}
$$

### 4.7. Depth

#### 4.7.1. Write into depth texture

$$
    texture.r =
    \begin{cases}
        \text{OpenGL}  : & \frac{z_{NDC}}{2} + \frac{1}{2}  \\
        \text{ or }                                         \\
        \text{DirectX} : & z_{NDC}                          \\
    \end{cases}
$$

#### 4.7.2. Read from depth texture

$$
    zP =
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
                \text{from 0} : & \frac{1}{zP.x \times r + zP.y}            \\
                \text{from n} : & \frac{1}{zP.x + \frac{zP.y}{r}}           \\
            \end{cases}                                                     \\
            eye &
            \begin{cases}
                \text{from 0} : & \frac{1}{zP.z \times r + zP.w}            \\
                \text{from n} : & \frac{1}{zP.z \times r + zP.w} - n        \\
            \end{cases}                                                     \\
        \end{cases}                                                         \\
    \end{cases}
$$
