# DX11 (On Going Project -- Do not follow, but you may find some interesting info. in this wiki)

From the [youtube]() tutorial series, and related [github repository](https://github.com/Pindrought/DirectX-11-Engine-VS2017/tree/Tutorial_1), a tutorial series on **directx 11** I am trying to illustrate for future reference. I am using visual studio 2019, win10 x64. 

### 1. Setting up and configuration
* create new project >> Windows Desktop Wizard (make it empty project, desktop .exe, and uncheck the precompiled header option)
* we need [directx toolkit](https://github.com/microsoft/DirectXTK). Clone the repository, `build` and `release` for both `x64` and `x86` separately. Only the `Inc` and `Bin (DirectXTK.lib)` folder is required for compiled any `dx11` program. 
* Linking: 
  * `Project Properties >> Configuration Properties >> VC++ Directories >> Include Directories >> (give the include folder directory)`  
  * `Project Properties >> Configuration Properties >> VC++ Directories >> Library Directories (add the lib for debug and release separately, x64 and x86 (optional for now))`
  * Depend on the Project directory and Solution directory, `$(ProjectDir)` or `$(SolutionDir)` is better way to use
  * `Project Properties >> Input >> Additional Dependencies >> Add (DirectXTK.lib)`
    * this can also be done by adding `#pragma comment (lib, "d3d11.lib")`, and `#pragma comment (lib, "DirectXTK.lib")` at the main source code

***  
### 2. Programming with DX11
* string conversation [from simple string to wide string](https://github.com/Pindrought/DirectX-11-Engine-VS2017/tree/Tutorial_2)
  * error handling -> create new header, and `.cpp` file for error handling, or check if there is built-in reusable 
  * **static function**
* creating render window 
   * the rendering `window header` should include the error handling `header` 
     * `bool Initialize (HINSTANCE hInstance, std::string window_title, std::string window_class, int width, int height);`
     * `this->` pointer to the current object instance that the method belongs to
* integrate mouse and keyboard with the render window (**eye-tracker**)
* initialization directx11 (`#include<d3d11.h>`)
  * `#include <wrl/client.h>` smart pointer 
    * `Microsoft::WRL::ComPtr<ID3D11Device> device;` -> to create the buffer
    * `Microsoft::WRL::ComPtr<ID3D11DeviceContext> deviceContext;` -> shader resources
    * `Microsoft::WRL::ComPtr<IDXGISwapChain> swapchain;` -> to swap out the frames while rendering 
    * `Microsoft::WRL::ComPtr<ID3D11RenderTargetView> renderTargetView;` -> where we are rendering the buffer 
***
### 3. Vertex shader
* create a file `Vertex.h` (for example)
   ```cpp
   #include <DirectXMath.h> // for math operations, include matrix and vector
   struct Vertex
   {
   Vertex () {} // default constructor
   Vertex (float x, float y)
          : pos (x, y) {} //position
   DirectX::XMFLOAT2 pos;
   };
   ```
* now create a `vertexshader.hlsl` for example 
  ```hlsl
  float4 main (float2 InPos:POSITION): SV_POSITION // it follows the XMFLOAT2 pos in .h file 
  {
  return float4(Inpos, 0, 1); // z = 0, w = 1
  }
  ```
* right click and then compile works for `.hlsl` code. 
* go to the properties of the `.hlsl` >> `HLSL Compiler` >> select `Shader Type` >> `Shader Model` 
* for all shaders file, a `header` and `cpp` file [required](https://www.youtube.com/watch?v=ma8KGpoSjWM&list=PLcacUGyBsOIBlGyQQWzp6D1Xn6ZENx9Y2&index=11&ab_channel=Jpres) 
* we need `D3DCompiler.lib` and `d3dcompiler.h` for **loading shaders**
* `vertexshader.cso` is the actual vertex shader after compilation, this is required for the the .header and .cpp file to include.  
    
    
### References
1. raster graphics pipeline: https://www.youtube.com/watch?v=4RFlOvVPuys&t=0s&ab_channel=LiveLessons
