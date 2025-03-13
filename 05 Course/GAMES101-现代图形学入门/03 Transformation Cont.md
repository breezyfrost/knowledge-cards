## 3D Transformation
 $$ \text{3D point} = (x,y,z,1)^T $$
$$ \text{3D vector} = (x,y,z,0)^T $$
affine transformations:
$$
\begin{pmatrix} x' \\ y' \\ z' \\ 1 \end{pmatrix} = 
\begin{pmatrix} a & b & c & t_x \\ d & e & f & t_y \\ g & h & i & t_z \\ 0 & 0 & 0 & 1 \end{pmatrix} 
\begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix}
$$
scale:
$$
S(s_x, s_y, s_z) = 
\begin{pmatrix} 
s_x & 0 & 0 & 0 \\ 
0 & s_y & 0 & 0 \\ 
0 & 0 & s_z & 0 \\ 
0 & 0 & 0 & 1 
\end{pmatrix}
$$
translation:
$$
T(t_x, t_y, t_z) = 
\begin{pmatrix} 
1 & 0 & 0 & t_x \\ 
0 & 1 & 0 & t_y \\ 
0 & 0 & 1 & t_z \\ 
0 & 0 & 0 & 1 
\end{pmatrix}
$$
rotation
rotation around x, y, or z-axis
$$
R_x(\alpha) = \begin{pmatrix} 
1 & 0 & 0 & 0 \\
0 & \cos\alpha & -\sin\alpha & 0 \\
0 & \sin\alpha & \cos\alpha & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$
$$
R_y(\alpha) = \begin{pmatrix} 
\cos\alpha & 0 & \sin\alpha & 0 \\
0 & 1 & 0 & 0 \\
-\sin\alpha & 0 & \cos\alpha & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$
$$
R_z(\alpha) = \begin{pmatrix} 
\cos\alpha & -\sin\alpha & 0 & 0 \\
\sin\alpha & \cos\alpha & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$
rodrigues' rotation formula
rotation by angle $\alpha$ around axis  $\mathbf{n}$
$$
R(n, \alpha) = \cos(\alpha) I + (1 - \cos(\alpha)) nn^T + \sin(\alpha) \begin{pmatrix} 
0 & -n_z & n_y \\ 
n_z & 0 & -n_x \\ 
-n_y & n_x & 0 
\end{pmatrix}
$$
[[03-1 罗德里格旋转公式推导]]
## View transformation
### camera
properties:
- Position $\vec{e}$
- Look at / gaze direction $\vec{g}$
- Up direction $\vec{t}$
![Pasted image 20250312204848](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250312204938022.png)
 key observation：
 - if the camera and all objects move together, the "photo" will be the same
 - set $\vec{e}$ to origin, up at Y look at -Z
 - ttansform the objects along with the camera
![Pasted image 20250313094502](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250313100720100.png)

transform the camera by $M_{view}$
$$
M_{view} = R_{view}T_{view}
$$
Translate $\vec{e}$ to origin
$$
T_{\text{view}} = 
\begin{bmatrix}
1 & 0 & 0 & -x_e \\
0 & 1 & 0 & -y_e \\
0 & 0 & 1 & -z_e \\
0 & 0 & 0 & 1
\end{bmatrix}
$$
$R_{view}^{-1}$：X to $\vec{g} \times \vec{t}$，Y to $\vec{t}$, Z to $-\vec{g}$
![Pasted image 20250313095739](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250313100720117.png)
$$
R_{\text{view}}^{-1} = 
\begin{bmatrix}
x_{\hat{g} \times \hat{t}} & x_t & x_{- g} & 0 \\
y_{\hat{g} \times \hat{t}} & y_t & y_{- g} & 0 \\
z_{\hat{g} \times \hat{t}} & z_t & z_{- g} & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$
because $R^-1 = R^T$, so
$$
R_{\text{view}} = 
\begin{bmatrix}
x_{\hat{g} \times \hat{t}} & y_{\hat{g} \times \hat{t}} & z_{\hat{g} \times \hat{t} }& 0 \\
x_t & y_t & z_t & 0 \\
x_{- g} & y_{- g} & z_{- g} & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$
### Projection
Perspective projection(P) vs. orthographic projection
![Pasted image 20250313154751](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250313154800643.png)

Orthographic projection
translate (center to origin) first, then scale (length/width/height to 2)
$$
M_{\text{ortho}} = 
\begin{bmatrix}
\frac{2}{r-l} & 0 & 0 & 0 \\
0 & \frac{2}{t-b} & 0 & 0 \\
0 & 0 & \frac{2}{n-f} & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 & -\frac{r+l}{2} \\
0 & 1 & 0 & -\frac{t+b}{2} \\
0 & 0 & 1 & -\frac{n+f}{2} \\
0 & 0 & 0 & 1
\end{bmatrix}
$$![Pasted image 20250313160000](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250313160006038.png)

perspective projection
- First "squish" the frustum into a cuboid($M_{persp \to ortho}$)
- Do orthographic project($M_{mortho}$)
![Pasted image 20250313182548](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250313182600972.png)
Find the relationship between transformed points $(x', y', z')$ and the original points $(x, y, z)$
![Pasted image 20250313183830](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250313190655459.png)
$$
y' = \frac{n}{z} y \quad x' = \frac{n}{z} x \quad (\text{similar to } y')
$$
so, in homogeneous coordinates:
$$
M_{persp \to ortho}
\begin{pmatrix}
x \\
y \\
z \\
1
\end{pmatrix}
\Rightarrow
\begin{pmatrix}
\frac{n x}{z} \\
\frac{n y}{z} \\
\text{unknown} \\
1
\end{pmatrix}
\overset{\text{mult. by z}}{\Longrightarrow}
\begin{pmatrix}
n x \\
n y \\
\text{still unknown} \\
z
\end{pmatrix}
$$
$$
M_{persp \to ortho} =
\begin{pmatrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
? & ? & ? & ? \\
0 & 0 & 1 & 0 \\
\end{pmatrix}
$$
because on the near or far plane, $z$ will not change:
$$
\begin{pmatrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
? & ? & ? & ? \\
0 & 0 & 1 & 0 \\
\end{pmatrix}
\begin{pmatrix}
x\\
y\\
n\\
1\\
\end{pmatrix}
= 
\begin{pmatrix}
nx\\
ny\\
n^2\\
n\\
\end{pmatrix}
$$
$$
\begin{pmatrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
? & ? & ? & ? \\
0 & 0 & 1 & 0 \\
\end{pmatrix}
\begin{pmatrix}
x\\
y\\
f\\
1\\
\end{pmatrix}
= 
\begin{pmatrix}
fx\\
fy\\
f^2\\
f\\
\end{pmatrix}
$$
so:
$$
M_{persp \to ortho} =
\begin{pmatrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
0 & 0 & A & B \\
0 & 0 & 1 & 0 \\
\end{pmatrix}
$$
$$
\begin{pmatrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
0 & 0 & A & B \\
0 & 0 & 1 & 0 \\
\end{pmatrix}
\begin{pmatrix}
x\\
y\\
n\\
1\\
\end{pmatrix}
= 
\begin{pmatrix}
nx\\
ny\\
n^2\\
n\\
\end{pmatrix}
$$
$$
\begin{pmatrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
0 & 0 & A & B \\
0 & 0 & 1 & 0 \\
\end{pmatrix}
\begin{pmatrix}
x\\
y\\
f\\
1\\
\end{pmatrix}
= 
\begin{pmatrix}
fx\\
fy\\
f^2\\
f\\
\end{pmatrix}
$$
solve for A and B
$$
\begin{aligned}
A n + B &= n^2 \\
A f + B &= f^2
\end{aligned}
\quad \Rightarrow \quad
\begin{aligned}
A &= n + f \\
B &= -n f
\end{aligned}
$$
$$
M_{persp \to ortho} =
\begin{pmatrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
0 & 0 & n+f & -nf \\
0 & 0 & 1 & 0 \\
\end{pmatrix}
$$