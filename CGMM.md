# Complete Study Notes: Units III, IV and V

These notes cover the supplied syllabus topic by topic, with beginner-friendly explanations, examples, comparisons, important formulas, and exam-relevant points.

- Unit III: Computer Graphics, 3D Rendering and Illumination
- Unit IV: Multimedia Concepts, Digital Media, Compression and Animation
- Unit V: UI/UX Design and Prototyping

Syllabus note: Unit V appears incomplete. The notes cover every topic supplied, ending at High-Fidelity Prototyping. No additional Unit V topics are assumed to be part of your syllabus.

# UNIT III: Computer Graphics and Rendering

## 1. Camera and Viewing Concepts

Computer graphics involves creating and displaying images of objects using a computer. A viewing system determines how objects in a 3D world appear on a 2D screen.

Imagine a 3D game. The player can move through a virtual world, but the monitor displays only a flat image. The camera and viewing system determine what the player sees.

### Important concepts

- Object: An entity in a scene, such as a car, tree or building.
- Scene: The complete collection of objects, lights and other elements.
- Camera: Defines the viewpoint from which the scene is observed.
- View direction: The direction in which the camera looks.
- View plane: The plane on which the scene is projected.
- Projection: The process of converting 3D coordinates into 2D coordinates.
- Viewport: The rectangular area of the screen where the rendered image appears.

### Camera parameters

| Parameter     | Meaning                                        |
| ------------- | ---------------------------------------------- |
| Position      | Where the camera is located                    |
| Target        | The point or direction the camera looks toward |
| Up vector     | Defines which direction is considered upward   |
| Field of view | The angular extent visible to the camera       |
| Near plane    | Closest clipping boundary                      |
| Far plane     | Farthest clipping boundary                     |

Example: In a racing game, moving the camera behind the car changes the viewpoint without changing the car's geometry.

## 2. Viewing Pipeline

The viewing pipeline is the sequence of operations that transforms a 3D object into pixels displayed on a screen.

3D Object Coordinates

Vertices defined relative to the object

Model Transformation

Position, rotate and scale objects

View Transformation

Convert world coordinates to camera coordinates

Projection Transformation

Convert 3D geometry to projected coordinates

Clipping and Viewport Mapping

Remove invisible regions and map to screen space

Rasterization and Display

Convert primitives into pixels

### Stages explained

1. Model transformation: Places an object in the world by translating, rotating or scaling it.
2. View transformation: Expresses the scene relative to the camera.
3. Projection transformation: Determines how depth affects the projected image.
4. Clipping: Removes geometry outside the visible viewing region.
5. Viewport mapping: Converts projected coordinates into screen coordinates.
6. Rasterization: Determines which pixels are covered by triangles and other primitives.
7. Fragment processing and output: Calculates pixel-related properties, tests depth, and writes the final visible result.

The exact order of some GPU operations can vary, but this is the standard conceptual pipeline.

Example: A cube is modeled at the origin, moved five units forward, viewed from a camera, projected onto the screen, clipped if necessary, and finally drawn as pixels.

## 3. Parallel Projection

Parallel projection is a projection method in which projection rays remain parallel to one another.

Objects do not become smaller merely because they are farther away from the camera.

[PPT - Engineering Communications GL2 Geometric modelling Projection Systems PowerPoint Presentation - ID:238241](https://images.openai.com/static-rsc-4/HE150amlA5ttpoDrMCEUhENj-wCHydwIfTulLeiHn7IREMV9NycX3VQIfAxjfkhuxljBs0VzU12E-BVJW5zPbPYeNHXAAP3uvV_kW-Ha4jWYTSJDhf3AYGS6_e2jAm6Asudy7WVAP1O1SCRQNTcUwPNmyi7QrKteRdn-nlIBRzc?purpose=inline)

[slideserve.com](https://www.slideserve.com/Ava/engineering-communications-gl2-geometric-modelling-projection-systems-powerpoint-ppt-presentation)

### Types

Orthographic projection: Projection rays are perpendicular to the view plane. Dimensions parallel to the projection plane retain their scale.

Applications include CAD drawings, engineering diagrams and architectural plans.

Oblique projection: Projection rays strike the view plane at an angle. The front face of an object may retain its shape while depth is shown diagonally.

Examples include cabinet and cavalier projections.

### Advantages

- Preserves useful dimensions and parallel relationships.
- Does not require perspective correction.
- Useful for accurate measurements.

### Disadvantages

- Does not naturally convey realistic depth.
- Distant objects do not appear smaller.

## 4. Perspective Projection

Perspective projection models the way a camera or human eye sees the world. Objects farther away appear smaller.

[File:Vanishing Point (PSF).png - Wikimedia Commons](https://images.openai.com/static-rsc-4/MBOe2fQjWfpOSVcnYT-ADvreJQS_ozsvBF-IMI6VK5oYraRj3J6k5RC-1HOHB1ni9KhFWEbiGVgE1lcZePzZP1uLmJ80sopwg5BjjE_C6y4mVP8dtmspc9HiKJY__Lf3hr93YAB6NFEhovO8GlS4zcLYWU5weJTWBupduEb50iU?purpose=inline)

[commons.wikimedia.org](https://commons.wikimedia.org/wiki/File%3AVanishing_Point_%28PSF%29.png)

### Characteristics

- Parallel lines may converge toward a vanishing point.
- Distant objects appear smaller.
- Depth is conveyed more realistically.
- Apparent size depends on distance and field of view.

For a simplified camera model, projected coordinates can be expressed as:

\\[ x' = f\frac{x}{z},\qquad y'=f\frac{y}{z} \\]

Here, \\(x,y,z\\) are camera-space coordinates and \\(f\\) is the focal-length scale factor. The equation assumes a camera looking along the positive \\(z\\)-axis; other conventions may use negative \\(z\\).

As \\(z\\) increases, the projected size decreases for a fixed object.

### Parallel vs perspective projection

| Feature         | Parallel                                     | Perspective               |
| --------------- | -------------------------------------------- | ------------------------- |
| Projection rays | Parallel                                     | Converge at the camera    |
| Distant objects | Same projected scale for equal-sized objects | Appear smaller            |
| Parallel lines  | Generally remain parallel                    | May converge              |
| Realism         | Lower                                        | Higher                    |
| Common use      | CAD, technical drawings                      | Games, films, photography |

Exam point: Orthographic projection is a type of parallel projection. Perspective projection is generally preferred for realistic 3D scenes.

## 5. Viewport Mapping

Viewport mapping converts coordinates in the projected viewing area into coordinates in a screen region called the viewport.

A normalized coordinate system commonly uses values from 0 to 1, although graphics systems can use different ranges.

For a viewport with lower-left corner \\((x_v,y_v)\\), width \\(W_v\\), and height \\(H_v\\), normalized coordinates \\((u,v)\\) map to:

\\[ x_s=x_v+uW_v \\]

\\[ y_s=y_v+vH_v \\]

This assumes a bottom-left origin and normalized coordinates. Systems using a top-left origin may reverse the vertical mapping.

Example: If \\(x_v=100\\), \\(W_v=800\\), and \\(u=0.5\\), then \\(x_s=100+0.5(800)=500\\).

Viewport mapping is different from projection: projection determines the projected position, while viewport mapping determines where that position lands on the screen.

## 6. 3D Object Representation

3D object representation describes how a computer stores the shape and structure of a three-dimensional object.

### Common methods

1\. Wireframe representation

Stores vertices and edges without filled surfaces.

- Advantage: Simple and lightweight.
- Limitation: Does not directly represent solid surfaces.

2\. Surface representation

Stores the surfaces forming an object's exterior.

Examples include polygon meshes and parametric surfaces.

3\. Solid representation

Represents the object's volume, not just its exterior.

Methods include boundary representation and constructive solid geometry.

4\. Parametric representation

Uses mathematical equations and parameters to define curves or surfaces.

For example, a sphere can be defined using its center and radius.

### Applications

- CAD and engineering.
- 3D modeling and animation.
- Medical visualization.
- Simulation and virtual reality.
- Video games.

## 7. Polygon Meshes

A polygon mesh is a representation of a 3D surface using connected polygons, usually triangles and quadrilaterals.

Its basic components are:

- Vertices: Points in 3D space.
- Edges: Connections between vertices.
- Faces: Polygonal surfaces bounded by edges.
- Normals: Vectors describing the orientation of a surface or vertex.

[Introduction](https://images.openai.com/static-rsc-4/jyFDWD1mfzFX_OrEfG4smzt40xHeZEnR0mopXTjDewXmV6ajigdChIqB6MMAnq2xb-ztrj9bbVplP9elw1aTuZSLZKfwsdAgg6IL5v4SVTPA0wAH_pkeT48wOVsffucdQQV9cxPhCnHHvYiU0vGr-JWIxX-XJU-ZsqwYJIRCD3Y?purpose=inline)

[coloringchaos.github.io](https://coloringchaos.github.io/form-fall-16/intro)

### Example

A cube can be represented by eight corner vertices and six quadrilateral faces, connected by twelve edges.

### Advantages

- Suitable for GPU rendering.
- Efficient for representing complex shapes.
- Can be subdivided to create smoother surfaces.

### Limitations

- Complex shapes may require many polygons.
- Low polygon counts can make curved surfaces look angular.
- Large meshes can consume considerable memory.

Important: Triangles are especially common in real-time rendering because three vertices always define a planar triangle, making it convenient for rasterization.

## 8. Hidden Surface Removal

Hidden surface removal determines which surfaces of a 3D scene are visible from the camera and which are obscured by other objects.

Without it, a renderer might draw the back of a cube over its front face.

### Common techniques

- Back-face culling: Discards polygon faces pointing away from the camera when their orientation and material properties make them irrelevant.
- Z-buffering: Uses depth values to determine which surface is nearest at each pixel.
- Painter's algorithm: Draws distant surfaces before nearer ones.
- BSP trees: Organize space to assist visibility ordering and rendering.

Each technique has different assumptions and limitations. Back-face culling alone cannot determine whether one front-facing surface is hidden behind another.

## 9. Z-Buffer or Depth Buffer

The Z-buffer is a common hidden-surface removal technique that stores a depth value for each pixel or sample.

### Working

1. Initialize the depth buffer to a value representing the farthest possible depth.
2. Render the scene's primitives.
3. For each covered pixel, calculate its depth.
4. Compare that depth with the stored value.
5. If the new fragment is nearer, update the depth buffer and color buffer.
6. If it is farther, discard it.

[PPT - OpenGL PowerPoint Presentation, free download - ID:2023339](https://images.openai.com/static-rsc-4/cFHN5Hh21RKMt_3j_q0RXB5a3s7xXP5lwe9Buw6E8pgIKK0v6qy2hMu2qaX5TEU8gacVXPioTRrfTGxOUEryg9FRUYoAQSv_apDGzwGILhrln7-4f_yRL-QeRBpVZk3pcqTNMCIqiXgWuSlq6V2htRW8TnYo4GUkb8l7EqKBCRM?purpose=inline)

[slideserve.com](https://www.slideserve.com/loren/opengl-powerpoint-ppt-presentation)

### Example

Two triangles cover the same pixel:

- Triangle A has depth 0.3.
- Triangle B has depth 0.7.

If smaller depth values represent nearer surfaces in the chosen depth convention, A wins. Many conventional depth buffers use this arrangement, but reversed-Z systems use the opposite ordering.

### Advantages

- Handles complex overlapping geometry.
- Does not require globally sorting all polygons.
- Works well with GPU hardware.

### Limitations

- Requires memory for depth values.
- Limited depth precision can cause z-fighting, where nearby surfaces flicker or overlap incorrectly.
- Transparency usually requires additional handling.

Memory estimate: At 1920 × 1080 resolution, a 32-bit depth buffer uses approximately \\(1920\times1080\times4=8,294,400\\) bytes, or about 7.9 MiB, excluding additional buffers and alignment overhead.

## 10. Illumination Models

An illumination model describes how light interacts with surfaces to determine their visible color and brightness.

A common introductory model divides lighting into ambient, diffuse and specular components.

\\[ I=I_a+I_d+I_s \\]

Here, \\(I_a\\) is ambient illumination, \\(I_d\\) is diffuse reflection, and \\(I_s\\) is specular reflection. Real renderers may use more complex physically based models.

### 10.1 Ambient Illumination

Ambient illumination approximates general background light in a scene.

It makes surfaces visible even when they are not directly illuminated by a modeled light source.

A simplified equation is:

\\[ I_a=k_a I\_{\text{ambient}} \\]

- \\(k_a\\): Material's ambient coefficient.
- \\(I\_{\text{ambient}}\\): Ambient light intensity.

Example: The shaded side of a wall remains slightly visible instead of becoming completely black.

Limitation: A simple ambient term does not accurately model indirect light bouncing between surfaces.

### 10.2 Diffuse Reflection

Diffuse reflection occurs when light scatters from a rough surface in many directions.

A Lambertian diffuse model is:

\\[ I_d=k_d I_l\max(0,\mathbf{N}\cdot\mathbf{L}) \\]

- \\(k_d\\): Diffuse reflectivity of the material.
- \\(I_l\\): Light intensity.
- \\(\mathbf{N}\\): Unit surface normal.
- \\(\mathbf{L}\\): Unit vector toward the light.

The dot product measures how directly the surface faces the light.

Example: A matte white wall appears brightest when facing a light and darker when tilted away.

### 10.3 Specular Reflection

Specular reflection produces highlights on surfaces such as polished metal, plastic and glass.

In the classic Phong illumination model, the specular term is:

\\[ I_s=k_s I_l\max(0,\mathbf{R}\cdot\mathbf{V})^n \\]

- \\(k_s\\): Specular coefficient.
- \\(\mathbf{R}\\): Reflected light direction.
- \\(\mathbf{V}\\): Direction toward the viewer.
- \\(n\\): Shininess exponent.

A larger \\(n\\) produces a smaller, sharper highlight in this model.

Example: A polished car body shows a bright reflection of a light source, unlike a matte surface.

### Comparison of illumination components

| Component | Purpose                              | Typical example                  |
| --------- | ------------------------------------ | -------------------------------- |
| Ambient   | Approximate general background light | Slight visibility in shadows     |
| Diffuse   | Matte surface brightness             | Chalk or an unpolished wall      |
| Specular  | Directional highlight                | Glossy plastic or polished metal |

## 11. Flat Shading

Flat shading calculates lighting for each polygon face and applies the resulting color across the entire face.

Each polygon appears uniformly shaded.

[File:Shading models.png - Wikimedia Commons](https://images.openai.com/static-rsc-4/er2i0ZP9nJ_0eGFpDGHzPKZeAdr561HIb3mH0IVcscMRD5wSWL6Uw4liLLdwtTB7o-8WCNL8Znd91jGfTgpXyqNJ2YduuNAr5jUp7xectc-e5rN4H2Vx-fqsnIWUFVfxm7LjpXt6Lk3xukp5xiFDZY4FF4WEfxqZM8cLcCquy9k?purpose=inline)

[commons.wikimedia.org](https://commons.wikimedia.org/wiki/File%3AShading_models.png)

### Advantages

- Computationally inexpensive.
- Makes polygon boundaries visually clear.
- Useful for low-poly art.

### Disadvantages

- Curved objects can appear faceted.
- Adjacent polygons may show abrupt brightness changes.

Example: A low-polygon rock with distinct triangular faces.

## 12. Gouraud Shading

Gouraud shading calculates lighting at the vertices of a polygon and interpolates the resulting colors across its surface.

### Steps

1. Estimate or calculate the lighting at each vertex.
2. Determine the vertex colors.
3. Interpolate colors across polygon edges.
4. Interpolate across the interior to shade the surface.

### Advantages

- Smoother appearance than flat shading.
- Usually less expensive than per-pixel Phong shading.
- Suitable for many simple scenes.

### Disadvantages

- Small specular highlights can be missed if they do not occur near a vertex.
- Interpolating colors is less accurate than calculating lighting at each pixel.

## 13. Phong Shading

Phong shading interpolates surface normals across a polygon and computes the illumination at each pixel or fragment.

It should not be confused with the Phong illumination model, which is the lighting equation involving ambient, diffuse and specular terms. Phong shading is a technique for applying lighting across a surface.

### Advantages

- Smooth surfaces.
- Better representation of specular highlights than Gouraud shading.
- Useful for curved objects.

### Disadvantages

- Requires more calculations than simple Gouraud shading.
- Depends on interpolated normals and the chosen lighting model.

### Shading comparison

| Feature                   | Flat                    | Gouraud       | Phong                            |
| ------------------------- | ----------------------- | ------------- | -------------------------------- |
| Lighting calculated at    | Face                    | Vertices      | Pixels or fragments              |
| Interpolated quantity     | No smooth interpolation | Color         | Surface normal                   |
| Appearance                | Faceted                 | Smooth        | Smooth                           |
| Small specular highlights | Limited                 | May miss them | Better represented               |
| Relative cost             | Low                     | Moderate      | Higher in the classic comparison |

## 14. Texture Mapping

Texture mapping applies an image or other texture data to a 3D surface.

Instead of representing every tiny surface detail with geometry, a renderer uses texture information to change the appearance of the surface.

[UV Coordinates | What Make Art](https://images.openai.com/static-rsc-4/gie6aByvT-Du_prtB-JGFvoLL-XNOdBOZiKZwUugLipT_wVvSml921keQjPbu6LBCHOHSE7PL30gKQ74YEWiYbKPTwAOrzu5JMNs1kkk3o2umn5Pvt2LfRE-_nblnFtYBHLeBDyeTy5uk2hfbNJtVjTcVD279lBtwuFrmrMvuKw?purpose=inline)

[whatmakeart.com](https://whatmakeart.com/3d-modeling/uv-coordinates/)

### Common texture maps

- Color or albedo map: Stores surface color.
- Normal map: Alters the apparent surface orientation used for lighting.
- Roughness map: Controls the spread of specular reflection in many modern materials.
- Metallic map: Describes which parts behave like metals in a metallic-roughness workflow.
- Opacity or alpha map: Controls transparency or cutout regions.
- Displacement map: Can modify actual surface geometry when supported.

### Advantages

- Adds detail without requiring equally detailed geometry.
- Reduces modeling effort.
- Supports realistic and stylized materials.

Example: A brick wall can be modeled as a relatively simple plane, with a brick texture providing the visual pattern.

## 15. UV Mapping

UV mapping defines how a 2D texture is positioned on a 3D object's surface.

The letters U and V represent the two texture-coordinate axes. They are used instead of X and Y to distinguish texture coordinates from object-space coordinates.

Each vertex can carry UV coordinates that identify a location in the texture.

### Working

1. Create or unwrap the 3D object's surface.
2. Assign UV coordinates to vertices.
3. Sample the texture using those coordinates.
4. Apply the sampled values when rendering.

[Master UV Unwrapping and Texturing in Blender: The Ultimate Guide | Auxo Digital](https://images.openai.com/static-rsc-4/QLHMDJgME4cYhVKa5J60EDyMDlTwKq1IxtuZKU0wK5eP3IqyLCsFjMComg9uh4z5WYHsjonVZ2AhUOTMMl2kgkDqbtytYMHHP03s42qyr3MM1EufahT8QZfX0MmkUoonNaaLwxC_l6f2wcCbIcmckfTwuAXddfPwpIgYLwsSXko?purpose=inline)

[test.auxo.digital](https://test.auxo.digital/a-comprehensive-guide-to-unleash-the-power-of-uv-unwrapping-in-blender/)

### Common problems

- Stretching: Texture details become distorted.
- Seams: Texture discontinuities appear where UV islands meet.
- Overlapping UVs: Multiple surface regions share texture space, intentionally or unintentionally.
- Poor resolution: Texture details look blurry when enlarged.

Example: A cube's six faces can be mapped to six squares of a single image.

Texture mapping is the general process of applying texture data; UV mapping is one common way to specify the correspondence between the surface and the texture.

## 16. Materials and Surface Appearance

A material defines how a surface interacts with light and how it is visually represented.

A material may contain:

- Base color or albedo.
- Roughness.
- Metallic value.
- Specular reflectance.
- Transparency or opacity.
- Emission.
- Normal or bump information.

### Material examples

| Material         | Typical appearance                |
| ---------------- | --------------------------------- |
| Matte plastic    | Broad, soft highlights            |
| Polished metal   | Strong reflections                |
| Wood             | Surface color and grain           |
| Glass            | Transmission and reflections      |
| Rubber           | Usually rough, subdued highlights |
| Emissive surface | Appears to emit light             |

Modern real-time engines commonly use Physically Based Rendering (PBR). PBR aims to use material parameters and lighting calculations that behave consistently with physical observations.

A metallic-roughness material workflow often uses:

- Base color.
- Metallic value.
- Roughness value.

Important: A material that appears bright is not necessarily an actual light source. Emission and illumination of surrounding surfaces are separate concepts.

## 17. Rasterization

Rasterization is the process of converting geometric primitives, such as triangles, into fragments that correspond to potential screen pixels.

A GPU commonly renders a 3D scene by rasterizing triangles.

### Working of rasterization

1. Primitive assembly: Vertices are combined into triangles or other supported primitives.
2. Clipping: Portions outside the viewing volume are removed.
3. Projection: Geometry is transformed into screen-related coordinates.
4. Rasterization: Determines which pixel samples are covered by each primitive.
5. Fragment shading: Calculates color and other properties for fragments.
6. Depth and stencil tests: Determine whether fragments pass the relevant tests.
7. Framebuffer output: Writes the resulting values to the appropriate buffers.

[CS184/284A: Lecture Slides](https://images.openai.com/static-rsc-4/BVwSMvYZfV6mD5-v7Weu0R2dMDizF9emY2NG2m1ZxR062CUt0MLnZcZsVQlBY9bY38taf3mWparLj7qSjqRmSaYiJxJLSq9FKttSDfg85HRqaQ39Y1MuKTzpwNvtcaYDwty3HogD00m5IcHw7CQjIBdAhyQwsT8rSCeS-u3rqOI?purpose=inline)

[cs184.eecs.berkeley.edu](https://cs184.eecs.berkeley.edu/sp24/lecture/6-41/the-rasterization-pipeline)

### Example

A triangle is defined by three vertices. Rasterization determines which pixels it covers, and the fragment shader calculates their colors.

### Advantages

- Highly efficient for rendering large numbers of triangles.
- Supported by specialized GPU hardware.
- Well suited to interactive applications.

### Limitations

- Traditional rasterization does not automatically calculate all physical light paths.
- Realistic reflections, shadows and indirect lighting may require additional algorithms.

## 18. Ray Tracing

Ray tracing simulates the paths of rays through a scene to determine what light reaches a camera or sensor.

In basic ray tracing, a ray is cast from the camera through a pixel into the scene. The renderer finds the nearest intersection and calculates the resulting color.

[DoC students recieve Computer Graphics design awards | Imperial News | Imperial College London](https://images.openai.com/static-rsc-4/7ZlbDIyNBDzvF2jBprn5szucpkcV32diADg_TPG4ztxzDbDEfSLYnAXv2kSgwbBi_V9kmLpeLtt4qqgow0HdzAq_7Lw7AeF-6S1iYtS46xyFAfHi1mCMnRYKiaTVWgIRbCoyGkwAmWAQHK3GUqAZ94OmidkN47EyDyPRQRKLoQk?purpose=inline)

[imperial.ac.uk](https://www.imperial.ac.uk/news/195458/doc-students-recieve-computer-graphics-design/)

### Basic algorithm

1. Generate a ray from the camera through a pixel.
2. Find the nearest object intersected by the ray.
3. Determine the intersection point and surface normal.
4. Calculate direct illumination and shadows.
5. Optionally trace reflection, refraction or indirect-light rays.
6. Combine the contributions to produce the pixel's color.

### Types of rays

- Primary rays: Travel from the camera into the scene.
- Shadow rays: Test whether a light source is blocked.
- Reflection rays: Follow reflected directions.
- Refraction rays: Follow transmitted light through transparent materials.

### Advantages

- Produces convincing reflections and shadows.
- Can model refraction and indirect illumination.
- Supports realistic lighting effects.

### Disadvantages

- Can be computationally expensive.
- Complex scenes may require many ray-object intersection tests.
- Noise can appear in stochastic or path-traced images when sampling is insufficient.

### Ray tracing vs rasterization

| Feature        | Rasterization                             | Ray tracing                                           |
| -------------- | ----------------------------------------- | ----------------------------------------------------- |
| Basic approach | Projects and fills geometry               | Traces rays and tests intersections                   |
| Performance    | Generally very fast                       | Often more computationally expensive                  |
| Reflections    | Often approximated or supplemented        | Can calculate ray-based reflections                   |
| Shadows        | Commonly use shadow maps or other methods | Can use shadow rays                                   |
| Typical use    | Real-time games and interfaces            | Offline rendering and increasingly real-time graphics |

Modern rendering engines frequently combine rasterization with ray tracing rather than relying on only one method.

## 19. Real-Time Rendering

Real-time rendering generates images quickly enough for interactive applications. The image is updated as the user moves, interacts or changes the scene.

Examples include video games, simulators, virtual reality and interactive 3D product viewers.

### Frame rate

Frame rate is the number of frames displayed per second, measured in FPS.

\\[ \text{Frame time}=\frac{1000}{\text{FPS}}\text{ ms} \\]

| Frame rate | Time available per frame |
| ---------- | ------------------------ |
| 30 FPS     | 33.33 ms                 |
| 60 FPS     | 16.67 ms                 |
| 120 FPS    | 8.33 ms                  |
| 144 FPS    | 6.94 ms                  |

At 60 FPS, the renderer needs to sustain approximately one completed frame every 16.67 milliseconds.

### Techniques used

- Level of detail (LOD).
- Frustum and occlusion culling.
- Texture compression.
- Shadow maps.
- Deferred or forward rendering.
- Temporal anti-aliasing.
- GPU parallel processing.
- Resolution scaling.

### Applications

- Video games.
- Flight and driving simulators.
- Augmented and virtual reality.
- Architectural visualization.
- Interactive medical visualization.

## 20. GPU-Based Rendering

A Graphics Processing Unit (GPU) is a processor designed to execute large numbers of graphics and parallel computations efficiently.

Unlike a CPU, which is optimized for a broad range of tasks and complex control flows, a GPU is particularly effective when many similar operations can be performed simultaneously.

### GPU rendering pipeline

Vertex Data and Buffers

Vertex Shader

Primitive Assembly and Rasterization

Fragment Shader

Depth Tests and Framebuffer

This is a simplified view of a common programmable graphics pipeline.

### Main components

- Vertex shader: Processes vertices and transforms their positions.
- Rasterizer: Converts primitives into fragments.
- Fragment shader: Calculates fragment properties such as color.
- Depth buffer: Helps resolve visibility.
- Framebuffer: Stores output color and possibly depth, stencil and other values.
- GPU memory: Stores geometry, textures, buffers and rendering resources.

### Advantages

- Parallel processing of vertices and fragments.
- High throughput for graphics operations.
- Supports interactive 3D scenes and advanced visual effects.

### Limitations

- GPU memory is finite.
- Transferring data between CPU and GPU can be costly.
- Performance depends on memory bandwidth, shader complexity and the rendering workload.

### Unit III: Quick revision

| Topic                  | Remember this                                    |
| ---------------------- | ------------------------------------------------ |
| Viewing pipeline       | Converts scene geometry into a displayed image   |
| Parallel projection    | Projection rays remain parallel                  |
| Perspective projection | Apparent size decreases with distance            |
| Viewport mapping       | Maps projected coordinates to screen coordinates |
| Polygon mesh           | Uses vertices, edges and faces                   |
| Hidden surface removal | Determines visible surfaces                      |
| Z-buffer               | Stores depth per pixel or sample                 |
| Ambient illumination   | Approximation of general background light        |
| Diffuse reflection     | Matte surface response                           |
| Specular reflection    | Directional highlights                           |
| Flat shading           | Lighting per face                                |
| Gouraud shading        | Lighting at vertices, interpolated colors        |
| Phong shading          | Interpolated normals, lighting per pixel         |
| Texture mapping        | Applies texture data to a surface                |
| UV mapping             | Maps surface locations to 2D texture coordinates |
| Rasterization          | Converts primitives into fragments               |
| Ray tracing            | Calculates visibility and light paths using rays |
| Real-time rendering    | Produces frames quickly for interaction          |
| GPU rendering          | Uses parallel hardware for graphics computations |

# UNIT IV: Multimedia

## 1. Introduction to Multimedia

Multimedia is the integration of two or more forms of media, such as text, images, audio, video and animation, to communicate information.

A multimedia application may allow users to view content, listen to narration, interact with controls and explore information.

Example: An educational application that combines written explanations, diagrams, recorded narration, animations and quizzes is a multimedia system.

### Major media elements

- Text.
- Graphics and images.
- Audio.
- Video.
- Animation.
- Interactivity.

## 2. Characteristics of Multimedia

### 2.1 Integration

Different media types are combined into one presentation.

Example: A video lecture includes a presenter, slides, narration and captions.

### 2.2 Interactivity

Users can influence the presentation through actions such as clicking, typing or selecting options.

Example: An interactive map allows users to zoom and select locations.

### 2.3 Digital representation

Digital multimedia stores information as binary data, allowing computers to process, copy and transmit it.

### 2.4 Non-linear navigation

Users can move between sections without following a fixed sequence.

Example: A website lets users jump directly to a topic through a navigation menu.

### 2.5 Synchronization

Related media must be coordinated in time.

Example: Video frames and spoken dialogue must remain synchronized.

### 2.6 Large storage and bandwidth requirements

Audio, images and video can require significant storage space and network bandwidth.

### 2.7 User engagement

Combining different media can make information easier to explore and understand when each element serves a clear purpose.

## 3. Applications of Multimedia

| Field         | Application                                      |
| ------------- | ------------------------------------------------ |
| Education     | E-learning, simulations, interactive lessons     |
| Entertainment | Films, games, music videos                       |
| Business      | Presentations, product demonstrations            |
| Healthcare    | Medical training, imaging visualization          |
| Communication | Video conferencing, social media                 |
| Advertising   | Digital advertisements and interactive campaigns |
| Engineering   | CAD visualization and simulation                 |
| Tourism       | Virtual tours and interactive maps               |
| E-commerce    | Product galleries and demonstration videos       |

## 4. Multimedia System Components

A multimedia system includes hardware and software for capturing, processing, storing, transmitting and presenting media.

### Hardware

- CPU and GPU.
- RAM and storage.
- Microphones and audio interfaces.
- Cameras and scanners.
- Speakers and headphones.
- Displays and projectors.
- Network interfaces.

### Software

- Image editors.
- Audio recording and editing software.
- Video editors.
- Animation tools.
- Media players.
- Codecs and encoding software.
- Streaming and content delivery software.

### Example

To create an instructional video, a user records footage with a camera, captures narration through a microphone, edits both using video software, exports a compressed video and distributes it through a streaming platform.

## 5. Multimedia Architecture

Multimedia architecture describes how media data moves through a system from capture or storage to processing, delivery and playback.

A typical conceptual architecture is:

Media Input and Capture

Encoding and Processing

Storage or Network Delivery

Decoding and Synchronization

Playback and User Interaction

### Explanation of components

1. Capture: Acquires images, sound or video.
2. Encoding: Converts media into a suitable digital representation and may compress it.
3. Storage: Saves media files or segments.
4. Transmission: Transfers media across a network when required.
5. Decoding: Reconstructs the media representation for playback.
6. Synchronization: Coordinates audio, video, captions and other timed elements.
7. Presentation: Displays or plays the media.

Not every multimedia system uses a network. An offline presentation may store and play all its resources locally.

## 6. Multimedia Authoring

Multimedia authoring is the process of creating an application or presentation by combining media resources, arranging their timing and layout, and defining user interactions.

### Main stages

1. Planning and defining objectives.
2. Preparing a storyboard or content outline.
3. Collecting or creating media assets.
4. Editing and organizing assets.
5. Adding navigation and interaction.
6. Testing and correcting errors.
7. Publishing or exporting the final project.

### Types of authoring tools

Card or page-based tools: Organize information into pages or screens.

Example: An interactive educational presentation.

Icon or event-based tools: Use events and actions to control how content behaves.

Example: A presentation that starts an animation when a user clicks a button.

Timeline-based tools: Arrange media on tracks according to time.

Examples: Video editors and animation software.

### Authoring vs editing

Editing focuses on modifying individual media assets. Authoring combines assets and often defines the overall structure and behavior of the final multimedia experience.

## 7. Text in Multimedia

Text communicates labels, instructions, descriptions, captions and detailed information.

### Types of text

- Plain text: Unformatted characters.
- Formatted text: Includes fonts, colors, size, boldness and alignment.
- Hypertext: Text containing links to other content.
- Captions and subtitles: Text synchronized with audio or video.

### Important design principles

- Use readable fonts.
- Establish a clear heading hierarchy.
- Maintain sufficient contrast.
- Avoid excessive decorative typography.
- Keep paragraphs and labels easy to scan.
- Use captions when needed for accessibility.

### Example

A tutorial video may include a title, section headings, on-screen instructions and subtitles. Each text element should be readable and timed appropriately.

## 8. Graphics in Multimedia

Graphics are visual elements used to represent objects, concepts, data or decoration.

### Types

Raster graphics: Images made of pixels.

Examples: Photographs, screenshots and scanned pictures.

Vector graphics: Images represented using mathematical paths and shapes.

Examples: Logos, icons and diagrams.

### Comparison

| Feature         | Raster                          | Vector                     |
| --------------- | ------------------------------- | -------------------------- |
| Structure       | Pixels                          | Paths and geometric shapes |
| Scaling         | Can become pixelated            | Usually remains sharp      |
| Best suited for | Photographs and detailed images | Logos, icons and diagrams  |
| Typical formats | JPEG, PNG, WebP                 | SVG                        |

Vector graphics can contain raster images, and some raster formats can store additional metadata, but the distinction above describes their primary representation.

## 9. Digital Images

A digital image is a representation of visual information in a digital format.

### Pixels

A pixel is a picture element, usually representing a sample of color or intensity at a particular location in an image.

A 1920 × 1080 image contains:

\\[ 1920\times1080=2,073,600\text{ pixels} \\]

This is approximately 2.07 megapixels.

### Resolution

Resolution commonly refers to the dimensions of an image in pixels.

A higher pixel count can preserve more detail, provided that the original image contains that detail.

### Color depth

Color depth indicates how many bits are used to represent a pixel or its channels.

For an RGB image with 8 bits per channel:

\\[ 2^8=256\text{ levels per channel} \\]

\\[ 256^3=16,777,216\text{ possible RGB colors} \\]

This is commonly called 24-bit RGB color.

### Image storage estimate

For an uncompressed RGB image:

\\[ \text{Size in bytes}= \frac{W\times H\times C}{8} \\]

Here, \\(W\\) and \\(H\\) are image dimensions and \\(C\\) is bits per pixel.

For a 1920 × 1080 image with 24 bits per pixel:

\\[ \frac{1920\times1080\times24}{8} =6,220,800\text{ bytes} \\]

This is approximately 5.93 MiB before accounting for headers, padding and other storage details.

## 10. Image File Formats

Different image formats serve different purposes. Some preserve pixels directly, some compress image data, and others store vector instructions.

[Download Stunning 4K HDR Mountain Landscape Wallpaper | Wallpapers.com](https://images.openai.com/static-rsc-4/vkyPT0jzGKNpcCD5PadGcVfN_F4ARjVgFktnKf0z_r6-TR79A4ywob981knL8Hv2meyZTyEctvkBS1u7GmLcBZt9muYx8DNZXHC4xejh5nCjWsZ5KqTwVEIgEzKLFAJU1in5ymKN3RduC7AB3SE539zav-GqKEboN_MsEA6XiUE?purpose=inline)

JPEG or JPG

A raster format commonly using lossy compression. It is effective for photographs and continuous-tone images.

- Advantages: Often small file sizes.
- Limitations: Repeated lossy editing or saving may introduce artifacts; transparency is not part of the conventional JPEG format.

[如何为 JPG 图片添加透明度 - 完整教程 | ImgConvert](https://images.openai.com/static-rsc-4/HhWAaiQ6_zbgPWsQ7LYsQMUUXkYjD85lPRTIFewU0nkjvHr1RMYR2QQcr6GCtmGAa3ekbbiQ4h32UP9iE9bKgATf9OuaROWQLPIrCKFQ5JHIRSyTZdPmgjVortLVtdMBgr5yNovwOxxD4IUi4W5sQE1D4sxJuLk2yM1xVk1AxEE?purpose=inline)

PNG

A raster format using lossless compression.

- Advantages: Supports transparency and preserves pixel values.
- Limitations: Photographs may produce larger files than JPEG.
- Common use: Screenshots, interface assets and logos.

[Shantae Video Games GIF - Shantae Video Games Dance - Discover & Share GIFs](https://images.openai.com/static-rsc-4/NiOarEhjXUuln0jgalYJb20CF5wXt1XguBLpfhX1mc0xM5eRwM400wy0LJzjMGho6TyFbG6tN91t2YUfH2TGBXryioxxJGrD4C2lSrgVzzBbF_kd-LBJxugRskVBYGczGtCZLEhzl6yaagSgVaNKmTTixtliJURj-ydCgbOpyAQ?purpose=inline)

GIF

Supports indexed color, basic transparency and simple frame-based animation.

- Advantages: Broad support for short animations.
- Limitations: Limited palette of up to 256 colors per frame, making it less suitable for photographic images.

[C41 Developing + Scan  – Triple D Minilab Centre](https://images.openai.com/static-rsc-4/bgCD8lh2AzdT4b9jg7eCMeu5gsmMtLED-CG7ecXPaHg8TZlbLvvRT3cuR-srzOPKPJE8-XRjsqHrc3BkJHNe_EQU5jnOZ8W6QEBA-z8desiSnFvtspwMYXHFfDc8oD7B4OmYXR61ZwxXC3Aymf07LJhtG4faIC-jEM1c5vEE-qA?purpose=inline)

TIFF

A flexible raster image format used in scanning, publishing and archival workflows.

- Advantages: Supports high-quality image data and several compression options.
- Limitations: Files can be large, and support varies across applications.

[A prism logo Images - Free Download on Magnific (formerly Freepik)](https://images.openai.com/static-rsc-4/v06QzsJStJBNMZ8oEk38kM4E9VZOfW85Zrdeknq4sPxg7x_4-mNFbWKvcQiRbPbMKIOdxKqFJW5ODLWEuOKRYtqeLTajR4RUVbK3a_N-kmNIwh5roTkrl_dlE_aWnPHLs2e4lGjMeod71CbXgJVoxLr0c__z2aDR0cJ_cNoS898?purpose=inline)

SVG

An XML-based vector graphics format.

- Advantages: Scales cleanly and can contain text and geometric shapes.
- Limitations: Complex filters or very detailed artwork may be expensive to render; SVG is not ideal as the primary format for ordinary photographs.

[JPEG vs PNG vs WebP：该选哪种图片格式？ - Zipic 博客](https://images.openai.com/static-rsc-4/oqYFY4cplbzypG6qKWot_rx3lO7-06rGVuNtC6AqSjaAnA9tOd8kV3VCCL7D73zyZLE04aVoq-mHsxOMIRtxXu-ENi6OWkMSaA-m-0xsWvZRH4AEpyweeGrjSuKpS-AU9Nc4xVlL-7jTc2e5yfqNKMVYis5GqNQ1pXezCRQs_4Q?purpose=inline)

WebP

A web-focused image format supporting lossy and lossless compression, as well as transparency and animation.

- Advantages: Can provide efficient file sizes for web delivery.
- Limitations: Compatibility and feature support should be checked for older software or workflows.

### Which format should you choose?

| Requirement                       | Suitable choice                 |
| --------------------------------- | ------------------------------- |
| Photographs with small file sizes | JPEG or lossy WebP              |
| Transparent interface graphics    | PNG or WebP                     |
| Simple animated images            | GIF or animated WebP            |
| Scalable logos and icons          | SVG                             |
| Archival or print workflows       | TIFF, depending on requirements |
| Screenshots with sharp text       | PNG is often suitable           |

## 11. Digital Audio

Digital audio represents sound as numerical samples that a computer can store, process and reproduce.

A microphone converts sound pressure variations into an electrical signal. An analog-to-digital converter then samples and quantizes the signal.

### Important terms

- Amplitude: Magnitude of the signal, related to perceived loudness.
- Frequency: Number of oscillations per second, measured in hertz (Hz).
- Sampling rate: Number of audio samples taken per second.
- Bit depth: Number of bits used to represent each sample.
- Channels: Separate audio streams, such as left and right stereo channels.

## 12. Sampling

Sampling measures an analog signal at regular intervals to produce discrete-time values.

The sampling rate determines how many samples are taken per second.

For example:

- 8,000 Hz means 8,000 samples per second.
- 44,100 Hz means 44,100 samples per second.
- 48,000 Hz means 48,000 samples per second.

### Nyquist sampling principle

For an ideal band-limited signal, the sampling rate must exceed twice the highest frequency present to allow reconstruction under the sampling theorem.

\\[ f_s>2f\_{\max} \\]

Here, \\(f_s\\) is the sampling rate and \\(f\_{\max}\\) is the highest signal frequency.

In practice, audio systems use anti-aliasing filters before sampling.

Example: A 48 kHz audio system can represent frequencies up to below 24 kHz under ideal sampling conditions, with practical limitations determined by filtering and system design.

### Aliasing

Aliasing occurs when frequencies above the supported range are not properly removed before sampling. These frequencies may appear as incorrect lower frequencies in the digital signal.

## 13. Quantization

Quantization maps each sampled amplitude to one of a finite set of representable values.

If a sample uses \\(n\\) bits, the number of possible levels is:

\\[ L=2^n \\]

Examples:

- 8-bit: 256 possible levels.
- 16-bit: 65,536 possible levels.
- 24-bit: 16,777,216 possible levels.

Increasing bit depth can reduce quantization error and increase the representable dynamic range.

For ideal uniform quantization, each additional bit provides approximately 6 dB of additional theoretical signal-to-quantization-noise ratio under standard assumptions.

Difference to remember: Sampling discretizes time, while quantization discretizes amplitude.

## 14. Audio Bit Rate

Bit rate is the number of bits processed or transmitted per second.

For uncompressed PCM audio:

\\[ \text{Bit rate}= f_s\times b\times c \\]

where:

- \\(f_s\\) = sampling rate.
- \\(b\\) = bits per sample.
- \\(c\\) = number of channels.

### Example

For stereo audio at 44,100 Hz with 16 bits per sample:

\\[ 44,100\times16\times2 =1,411,200\text{ bits/s} \\]

Therefore, the uncompressed bit rate is 1,411.2 kbps.

For compressed audio, the actual bit rate depends on the codec and encoding settings, not just the sampling rate and bit depth.

## 15. Audio Channels

Audio channels represent separate audio signals.

- Mono: One channel.
- Stereo: Two channels, commonly left and right.
- 5.1 surround: Six channels, conventionally including five main channels and one low-frequency effects channel.
- 7.1 surround: Eight channels in a common configuration.

### Applications

- Mono: Telephone calls and voice announcements.
- Stereo: Music, headphones and general media playback.
- Surround: Films, games and home theater systems.

A stereo file is not necessarily higher quality than a mono file. Quality also depends on the recording, encoding and playback system.

## 16. Audio File Formats

| Format     | Typical characteristics                                                 | Common use                       |
| ---------- | ----------------------------------------------------------------------- | -------------------------------- |
| WAV        | Container commonly used for PCM audio; can also support other encodings | Recording and editing            |
| AIFF       | Audio file format commonly used with PCM                                | Professional audio workflows     |
| MP3        | Lossy compressed audio                                                  | Music distribution               |
| AAC        | Lossy compressed audio codec, often stored in MP4 or related containers | Streaming and mobile media       |
| FLAC       | Lossless compressed audio                                               | Archiving and high-quality music |
| Ogg Vorbis | Lossy audio codec commonly carried in an Ogg container                  | Music and open-media workflows   |
| Opus       | Efficient lossy codec suited to speech and music                        | Calls and streaming              |

### Important distinction

A container stores and organizes media streams and metadata. A codec encodes and decodes audio or video.

For example, MP4 is a container that can carry AAC audio and H.264 video.

## 17. Audio Compression

Audio compression reduces the number of bits needed to store or transmit sound.

### Lossless compression

Reconstructs the original digital audio data exactly.

Examples: FLAC and lossless audio stored in supported WAV workflows.

Advantages:

- Preserves the original data.
- Useful for archiving and editing.

Disadvantages:

\- Usually achieves less size reduction than lossy compression for perceptual audio.

### Lossy compression

Removes or approximates information to reduce file size.

Examples: MP3 and AAC.

Advantages:

- Smaller files.
- More efficient delivery over limited bandwidth.

Disadvantages:

- Cannot reconstruct the original audio samples exactly.
- Excessive compression may introduce audible artifacts.

### Example

An audio file encoded at a lower bit rate will often be smaller than the same content encoded at a higher bit rate, but it may also have lower perceptual quality.

## 18. Digital Video

Digital video is a sequence of images displayed over time, usually accompanied by audio and timing information.

Each image is called a frame.

### Key concepts

- Resolution.
- Frame rate.
- Bit rate.
- Color representation.
- Codec.
- Container.
- Compression.

### Example

A video recorded at 1920 × 1080 and 30 FPS contains frames with 2,073,600 pixels each, with 30 frames displayed per second.

The actual file size depends on color representation, compression, bit rate, duration and audio content.

## 19. Video Resolution

Video resolution specifies the number of pixels in each frame.

| Common label    | Resolution                                   |
| --------------- | -------------------------------------------- |
| 480p            | 854 × 480 approximately for widescreen video |
| 720p            | 1280 × 720                                   |
| 1080p           | 1920 × 1080                                  |
| 1440p           | 2560 × 1440                                  |
| 2160p or 4K UHD | 3840 × 2160                                  |

The 480p dimensions may vary with aspect ratio and format.

### Effects of resolution

Higher resolution can preserve more spatial detail, but it usually increases processing and storage requirements when other parameters remain comparable.

Important: Resolution alone does not determine image quality. Compression, focus, source quality, color depth and display size also matter.

## 20. Frame Rate

Frame rate is the number of video frames displayed per second, measured in FPS.

Common frame rates include 24, 25, 30, 50, 60 and 120 FPS.

### Effects

- Lower frame rates can create a more staccato appearance during rapid movement.
- Higher frame rates can produce smoother motion.
- Higher frame rates usually require more data or more compression for comparable frame quality.

### Applications

- 24 FPS: Common in film presentation.
- 25 FPS: Common in some broadcast workflows.
- 30 FPS: Common in online video.
- 60 FPS: Often used for sports, games and smoother motion.
- 120 FPS: Useful for high-frame-rate playback and slow-motion capture workflows.

These are common uses, not strict rules.

## 21. Video Bit Rate

Video bit rate measures how many bits are used to represent the video stream per second.

It is commonly measured in Mbps.

A higher bit rate often allows better image quality at the same resolution and frame rate, but the result depends on the codec, encoder and content.

### Approximate file size

\\[ \text{File size in bytes}\approx \frac{\text{Bit rate in bits/s}\times\text{duration in seconds}}{8} \\]

This estimates file size for a stream with a constant bit rate and excludes container overhead and other streams.

Example: A 5 Mbps video lasting 10 minutes:

\\[ \frac{5,000,000\times600}{8} =375,000,000\text{ bytes} \\]

That is approximately 375 MB in decimal units for the video stream alone.

If audio and other streams are present, their sizes must also be included.

## 22. Video File Formats

| Format  | Characteristics                                                           |
| ------- | ------------------------------------------------------------------------- |
| MP4     | Widely used container for online video and general playback               |
| MKV     | Flexible container supporting multiple tracks and subtitles               |
| MOV     | Container associated with Apple QuickTime and editing workflows           |
| AVI     | Older Microsoft multimedia container                                      |
| WebM    | Web-oriented container commonly used with VP9 or AV1 video and Opus audio |
| MPEG-TS | Transport stream used in broadcasting and streaming workflows             |

The file extension identifies the container or file format, not necessarily the codec inside it.

## 23. Video Codecs

A video codec encodes and decodes video data, often using compression.

| Codec        | Important characteristics                                                  |
| ------------ | -------------------------------------------------------------------------- |
| H.264 / AVC  | Widely compatible; used for streaming, recording and distribution          |
| H.265 / HEVC | Designed for improved compression efficiency over H.264 in many situations |
| VP9          | Open video codec used in web video workflows                               |
| AV1          | Modern royalty-free video codec designed for efficient compression         |
| MPEG-2 Video | Older codec used in DVD and broadcast applications                         |

### Codec vs container

Consider an MP4 file containing H.264 video and AAC audio.

- MP4 is the container.
- H.264 is the video codec.
- AAC is the audio codec.

A container can support multiple codecs, depending on the format specification and implementation.

## 24. Video Compression

Video compression reduces the amount of data required to represent moving images.

Video codecs commonly use several techniques.

### Spatial compression

Reduces redundancy within an individual frame.

Example: Encoding similar neighboring pixel values efficiently.

### Temporal compression

Reduces redundancy between frames.

Example: Instead of storing an entire unchanged background in every frame, an encoder may represent the changes from a reference frame.

### Transform coding

Transforms blocks or regions into coefficients that can be represented efficiently.

### Motion estimation and compensation

Estimates movement between frames and uses that information to encode changes.

### Intra and inter frames

- Intra-coded frames: Coded largely from information within the frame.
- Inter-coded frames: Use information from other frames, such as motion-compensated prediction.

### Advantages

- Smaller files.
- Lower network bandwidth requirements.
- More practical storage and distribution.

### Limitations

- Encoding and decoding require computation.
- Excessive compression can create blocking, ringing, blur or motion artifacts.
- Inter-frame compression may increase editing complexity and seeking requirements.

## 25. Lossy and Lossless Compression

| Feature                          | Lossy                                     | Lossless                         |
| -------------------------------- | ----------------------------------------- | -------------------------------- |
| Original data recovered exactly? | No                                        | Yes                              |
| Typical size reduction           | Often greater                             | Often smaller                    |
| Common applications              | Streaming video, MP3, JPEG                | Archiving, PNG, FLAC             |
| Quality loss                     | Possible                                  | No loss of original encoded data |
| Best when                        | Small files and efficient delivery matter | Exact reconstruction matters     |

Important: Lossless does not mean uncompressed. PNG and FLAC use compression while preserving the original data they encode.

Some media formats support both lossy and lossless modes, so the format name alone does not always establish which method was used.

## 26. Multimedia Streaming

Streaming allows media to be played while data is delivered, rather than requiring the complete file to be downloaded first.

Examples include online video platforms, live broadcasts and internet radio.

### Types

On-demand streaming: Content can be started, paused and resumed at the viewer's convenience.

Live streaming: Content is captured and delivered while an event is taking place.

### Common streaming workflow

1. Capture audio and video.
2. Encode the streams.
3. Divide content into segments when using a segmented streaming protocol.
4. Deliver the content over a network.
5. Buffer upcoming media.
6. Decode and play it.
7. Adapt quality when supported and needed.

### Adaptive bitrate streaming

Adaptive bitrate streaming provides multiple encoded versions of content. A client selects an appropriate version based on available bandwidth, device capability and buffer conditions.

Common technologies include HLS and MPEG-DASH.

### Buffering

A buffer stores some media in advance of playback.

- A larger buffer may help withstand temporary network slowdowns.
- A smaller buffer may reduce startup delay.
- Buffering cannot compensate indefinitely for a network that is persistently slower than the media's delivery requirements.

### Challenges

- Network variability.
- Latency.
- Packet loss.
- Synchronization.
- Device compatibility.
- Bandwidth limits.

## 27. Principles of Animation

Animation creates the appearance of movement by displaying a sequence of changing images or states.

The classic animation principles help make motion clear, convincing and expressive.

[ANIMATION PRINCIPLES](https://images.openai.com/static-rsc-4/7tTXVQVOtKRrCA7oNycKRVux50akyallKRcqBIIoC4BluAT4kGA-TzTChFhKb1qayP2l7jaFUQmuWACyjOaWaLxKVoW5w4LeAl2uZLlNyeDCc3ufwNLp89EActC82XyyLeYzrMRmxRzCN4vXhZkjcm0pPWd8FGfQQUP2e9xs6to?purpose=inline)

1\. Squash and Stretch

Objects deform to communicate flexibility, impact and weight while maintaining a convincing overall volume.

Example: A bouncing ball squashes on impact and stretches while moving quickly.

[PPT - 11 Prinsip Animasi PowerPoint Presentation, free download - ID:3820421](https://images.openai.com/static-rsc-4/27gUoUtINFCQu7qMz14UZ3kipeG19nq_0FQAVUzvDbfEf2Z3XHKOYrivAU2LHEcfszUWEaXLAxMoFNali-bfp2DfIAKToyRQZwXnR3En-_iZpGgB_eoZMhUlqw0aN1HgF1nRQ62-WvKXnQo0zb0RJKEiFtMKE3Z7o2nWDgweSO0?purpose=inline)

2\. Anticipation

A preparatory action signals what is about to happen.

Example: A character bends their knees before jumping.

[PPT - Animation Principles The following are a set of principles to keep in mind: 1. Squash and stretch PowerPoint Presentation - ID:5245324](https://images.openai.com/static-rsc-4/L0Tm1D32tYaayxZPvu856zYFZ_7dMgMNJ74hBmuhoiqFGBcue8NJXhgQdNaNTb23AvaG0VoNnHpLrw8F1hHZEr1OsyHE-SXt6UJDfUhESEwpcpm67b_TXZ9gNZLRyPw6zjvNX-koDWH_E5HqAOtqhsy_dv8HXw5ohXaKm-OqBYY?purpose=inline)

3\. Staging

Arranges the scene so that the main action or idea is easy to understand.

Example: A character's important gesture is placed clearly within the frame.

[Straight Ahead, Pose to Pose, and Mixed Animation // Study from The Animator’s Survival Kit](https://images.openai.com/static-rsc-4/bMlWmaPhIPhYiZqhY40_iPgEXPpYfuLUhcSYtzerJm24N7D9q6elhO27q_dDuEYXzZQPs_hEdY2-9XQuOoAJcHFccafpaH6E6FITo5f8zhDPmaBkkQ467m2aPbDEE6xBApUJfoPDPy5h8HVVb6deR18SbH-LV1b3pPE7zzF5dm0?purpose=inline)

4\. Straight Ahead Action and Pose to Pose

Straight-ahead animation creates movement sequentially, while pose-to-pose animation plans major poses and fills the transitions between them.

[05 - Follow Through & Overlapping action by Dan Stack on Dribbble](https://images.openai.com/static-rsc-4/DVusXpjc5NSANL8E_vgRkk-gqAq_SJmvE0EGoH9SRSZXYR-59C_TJf-sQqLsvgP9zR-ikW-Lda302ELucgfJDeaTnrHf6VMEV_35PgqOp1DOScNlaBE4WagevzN7rGX2NbGW3SgGL107T_HFClVowG1ytW_2YXD6wjTZRv2XjVo?purpose=inline)

5\. Follow Through and Overlapping Action

Different body parts continue or change movement at different times.

Example: A character stops running, but their hair and loose clothing continue moving briefly.

[12 Nguyên Tắc Animation - Nguyên Tắc 6: Slow In & Slow Out](https://images.openai.com/static-rsc-4/a0K0bVFYf18n0t3wp8U7bqZtyD4rqIGo0ZRBaV8tfH1NZWmQZGOIatlc_hZYb7Z_UTUWPxRarIAyT0PEN6VeEoF3RSX_VO4LaXKs_DMLwfgjtjtAuGjGTIyhptNZ9SL-82yor5o96i-JKPlL4E-3P_ODPzwuVul2a7vYYLP3tvc?purpose=inline)

6\. Slow In and Slow Out

Movement accelerates or decelerates gradually rather than always beginning and ending abruptly.

[ANIMATION PRINCIPLES](https://images.openai.com/static-rsc-4/p_FMLZkq1v0n6g6ICt6KHSdud6JNzL2gpDCLFTiDuoQidSW0dCt4IKR93S23i619_r7YiOUb6Ed3qVYEgyRE57n4jBc2O0sREA8rMccG37l9v9PAj4OpCZLPSDk-jwnsJ0zDtPi7jSyETSkcbRK8RP5hjwp9j3peNLLpwv05B6c?purpose=inline)

7\. Arcs

Many natural movements follow curved paths.

Example: A hand swings in an arc when a character waves.

[Animation: Secondary Action](https://images.openai.com/static-rsc-4/xqKDknj_z9om0syWmaeKdeL_aUJf5SRVJSLWi0D3SVHuPfZ0CD5MiPokVlRJwwqdU39nb0iBSXm_hc55AaWWjgGXacMaXo7oNZM_jfYVZuDWOib57u8n3MLDe0hhnFoduRLJtZbqotP6vUzC5Hzkio1iDJyP7YriOAqKxJgaikI?purpose=inline)

8\. Secondary Action

Supporting actions add personality without distracting from the main action.

Example: A walking character also whistles.

[Adobe Animate: Bouncing ball tutorial – MMP260-1301, Introduction to 2D Animation, Spring 2019](https://images.openai.com/static-rsc-4/JSo1rvT3JuyYxd7E7bLhrDbEov9e7kku3wAaSBLO_aT-2LChRXcPxotwT0DA6DI3Nc1LDVhcNlm5znI8BoDGZWGQe2RDBNxyVlLpNh10ZbLns1BOL1RHSnGG2DJ-M9Q8lviIHleDJlDPPIC401bdNRIjo3i_ZBCa_Kfgjsq1hmM?purpose=inline)

9\. Timing

The number and spacing of frames determine the pace, perceived weight and character of movement.

[bulb](https://images.openai.com/static-rsc-4/11bzRyqKtDsloldqjLHiYOc-hB0EOlgrGN2EVkftou57mrmeOwzLGADYb0JB8Xi123s8E5ej4aIpCdO9GwlrkiYOh3jF7J396VuN6YqnnaHhuRMHo1OsTx9RVaVEjPkL_138FwUGTmVrCLDagI5sn9m8ZQipDsWTt2jYQmzmF88?purpose=inline)

10\. Exaggeration

Actions or expressions are emphasized to improve clarity and impact.

[教主某狐仙(Kitune)的東方美學角色實戰，掌握動態立繪與商業三視圖完整流程！ | Coloso.](https://images.openai.com/static-rsc-4/XRajmL5WjYF8UWMsON4hGlNfzZc-XrrMgQGSr0m5T1WdIQdPfPibrndTAL0-ShY7v08MU-Q7xshdx3Pg_W1KYC4oZAbqsnITrlCBJ03OFyIPgNN3q8v2so1AFsND3nvb441zA6MX43ZQUjupFaV1iXnkEKFc_VVq7q3J9hBKLnI?purpose=inline)

11\. Solid Drawing

Drawings should communicate volume, balance, anatomy and three-dimensional form.

[12 Principles of Animation](https://images.openai.com/static-rsc-4/NVLaAm5Ovvzxxd8Qj6cAjyiQuHcaij2LhpSeHJBKGYf_v7WNdVSLs_UKAusqUQuGei5IM44eFipn517OGquCq0L4PhrGHRsfhxoaI7n-iMqpRDkx5G-iAgx787tAmzYZLEby_vslYwfgvNOOVR0GwLglfagI-aQV5SsHC0bIsJM?purpose=inline)

12\. Appeal

Characters and actions should be visually engaging and easy to read. Appeal does not necessarily mean making every character beautiful.

These principles originated in traditional character animation, but many also apply to 3D animation, motion graphics and interface animations.

## 28. Keyframe Animation

Keyframe animation defines important states of an object at selected times. The software calculates or helps create the transitions between those states.

### Example

Suppose a ball moves horizontally from position \\(x=0\\) at time \\(t=0\\) to position \\(x=100\\) at time \\(t=2\\) seconds.

For constant-speed linear interpolation:

\\[ x(t)=x_0+\frac{t-t_0}{t_1-t_0}(x_1-x_0) \\]

At \\(t=1\\) second, the ball reaches \\(x=50\\).

This example assumes linear motion. Easing curves can change how the object accelerates and decelerates.

### Properties that can be animated

- Position.
- Rotation.
- Scale.
- Opacity.
- Color.
- Camera parameters.
- Lighting.
- Material properties.

### Advantages

- Precise control over important moments.
- Easy to revise major poses.
- Supports complex motion through interpolation.

### Limitations

- Realistic motion can require many carefully chosen keyframes.
- Poor interpolation may create unnatural movement.

## 29. Motion Graphics

Motion graphics combines graphic design and animation to communicate information or create visual interest.

Common elements include text, shapes, icons, diagrams, logos and animated transitions.

### Examples

- Animated titles.
- Logo reveals.
- Infographics.
- Data visualizations.
- Explainer videos.
- Animated user-interface elements.

### Benefits

- Makes abstract information easier to follow.
- Directs attention.
- Adds visual interest to presentations and videos.

### Motion graphics vs character animation

Motion graphics focuses mainly on animated graphic elements and communication, whereas character animation often emphasizes performance, expression, personality and movement.

The categories can overlap.

## 30. Visual Effects

Visual effects (VFX) involve creating or modifying imagery to produce results that would be difficult, expensive or impossible to capture directly.

### Common techniques

- Compositing.
- Green-screen keying.
- Motion tracking.
- Rotoscoping.
- Particle simulation.
- Digital environments.
- CGI objects.
- Color matching and grading.

### Examples

- Adding a digital creature to a live-action film.
- Replacing a sky.
- Creating explosions.
- Removing unwanted objects.
- Combining footage from multiple cameras.

### VFX vs CGI

CGI means computer-generated imagery. It may refer to a 3D model, animation or rendered image.

VFX is the broader process of creating or modifying visual elements in a production. VFX may include CGI, but it also includes techniques applied to filmed footage.

## 31. Green-Screen Technology

Green-screen technology allows a selected background color to be removed or replaced during post-production.

The technique is commonly called chroma keying.

[Pantalla verde: técnica clave en postproducción audiovisual - Universidad ORT Uruguay](https://images.openai.com/static-rsc-4/OKumxEIl2JGak6liOhhGU4NewJL_XO4Ah2PanT7xNRcglvc8pd7-o4zK5JzyBGjj1dvoFSex8QoMobKbTyNFKwWFTojEb247uH6zVCig7YX0H59Lf_CvbtNVUQqTbXEOj0ToAOqAVVXM5ZTvYlQ6_kk-xb-G06KB2wHD59hosKc?purpose=inline)

[fc.ort.edu.uy](https://fc.ort.edu.uy/blog/pantalla-verde-en-postproduccion-audiovisual)

### Working process

1. Record the subject in front of a green background.
2. Import the footage into compositing or editing software.
3. Select the green color range.
4. Create a transparency mask for the selected background.
5. Refine edges and remove green spill.
6. Place a replacement background behind the subject.
7. Match lighting, color and perspective.

### Why green?

Digital cameras often record green with strong sensitivity, and green can be separated effectively from many skin tones and costumes. Blue screens are also used when green is unsuitable.

### Advantages

- Replaces real locations with virtual backgrounds.
- Supports fantasy and science-fiction scenes.
- Reduces the need to film at every location.

### Common problems

- Green spill on skin, hair or clothing.
- Uneven background lighting.
- Shadows on the screen.
- Motion blur and fine hair edges.
- Green objects that should remain visible.

Example: A presenter is filmed in a studio and composited in front of a virtual classroom.

## 32. Overview of Multimedia Authoring Tools

### 32.1 Audacity

[Audacity | Muse Group](https://images.openai.com/static-rsc-4/X2TFvxBVppXgcYHNYeeZ4OPtBAc6HJYaGTVST42ycrgUanA6JgI6_BexqL6r6Pw7LCFNfXhmxRZ2nzttnSxJeq0V9yfiGC6Nibxy_tOZ7GswWOGz_nvq7ihVIkIB3MAlrcYPFAJfVgEGm1cdI49gtfx1EiXzGNwMghi2Zh0xuDI?purpose=inline)

Audacity

Audacity is audio recording and editing software used to capture, edit and process sound.

Features

- Record audio.
- Edit waveforms.
- Remove or reduce unwanted noise using available tools.
- Apply effects.
- Export audio to supported formats.

Applications: Podcasts, voiceovers, basic music editing and educational narration.

### 32.2 DaVinci Resolve

[7 Best 4K Video Editing Software to Use in 2026](https://images.openai.com/static-rsc-4/fqgADlpCeOiH2lJOKYhO4Kt9Jj-12KwpZL3mzARuNfkrhWbQCUhoK5c5aDbY7CHQoTgmJCJGVfsYyiCvei_GEazuKPbVrvx_0i8QAx9vJrv2a8wxIfISJ_mSsiRBNGlj_MHnXGyGz2e195xrS-63pCg1g62u67NkuFCjdY7Za_0?purpose=inline)

DaVinci Resolve

DaVinci Resolve is a video post-production application.

Features

- Video editing.
- Color correction and grading.
- Audio post-production.
- Visual effects and compositing through its Fusion tools.
- Export and delivery controls.

Applications: Short films, tutorials, promotional videos and professional post-production.

### 32.3 CapCut

[How to Write an Advertising Script That's Seen & Remembered?](https://images.openai.com/static-rsc-4/2lJIb1Nmsikd25-XivVFCMNLzKExBM5RvMkOuDkQh55O5sLntf9vgq8YOlK-tQKEPukJkbSGXinihi7ZSi0Opycxmszc63lZuPtgxEeR1-VhODpSrBiqlmcIVsexYyfZnsFEj2bMGyHLlh56xS0dBQUNmL3vnfiV6CQ7T8-mVKY?purpose=inline)

CapCut

CapCut is a video editing tool oriented toward accessible editing and social-media content.

Features

- Timeline-based editing.
- Titles and transitions.
- Captions and text overlays.
- Filters and effects.
- Audio and music integration.

Applications: Short-form videos, reels, social-media posts and quick promotional edits.

### 32.4 Adobe Premiere Pro

[Adobe Learn - A következő megtanulása Premiere Pro Try basic video editing techniques](https://images.openai.com/static-rsc-4/NqJfpyzKbvd_4rmtakzQpvnJ-Y3U8eOWYSvtVAMaBE5IiDTbdS1FRsIaActc0SsdLO8NVbA8sRbOwk3Rg8IPpc5gsM7kyG9Fuc6jZPddWxnw7lQm83rMkasw0ci3DM3kFkKC0N4obrMOltR2DbX5D-KcBQkjlDckq7t6blI59O0?purpose=inline)

Adobe Premiere Pro

Adobe Premiere Pro is a professional timeline-based video editing application.

Features

- Multitrack editing.
- Transitions and effects.
- Audio mixing tools.
- Color correction.
- Captions and titles.
- Integration with other creative applications.

Applications: Films, interviews, documentaries, advertisements and online video.

### 32.5 Blender Animation

[Blender İndir - Ücretsiz İndir - Tamindir](https://images.openai.com/static-rsc-4/80hV2hkgBc0_fh8DeiN9x4_0dpZ4jNdL1vnxuxp9vJ2PMXV6IqmKwxyn4yqOocKZUx4m0DgsNV0QwZWd-lVI8TUhPBEoXViZvPN77kwNumjIYMUsGm64IoXD46FRsW905qIXpbSmPsL4j4Di-vRiYqum-oWAsdfLAwHrCSa3k1M?purpose=inline)

Blender

Blender is a 3D creation application that supports modeling, animation, rendering and compositing.

Features

- 3D modeling.
- Materials and textures.
- Keyframe and rig-based animation.
- Camera and lighting setup.
- Rendering.
- Visual effects and compositing.

Applications: Animated films, game assets, product visualization, motion graphics and 3D scenes.

### Tool comparison

| Tool               | Primary purpose            | Typical output                   |
| ------------------ | -------------------------- | -------------------------------- |
| Audacity           | Audio editing              | Audio files                      |
| DaVinci Resolve    | Video post-production      | Edited video                     |
| CapCut             | Accessible video editing   | Social-media and general videos  |
| Adobe Premiere Pro | Professional video editing | Edited video                     |
| Blender            | 3D creation and animation  | 3D animation and rendered scenes |

### Unit IV: Quick revision

- Multimedia integrates text, images, audio, video, animation and interaction.
- Multimedia architecture describes capture, processing, storage, delivery and playback.
- Multimedia authoring combines assets into an organized presentation or application.
- Raster graphics use pixels; vector graphics use geometric descriptions.
- Sampling discretizes time; quantization discretizes amplitude.
- Audio bit rate depends on sampling rate, bit depth and channel count for uncompressed PCM.
- A video codec compresses and reconstructs video; a container organizes streams.
- Lossy compression sacrifices some original information; lossless compression preserves the encoded data exactly.
- Streaming enables playback while media is being delivered.
- Keyframes define important animation states.
- Chroma keying replaces a selected background color.
- Audacity is audio-focused, Premiere Pro and DaVinci Resolve are video-focused, CapCut is accessible video editing software, and Blender focuses on 3D creation and animation.

# UNIT V: UI/UX Design and Prototyping

Syllabus coverage: All supplied topics are included. The provided Unit V syllabus ends at High-Fidelity Prototyping and appears incomplete.

## 1. Introduction to UI/UX

UI and UX are closely related concepts in the design of websites, mobile applications, desktop software and other interactive systems.

### 1.1 User Interface (UI)

A User Interface is the part of a system through which users interact with it.

It includes visual and interactive elements such as:

- Buttons.
- Text fields.
- Navigation menus.
- Icons.
- Images.
- Colors and typography.
- Dialog boxes.
- Layouts and controls.

Example: In a banking app, the buttons for checking your balance, transferring money and viewing transactions are part of its UI.

### 1.2 User Experience (UX)

User Experience refers to the overall experience a person has while using a product or service.

It includes:

- Ease of use.
- Efficiency.
- Learnability.
- Accessibility.
- User satisfaction.
- Error prevention and recovery.
- The user's feelings and perceptions during the interaction.

Example: If a banking app allows a user to transfer money quickly, clearly explains transaction status and prevents accidental transfers, it provides a better UX.

### UI vs UX

| Feature        | UI                                   | UX                                              |
| -------------- | ------------------------------------ | ----------------------------------------------- |
| Full form      | User Interface                       | User Experience                                 |
| Main focus     | Interface and interaction elements   | Overall user journey and experience             |
| Concerned with | Layout, colors, typography, controls | Usability, task completion, satisfaction        |
| Example        | Design of a payment button           | Making the payment process clear and reliable   |
| Relationship   | Contributes to UX                    | Includes UI and other aspects of the experience |

Important: UI is a part of UX, but UX includes more than visual design. A beautiful interface can still provide poor UX if it is confusing or unreliable.

## 2. User-Centered Design

User-Centered Design (UCD) is a design approach that places users, their needs, abilities and limitations at the center of the development process.

Instead of designing based only on assumptions, designers study users and evaluate designs with them.

### Principles of User-Centered Design

1. Understand users: Identify the intended audience, their skills, goals and limitations.
2. Understand the context: Determine where, when and why users will use the product.
3. Involve users: Collect feedback and observe actual behavior.
4. Iterative design: Improve the design through repeated evaluation.
5. Evaluate usability: Test whether users can complete tasks effectively.
6. Consider accessibility: Ensure the product can be used by people with diverse abilities.

### UCD process

Understand Users and Their Needs

Define Requirements

Design Solutions

Evaluate with Users

Refine and Repeat

This is an iterative cycle, not necessarily a one-way process. Testing may reveal a need to revisit requirements or redesign an earlier stage.

### Example: Designing a college attendance app

A user-centered approach would involve:

1. Asking students and faculty about attendance-related tasks.
2. Identifying requirements, such as viewing attendance percentage and subject-wise records.
3. Creating wireframes.
4. Testing whether students can find attendance information.
5. Improving navigation based on observed difficulties.

### Advantages

- Better usability.
- Fewer unnecessary features.
- Reduced user confusion.
- Earlier identification of design problems.
- Better alignment with actual user needs.

## 3. Design Thinking

Design Thinking is a human-centered approach to solving problems through research, idea generation, experimentation and testing.

A commonly used model has five stages.

1\. Empathize

Understand users' experiences, needs and difficulties through interviews, observation and research.

Example: Interview students about the problems they face while checking attendance.

2\. Define

State the problem clearly using insights from research.

Example: Students need a quick way to identify subjects where their attendance is below the required threshold.

3\. Ideate

Generate multiple possible solutions before selecting one.

Example: Consider color-coded attendance indicators, alerts, a dashboard and subject filters.

4\. Prototype

Build a simple representation of the proposed solution.

Example: Create a clickable attendance dashboard using a prototyping tool.

5\. Test

Ask users to try the prototype and observe their difficulties.

Example: Check whether students can identify low-attendance subjects without assistance.

### Important characteristics

- Human-centered.
- Iterative.
- Encourages creative alternatives.
- Uses prototypes to test assumptions.
- Treats feedback as an opportunity to improve.

### Design Thinking vs User-Centered Design

Design Thinking is a problem-solving approach often organized around empathizing, defining, ideating, prototyping and testing.

User-Centered Design is a broader design philosophy and process that consistently prioritizes users throughout development.

They overlap considerably and are often used together.

## 4. Usability

Usability measures how effectively, efficiently and satisfactorily specified users can achieve specified goals in a particular context.

### Main dimensions of usability

1\. Effectiveness: Can users complete the task correctly?

Example: A user successfully submits a form.

2\. Efficiency: How much time and effort does task completion require?

Example: A user completes a purchase in a few clear steps.

3\. Satisfaction: Is the experience comfortable and acceptable to users?

Example: Users find the checkout process clear rather than frustrating.

Other important considerations include learnability, memorability and error prevention.

### Usability metrics

Common metrics include:

- Task completion rate.
- Time on task.
- Error rate.
- Number of attempts.
- User satisfaction ratings.
- Abandonment rate.

For example, if 18 out of 20 participants successfully complete a task:

\\[ \text{Completion rate}=\frac{18}{20}\times100=90\\% \\]

The metric is meaningful only when the task, participant group and success criteria are clearly defined.

### Usability principles

- Maintain consistency.
- Provide clear feedback.
- Prevent errors where possible.
- Use understandable language.
- Keep navigation predictable.
- Make important actions easy to find.
- Provide clear recovery options.

### Example

A login form should identify missing required fields, explain incorrect credentials without exposing sensitive information, and provide a clear password-recovery path.

## 5. Accessibility

Accessibility means designing digital products so that people with different abilities can perceive, understand, navigate and interact with them.

Accessibility benefits people with permanent disabilities, temporary impairments and situational limitations.

### Types of accessibility considerations

Visual accessibility

- Sufficient text contrast.
- Screen-reader support.
- Alternative text for meaningful images.
- Resizable text.
- Avoiding color as the only way to communicate information.

Auditory accessibility

- Captions for video.
- Transcripts for audio content.
- Visual alternatives to important sound alerts.

Motor accessibility

- Keyboard navigation.
- Adequate clickable target sizes.
- Avoiding interactions that require unnecessary precision.
- Supporting alternative input devices.

Cognitive accessibility

- Clear language.
- Consistent layouts.
- Predictable navigation.
- Simple instructions.
- Helpful error messages.

### POUR principles

The Web Content Accessibility Guidelines (WCAG) organize accessibility around four principles.

| Principle      | Meaning                                                                  | Example                                 |
| -------------- | ------------------------------------------------------------------------ | --------------------------------------- |
| Perceivable    | Information must be presentable to users in ways they can perceive       | Captions for spoken content             |
| Operable       | Interface controls must be usable                                        | Keyboard-operable menus                 |
| Understandable | Content and behavior must be understandable                              | Clear labels and predictable navigation |
| Robust         | Content must work with a range of user agents and assistive technologies | Semantic HTML controls                  |

### Example

A button that changes color on hover but has no accessible name may be difficult for a screen-reader user to identify. Providing an appropriate accessible name and keyboard support improves accessibility.

### Usability vs accessibility

Usability focuses on how effectively and efficiently users can complete tasks. Accessibility focuses on ensuring people with diverse abilities can access and use the product.

Both are necessary for good UX.

## 6. Information Architecture

Information Architecture (IA) is the organization, labeling and structuring of information so users can find and understand it.

It is especially important in websites, applications, digital libraries and content-heavy systems.

### Main components

1\. Organization systems

Group information into meaningful categories.

Example: An online store groups products into electronics, clothing and home appliances.

2\. Labeling systems

Use understandable names for sections, buttons and categories.

Example: Using "Order History" rather than an unclear label such as "Past Activity Data."

3\. Navigation systems

Help users move between different parts of the product.

Example: A navigation bar containing Home, Courses, Assignments and Results.

4\. Search systems

Allow users to locate information through keywords, filters or suggestions.

Example: Searching a course catalog by subject or course code.

### Common organization structures

- Hierarchical: Categories and subcategories form a tree.
- Sequential: Users follow an ordered sequence.
- Matrix: Users navigate using multiple dimensions, such as price and category.
- Network: Items connect through multiple relationships.

### Example: College portal

College Portal

Academics

Courses

Results

Administration

Fees

Documents

### Benefits

- Improves findability.
- Reduces confusion.
- Supports predictable navigation.
- Helps users understand the relationship between content categories.

## 7. User Flow

A user flow is a visual or written representation of the steps a user takes to complete a particular task in an application or website.

It shows the sequence of screens, decisions, actions and possible outcomes.

### Example: Login flow

Open Application

Enter Login Details

Validate Credentials

Are credentials valid?

Yes

Dashboard

No

Show Error and Retry

### Steps to create a user flow

1. Identify the user's goal.
2. Define the starting point.
3. List the actions required.
4. Identify decision points and alternative paths.
5. Define success and error outcomes.
6. Check whether the flow is understandable and efficient.

### Benefits

- Identifies unnecessary steps.
- Makes navigation problems easier to find.
- Helps developers understand expected behavior.
- Supports usability testing and prototyping.

### User flow vs information architecture

Information architecture organizes the product's content and navigation structure.

A user flow describes the steps taken to accomplish a particular goal within that structure.

For example, IA defines where the checkout page belongs, while a user flow shows how a shopper reaches it and completes an order.

## 8. Wireframing

A wireframe is a simplified representation of a screen's layout and structure.

It focuses on where elements belong and how users move through the interface rather than on final visual styling.

### Typical wireframe elements

- Header.
- Navigation.
- Main content area.
- Images or image placeholders.
- Buttons.
- Text fields.
- Sidebars.
- Footer.

Example: Low-detail website wireframe

LOGO

Navigation: Home | Courses | About

Main Heading

Image Placeholder

Primary Action Button

Footer

Illustrative structural layout, not a finished interface design.

### Types of wireframes

Low-fidelity wireframes: Simple boxes, rough layouts and minimal detail.

High-fidelity wireframes: More precise spacing, content, controls and layout details. They may include visual styling but do not necessarily contain working interactions.

### Advantages

- Quick to create.
- Easy to change.
- Helps establish content hierarchy.
- Reduces time spent polishing an untested design.
- Improves communication between designers, developers and stakeholders.

## 9. Low-Fidelity Prototyping

A low-fidelity prototype is a rough, inexpensive representation of a proposed product or interface.

It is used to test ideas, structure and basic interaction before investing time in detailed visuals or implementation.

### Common forms

- Paper sketches.
- Hand-drawn screens.
- Simple grayscale wireframes.
- Basic clickable mockups.
- Storyboards showing screen transitions.

### Example

For an online shopping app, a designer sketches three screens:

1. Product listing.
2. Product details.
3. Checkout.

The prototype can be used to ask users whether they understand how to move from browsing to purchasing.

### Advantages

- Fast to create.
- Cheap to revise.
- Encourages experimentation.
- Helps detect structural issues early.
- Keeps attention on task flow rather than visual polish.

### Limitations

- Does not accurately represent final visual appearance.
- May not demonstrate realistic animation or feedback.
- Cannot validate every technical requirement.

## 10. High-Fidelity Prototyping

A high-fidelity prototype closely resembles the intended final product in visual appearance and interaction.

It may include realistic typography, colors, images, spacing, transitions and clickable controls.

### Common features

- Detailed visual design.
- Realistic screen content.
- Interactive buttons and navigation.
- Clickable flows.
- Simulated validation and error states.
- Responsive layouts.
- Transitions and micro-interactions.

### Example

A high-fidelity prototype of a banking app might let a participant navigate from the dashboard to a transfer form, select a recipient, review transaction details and view a simulated confirmation screen.

The prototype may imitate the flow without actually moving money or connecting to the bank's backend.

### Advantages

- Provides a realistic experience for user testing.
- Helps stakeholders understand the intended product.
- Reveals visual hierarchy and interaction issues.
- Supports more detailed usability evaluation.

### Limitations

- Takes longer to build and maintain.
- Can be expensive to revise if foundational assumptions are wrong.
- May look functional without implementing real backend behavior.
- Can encourage premature attention to visual polish.

### Low-fidelity vs high-fidelity prototypes

| Feature            | Low-fidelity                   | High-fidelity                                    |
| ------------------ | ------------------------------ | ------------------------------------------------ |
| Visual detail      | Minimal                        | Close to final design                            |
| Creation time      | Usually short                  | Usually longer                                   |
| Cost of changes    | Usually low                    | Can be higher                                    |
| Main purpose       | Test structure and early ideas | Test detailed interactions and visual design     |
| Typical example    | Paper sketches                 | Interactive, styled app mockup                   |
| Real functionality | Limited or simulated           | Often simulated, sometimes partially implemented |

Important distinction: Fidelity describes how closely a prototype represents the proposed product. Interactivity and fidelity are related but separate: a low-fidelity prototype can be clickable, and a high-fidelity prototype can still have nonfunctional controls.

# Final revision guide

## Most important comparisons

These are useful for preparing descriptive answers and short exam questions.

| Comparison                              | Key distinction                                                               |
| --------------------------------------- | ----------------------------------------------------------------------------- |
| Parallel vs perspective projection      | Parallel rays vs perspective convergence                                      |
| Gouraud vs Phong shading                | Interpolate colors vs interpolate normals and calculate lighting per pixel    |
| Rasterization vs ray tracing            | Primitive-to-pixel conversion vs ray-based intersection and lighting          |
| Sampling vs quantization                | Discretizing time vs discretizing amplitude                                   |
| Lossy vs lossless compression           | Approximate reconstruction vs exact reconstruction                            |
| Codec vs container                      | Encodes/decodes media vs organizes media streams                              |
| Raster vs vector graphics               | Pixels vs geometric descriptions                                              |
| Motion graphics vs VFX                  | Animated graphic communication vs broader image creation and modification     |
| UI vs UX                                | Interface elements vs overall user experience                                 |
| Usability vs accessibility              | Task effectiveness and efficiency vs access for people with diverse abilities |
| Information architecture vs user flow   | Content structure vs steps toward a goal                                      |
| Wireframe vs prototype                  | Layout representation vs representation used to explore or test behavior      |
| Low-fidelity vs high-fidelity prototype | Rough and inexpensive vs detailed and realistic                               |

## Formula sheet

| Topic                     | Formula                                                       |
| ------------------------- | ------------------------------------------------------------- |
| Perspective projection    | \\(x'=fx/z,\ y'=fy/z\\) for the stated camera convention      |
| Viewport mapping          | \\(x_s=x_v+uW_v,\ y_s=y_v+vH_v\\)                             |
| Diffuse illumination      | \\(I_d=k_dI_l\max(0,\mathbf N\cdot\mathbf L)\\)               |
| Phong specular term       | \\(I_s=k_sI_l\max(0,\mathbf R\cdot\mathbf V)^n\\)             |
| Frame time                | \\(1000/\text{FPS}\\) milliseconds                            |
| Pixels in an image        | Width × height                                                |
| Color levels              | \\(2^n\\) levels for an \\(n\\)-bit value                     |
| Uncompressed PCM bit rate | Sampling rate × bits per sample × channels                    |
| Approximate stream size   | Bit rate × duration ÷ 8                                       |
| Audio Nyquist condition   | Sampling rate greater than twice the highest signal frequency |
| Task completion rate      | Successful completions ÷ total attempts × 100%                |

## Suggested preparation priorities

1. Unit III: Viewing pipeline, parallel vs perspective projection, Z-buffer, illumination models, shading comparison, texture mapping, rasterization vs ray tracing.
2. Unit IV: Multimedia architecture, image formats, sampling and quantization, audio bit rate calculations, codecs vs containers, compression, streaming and animation principles.
3. Unit V: UI vs UX, User-Centered Design, Design Thinking, usability, accessibility and POUR, information architecture, user flows, wireframes and prototype comparisons.

For a descriptive exam, practice writing each major topic using this sequence: definition → working or process → diagram → example → advantages and limitations → comparison where applicable.

Unit V remains incomplete in the supplied syllabus. The notes above cover its ten supplied topics, from Introduction to UI/UX through High-Fidelity Prototyping. Any additional topics after that point would require the remaining syllabus.