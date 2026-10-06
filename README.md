# SYMMETRIA-AI

**Geometry-Constrained AI for Hidden Symmetry Discovery and Verification in Deformed Black-Hole Geodesics**

SYMMETRIA-AI is a computational framework designed to investigate hidden geometric structures in black-hole spacetimes through the integration of artificial intelligence, geodesic dynamics, and differential geometry.

The project focuses on identifying candidate Carter-like invariants from geodesic trajectories and evaluating whether the discovered structures correspond to genuine or approximate geometric symmetries.

## Motivation

The Kerr spacetime represents one of the most important examples of an integrable relativistic system. Its geodesic dynamics admit conserved quantities associated with spacetime symmetries, including the Carter constant arising from a hidden rank-2 Killing tensor.

When the Kerr geometry is deformed, however, this hidden structure may persist approximately, change its form, or disappear entirely. Determining this transition is important for understanding the relationship between spacetime geometry, integrability, and chaotic orbital dynamics.

SYMMETRIA-AI approaches this problem by combining data-driven discovery with explicit geometric verification.

## Core Idea

The framework follows three main stages:

1. **Geodesic Generation**  
   Simulate geodesic trajectories in Kerr and deformed black-hole geometries.

2. **Invariant Discovery**  
   Use a geometry-constrained AI model to identify candidate quantities that remain approximately conserved along the trajectories.

3. **Geometric Verification**  
   Reconstruct candidate tensor structures and evaluate their consistency with the Killing tensor equation and geodesic conservation.

This allows candidate symmetries to be classified as:

- **Exact**
- **Approximate**
- **Broken**

## Physical Benchmark

For the undeformed limit,

\[
\epsilon = 0,
\]

the framework uses the Kerr spacetime as the primary benchmark, where the existence of the Carter constant and the associated Killing tensor is known analytically.

The deformation parameter \(\epsilon\) is then varied to investigate how hidden symmetry and integrability evolve away from the Kerr limit.

## Evaluation

Candidate structures can be evaluated using metrics such as:

- tensor reconstruction error,
- Killing-equation residual,
- invariant drift along geodesics,
- consistency across multiple initial conditions,
- indicators of integrable or chaotic orbital behavior.

The central objective is not merely to predict trajectories, but to determine whether a discovered dynamical regularity can be connected to an underlying geometric symmetry.

## Research Direction

SYMMETRIA-AI explores the intersection of:

- General Relativity
- Black-Hole Physics
- Differential Geometry
- Dynamical Systems
- Scientific Machine Learning
- AI-Assisted Scientific Discovery

The broader goal is to investigate whether artificial intelligence can function not only as a numerical predictor, but also as a tool for discovering and testing physically meaningful mathematical structures.

## Status

Research prototype / experimental framework.

Development is focused on validating the methodology using Kerr spacetime before extending the analysis toward progressively deformed geometries.

## License

This repository is intended for research and educational purposes.
