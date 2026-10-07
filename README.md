#qsim - A Quantum simulator from scratch

A very minimal quantum simulator built from scratch using numpy for my own understanding and self-learning, helps me also to decode whats happening under the hood of Qiskit.

### Features ###

-> n-qubit state vectors

-> Single-qubit gates:

	X gate
	Y gate
	Z gate
	H gate
	I gate
	R(theta)

-> Two-qubit gates:

	CNOT
	CZ

-> Gate application via numpy Kron function

-> Measurement done multiple times (shots count), qiskit style

### HOW IT WORKS? ###

The state 2 qubits can have 4 possible outcomes, which are <|00>,|01>,|10>,|11>>. The simulator stores one number for each state called amplitudes. For eg: |00> -> [1,0,0,0]. 

The example shows that the amplitude for the 1st possible state is 1 while the rest have 0 as their amplitude. Probability of a state to be measured when a measurement is done is calculated as |amplitude|^2. Therefore the example means that the probability to get |00> as the outcome is 100%.

**Gates.** Every gate (H, X, Z…) is a small 2×2 matrix. Applying a gate means multiplying the state by that matrix.

The catch is that a 2×2 matrix doesn't fit a list of 4 numbers. So to apply a gate to just one qubit, I combine it with "do nothing" matrices (I) for every other qubit, using the tensor product. For example, H on qubit 0 of 2 qubits becomes H ⊗ I, a 4×4 matrix that fits.

**CNOT.** CNOT means "if the control qubit is 1, flip the target". I built it by splitting it into the two cases and adding them:

- control is 0 → leave the target alone
- control is 1 → flip the target with X

### EXAMPLES ###

###  Bell state (entanglement)
```python
bell = QuantumCircuit(2)
bell.apply(H, 0)    # put qubit 0 in superposition
bell.cnot(0, 1)     # entangle it with qubit 1
print(bell.measure(1000))
```
Output: roughly `{'00': 500, '11': 500}`.
You only ever get 00 or 11, never 01 or 10. The two qubits always agree, which is entanglement.

### Quantum teleportation
Sends a qubit's state from qubit 0 to qubit 2 using a shared Bell pair, without moving the qubit itself.

To check it worked, I prepared qubit 0 with a rotation by angle θ, so the chance of reading 1 is sin²(θ/2). After teleportation, qubit 2 gives exactly that same probability for any θ I tried.

Normally teleportation needs a mid-circuit measurement plus corrections based on the result. Here I used the *deferred measurement principle*: swapping those for controlled gates (CNOT and CZ), which gives the same result.

## Run it
```
pip install numpy
```
Then open `qsim.ipynb` in VS Code or Jupyter and run all cells.


