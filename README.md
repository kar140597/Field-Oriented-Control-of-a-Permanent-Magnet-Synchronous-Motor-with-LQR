# Optimal LQI-Based Field-Oriented Control (FOC) for PMSM Drives

A modern state-space approach to Field-Oriented Control (FOC) for Permanent Magnet Synchronous Machines (PMSMs). This project moves beyond standard cascaded Proportional-Integral (PI) control loops by designing, tuning, and benchmarking a **Linear Quadratic Regulator with Integral Action (LQI)** within the synchronous $d$-$q$ reference frame.

---

## 📌 Project Overview

Standard industrial implementations of FOC predominantly rely on classical PI loops. However, during rapid speed and torque transients, cross-coupling terms ($-\omega_e L_q i_q$ and $\omega_e L_d i_d + \omega_e \lambda_{pm}$) introduce significant disturbances, leading to $d$-axis current excursions, integrator lag, and degraded tracking performance.

This project implements a full MIMO state-space controller with integral augmentation to:
- Formulate the coupled electrical dynamics of a PMSM in the synchronous $d$-$q$ domain.
- Systematically synthesize optimal feedback ($K_s$) and integral ($K_{IA}$) gain matrices via **Bryson's inverse-square weighting rule**.
- Evaluate transient decoupling, tracking recovery, and sensitivity against control saturation, sensor noise, and parameter deviations.

---

## 🚀 Key Features

* **State-Space LQI Architecture:** Augmented state formulation incorporating tracking error integrators to ensure zero steady-state error under parameter uncertainty.
* **Systematic Tuning via Bryson’s Rule:**
  $$\mathbf{Q} = \frac{1}{n} \operatorname{diag}\left(\frac{1}{\varepsilon_1^2}, \dots, \frac{1}{\varepsilon_5^2}\right), \quad \mathbf{R} = \frac{1}{u_{\max}^2} \mathbf{I}$$

---

## 📊 Performance & Sensitivity Analysis

Three tuning regimes were evaluated over torque-step profiles ($i_q^* = 5\text{ A} \to 40\text{ A}$, speed scaling from $\approx 200\text{ RPM}$ to $1350\text{ RPM}$):

| Case | $u_{\max}$ | $R_{ii}$ | $\varepsilon_{4,\max}$ ($e_{\int,d}$) | $\varepsilon_{5,\max}$ ($e_{\int,q}$) | Observed Performance |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Iter. 1** | $20.0$ | $0.0025$ | $0.001$ | $0.001$ | Aggressive tracking, minimal $i_d$ excursion ($\le 1\text{ A}$), visible high-frequency chatter |
| **Iter. 2** | $2.12$ | $0.2225$ | $0.001$ | $0.001$ | **Optimal balanced design:** smooth actuation, clean noise rejection, safe transient peaks |
| **Iter. 3** | $2.12$ | $0.2225$ | $0.100$ | $0.001$ | Relaxed $d$-axis error weight: severe cross-coupling spikes ($-25\text{ A}$ during unloading) |

---

## 📂 Repository Structure

```text
├── models/
│   ├── FOC_Model.slx           # Simulink model with LQI FOC architecture
│  
├── scripts/
│   ├── FOC_Main.m               # Bryson's rule script & Riccati solver
│   └── FOC_func.m              # Function script generation
├── docs/
│   ├── FOC.pdf                # Detailed analytical report
│   └── Simulink model and Inverse Park Transform Scips/               # Scope captures and step response waveforms
└── README.md
