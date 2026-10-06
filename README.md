# SYMMETRIA-AI

Geometry-constrained neural discovery of Carter-like hidden symmetries in deformed Kerr geodesics.

SYMMETRIA-AI is a computational experiment that investigates whether a neural network can recover tensor structures associated with conserved quantities in black-hole geodesic dynamics.

The main object learned by the model is a symmetric rank-2 contravariant tensor

$$
K^{\mu\nu}(r,\theta),
$$

which defines the quadratic quantity

$$
I = K^{\mu\nu}p_\mu p_\nu.
$$

A useful candidate should remain approximately conserved along geodesics and, after lowering its indices, approximately satisfy the Killing tensor equation.

The Carter constant is not provided to the neural network as a target. Instead, the model is trained from dynamical and geometric constraints and is later compared with the known Kerr result.

## Model

The neural network takes the normalized coordinates $(r,\theta)$ as input and predicts the ten independent components of a symmetric $K^{\mu\nu}$.

The configuration used for the reported experiments is:

- MLP architecture: `2 -> 24 -> 24 -> 10`
- activation: `tanh`
- parameters: 922
- numerical precision: `float64`
- 6 training geodesics
- 768 off-shell training probes
- Adam followed by L-BFGS optimization
- additional unsupervised SVD refinement of the output head

A projection and normalization step is used during training to suppress trivial solutions and simple combinations of already known conserved quantities.

The deformation values used in the experiment are:

```text
epsilon = 0, 0.01, 0.02, 0.05, 0.10
```

Each configuration is evaluated using model seeds `0`, `1`, and `2`.

The training configuration used for these experiments is stored in `FROZEN_PROTOCOL.json`.

## Verification

The final evaluation is implemented in `symmetria/verification_v2.py`.

Eight held-out orbit families are used at every value of epsilon. The same initial phase-space skeleton is retained between geometries, while $p_t$ is recomputed from the corresponding metric so that each orbit satisfies the unit-mass timelike mass-shell condition.

An orbit is included only when its complete numerical segment remains valid across all tested deformation values. Orbit selection does not depend on neural-network performance.

The verification also uses 512 common spacetime points for each epsilon.

### Killing residual

`R_K` measures the relative RMS residual of

$$
\nabla_{(\alpha}K_{\beta\gamma)} = 0
$$

in an orthonormal frame.

A smaller value indicates that the learned tensor is closer to satisfying the Killing tensor equation over the sampled domain.

### Invariant drift

`delta_I` measures the maximum relative change of

$$
I = K^{\mu\nu}p_\mu p_\nu
$$

along the held-out geodesics.

It is calculated as

$$
\delta_I =
\max_i
\frac{|I_i-I_0|}
{\max(1,|I_0|)}.
$$

At $\epsilon=0$, the learned tensor is also aligned with the analytical Kerr tensor so that its reconstruction error can be evaluated directly.

## Kerr comparator

The analytical Kerr tensor at $\epsilon=0$ is also evaluated in every deformed geometry without changing or refitting it.

This provides a fixed reference for studying how the original Kerr hidden symmetry behaves when the metric is deformed.

For the metric used here, the deformation is introduced through

$$
\epsilon
\left(\frac{M}{r}\right)^2
\frac{3\cos^2\theta-1}{2}
$$

in $g_{tt}$.

This is a controlled toy deformation used for the numerical experiment. It is not intended to represent a validated astrophysical black-hole solution.

## Exact audit

The repository also contains a symbolic check implemented in:

```text
symmetria/exactness_audit.py
```

For $M=1$ and $a=0.6$, evaluated exactly at

$$
r=8,
\qquad
\theta=\frac{\pi}{3},
$$

SymPy gives

$$
R_{(tr\phi)}
=
\frac{277127271}{84753091804160}\epsilon.
$$

This shows that the unchanged Kerr tensor does not remain an exact Killing tensor of the deformed metric when $\epsilon \neq 0$.

The result applies specifically to the fixed Kerr tensor. It does not prove that another epsilon-dependent exact Killing tensor cannot exist.

The tensors produced by the neural network are therefore treated as numerical candidates over the finite domain studied here rather than as globally certified exact solutions.

## Results

The main numerical outputs are stored in `results/`.

```text
results/
├── kerr_benchmark.csv
├── deformation_sweep.csv
├── final_summary.csv
├── comparison_summary.csv
├── detailed_metrics.json
├── exactness_audit.json
├── matched_verification_data.npz
└── verification_manifest.json
```

`kerr_benchmark.csv` contains the epsilon-zero benchmark for each model seed.

`deformation_sweep.csv` contains the results of the learned candidates and the fixed Kerr comparator across the deformation grid.

`final_summary.csv` contains the aggregate statistics used for reporting the experiment.

The main deformation figure is available at:

```text
figures/Figure_2_4_1_Deformation_Response.png
```

The Kerr benchmark is available in:

```text
figures/Table_2_4_1_Kerr_Benchmark.png
```

## Reproducing the verification

Python 3.10 or newer is recommended.

Create an environment and install the package:

```bash
python3 -m venv .venv
source .venv/bin/activate

python -m pip install -r requirements.txt
python -m pip install -e .
```

On Windows, activate the environment with:

```bash
.venv\Scripts\activate
```

Run the main tests with:

```bash
python -m unittest tests.test_verification_v2 tests.test_exactness_audit tests.test_neural tests.test_optimization -v
```

The matched verification can be reproduced from the included checkpoints using:

```bash
python -m symmetria.verification_v2 --source checkpoints --out verification_rerun
```

Regenerate the figures with:

```bash
python -m symmetria.figures_v2 --results verification_rerun --out figures_rerun
```

Run the symbolic audit with:

```bash
python -m symmetria.exactness_audit --out verification_rerun/exactness_audit.json
```

Existing result directories are not silently overwritten.

## Interpretation

At $\epsilon=0$, the learned candidates approach the known Kerr tensor while maintaining low Killing residuals and low invariant drift on held-out geodesics.

For non-zero deformation, the unchanged Kerr tensor no longer satisfies the same geometric condition exactly. Across the tested finite domain, the re-learned SYMMETRIA-AI candidates show lower invariant drift than the fixed Kerr comparator for all tested non-zero epsilon values.

The median Killing residual of the learned candidates is also lower than that of the fixed Kerr comparator at every tested non-zero deformation, although variation between individual seeds is still visible, particularly for small epsilon.

These results should be interpreted within the numerical scope of the experiment. They do not establish a global exact Killing tensor, prove the absence of hidden symmetry when optimization fails, or by themselves establish chaos or non-integrability.

## Repository structure 

```text
SYMMETRIA-AI/
├── README.md
├── LICENSE
├── CITATION.cff
├── FROZEN_PROTOCOL.json
├── MANIFEST_SHA256.json
├── requirements.txt
├── requirements-tested.txt
├── pyproject.toml
├── symmetria/
├── tests/
├── checkpoints/
├── results/
└── figures/
```

The source code is contained in `symmetria/`, while `tests/` contains the numerical and physics-related regression tests. Frozen model checkpoints are stored in `checkpoints/`, and the reported outputs and figures are provided in `results/` and `figures/`.
