# Quantum Optical Mirror Memory Simulator (V1)

An interactive, educational Jupyter notebook for exploring how **optical loss in an idealized free-space multipass mirror memory** affects three quantum optical resources:

* **Single-photon Fock state** (|1\\rangle): photon retrieval probability, vacuum contribution, and output purity.
* **Single-mode squeezed vacuum**: covariance-matrix evolution, retrieved squeezing (dB), and purity.
* **Two-mode squeezed vacuum**: Gaussian entanglement, logarithmic negativity, purity, and EPR quadrature variances when one or both modes are stored.

The notebook is designed to make the connection between optical geometry, storage time, transmission, and quantum-state quality explicit. It is a **teaching model**, not a hardware-calibrated simulation or prediction for any particular commercial device.

## Files

|File|Description|
|-|-|
|`sim_quantum_memory_V1.ipynb`|Interactive simulator, physics modules, plots, and validation tests.|
|`quantum_optical_memory_educational_brief.pdf`|Theory, numerical benchmark, educational exercises, and expected answers.|
|`requirements.txt`|Python dependencies.|
|`LICENSE`|MIT license.|

## Quick start

Python **3.10 or later** is recommended. From the repository directory:

```bash
python -m venv .venv
# Linux/macOS:
source .venv/bin/activate
# Windows PowerShell:
# .venv\\Scripts\\Activate.ps1

python -m pip install -r requirements.txt
jupyter lab
```

Open **`sim\_quantum\_memory\_V1.ipynb`** in JupyterLab, select the environment's Python kernel, and run the cells **in order**. The final cell displays the `ipywidgets` control panel. Adjust the optical parameters, select a quantum state, and inspect the numerical diagnostics and plots. Notebook cells include validation assertions; check that these pass before interpreting the interactive results.

> \*\*Widget note:\*\* The final UI is interactive only in an active notebook with working Jupyter widgets. A static notebook preview on GitHub will not reproduce the live sliders and dropdowns.

## Model and conventions

For mirror power reflectivity (R), input/output coupling efficiencies (\\eta\_{\\mathrm{in}},\\eta\_{\\mathrm{out}}), effective optical path per reflection (L), and number of reflections (N):

\[
\\eta(N) = \\eta\_{\\mathrm{in}}\\eta\_{\\mathrm{out}}R^N,
\\qquad t(N) = \\frac{NL}{c}.
]

When the **requested storage time** is specified, the simulator uses the smallest nonnegative integer (N) reaching that time, so the actual simulated duration may be slightly longer. The optical path **per reflection is not the device footprint**.

The quantum evolution is an **ideal pure-loss channel**, optionally followed by a deterministic phase rotation:

* **Fock input:** (\\rho\_{\\mathrm{out}}=(1-\\eta)|0\\rangle\\langle0|+\\eta|1\\rangle\\langle1|). Photon retrieval probability is (\\eta); the loss-only conditional state, when a photon survives, is unchanged.
* **Squeezed input:** (V\_{\\mathrm{out}}=\\eta,\\mathcal R(\\phi)V\_{\\mathrm{in}}\\mathcal R(\\phi)^T+(1-\\eta)I/2), using vacuum quadrature variance (1/2).
* **Two-mode squeezed input:** independent loss maps are applied to either one or both modes; Gaussian covariance matrices yield logarithmic negativity and EPR variances.

**Scope:** The model neglects phase *noise*, mode mismatch, dispersion, finite bandwidth, active switching, detector imperfections, and hardware-specific engineering constraints. A fixed phase rotation changes the orientation of quadratures but does **not** by itself destroy entanglement. A free-space reflection-loss trend should **not** be extrapolated directly to photonic integrated circuits (PICs), whose loss mechanisms are different.

## Reproducibility benchmark

Set:

|Parameter|Value|
|-|-:|
|Mirror reflectivity (R)|0.9999|
|Injection/extraction efficiencies|0.99 / 0.99|
|Optical path per reflection (L)|1.0 m|
|Number of reflections (N)|1000|
|Phase per reflection|0 rad|
|Single-mode input squeezing|10 dB|
|Two-mode squeezing parameter (r)|0.8|

Expected results (rounded):

|Quantity|Expected|
|-|-:|
|Storage time|3.33564 µs|
|Total transmissivity (\\eta)|0.886827|
|Fock output purity|0.799270|
|Output squeezing for 10 dB input|6.9496 dB|
|Squeezed-state output purity|0.742688|
|Two-mode log negativity (both modes stored, (r=0.8))|1.774871|
|Two-mode output purity (both stored)|0.759506|

**Important:** The UI uses a *single input squeezing slider* for both single-mode and two-mode cases, converted as (r = s\_{\\mathrm{dB}}\\ln(10)/20). Thus, to reproduce the **two-mode (r=0.8)** benchmark in the UI, set the slider to approximately **6.949 dB**, rather than 10 dB. Alternatively, use the notebook's underlying `entanglement\_diagnostics(memory, 1000, 0.8, "both")` directly.

For an exact (N=1000) benchmark use the physics functions (or the reflection-count interface if present). With the V1 final UI's **time-based slider**, requesting 3.0 µs instead produces (N\\approx900) and different results.

## Suggested learning activities

1. **Time versus survival:** Increase requested storage time at fixed (R) and (L). Predict photon retrieval and output purity before using the slider.
2. **Geometry versus loss:** At fixed requested time, compare (L=0.5) m with (L=2.0) m. Explain the change in reflection count and why this is a free-space mirror-model result, not a general PIC scaling law.
3. **Loss of squeezing:** Compare 10 dB input squeezing with the retrieved squeezing after 1000 reflections. Determine the limiting state when transmissivity approaches zero.
4. **Storage of entanglement:** Compare storing one versus both halves of a two-mode squeezed state. Identify which metrics are affected by local deterministic phase rotations.

See the accompanying educational brief for theory, worked benchmarks, and expected exercise results.

## Dependencies

The numerical backend uses **NumPy** and **Matplotlib**. The interactive display uses **ipywidgets** and **IPython**; **JupyterLab** provides the notebook environment. No QuTiP or commercial SDK is required.

## License

Released under the **MIT License**. See [LICENSE](LICENSE).

Copyright © 2026 Boris Kiefer.

