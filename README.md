# Flow-Over-a-Cylinder---Von-Karman-Effect
## Project Overview
This repository contains the case setup, mesh files, and results for a 2D transient Computational Fluid Dynamics (CFD) simulation of flow past a circular cylinder using **ANSYS Fluent**. 

The primary objective of this project is to simulate and analyze the **Von Kármán vortex street** phenomenon, computing critical aerodynamic parameters such as the drag coefficient ($C_D$), lift coefficient ($C_L$), and the Strouhal number ($St$).

---

## Problem Specification

### Geometry & Domain
The computational domain is designed to ensure that boundary effects do not artificially influence the flow field around the cylinder.
* **Cylinder Diameter ($D$):** 1.0 m (Reference length)
* **Upstream Length:** $10D$ (Inlet to cylinder center)
* **Downstream Length:** $20D$ (Cylinder center to outlet)
* **Transverse Width:** $10D$ (Top and bottom boundaries)

### Mesh Details
A high-quality mesh is critical for resolving the boundary layer and the wake region.
* **Mesh Type:** Hybrid (Quad/Tri) or fully Structured Quad mesh.
* **Boundary Layer:** Inflation layers around the cylinder wall to ensure $y^+ \approx 1$.
* **Refinement:** High cell density in the wake region to capture vortex shedding accurately.

---

## Physics & Solver Settings

The simulation is set up to capture unsteady, laminar (or mildly turbulent) flow. For a classic Von Kármán vortex street, the Reynolds number ($Re$) is typically set around $100 - 150$.

| Parameter | Setting |
| :--- | :--- |
| **Solver Type** | Pressure-Based, 2D Planar |
| **Time Dependence** | Transient (Unsteady) |
| **Viscous Model** | Laminar (for $Re = 100$) / SST $k-\omega$ (for higher $Re$) |
| **Fluid Material** | Air (Density: $1.225 \text{ kg/m}^3$, Viscosity: $1.789 \times 10^{-5} \text{ kg/(m}\cdot\text{s})$) |

### Boundary Conditions
* **Inlet:** Velocity-Inlet (Constant uniform velocity based on target $Re$)
* **Outlet:** Pressure-Outlet (0 Pa gauge pressure)
* **Cylinder:** Wall (No-slip condition)
* **Top/Bottom Boundaries:** Symmetry or Slip-Wall

### Numerical Schemes
* **Pressure-Velocity Coupling:** PISO or SIMPLEC (PISO recommended for transient)
* **Spatial Discretization (Gradient):** Least Squares Cell Based
* **Spatial Discretization (Pressure):** Second Order
* **Spatial Discretization (Momentum):** Second Order Upwind
* **Transient Formulation:** Second Order Implicit

### Time Stepping
* **Time Step Size ($\Delta t$):** Calculated to maintain a Courant (CFL) number $< 1$. (e.g., $0.01$ s - $0.05$ s depending on velocity and mesh size).
* **Max Iterations per Time Step:** 20 - 30

---

## Post-Processing & Results
The simulation yields the following key insights:
1.  **Vortex Shedding:** Visualized using vorticity magnitude contours and velocity streamlines.
2.  **Aerodynamic Forces:** Monitor plots of $C_D$ and $C_L$ over flow time. The lift coefficient will exhibit a harmonic sinusoidal oscillation once shedding begins.
3.  **Strouhal Number ($St$):** Calculated using the frequency of vortex shedding ($f$), extracted via Fast Fourier Transform (FFT) of the $C_L$ history: 
    $$St = \frac{f \cdot D}{V}$$

---

## How to Run the Case

1. **Prerequisites:** You must have ANSYS Fluent installed (Version 2020 R1 or newer recommended).
2. **Launch Fluent:** Open ANSYS Fluent in 2D mode with Double Precision enabled.
3. **Load Case File:** Go to `File -> Read -> Case...` and select the provided `.cas` file.
4. **Initialization:** * Standard or Hybrid Initialization.
    * (Optional) Patch a slight cross-flow velocity perturbation behind the cylinder to trigger vortex shedding faster.
5. **Run Calculation:**
    * Navigate to `Run Calculation`.
    * Verify the time step size and number of time steps.
    * Click **Calculate**.

---

## Repository Structure

```text
├── geometry/           # Geometry files (.agdb, .step)
├── mesh/               # Mesh files (.msh)
├── setup/              # Fluent case files (.cas)
├── results/            # Data files (.dat) and exported data (CSV)
├── images/             # Contours, plots, and animations
└── README.md           # Project documentation
