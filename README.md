<h1 align="center" style="font-size100px;">🔧 SOLIDWORKS</h1>


# Beam Deflection Study – Finite Element Analysis (FEA)

## 1. Introduction and Objective

This project investigates the vertical deflection behavior of a cantilever beam subjected to two concentrated loads using both **classical beam theory (hand calculations)** and **Finite Element Analysis (FEA)** in **SOLIDWORKS Simulation**. The primary objective is to validate the analytical solution using superposition against numerical simulation results and to demonstrate how FEA can be used as a reliable engineering tool for predicting structural response.

This study answers:

* What was analyzed
* Why the analysis matters
* What the results mean
* How confident we are in the conclusions

---

## 2. Problem Description

### 2.1 Beam Geometry and Material

* **Beam type:** Cantilever beam (fixed at one end, free at the other)
* **Material:** 7079 Aluminum
* **Young’s Modulus (E):**

```text
E = 10.4 × 10^6 psi
```

* **Outside Diameter:** 2.0 in
* **Inside Diameter:** 1.5 in
* **Area Moment of Inertia (I):**

```text
I = 0.5369 in^4
```

The beam is modeled as a hollow circular tube, which is commonly used in lightweight structural applications due to its high stiffness-to-weight ratio.

---

### 2.2 Loading and Boundary Conditions

* The beam is **fully fixed at point O**, meaning it cannot translate or rotate.
* Two vertical downward point loads are applied:

```text
Load at A = 300 lbf (downward)
Load at B = 200 lbf (downward)
```

* Locations:

```text
Distance O → A = 2 ft = 24 in
Distance O → B = 4 ft = 48 in
```

---

## 3. Analytical (Hand) Calculation

### 3.1 Methodology

The analytical solution is based on **Euler–Bernoulli beam theory** and the **principle of superposition**. Because the beam remains within the elastic range and deformations are small, the total deflection at point B can be found by summing the individual deflections caused by each load acting independently.

---

### 3.2 Governing Equation

```text
y_B = - (F_B * l^3) / (3 * E * I)
      + (F_A * a^2) / (6 * E * I) * (a - 3 * l)
```

Where:

```text
F_B = load applied at point B
F_A = load applied at point A
l   = total beam length
a   = distance from fixed end to point A
E   = Young’s Modulus
I   = Area Moment of Inertia
```

---

### 3.3 Substitution of Values

```text
y_B = [ -200 * (4 * 12)^3 ] / [ 3 * (10.4 × 10^6) * (0.5369) ]
      + [ 300 * (2 * 12)^2 ] / [ 6 * (10.4 × 10^6) * (0.5369) ]
        * [ (2 * 12) - 3 * (4 * 12) ]
```

---

### 3.4 Analytical Result

```text
Deflection at point B ≈ -1.94 in
```

The negative sign indicates **downward deflection**, consistent with the direction of the applied loads.

---

## 4. Finite Element Analysis (FEA)

### 4.1 Software and Tools Used

```text
CAD & Simulation Software : SOLIDWORKS Simulation
Study Type                : Static Structural Analysis
Element Type              : Beam Elements
Solver                    : Linear Static Solver
```

These tools allow the beam to be discretized into finite elements, enabling numerical approximation of the governing equations of elasticity.

---

### 4.2 FEA Model Setup

* Geometry identical to analytical model
* Material assigned as 7079 Aluminum
* Fixed constraint applied at point O
* Point loads applied:

```text
300 lbf at point A
200 lbf at point B
```

* Adequate mesh refinement to ensure convergence and numerical accuracy

---

## 5. Results and Interpretation

### 5.1 Displacement Results

```text
Maximum displacement ≈ 1.94 in (at point B)
Minimum displacement ≈ 0 in (at fixed end)
```

The deformation plot exhibits smooth curvature, which is characteristic of bending-dominated cantilever behavior.

---

### 5.2 Comparison Between Analytical and FEA Results

```text
-----------------------------------------
Method              | Deflection at B (in)
-----------------------------------------
Hand Calculation    | 1.94
FEA (SOLIDWORKS)    | 1.94
-----------------------------------------
```

The strong agreement confirms:

* Correct analytical formulation
* Proper boundary conditions and loading in FEA
* Validity of linear elastic assumptions

---

## 6. Significance of the Study

This study demonstrates how **classical engineering theory and modern simulation tools complement one another**:

* Hand calculations provide physical insight and validation
* FEA provides visualization, scalability, and design flexibility

For non-technical stakeholders, this confirms:

* Predictable structural behavior
* Trustworthy simulation results
* Reduced design and safety risk

---

## 7. Engineering Insights and Observations

* Maximum deflection occurs at the free end, as expected
* The beam remains within the elastic range
* The hollow circular section provides excellent stiffness-to-weight efficiency

---

## 8. Applications of This Beam Configuration

Common applications include:

```text
• Aerospace structural members
• Robotic arms and manipulators
• Cantilevered signposts
• Lightweight structural frames
• Automotive and motorsport components
```

Deflection control in these systems is critical for alignment, performance, and safety.

---

## 9. Recommendations and Future Work

```text
• Perform stress and factor-of-safety analysis
• Optimize wall thickness and material selection
• Investigate dynamic and fatigue loading
• Study nonlinear behavior for large deflections
```

---

## 10. Conclusion

This beam deflection study successfully validates analytical predictions using Finite Element Analysis. The near-perfect agreement between theory and simulation confirms the accuracy of the model and highlights the importance of combining first-principles engineering with modern computational tools. The study provides a robust foundation for future structural design and optimization efforts.

---

**End of README Documentation**
