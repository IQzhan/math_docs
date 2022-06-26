# Game Math & Algorithm<!-- omit in toc -->

1. [Vector](#vector)
   1. [Dot](#dot)
   2. [Cross](#cross)
   3. [Interpolation](#interpolation)
2. [Matrix](#matrix)
   1. [Identity](#identity)
   2. [Transpose](#transpose)
   3. [Inverse](#inverse)
3. [Transform](#transform)
   1. [Translation](#translation)
   2. [Scale](#scale)
   3. [Euler rotation](#euler-rotation)
      1. [Rotate around x axis by $\theta$](#rotate-around-x-axis-by-theta)
      2. [Rotate around y axis by $\theta$](#rotate-around-y-axis-by-theta)
      3. [Rotate around z axis by $\theta$](#rotate-around-z-axis-by-theta)
   4. [Quaternion](#quaternion)
      1. [Rotate around any axis by $\theta$](#rotate-around-any-axis-by-theta)
      2. [Multiply](#multiply)
      3. [Dot](#dot-1)
      4. [Interpolation](#interpolation-1)
      5. [Matrix](#matrix-1)
   5. [Euler to Quaternion](#euler-to-quaternion)
   6. [Look at](#look-at)

## Vector

$$
    \vec{v} = (x, y, z, w) =
    \begin{bmatrix}
        x \\
        y \\
        z \\
        w \\
    \end{bmatrix}
$$

### Dot

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

### Cross

$$
    \vec{a} \times \vec{b} = (|\vec{a}| |\vec{b}| \sin\theta) \vec{n} \\
    = (y_a z_b - z_a y_b, z_a x_b - x_a z_b, x_a y_b - y_a x_b)
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

### Interpolation

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

## Matrix

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
        & or \\
        \vec{v_0}.w=1 & \vec{v_0} \text{ as point} 
    \end{cases}
$$

### Identity

$$
    M_1 =
    \begin{bmatrix}
        1 & 0 & 0 & 0 \\
        0 & 1 & 0 & 0 \\
        0 & 0 & 1 & 0 \\
        0 & 0 & 0 & 1 \\
    \end{bmatrix}
$$

### Transpose

$$
    M^T =
    \begin{bmatrix}
        c_{00} & c_{01} & c_{02} & c_{03} \\
        c_{10} & c_{11} & c_{12} & c_{13} \\
        c_{20} & c_{21} & c_{22} & c_{23} \\
        c_{30} & c_{31} & c_{32} & c_{33} \\
    \end{bmatrix}
$$

### Inverse

$$
    M M^{-1} = M^{-1} M = M_1
$$

$$
    (M^T)^{-1} = (M^{-1})^T
$$

$$
    (M_A M_B)^{-1} = M_A^{-1} M_B^{-1}
$$

## Transform

$$
    M_{TRS} = M_T M_R M_S \\ =
    \begin{bmatrix}
        M_R.c_{00} \times M_S.c_{00} & M_R.c_{10} \times M_S.c_{11} & M_R.c_{20} \times M_S.c_{22} & M_T.c_{30} \\
        M_R.c_{01} \times M_S.c_{00} & M_R.c_{11} \times M_S.c_{11} & M_R.c_{21} \times M_S.c_{22} & M_T.c_{31} \\
        M_R.c_{02} \times M_S.c_{00} & M_R.c_{12} \times M_S.c_{11} & M_R.c_{22} \times M_S.c_{22} & M_T.c_{32} \\
        0                            & 0                            & 0                            & 1          \\
    \end{bmatrix}
$$

$$
    M_{TRS}^{-1} = M_S^{-1} M_R^{-1} M_T^{-1}
$$

### Translation

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

### Scale

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

### Euler rotation

$$
    M_R = M_{Ra} M_{Rb} M_{Rc} \\
    (a, b, c) \in \{(x,y,z), (x,z,y), (y,x,z), (y,z,x), (z,x,y), (z,y,x)\}
$$

#### Rotate around x axis by $\theta$

$$
    M_{Rx} =
    \begin{bmatrix}
        1 & 0           & 0            & 0 \\
        0 & \cos \theta & -\sin \theta & 0 \\
        0 & \sin \theta & \cos \theta  & 0 \\
        0 & 0           & 0            & 1 \\
    \end{bmatrix}
$$

#### Rotate around y axis by $\theta$

$$
    M_{Ry} =
    \begin{bmatrix}
        \cos \theta  & 0 & \sin \theta & 0 \\
        0            & 1 & 0           & 0 \\
        -\sin \theta & 0 & \cos \theta & 0 \\
        0            & 0 & 0           & 1 \\
    \end{bmatrix}
$$

#### Rotate around z axis by $\theta$

$$
    M_{Rz} =
    \begin{bmatrix}
        \cos \theta & -\sin \theta & 0 & 0 \\
        \sin \theta & \cos \theta  & 0 & 0 \\
        0           & 0            & 1 & 0 \\
        0           & 0            & 0 & 1 \\
    \end{bmatrix}
$$

### Quaternion

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

#### Rotate around any axis by $\theta$

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

#### Multiply

$$
    q_1 = (\vec{u_1}, w_1) = (x_1 i + y_1 j + z_1 k) + w_1
$$

$$
    q_2 = (\vec{u_2}, w_2) = (x_2 i + y_2 j + z_2 k) + w_2
$$

$$
    q_1 q_2 = (w_2 \vec{v_1} + w_1 \vec{v_2} + \vec{v_1} \times \vec{v_2},
    w_1 w_2 - \vec{v_1} \cdot{} \vec{v_2}) \\ =
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
$$

#### Dot

$$
    q_1 \cdot q_2 = |q_1| |q_2| \cos \theta = \cos \theta =
    x_1 x_2 + y_1 y_2 + z_1 z_2 + w_1 w_2
$$

#### Interpolation

$$
    \Delta q = q_2 (q_1^{-1})
$$

$$
    q_t =
    \left.
    \begin{cases}
        {\rm NLerp}(q_1, q_2, t) & \text{if is small} \\
        \text{ or } \\
        {\rm SLerp}(q_1, q_2, t) \\
    \end{cases}
    \right.
    t \in [0, 1]
$$

#### Matrix

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
    \left \{
    \begin{array}{c}
    x = & \frac{c_{21}-c_{12}}{4w} \\ 
    y = & \frac{c_{20}-c_{02}}{4w} \\ 
    z = & \frac{c_{10}-c_{01}}{4w} \\
    w = & \frac{1}{2} \sqrt{1+c_{00}+c_{11}+c_{22}}\\
    \end{array}
    \right.
$$

### Euler to Quaternion

$$
    s_x = \sin{\frac{\theta_x}{2}} \quad
    s_y = \sin{\frac{\theta_y}{2}} \quad
    s_z = \sin{\frac{\theta_z}{2}}
$$

$$
    c_x = \cos{\frac{\theta_x}{2}} \quad
    c_y = \cos{\frac{\theta_y}{2}} \quad
    c_z = \cos{\frac{\theta_z}{2}}
$$

$$
    q_x = s_x i + c_x \quad q_y = s_y j + c_y \quad q_z = s_z k + c_z
$$

$$
    \begin{split}
    q_a q_b q_c =
    (
        &(s_x c_y c_z + sign_1 s_y s_z c_x) i \\ +
        &(s_y c_x c_z + sign_2 s_x s_z c_y) j \\ +
        &(s_z c_x c_y + sign_3 s_x s_y c_z) k \\ +
        &(c_x c_y c_z + sign_4 s_x s_y s_z)
    )
    \end{split}
$$

$$
    (a, b, c, sign_1, sign_2, sign_3) \in
    \left\{
    \begin{array}{c}
        (x, y, z, -1,  1, -1,  1) \\
        (x, z, y,  1,  1, -1, -1) \\
        (y, x, z, -1,  1,  1, -1) \\
        (y, z, x, -1, -1,  1,  1) \\
        (z, x, y,  1, -1, -1,  1) \\
        (z, y, x,  1, -1,  1, -1) \\
    \end{array}
    \right.
$$

### Look at

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