# Qlkit python library

<p align="center">
 <img alt="CI build" src="https://github.com/mspronesti/qlkit/actions/workflows/build-and-test.yml/badge.svg"/> 
 <!-- <img alt="License"  src="https://img.shields.io/github/license/mspronesti/qlkit"/> -->
 <!-- <img alt="Release"  src ="https://img.shields.io/github/v/release/mspronesti/qlkit"/> -->
</p> 


Qlkit (pronounced cool-kit) is a simple python library implementing well-know supervised and unsupervised machine learning algorithms for a gated quantum computer, build with [Qiskit](https://github.com/Qiskit/qiskit)

## Install Qlkit
Install Qlkit running 
```bash
sudo python3 setup.py install
```
In the near future, we will make it installable via pip.

## Getting started with Qlkit
Now that Qlkit is installed, it's time to begin working with the Machine Learning module. 
Let's try an experiment using the KNN Classifier algorithm to train and test samples from a 
data set to see how accurately the test set can be classified.

```python
from qlkit.algorithms import QKNeighborsClassifier
from qlkit.encodings import AmplitudeEncoding
from qiskit import BasicAer
from qiskit.utils import QuantumInstance, algorithm_globals

from qlkit.datasets import load_iris

seed = 42
algorithm_globals.random_seed = seed

quantum_instance = QuantumInstance(BasicAer.get_backend('qasm_simulator'),
                                   shots=1024,
                                   optimization_level=1,
                                   seed_simulator=seed,
                                   seed_transpiler=seed)

encoding_map = AmplitudeEncoding()

qknn = QKNeighborsClassifier(
    n_neighbors=3,
    quantum_instance=quantum_instance,
    encoding_map=encoding_map
)
# Use iris data set for training and test data

train_size = 32
test_size = 8
num_features = 4 # all features
 

X_train, X_test, y_train, y_test = load_iris(train_size, test_size, num_features)
qknn.fit(X_train, y_train)

print(f"Testing accuracy: "
      f"{qknn.score(X_test, y_test):0.2f}")
```

## Developement notes

After cloning this repository, create a virtual environment

```bash
python3 -m venv .venv
```

and activate it

```bash
source .venv/bin/activate 
```

now you can install the requirements

```bash
pip install -r requirements-dev.txt
```

now run the tests

```bash
python -m pytest
```
