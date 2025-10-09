# master-sciml

This repository contains the materials, code implementations, and literature reviews for the Directed Study course in Scientific Machine Learning (SciML), as part of the Master's program in Computer Science.

The primary goal of this course is to explore the intersection of machine learning and scientific computation, focusing on modern techniques to model, simulate, and analyze complex physical systems from data.

## Syllabus

The course is structured into several key modules, each covering a fundamental area of SciML. Each module includes theoretical concepts, a primary reading assignment, and a hands-on implementation project.

0. Scientific Computer: Review of Scientific Computing concepts.
1. Equation Discovery: Concepts and Algorithms related to Symbolic Regression theory and Dynamical Systems Identification.
2. Dynamic Systems: Techniques for extracting coherent patterns (modes) and their dynamics (how they oscillate and grow/decay) from a dataset.
3. Probabilistic Modelling: Creation of surrogate (probabilistic) models and uncertainty quantification.
4. Neural Solvers: Formulation, architecture and training of Physics-Informed Neural Networks for direct and inverse problems.
5. Graph Learning: Simulation of particle systems and flows in unstructured meshes using Graph Neural Networks.
6. Neural Operators: Theory, architecture and applications of Neural Operators.

## Repository Structure

This repository is organized into folders, one for each module listed above. Inside each folder, you will find:

- `data/`: Training data for physics problems, generated numerically or experimentally (publicly available).
- `docs/`: The main research document(s) for the module.
- `notebooks/`: Jupyter notebooks with the code implementation for the module's project.
- `results/`: Some results generated in the module implementations.

## How to Use

To run the notebooks in this repository, it is recommended to create a dedicated Python environment.

### Clone the repository:

```bash
git clone https://github.com/thiagoneye/master-sciml.git
cd master-sciml
```

### Install the required dependencies:

```bash
pip install -r requirements.txt
```