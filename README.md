# class_double_ide_type2

Based on **CLASS v2.9.3**.

This repository contains the modified CLASS code for an coupled quintessence model with a double-exponential potential:

$$V(\phi) = V_1 \left(e^{-\kappa\lambda_1\phi} + A e^{-\kappa\lambda_2\phi}\right)$$

where:

$$A = \frac{V_2}{V_1}$$

The starting point is the `class_IDE` CLASS fork by Alkistis Pourtsidou.

---

### Modifications to the code to include the double-exponential potential

The quintessence potential is modified from the single exponential to the double-exponential form given above. The effective slope of the potential is also included as a derived quantity:

$$\lambda_{\rm eff} = -\frac{1}{\kappa V}\frac{dV}{d\phi}$$

For more details on the implementation, see the modifications in `input.c`, `background.h`, `background.c`, and `perturbations.c`.

The test run file is `Double_parameters.ini`, so you need to compile and then run:

```bash
./class Double_parameters.ini
