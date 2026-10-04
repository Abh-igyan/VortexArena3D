# VortexArena3D: 3D Physics Simulation & Arena Combat Game Engine

![VortexArena3D Gameplay & Physics Demo](assets/VortexArena3D_Demo.gif)

**VortexArena3D** is a high-performance 3D game engine and rigid-body combat simulation built from scratch in **Modern C++ (C++20)**. 

Designed without external physics black-boxes (no PhysX or Bullet dependencies), the engine implements a custom numerical physics pipeline to model accurate rotational dynamics, angular momentum, gyroscopic precession, and high-energy collision restitution. Hardware-accelerated rendering is driven via **OpenGL**, with **GLM** for 3D mathematics and an integrated **Dear ImGui** HUD for real-time physics telemetry and framerate profiling.

---

## Technical Highlights & Architecture

### 1. Custom 3D Physics & Rotational Dynamics
* **First-Principles Physics:** Custom rigid-body simulator calculating instantaneous torque, rotational inertia tensors, gyroscopic precession, and surface friction models.
* **Fixed-Timestep Integrator:** Implements a decoupled accumulator game loop (`dt = 1/60s`) with interpolated state rendering to guarantee deterministic physics and eliminate visual micro-stuttering.
* **Collision Detection & Response:** Fast geometric collision detection with impulse-based momentum transfer and contact normal resolution.

### 2. Graphics Pipeline & 3D Math
* **Modern OpenGL Architecture:** Programmable shader pipeline utilizing Vertex Array Objects (VAOs), Vertex Buffer Objects (VBOs), and Element Buffer Objects (EBOs).
* **6-DOF Camera & 3D Linear Algebra:** Full quaternion-based orientation and orbit camera to prevent gimbal lock, combined with view-projection matrices computed via GLM.
* **Asynchronous Asset Pipeline:** Multi-threaded mesh ingestion (`.obj`) offloaded from the main render thread to prevent frame spikes during asset loading.
* **Shading & Materials:** Directional and point lighting with Phong specular highlights and texture mapping.

### 3. Real-Time Telemetry & In-Engine HUD
* **Dear ImGui Integration:** In-engine diagnostics dashboard profiling frame times (ms), draw calls, contact manifold normals, angular velocities, and active entities.
* **Live Physics Tuning:** Real-time parameter tweaking for restitution, friction coefficients, tilt stability, and spin speed.

### 4. Engine Architecture & Tooling
* **State Machine & Management:** Decoupled game states (Pre-match arena configuration, real-time battle simulation, match analytics).
* **Cross-Platform CMake Setup:** Automated `FetchContent` dependency resolution for GLFW, GLEW, GLM, Dear ImGui, and doctest.
* **Unit Testing:** Comprehensive test coverage via `doctest` verifying physics calculations, vector transforms, and file I/O.

---

## Project Structure

```
VortexArena3D/
├── src/                     # Core engine source files
│   ├── engine/              # Game loop, state machine, input handling
│   ├── physics/             # Custom rigid-body & rotational physics models
│   ├── rendering/           # OpenGL shaders, camera, mesh & texture loaders
│   └── ui/                  # Dear ImGui telemetry and diagnostics overlay
├── assets/                  # 3D models (.obj), textures, and shaders
├── game_data/               # Configuration and state files
├── tests/                   # Unit test suite (doctest)
└── CMakeLists.txt           # Cross-platform CMake build configuration
```

---

## Building and Running

### Prerequisites
* **C++ Compiler:** Supporting C++20 (MSVC 2022, GCC 11+, or Clang 13+)
* **Build System:** CMake 3.14 or newer

All third-party libraries (GLFW, GLEW, GLM, Dear ImGui, doctest) are automatically fetched and compiled via CMake `FetchContent`.

### 1. Clone the Repository
```bash
git clone https://github.com/Abh-igyan/VortexArena3D.git
cd VortexArena3D
```

### 2. Configure & Build

#### Windows (Command Line / PowerShell)
```powershell
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release -j
```

#### Windows (Visual Studio)
1. Open the generated `build/VortexArena3D.sln` in **Visual Studio 2022**.
2. Select **Release** or **Debug** configuration.
3. Set `VortexArena3D` as the startup project and press `F5` or `Ctrl+F5`.

#### Linux (Debian / Ubuntu)
```bash
sudo apt update && sudo apt install -y build-essential cmake pkg-config libgl1-mesa-dev \
    libwayland-dev libxkbcommon-dev libx11-dev libxrandr-dev \
    libxinerama-dev libxcursor-dev libxi-dev

cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j$(nproc)
```

### 3. Launch Executable

```bash
# Windows
.\build\Release\VortexArena3D.exe

# Linux / macOS
./build/VortexArena3D
```

### 4. Running Unit Tests
```bash
ctest --test-dir build --output-on-failure
# or execute directly
./build/battlebeyz_tests
```

---

## Author & Acknowledgements
* **Developer:** Abhigyan Tiwari ([GitHub](https://github.com/Abh-igyan) | [LinkedIn](https://www.linkedin.com/in/abhigyan-tiwari-570536314/))
* Core concept inspired by physical spinning top dynamics and custom engine exploration.
