# 3D Ray Tracer

A small ray tracer written in C99 that renders sphere scenes to plain-text PPM images. The implementation is intentionally low-level: camera rays, sphere intersections, lighting, shadows, color conversion, and supersampling are implemented directly in C.

<p align="center">
  <img src="assets/main.png" alt="Ray-traced scene" width="45%" />
  <img src="assets/FS11.png" alt="Ray-traced scene with final renderer" width="45%" />
</p>

## What is implemented

- 3D vector addition, subtraction, scaling, normalization, dot products, distances, and lengths
- ray generation from a pinhole camera through a configurable viewport
- quadratic ray-sphere intersection with nearest positive-hit selection
- nearest-object visibility across multiple spheres
- Lambertian diffuse shading from a point light
- inverse-square light falloff with intensity clamping
- hard shadows using secondary shadow rays
- 3 × 3 supersampling per pixel in the final renderer
- hexadecimal palette parsing and RGB conversion
- ASCII PPM output

The scene format and rendering behavior below reflect the source currently in <code>src/</code>; planned features are intentionally not presented as implemented.

## Source layout

~~~text
3D-Ray-Tracer/
├── src/
│   ├── assg.c       # camera, scene parsing, ray generation, shading, render loop
│   ├── vector.c     # Vec3 operations
│   ├── vector.h
│   ├── spheres.c    # sphere storage and ray-sphere intersection
│   ├── spheres.h
│   ├── color.c      # packed RGB conversion and PPM color output
│   └── color.h
├── assets/          # sample renders
├── *_Testcases/     # assignment/sample inputs and expected outputs
├── Makefile
├── ppmcmp.py        # helper for comparing PPM output
└── viewppm          # PPM viewing helper
~~~

## Build targets

The checked-in Makefile uses GCC with C99, warnings-as-errors, and libm.

~~~bash
git clone https://github.com/muhzain05/3D-Ray-Tracer.git
cd 3D-Ray-Tracer
make
~~~

It defines three executables:

- <code>MS1_assg</code> — milestone-1 build
- <code>MS2_assg</code> — milestone-2 build
- <code>FS_assg</code> — final renderer with 3 × 3 supersampling

## Usage

The renderer accepts an input scene and an output file:

~~~bash
./FS_assg path/to/input.txt output.ppm
~~~

The scene parser expects, in order:

1. image width and height
2. viewport height
3. focal length
4. light position <code>x y z</code> and brightness
5. number of palette colors
6. one hexadecimal color per palette entry
7. background-color index
8. number of spheres
9. one line per sphere: <code>x y z radius color_index</code>

The camera is placed at the origin and the viewport lies along negative z.

## Implementation notes

The final render path traces nine samples per pixel and averages their colors. A hit point is shaded from the surface normal and light direction; a secondary ray is then tested against the sphere set to apply a hard-shadow attenuation factor.

This repository originated as a graphics assignment and is kept public as a compact example of C, geometry, memory management, and a complete rendering pipeline.

## Author

Muhammad Zain Asad — [GitHub](https://github.com/muhzain05)
