# class_double_ide_type2

### Double-Exponential Interacting Dark Energy

Based on **CLASS v2.9.3**.

This repository contains a modified version of the **Cosmic Linear Anisotropy Solving System (CLASS)** implementing an **interacting dark energy (IDE) model**. The model features a quintessence scalar field governed by a **double-exponential potential** alongside a **pure momentum-transfer coupling** between dark matter and dark energy.

This codebase builds upon the [`class_IDE`](https://github.com/Alkistis/class_IDE) implementation by Alkistis Pourtsidou. The momentum-transfer coupling framework follows the **Type 3 theories** introduced by [Pourtsidou, Skordis, and Copeland (2013)](https://arxiv.org/abs/1307.0458), with implementation details for quadratic coupling functions further detailed in [Pourtsidou and Tram (2016)](https://arxiv.org/abs/1604.04222).

---

## Dark-Sector Model

### Coupling Mechanism & Interacting Physics

The interaction in this model belongs to **Type 3 momentum-transfer couplings**. Unlike standard coupled dark energy models (such as Type 1 or Type 2 implementations) where energy is exchanged directly between dark matter and dark energy at the background level, pure momentum-transfer couplings behave distinctly:
* **Background Dynamics:** There is no energy exchange between dark matter and dark energy ($Q_0 = 0$). Consequently, unpolarized background energy densities `rho_cdm` and `rho_phi` evolve independently, following standard energy conservation laws:

$$
\dot{\rho}_{\text{cdm}} + 3 H \rho_{\text{cdm}} = 0
$$
* **Linear Perturbations:** Energy transfer vanishes at linear order, but momentum transfer occurs dynamically through gradient interactions between the scalar field $\phi$ and dark matter velocity perturbations.
* **Coupling Function:** The coupling is constructed via a quadratic interaction function of the form $$\beta Z^2$$, where $Z$ parameterizes the dark matter four-velocity and scalar field derivative contractions. The coupling strength is set by the parameter `scf_veta`, corresponding directly to the coupling parameter $\beta$.

Setting `scf_veta = 0` completely deactivates the interaction, recovering uncoupled double-exponential quintessence.

---

### Quintessence Potential

While the baseline `class_IDE` implementation uses a single-exponential potential $$V(\phi) = V_0 e^{-\kappa \lambda \phi}$$, this modified code incorporates a **double-exponential potential**:

$$
V(\phi) = V_1 \left( e^{-\kappa \lambda_1 \phi} + A e^{-\kappa \lambda_2 \phi} \right)
$$

where $\kappa = \sqrt{8\pi G}$ and the relative normalization amplitude $A$ is defined as:

$$
A = \frac{V_2}{V_1}
$$

The scalar field evolution is controlled by two distinct slope parameters, $\lambda_1$ and $\lambda_2$.

---

### Effective Slope

The effective slope of the potential, $$\lambda_{\mathrm{eff}}$$, is evaluated as a derived quantity:

$$
\lambda_{\mathrm{eff}} = -\frac{1}{\kappa V} \frac{dV}{d\phi} = \frac{\lambda_1 e^{-\kappa \lambda_1 \phi} + A \lambda_2 e^{-\kappa \lambda_2 \phi}}{e^{-\kappa \lambda_1 \phi} + A e^{-\kappa \lambda_2 \phi}}
$$

For the double-exponential potential, $\lambda_{\mathrm{eff}}$ dynamically evolves across cosmic time as the scalar field transitions between the dominant exponential regimes.

---

## Modifications to CLASS

This implementation updates the background scalar field equations, initial conditions, derived parameter allocations, and linear perturbation routines within CLASS.

### Core Modified Files
* `source/input.c` — Parses user parameters for the double-exponential potential ($\lambda_1, \lambda_2, A$) and the momentum-transfer parameter (`scf_veta`).
* `include/background.h` — Expands internal structures for background variables, effective slope calculations, and potential tracking.
* `source/background.c` — Implements background integration of $V(\phi)$, $V'(\phi)$, and $\lambda_{\mathrm{eff}}(\phi)$.
* `source/perturbations.c` — Modifies fluid and scalar field perturbation equations to incorporate pure momentum-transfer terms ($\beta Z^2$ interaction).

> **Note:** Search the repository codebase for `scf_veta` to locate all explicit momentum-transfer additions.

---

### Nonlinear Corrections & HMcode

This repository allows non-linear power spectrum modifications using `HMcode`.

* **Caution:** `HMcode` fitting formulas were calibrated under standard $\Lambda\text{CDM}$ and non-interacting smooth dark energy models. Applying non-linear halo models to pure momentum-transfer interacting dark energy should be interpreted with care.

---

## Installation & Execution

### Prerequisites
A C compiler (`gcc` or `clang`) and standard `make` build utilities.

### Compilation
Build CLASS from the repository root:

```bash
make clean
make -j
```
## Running the Code

Run a test simulation using the provided parameter file:

```bash
./class Double_parameters.ini
```
### References:
* Pourtsidou, A., Skordis, C., & Copeland, E. J. (2013) — Models of coupled dark matter to dark energy, Phys. Rev. D 88, 083505.
* Pourtsidou, A., & Tram, T. (2016) — Reconciling CMB and structure growth measurements with dark energy interactions, Phys. Rev. D 94, 043518.
* Lesgourgues, J. (2011) — Cosmic Linear Anisotropy Solving System (CLASS) I: Overview, arXiv:1104.2932.
