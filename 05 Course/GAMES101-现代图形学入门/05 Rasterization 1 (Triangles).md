## Screen
### What is a screen
- An array of pixels
- Size of the array: resolution
- A typical kind of raster display
Raster: screen in German
Rasterize: drawing onto the screen
Pixel: for now, a pixel is a little square with uniform color
Color: mixture for red, green, blue

### Defining the screen space
 - Pixels' indices are in the form of (x, y), where both x and y are integers
 - Pixels' indices are for (0, 0) to (width -1, height -1)
 - Pixel (x, y) is centeed ant (x + 0.5, y + 0.5)
![Pasted image 20250318161420](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250318162509396.png)
 ### Cononical Cube to Screen
 - Irrelevant to z
 - Transform in xy plane: $[-1, 1]^2$ to $[0, width] \times [0, height]$
![Pasted image 20250318161954](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250318162509421.png)

$$
M_{\text{viewport}} = 
\begin{pmatrix}
\frac{\text{width}}{2} & 0 & 0 & \frac{\text{width}}{2} \\
0 & \frac{\text{height}}{2} & 0 & \frac{\text{height}}{2} \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$
## Rasterization: Drawing to Raster Displays
### Triangles
- Most basic polygon
- Break up other polygons
- Guaranteed to be planar
- Well-defined interior
- Well-defined method for interpolating values at vertices over triangle (barycentric interpolation)

### What Pixel Values Approximate a Triangle
![Pasted image 20250318170155](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250318170218051.png)
Sampling

sample if each pixel center is inside triangle.![Pasted image 20250318185446](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250318185554403.png)
bounding box
![Pasted image 20250318185541](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250318185554421.png)