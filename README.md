
---

# 📘 Physics-Informed Neural Networks for Fluid Flow (with OpenFOAM Validation)

This repository presents a hybrid approach to solving fluid dynamics problems using **Computational Fluid Dynamics (CFD)** and **Physics-Informed Neural Networks (PINNs)**.
The work focuses on extending PINNs to **complex 2D and 3D geometries** and validating them against high-fidelity CFD simulations.

📄 Based on our research paper: 

---

## 🚀 Project Overview

Traditional CFD methods are highly accurate but computationally expensive and require manual mesh generation.
This project explores **PINNs as a mesh-free alternative**, embedding physical laws (Navier–Stokes equations) directly into neural networks.

### 🔑 Key Contributions

* Solved **2D and 3D incompressible Navier–Stokes equations**
* Developed and compared:

  * **Simple PINN (MLP-based)**
  * **FPINN (Fourier-feature enhanced PINN)**
* Validated results using **OpenFOAM simulations**
* Benchmarked across **multiple complex geometries**

---

## 🔬 Experiment 1: CFD Simulations (OpenFOAM)

### 🎯 Objective

Generate high-quality **ground truth data** for training and validating PINNs.

### 🧪 Simulations Performed

As described in the paper (Section 3), three 2D and one 3D CFD experiments were conducted:

1. **Trapezoidal Channel Flow**
2. **Flow through Stacked Rectangular Blocks**
3. **2D Nozzle Flow**
4. **3D Trapezoidal Geometry**

(Refer to meshes on page 3–4 of the paper)

### ⚙️ Simulation Details

* Solver: `icoFoam` (laminar incompressible flow)
* Coupling: **PISO algorithm**
* Fluid: Incompressible Newtonian
* Visualization: ParaFoam

### 📂 Configuration Files

* `blockMeshDict` – Mesh generation
* `fvSchemes` – Discretization
* `fvSolution` – Solver settings
* `controlDict` – Run control
* `physicalProperties` – Fluid parameters

---

## 🧠 Experiment 2: PINNs for Navier–Stokes

### 🎯 Objective

Learn fluid flow fields by minimizing both:

* **Data loss** (fit to CFD results)
* **Physics loss** (Navier–Stokes residuals)

---

## 📘 Governing Equations

The system models **incompressible Newtonian flow**:

### Continuity Equation

[
\nabla \cdot \mathbf{U} = 0
]

### Momentum Equation

[
\frac{\partial \mathbf{U}}{\partial t} + (\mathbf{U} \cdot \nabla)\mathbf{U} = -\frac{1}{\rho}\nabla p + \nu \nabla^2 \mathbf{U}
]

These equations are enforced through **automatic differentiation** in the loss function .

---

## 🏗️ Model Architectures

### 1️⃣ Simple PINN (SPINN)

* Fully connected MLP
* 8–12 layers, 256 neurons each
* Activation: `tanh`
* Learns directly from coordinates → (u, v, w, p)

### 2️⃣ FPINN (Best Model)

* Fourier feature embeddings
* PirateNet-style residual blocks
* Better captures **high-frequency flow features**
* Improved convergence and accuracy

📌 Architecture diagrams shown in paper (page 3)

---

## 📉 Loss Function

Total loss:
[
L = L_{data} + \lambda L_{PDE}
]

* **Data Loss:** Matches CFD outputs
* **PDE Loss:** Enforces physics constraints
* **Challenge:** PDE loss dominates and is unstable (key finding)

---

## 📊 Results Summary

### 🏆 Best Performances (from Tables, page 4–5)

| Experiment        | Best Model | Key Result                             |
| ----------------- | ---------- | -------------------------------------- |
| Exp 1 (Trapezoid) | FPINN      | MAE(u) = **0.0138**                    |
| Exp 2 (Blocks)    | SPINN      | Stable but higher error                |
| Exp 3 (Nozzle)    | FPINN      | Performance drop (hard case)           |
| 3D Case           | SPINN      | w-MAE = **0.0013**, p-MAE = **0.0006** |

### 🔍 Key Observations

* FPINN consistently outperforms standard PINNs
* Model performance is **highly geometry-dependent**
* PDE loss remains **significantly higher than data loss**
* 3D predictions achieved **very high accuracy**

(See detailed tables and loss curves in pages 4–5)

---

## ⚠️ Challenges

* ⚖️ **Loss imbalance:** PDE loss >> Data loss
* 📉 Training instability (especially FPINN spikes)
* 🔁 Sensitivity to geometry & hyperparameters
* 🧠 Difficulty enforcing physical constraints fully

---

## 💡 Key Takeaways

* PINNs can act as a **mesh-free alternative to CFD**
* FPINN significantly improves accuracy and convergence
* 3D flow prediction is feasible with low error
* **Loss balancing is the main bottleneck**

---

## 🔮 Future Work

* Adaptive loss weighting / curriculum learning
* Better enforcement of physics constraints
* Scaling to turbulent flows
* Real-time CFD surrogate modeling

---

## 🛠️ Tech Stack

* **CFD:** OpenFOAM
* **DL Framework:** PyTorch / TensorFlow (PINNs)
* **Visualization:** ParaFoam, Matplotlib
* **Environment:** Linux (WSL)

---

## 👥 Authors

* Arko Bera
* Vishal Singh
* Raghav Kankane
* Ashutosh Singh
* Adil Khan

(IIIT Nagpur)

---
