# Comparative 3D CFD Analysis of a LEGO Formula 1 Car

**Author:** Timothy Owen Chandra
**Tools:** Siemens Designcenter (formerly NX) Student Edition • ANSYS SpaceClaim Student • ANSYS Fluent & CFD-Post Student

---

## 1. Project Overview
This project evaluates the aerodynamic performance of a reverse-engineered **LEGO Speed Champions 2023 McLaren Formula 1** car (`~19 cm` model length) using steady-state 3D Reynolds-Averaged Navier–Stokes (**RANS**) simulations in **ANSYS Fluent**. Specifically, the study compares the stock LEGO rear wing (**Simplified Baseline**) against a modified rear wing featuring an **inverted Selig S1223 airfoil (`5° AoA`)**.

* **Aerodynamic Rationale:** At LEGO Speed Champions scale and a `30 m/s` freestream velocity, the rear wing operates in a low-Reynolds-number regime (`Re ≈ 4.2 × 10⁴` based on wing chord. The high-camber **Selig S1223** profile is specifically designed to maintain high lift-to-drag efficiency in low-Reynolds-number flows.
* **Visual Presentation:** See [`LEGO_F1_CFD.pdf`](./LEGO_F1_CFD.pdf) in this repository for the 6-slide visual walkthrough of the CAD assembly, mesh refinement, and flow-field contours.

---

## 2. Engineering Workflow & Pre-Processing Solutions
Navigating Student edition software constraints required dedicated CAD-to-CFD pre-processing workarounds:

1. **Reverse Engineering & Watertight Simplification (Siemens Designcenter / NX):**
* Modeled the full multi-part LEGO McLaren F1 assembly from scratch.
* Removed components (mirrors, halo, suspension arms, surface studs,etc.) and manually sealed all gaps to ensure a watertight geometry prior to export.
2. **STL-to-Solid Conversion & Trailing-Edge Treatment (ANSYS SpaceClaim):**
* Because neutral CAD export in the Student license is restricted to faceted `.stl` files rather than `.step` solids, the watertight `.stl` was converted back into a usable solid body via **Shrinkwrap**.
* Modeled the replacement inverted **Selig S1223 (`5° AoA`)** rear wing and blunted its trailing edge to prevent boundary-layer prism collapse.
3. **Constrained Mesh Generation (ANSYS Meshing):**
* Built a half-car symmetry domain with local refinement box, `0.5 mm` tire-to-ground Pinch controls, and `3-layer` boundary-layer inflation while staying strictly under the **1,048,576-element** ANSYS Student cap:
* **Simplified Baseline Mesh:** `898,000` elements (`0.31` avg. skewness, '0.68' avg. orthogonal quality)
* **Modified Rear Wing Mesh (Selig S1223):** `1,041,000` elements (`0.30` avg. skewness, `0.69` avg. orthogonal quality)

---

## 3. Solver Setup & Boundary Conditions
* **Solver:** ANSYS Fluent 3D Steady RANS (Pressure-Based)
* **Turbulence Model:** `k-ω SST`
* **Inlet:** Velocity Inlet at `30 m/s`
* **Outlet:** Pressure Outlet at `0 Pa` (gauge)
* **Ground & Symmetry:** Moving ground at `30 m/s` (no-slip); vertical centerline symmetry plane

---

## 4. Quantitative Results & Physical Findings

| Aerodynamic Metric | Simplified Baseline | Inverted Selig S1223 (5° AoA) | Relative Change |
| :--- | :---: | :---: | :---: |
| **Drag Coefficient (`Cd`)** | `0.4688` | `0.4634` | **`-1.15%`** |
| **Lift Coefficient (`Cl`)** | `+0.0415` | `+0.0416` | **`+0.24%`** |
| **Aerodynamic Efficiency (`L/D`)** | `0.0885` | `0.0898` | **`+1.41%`** |

### Key Flow-Field Takeaways
* **Drag Reduction (`-1.15%`):** Replacing the blunt brick rear wing with the cambered Selig S1223 profile reduced overall aerodynamic drag.
* **Downforce Limitation (`Cl` Nearly Unchanged):** Symmetry-plane velocity contours revealed that the blocky LEGO airbox generate a large pocket of low-velocity, turbulent wake air directly ahead of the rear wing. While outboard slices confirmed that clean, high-speed airflow reaches the outer span of the curved wing, centerline wake immersion prevented net downforce gains—identifying **upstream bodywork smoothing** as the primary priorities for future design iterations.
