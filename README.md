# Chaos — Real-Time Fluid Dynamics Simulation

A Python-based 2D fluid dynamics simulator built on the **Navier-Stokes equations**. The project provides an interactive, real-time visualization of fluid motion, with both a fully functional CPU implementation and an in-progress GPU-accelerated variant.

---

## Features

- **Real-time fluid simulation** rendered interactively in a Pygame window
- **Navier-Stokes solver** covering diffusion, advection, and pressure projection
- **Gauss-Seidel iterative linear solver** for stable, divergence-free velocity fields
- **Density and velocity injection** at arbitrary grid cells
- **Proper boundary conditions** (reflective walls on all four sides)
- **Configurable parameters** — grid size, viscosity, diffusion rate, timestep, and solver iterations
- **GPU-accelerated variant** (work-in-progress) targeting NVIDIA hardware via Numba and PyculIB

---

## Demo

The simulation opens a 256 × 256-pixel window (a 64 × 64 grid scaled 4×). Density is continuously injected at the centre and advected by a constant velocity field, producing a flowing smoke-like effect.

---

## Project Structure

```
Chaos/
├── FluidCPU.py   # Fully functional CPU-based Navier-Stokes fluid simulator
└── FluidGPU.py   # GPU-accelerated implementation (work-in-progress)
```

---

## Prerequisites

- Python 3.8+
- An NVIDIA GPU with CUDA support is required **only** for `FluidGPU.py`

### Python dependencies

| Package | Purpose |
|---------|---------|
| `numpy` | Numerical array operations |
| `pygame` | Window creation and rendering |
| `perlin-noise` | Procedural noise generation |
| `numba` | JIT compilation / CUDA kernels (GPU only) |
| `pyculib` | CUDA BLAS / sparse routines (GPU only) |

---

## Installation

```bash
# 1. Clone the repository
git clone https://github.com/srmlcn/Chaos.git
cd Chaos

# 2. Create and activate a virtual environment (recommended)
python -m venv venv
source venv/bin/activate       # Linux / macOS
venv\Scripts\activate.bat      # Windows

# 3. Install CPU dependencies
pip install numpy pygame perlin-noise

# 4. (Optional) Install GPU dependencies — requires CUDA toolkit
pip install numba pyculib
```

---

## Usage

### CPU simulation

```bash
python FluidCPU.py
```

A Pygame window will open. Close it (or press the window's close button) to exit.

### GPU simulation *(experimental)*

```bash
python FluidGPU.py
```

> **Note:** The GPU implementation is still under active development and is not yet fully functional.

---

## Configuration

Key constants at the top of `FluidCPU.py` can be adjusted to explore different behaviours:

| Constant | Default | Description |
|----------|---------|-------------|
| `N` | `64` | Grid resolution (N × N cells) |
| `SCALE` | `4` | Pixel size of each grid cell |
| `iter` | `4` | Gauss-Seidel solver iterations (higher = more accurate) |

The `FluidSquare` constructor accepts:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `diffusion` | `0` | Rate at which density spreads |
| `viscosity` | `0.0000001` | Fluid thickness / resistance to flow |
| `dt` | `0.2` | Simulation timestep |

---

## Technical Overview

The simulation implements the **Jos Stam stable fluids** algorithm:

1. **Diffuse** — spread velocity (and density) according to viscosity / diffusion using a linear solver
2. **Project** — enforce incompressibility by removing divergence from the velocity field
3. **Advect** — move density and velocity along the current velocity field using semi-Lagrangian back-tracing

Boundary conditions reflect velocity components at the grid edges to simulate solid walls.

---

## Roadmap

- [x] CPU Navier-Stokes solver with real-time Pygame visualisation
- [ ] Complete GPU kernel implementations in `FluidGPU.py`
- [ ] Mouse-driven density and velocity injection
- [ ] Colour-mapped density rendering
- [ ] Configurable simulation parameters via CLI or GUI

---

## License

This project does not currently include a license file. Please contact the repository owner for usage terms.
