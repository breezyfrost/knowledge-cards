## Vectors
### Vector Addition
*  Geometrically: Parallelogram law & Triangle law
* Algebraically: Simply add coordinates
### Dot(scalar) Product
![Pasted image 20250308210615](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250308210615.png)
$$ \mathbf{a} \cdot \mathbf{b} = |\mathbf{a}| |\mathbf{b}| \cos \theta
 $$
 for unit vectors: $$ \cos \theta = \hat{a} \cdot \hat{b}
 $$
 Properties:
 $$
\mathbf{a} \cdot \mathbf{b} = \mathbf{b} \cdot \mathbf{a}
$$
$$
\mathbf{a} \cdot (\mathbf{b} + \mathbf{c}) = \mathbf{a} \cdot \mathbf{b} + \mathbf{a} \cdot \mathbf{c}
$$
$$
(k\mathbf{a}) \cdot \mathbf{b} = \mathbf{a} \cdot (k\mathbf{b}) = k (\mathbf{a} \cdot \mathbf{b})
$$
in Cartesian Coordinates:
$$ \vec{a} \cdot \vec{b} = \begin{pmatrix} x_a \\ y_a \end{pmatrix} \cdot \begin{pmatrix} x_b \\ y_b \end{pmatrix} = x_a x_b + y_a y_b
 $$
 $$ \vec{a} \cdot \vec{b} = \begin{pmatrix} x_a \\ y_a \\ z_a \end{pmatrix} \cdot \begin{pmatrix} x_b \\ y_b \\ z_b \end{pmatrix} = x_a x_b + y_a y_b + z_a z_b
 $$
[[证明：向量点乘代数意义]]

### Cross(vector) Product
![Pasted image 20250309211759](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250310202658426.png)
$$ |\vec{a} \times \vec{b}| = |\vec{a}| |\vec{b}| \sin \theta
 $$
Properties:
$$
\vec{a} \times \vec{b} = -\vec{b} \times \vec{a}
$$
$$
\vec{a} \times \vec{a} = \vec{0}
$$
$$
\vec{a} \times (\vec{b} + \vec{c}) = \vec{a} \times \vec{b} + \vec{a} \times \vec{c}
$$
$$
\vec{a} \times (k\vec{b}) = k(\vec{a} \times \vec{b})
$$
in Cartesian Coordinates:
$$
\mathbf{a} \times \mathbf{b} =
\begin{pmatrix}
y_a z_b - y_b z_a \\
z_a x_b - x_a z_b \\
x_a y_b - y_a x_b
\end{pmatrix}
$$
because of in Cartesian Coordinates:
$$
\begin{align}
\mathbf{x} \times \mathbf{x} &= \vec{0} \\
\mathbf{y} \times \mathbf{y} &= \vec{0} \\
\mathbf{z} \times \mathbf{z} &= \vec{0} \\
\mathbf{x} \times \mathbf{y} &= \mathbf{z} \\
\mathbf{y} \times \mathbf{x} &= -\mathbf{z} \\
\mathbf{x} \times \mathbf{z} &= \mathbf{y} \\
\mathbf{z} \times \mathbf{x} &= -\mathbf{y} \\
\mathbf{y} \times \mathbf{z} &= -\mathbf{x} \\
\mathbf{z} \times \mathbf{y} &= \mathbf{x} \\
\end{align}
$$
so:
$$
\begin{align}
\mathbf{a} \times \mathbf{b} =&\ \left( x_a \mathbf{x} + y_a \mathbf{y} + z_a \mathbf{z} \right) \times \left( x_b \mathbf{x} + y_b \mathbf{y} + z_b \mathbf{z} \right) \\
=&\ x_a x_b \mathbf{x} \times \mathbf{x} + x_a y_b \mathbf{x} \times \mathbf{y} + x_a z_b \mathbf{x} \times \mathbf{z} \\
&+ y_a x_b \mathbf{y} \times \mathbf{x} + y_a y_b \mathbf{y} \times \mathbf{y} + y_a z_b \mathbf{y} \times \mathbf{z} \\
&+ z_a x_b \mathbf{z} \times \mathbf{x} + z_a y_b \mathbf{z} \times \mathbf{y} + z_a z_b \mathbf{z} \times \mathbf{z} \\
=&\ \left( y_a z_b - z_a y_b \right) \mathbf{x} + \left( z_a x_b - x_a z_b \right) \mathbf{y} + \left( x_a y_b - y_a x_b \right) \mathbf{z}
\end{align}
$$
