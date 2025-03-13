## Scale
![Pasted image 20250310202624](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250310202706804.png)
$$
\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} s_x & 0 \\ 0 & s_y \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix}
$$
## Reflection Matrix
![Pasted image 20250310203526](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250310210047460.png)$$
\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} -1 & 0 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix}

$$
## Shear Matrix
 ![Pasted image 20250310204429](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250310210047481.png)
 $$
 \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} 1 & a \\ 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix}
 $$
## Rotate
![Pasted image 20250310210310](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250310210316085.png)
$$
R_\theta = \begin{bmatrix} \cos \theta & -\sin \theta \\ \sin \theta & \cos \theta \end{bmatrix}
$$

## Homogenous Coordinates
The purpose of homogeneous coordinates is to simplify geometric transformations, enabling them to be represented as matrix operations and unifying linear and non-linear transformations.

2D point
 $$ \text{2D point} = \begin{pmatrix} x \\ y \\ 1 \end{pmatrix}
 $$
2D vector
 $$ \text{2D vector} = \begin{pmatrix} x \\ y \\ 0 \end{pmatrix}
 $$
Matrix representation of the translation
$$ 
\begin{pmatrix} x' \\ y' \\ w' \end{pmatrix} = 
\begin{pmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{pmatrix} 
\begin{pmatrix} x \\ y \\ 1 \end{pmatrix} 
= \begin{pmatrix} x + t_x \\ y + t_y \\ 1 \end{pmatrix}
$$
 
 Affine map = linear map + translation
$$ \begin{pmatrix} x' \\ y' \end{pmatrix} = 
\begin{pmatrix} a & b \\ c & d \end{pmatrix} 
\begin{pmatrix} x \\ y \end{pmatrix} + 
\begin{pmatrix} t_x \\ t_y \end{pmatrix}
 $$
Using homogenous coordinates
$$
\begin{pmatrix} x' \\ y' \\ 1 \end{pmatrix} = 
\begin{pmatrix} a & b & t_x \\ c & d & t_y \\ 0 & 0 & 1 \end{pmatrix} 
\begin{pmatrix} x \\ y \\ 1 \end{pmatrix}
$$
