<h1 align="center">EigenProbe</h1>
<p align="center"><i>A practical and precise toolkit for optical scanned probe microscopies, s-SNOM, nano-FTIR, nano-IR, and beyond.</i></p>

<p align="center">
    <img src="./Documentation/EigenProbe_Logo.png" title="" alt="" width="400">
</p>

* [1. Introduction ♟️](#1-introduction)
* [2. Quick start 🚀](#2-quick-start)
* [3. Features ✅](#3-features)
  * [3.1 Build an electromagnetic model of an optical scanned probe 💻](#31-build-an-electromagnetic-model-of-an-optical-scanned-probe)
  * [3.2 Examine the optical response of materials 🔦](#32-examine-the-optical-response-of-materials)
  * [3.3 Simulate near-field interactions between an optical probe and nearby surfaces ⚡️](#33-simulate-near-field-interactions-between-an-optical-probe-and-nearby-surface)
  * [3.4 Recover optical properties from nano-imaging and -spectroscopy 📈](#34-recover-optical-properties-from-nano-imaging-and--spectroscopy)
* [4. Getting started 🏁](#4-getting-started)
* [5. Coming soon 🛠️ 🔜](#5-coming-soon)

## 1. Introduction ♟️

Optical scanned probes enable super-resolution optical imaging and spectroscopy of nano-scale materials - *optical nanoscopy*.  However, quantitative prediction and interpretation of these experiments usually demands a multi-scale physics solution that is easily beyond the reach of simple analytic models.  Nanoscopies can exploit both the *antenna* characteristics of an elongated probe together with the nanometer-scale confinement of light at its sharp apex.   Not only can an optically driven sharp probe excite a surface with intense nano-focused light, but scattering of light by the probe shows fine sensitivity to properties of the surface.  `EigenProbe` is a multi-scale physics solution that describes these aspects and more, allowing to predict and interpret optical nanoscopies of materials within a convenient software package.

The all-python `EigenProbe` package compactly describes mutual electromagnetic interactions between a metallic probe, an optical surface, and incoming/outgoing radiation, in terms of mathematically defined *eigenmodes*.  Much like the modes of an optical cavity, a combination of *eigenmodes* fully describes the electromagnetic field excited in the gap between a *nanoscopy probe* and a nearby optical medium -  a material surface, layered heterostructure, or even a nano-structure!  `EigenProbe` illuminates the connection between nanoscopy measurements of scattered fields, optical forces, absorption by materials, *etc.*, and the underlying properties of materials under study.  Through optimized numerics, `EigenProbe` further provides an interface to solve *inverse problem*s of optical nanoscopy, including direct recovery of optical constants from through nano-imaging or -spectroscopy of nanoscale materials.

> **Read more:** To learn about the efficient physical formalism implemented by `EigenProbe`, refer to our detailed pre-print on arXiv:
> 
> Allen, B.; Lukaskawcez, B.; Bragg, A.; Hirshberg, N.; Fogler, M.; Basov, D. N.; Gilbert-Corder, S.; Bechtel, H.; McLeod, A. S. *Quantitative Infrared Nanoscopy: Probe-Cavity Eigenmodes and Nano-Gap Polaritons for Strongly Coupled Nanoscale Optics*. arXiv July 27, 2026. https://doi.org/10.48550/arXiv.2607.23950.

## 2. Quick start 🚀

As a simple example, the following is all you need to simulate an infrared nano-spectroscopy experiment that measures light scattered from a sharp metal probe "tapped" over the surface of a hexagonal boron nitride crystal (as in infrared *scattering-type scanning near-field optical microscopy*, or *s-SNOM*):

```
# --- Build the probe ---
import EigenProbe as EP
P=EP.Probe(name='MyProbe',taper_angle=25)
P.solve_eigenmodes(plot=False);

# --- Simulate the experiment ---
import numpy as np
from EigenProbe.Materials import BN_GPR as hBN,Au
light_energies = np.linspace(500,1700,100) # in wavenumbers
experiment = P.getNormalizedSignal(light_energies,Nkappas=244*2,
                                   rp=hBN.reflection_p,
                                   rp_norm = Au.reflection_p)
#--- Plot the result ---
from matplotlib import pyplot as plt
plt.figure(figsize=(6,4))
for harmonic in [1,2,3]:
    np.abs(experiment['Sn_norm'][harmonic]).plot(label='$S_%i$'%harmonic)
plt.title('Demodulated probe-scattered light, hBN / Au')
plt.legend(loc=(.1,.02),ncol=3)
```

This example exploits the built-in library of `EigenProbe.Materials` to compute the energy-resolved interaction between a boron nitride surface and a realistically modeled, pyramidal, sharp metallic probe.  The code above will immediately predict several (normalized) scattering spectra and generate a publication-quality plot:

<p align="center">
<img title="" src="./Documentation/img/hBN_spectra.png" alt="" width="400">
</p>

## 3. Features ✅

> **Learn more:** `EigenProbe` conveniently packages a series of several included <mark>jupyter</mark> <mark>notebooks</mark>, each showcasing one of the features described below. These notebooks include documentation, and serve as brief tutorials. 
> 
> ***Take a look:***   <mark>EigenProbe Notebooks</mark>

### 3.1 Build an electromagnetic model of an optical scanned probe 💻

![](/Users/alexandersmcleod/Library/CloudStorage/Dropbox/Git/EigenProbe/Documentation/img/Eigenmode_Fields.jpg)

| ![](/Users/alexandersmcleod/Library/CloudStorage/Dropbox/Git/EigenProbe/Documentation/img/Eigenvalues_vs_gap.png) | <img title="" src="./Documentation/img/Eigenmode_Radiation.png" alt="" width="607"> |
| ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |

### 3.2 Examine the optical response of materials 🔦

![](/Users/alexandersmcleod/Library/CloudStorage/Dropbox/Git/EigenProbe/Documentation/img/TransferMatrix.png)

### 3.3 Simulate near-field interactions between an optical probe and nearby surfaces ⚡️

| ![](/Users/alexandersmcleod/Library/CloudStorage/Dropbox/Git/EigenProbe/Documentation/img/SpectrumSumsEigenmodes.jpg) | <img title="" src="./Documentation/img/Barycentric.jpg" alt="" width="1800"> |
| --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |

![](/Users/alexandersmcleod/Library/CloudStorage/Dropbox/Git/EigenProbe/Documentation/img/UnconventionalProbe_doube.jpg)

blah

### 3.4 Recover optical properties from nano-imaging and -spectroscopy 📈

<!-- EigenProbe-as-service_Si-SiO2grating.mp4 -->

| <img src="./Documentation/Movies/FittingPMMA.gif" title="" alt="" width="1200"> | ![](/Users/alexandersmcleod/Library/CloudStorage/Dropbox/Git/EigenProbe/Documentation/img/PMMA_RecoveredPermittivity.jpg) |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |

| <img src="./Documentation/Movies/FittingSiO2.gif" title="" alt="" width="1700"> | ![](/Users/alexandersmcleod/Library/CloudStorage/Dropbox/Git/EigenProbe/Documentation/img/SiO2_RecoveredPermittivity.jpg) |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |

<p align="center">
   <img title="" src="./Documentation/img/EigenProbe_as_service.jpg" title="" alt="" width="500">
</p>

https://github.com/user-attachments/assets/450f8412-f051-43f9-aafa-3d0de09d1734

## 4. Getting started 🏁

## 5. Coming soon 🛠️ 🔜

![](/Users/alexandersmcleod/Library/CloudStorage/Dropbox/Git/EigenProbe/Documentation/img/EigenProbeImaging.jpg)
