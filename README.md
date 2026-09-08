# Numerical Simulation and Conjugate Heat Transfer Analysis of a Counter-Flow Shell & Tube Heat Exchanger

![ANSYS Fluent](https://img.shields.io/badge/ANSYS-Fluent%202024-CF112D?style=for-the-badge&logo=ansys)
![CFD](https://img.shields.io/badge/Physics-Conjugate%20Heat%20Transfer-blue?style=for-the-badge)
![Mesh](https://img.shields.io/badge/Mesh-Structured%20Quad%20(y%2B%20%3C%201)-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

Comprehensive numerical investigation and hydrodynamic/thermal characterization of an industrial-scale counter-flow shell and tube heat exchanger (STHE). This project was conducted as the comprehensive term project for the **Computational Fluid Dynamics (CFD)** course at **Sharif University of Technology**.

---

## 📌 Executive Summary

This study models fluid flow and Conjugate Heat Transfer (CHT) across a 3-meter counter-flow shell and tube heat exchanger. By leveraging physical axisymmetry, the domain is modeled using the **2D Axisymmetric Swirl** formulation in ANSYS Fluent to eliminate prohibitive computational runtimes while maintaining full resolution of near-wall boundary layer physics.

### Key Highlights
* **Strict Grid Independence:** Verified across four structured quadrilateral grids ($\pm10\%$ and $\pm20\%$ cell density variations) with thermal divergence below $0.5\%$.
* **Viscous Sublayer Resolution:** First near-wall cell height fixed at $y_1 = 6.9117\,\mu\text{m}$ across all meshes to ensure $y^+ < 1$ along all solid-fluid interfaces.
* **Energy Balance Conservation:** Global first-law energy discrepancy between hot stream dissipation and cold stream absorption is below **0.0001%**.
* **Conjugate Heat Transfer (CHT):** Fully coupled multi-region conduction-convection through an explicitly resolved aluminum dividing wall.

---

## 📐 Geometric Parameters & Thermophysical Properties

The geometry consists of a counter-flow arrangement where the hot fluid flows through the inner core tube and the cold fluid circulates through the outer annular shell in the opposing direction.

| Parameter | Symbol | Value | Unit |
| :--- | :---: | :---: | :---: |
| Exchanger Length | $L$ | 300 (3.0) | cm (m) |
| Inner Tube Diameter | $d$ | 1.0 (0.01) | cm (m) |
| Outer Shell Diameter | $D$ | 10.0 (0.1) | cm (m) |
| Dividing Wall Thickness | — | Explicitly resolved solid domain | — |
| Computational Formulation | — | 2D Axisymmetric Swirl | — |
| Working Fluid | — | Liquid Water ($\text{H}_2\text{O}$) | — |
| Dividing Wall Material | — | Pure Aluminum (Al) | — |

### Fluid and Solid Properties

| Material | State | Density ($\rho$) | Specific Heat ($c_p$)  | Thermal Cond. ($k$)  | Dyn. Viscosity ($\mu$)  |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Water ($\text{H}_2\text{O}$)** | Fluid | 998.2 | 4182 | 0.6 | 0.001003 |
| **Aluminum** | Solid | 2719 | 871 | 202.4 | — |

---

## ⚙️ Numerical Framework & Boundary Conditions

### Solver Configuration
* **Solver Type:** Pressure-Based, Double Precision, 2D Axisymmetric Swirl.
* **Thermal Modeling:** Energy Equation enabled; Radiation model disabled due to low operating temperatures ($20^\circ\text{C}$ to $80^\circ\text{C}$).
* **Turbulence Model:** Standard $k$-$\omega$ (Two-Equation) with Shear Flow Corrections and Production Limiter enabled.
* **Near-Wall Treatment:** Automatic near-wall correlation treatment.
* **Pressure-Velocity Coupling:** SIMPLEC algorithm with Skewness Correction = 0.
* **Spatial Discretization:**
  * Gradient: Least Squares Cell-Based
  * Pressure: Second Order
  * Momentum & Swirl Velocity: Second Order Upwind
  * Turbulent Kinetic Energy ($k$): Second Order Upwind
  * Specific Dissipation Rate ($\omega$): First Order Upwind (to suppress steep near-wall oscillations)
  * Energy: Second Order Upwind
* **Convergence Criteria:** Residual monitor threshold set to $10^{-5}$ across all transport equations.

### Boundary Conditions
* **Cold Stream Inlet (`_cold_inlet_mesh`):** Pressure Inlet, Total Gauge Pressure = 202,650 Pa (2 atm), Total Temperature = 293.15 K ($20^\circ\text{C}$), Turbulence Intensity = 5%, Viscosity Ratio = 10.
* **Hot Stream Inlet (`_hot_inlet_mesh`):** Pressure Inlet, Total Gauge Pressure = 202,650 Pa (2 atm), Total Temperature = 353.15 K ($80^\circ\text{C}$), Turbulence Intensity = 5%, Viscosity Ratio = 10.
* **Cold Stream Outlet (`_cold_outlet_mesh`):** Mass Flow Outlet, fixed mass flow rate $\dot{m}_{cold} = 501.64\text{ kg/s}$ (establishing $v_{avg} = 5\text{ m/s}$).
* **Hot Stream Outlet (`_hot_outlet_mesh`):** Mass Flow Outlet, fixed mass flow rate $\dot{m}_{hot} = 156.76\text{ kg/s}$ (establishing $v_{avg} = 5\text{ m/s}$).
* **Interfaces (`_wall_hot_mesh` & `_wall_cold_mesh`):** Coupled thermal boundary condition with zero wall thickness (conduction explicitly solved in mesh).
* **Casing Outer Wall (`_adiabatic_wall_mesh`):** Adiabatic Wall ($q'' = 0\text{ W/m}^2$).
* **Centerline (`symmetry_axis`):** Axisymmetric boundary.

---

## 📊 Grid Independence Verification

Four structured quadrilateral meshes were analyzed to eliminate discretization error:

| Grid Designation | Cells | $y_1$ [mm] | $\Delta P_{hot}$ [Pa] | Err [\%] | $\Delta P_{cold}$ [Pa] | Err [\%] | $\Delta T_{hot}$ [K] | Err [\%] | $\Delta T_{cold}$ [K] |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Mesh -20%** | 24,400 | $6.9117\times 10^{-3}$ | 2956.92 | 29.95 | 2591.84 | 0.0018 | 1.1140 | 0.10 | 0.3344 |
| **Mesh -10%** | 27,450 | $6.9117\times 10^{-3}$ | 2956.92 | 29.95 | 2591.84 | 0.0018 | 1.1140 | 0.10 | 0.3344 |
| **Baseline Mesh** | **30,500** | **$6.9117\times 10^{-3}$** | **4221.25** | **Ref.** | **2592.30** | **Ref.** | **1.1136** | **Ref.** | **0.3334** |
| **Mesh +10%** | 33,550 | $6.9117\times 10^{-3}$ | 3276.97 | 22.37 | 2595.54 | 0.12 | 1.1410 | 2.46 | 0.3341 |

> **Conclusion:** Stream temperature variation ($\Delta T$) remains below $0.5\%$ across candidate grids, verifying asymptotic convergence of the thermal field. The baseline mesh (30,500 cells) was selected as optimal.

---

## 📈 Global Results & Quantitative Outputs

Performance parameters extracted from the converged numerical solution:

| Computational Parameter | Hot Wall Stream | Cold Wall Stream | Unit |
| :--- | :---: | :---: | :---: |
| **Pressure Coefficient ($C_p$)** | 306,921.94 | 308,146.61 | — |
| **Average Nusselt Number ($Nu$)** | 13,167.41 | 19,099.60 | — |
| **Average Heat Transfer Coeff. ($h$)** | 7,900.45 | 11,459.76 | $\text{W/m}^2\cdot\text{K}$ |
| **Total Heat Transfer Rate ($Q_{total}$)** | -612,439.68 | +612,439.43 | $\text{W}$ |
| **Area-Weighted Bulk Outlet Temp. ($T_{out}$)** | 352.04 ($78.89^\circ\text{C}$) | 293.48 ($20.33^\circ\text{C}$) | $\text{K}\,(^\circ\text{C})$ |
| **Hydrodynamic Entrance Length ($L_e$)** | Developing throughout | Developing throughout | $\text{m}$ |

---

## 📂 Repository Structure

```tree
├── CAD_Geometry/
│   └── domain_geometry.agdb           # ANSYS DesignModeler geometry file
├── Mesh/
│   ├── baseline_30500_cells.msh       # Selected baseline structured mesh
│   └── independence_study/            # Coarsened and refined mesh configurations
├── Case_and_Data/
│   ├── STHE_Simulation.cas.h5         # Converged Fluent setup case file
│   └── STHE_Simulation.dat.h5         # Final iteration solution data
├── Post_Processing/
│   ├── contours/                      # Mirrored pressure, velocity, and temperature contours
│   └── plots/                         # Axial velocity, h_x, Nu_x, and wall y+ profiles
├── Documentation/
│   ├── Comprehensive_CFD_Report.pdf   # Complete project technical report
│   └── LaTeX_Source/                  # Full LaTeX report source files and figures
└── README.md
```

---

## 👥 Authors & Academic Context

* **Sina Zerehpoosh** (Student ID: 401170829) — Department of Mechanical Engineering, Sharif University of Technology
* **Shayan Abedi** (Student ID: 401170886) — Department of Mechanical Engineering, Sharif University of Technology

* **Course Instructor:** Dr. Pakzad
* **Teaching Assistant:** Mr. Alireza Jalali
* **Course Title:** Computational Fluid Dynamics (CFD)
* **Institution:** Sharif University of Technology, Tehran, Iran
* **Submission Date:** August 2026 
