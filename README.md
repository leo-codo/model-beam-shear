# Boundary Control of a Shear Beam Model via Finite Differences

This repository provides a Python implementation of a finite difference scheme
with boundary control for a shear-type beam model. The control is applied at the
boundary and computed implicitly at each time step through the solution of a
nonlinear scalar equation coupled with an elliptic problem.

The code is intended to illustrate stability, energy dissipation, and optimal
boundary control behavior at the discrete level.

---

## Mathematical Model

The model under consideration is given by
\[
\varphi_{tt} = \frac{\kappa}{\rho_1} (\varphi_x + \psi)_x,
\]
where:
- \(\varphi(x,t)\) is the transversal displacement,
- \(\psi(x,t)\) is an auxiliary elliptic variable,
- \(\kappa\) is the shear correction factor,
- \(\rho_1 = \rho A\) is the effective mass density.

The variable \(\psi\) is obtained at each time step by solving the elliptic
problem
\[
- \psi_{xx} + \kappa \psi = - \kappa \varphi_x,
\]
subject to Neumann boundary conditions with control:
\[
\psi_x(0,t) = u(t), \qquad \psi_x(L,t) = u(t).
\]

---

## Boundary Control Law

The boundary control is computed from the feedback relation
\[
u(t) = \frac{1}{\alpha}\big(\psi(L,t) - \psi(0,t)\big),
\]
which is enforced at each time step by solving a nonlinear scalar equation using
a root-finding procedure (Brent's method).

---

## Numerical Method

- Spatial discretization: second-order finite differences on a uniform grid  
- Time discretization: explicit second-order leapfrog scheme  
- Elliptic problem: solved with `scipy.linalg.solve_banded`  
- Boundary conditions: enforced using ghost points  
- Control computation: nonlinear scalar equation solved at each time step  

A CFL-type condition is required for stability:
\[
\Delta t \lesssim C_{\mathrm{CFL}} \, \Delta x
\sqrt{\frac{\rho_1}{\kappa}}.
\]

---

## Cost Functional

At the final time \(T\), the cost functional is given by
\[
J(u) =
\frac{1}{2}\int_0^L
\big(
\mu_1 \varphi^2(x,T) + \mu_2 \varphi_t^2(x,T)
\big)\,dx
+
\frac{\alpha}{2}\int_0^T u^2(t)\,dt.
\]

---

## Requirements

The following Python packages are required:

- numpy
- scipy
- matplotlib

Install them via:

```bash
pip install numpy scipy matplotlib
