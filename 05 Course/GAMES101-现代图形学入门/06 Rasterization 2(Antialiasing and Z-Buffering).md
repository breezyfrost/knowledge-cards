### MSAA(Multisample Anti-Aliasing)
![Pasted image 20250320180452](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250320182246310.png)
Step 1: Take NxN samples in each pixel.
![Pasted image 20250320180638](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250320182246325.png)
Step 2: Average the NxN samples “inside” each pixel.

In rasterizing one triangle, the average value inside a pixel
area of f(x,y) = inside(triangle,x,y) is equal to the area of the
pixel covered by the triangle.
![Pasted image 20250320180751](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250320182246343.png)
![Pasted image 20250320180814](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/20250320182246358.png)