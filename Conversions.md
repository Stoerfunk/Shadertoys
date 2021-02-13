# Converting between different Shader Languages

... just wanted to try out tables in Markdown :-)

## General Concepts

The entry point for a compute shader is the *compute kernel* - this is the code executed by the GPU's shader unit. In GLSL this is the code's `main` function; in OpenCL it is a function with its definition preceded by the symbol `kernel`; in DCTL it's a function preceded by `__KERNEL__`. So for compatibility avoid naming your Kernel `main` or `kernel` as this might work on one, but not the other platform.


## Data Types and Type Conversion

### 2-dimensional Vector

Schema of what I guess a `vec2` looks like:

```C++
struct vec2
{
  float x;
  float y;

  vec2(float s)
  {
    x=y=s;
  }

  vec2(const vec2& s)
  {
    x=s.x;
    y=s.y;
  }
};
```

... I'll bet there is some Open GL ES spec and/or include files that perfectly describe the WebGL data types and functions?!?


| System | Equivalent                                                   |
|--------|--------------------------------------------------------------|
| GLGS   |`vec2`; Construction: `vec2(float,float)`                                                        |
| DCTL   | L-Value: `float2`; R-Value: `to_float2(float,float)`, `to_float2_s(float)` |
| Cuda   |                                                              |
| Metal  |                                                              |
| OpenCL |                                                              |

Probably DCTL does schon define some macros and functions, that make the GLSL digestable for the differen Shader Language dialects?!? It would be of great help, if these definitions are accessible anywhere!?!




### 3-dimensional Vector

| System | Equivalent                                                   |
|--------|--------------------------------------------------------------|
| GLGS   |`vec3`; Construction: `vec3(float,float,float)`, `vec3(vec2,float)`                        |
| DCTL   | L-Value: `float3`; R-Value: `to_float3(float,float,float)`, `to_float3_aw(float2,float)` |
| Cuda   |                                                              |
| Metal  |                                                              |
| OpenCL |                                                              |




### 3x3 Matrix

| System | Equivalent                                                                    |
|--------|-------------------------------------------------------------------------------|
| GLGS   |`mat3`; Construction: `mat3(vec3,vec3,vec3)`                                   |                
| DCTL   | **Workaround:** L-Value: `mat3`; Construction: `to_mat3(float3,float3,float3)`|
| Cuda   |                                                                               |
| Metal  |                                                                               |
| OpenCL |                                                                               |


Workaround implementation:

```c
typedef struct
{
  float3 r0, r1, r2;
} mat3;


__DEVICE__ inline mat3 to_mat3( float3 a, float3 b, float3 c)
{ 
  mat3 d; 
  d.r0 = a; 
  d.r1 = b; 
  d.r2 = c; 
  return d; 
} 
```




### 2D Texture

| System | Equivalent                                                   |
|--------|--------------------------------------------------------------|
| GLGS   |`sampler2D`                                                   |
| DCTL   | `__TEXTURE2D__`                                              |
| Cuda   |                                                              |
| Metal  | `texture2d<float,access::sample>`                            |
| OpenCL |                                                              |






----

Could be better for an overview, but the table gets too wide:

| GLGS / WebGL / OpenGL ES | DCTL     | Cuda | Metal | OpenCL |
|--------------------------|----------|------|-------|--------|
| `vec3` (L-Value)         | `float3` |      |       |        |
| `vec` (R-Value)          | `to_float3(float,float,float)` | | | |
| `sampler2D` | `__TEXTURE2D__` | | `texture2d<float,access::sample>` | |



----

... okay, you guys get the idea what I tried with this page - but I guess this approach is totally insane.