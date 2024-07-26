# BioRealistic-SRM-STDP-TensorFlow
This repository contains the implementation of a bio-realistic neuron model using TensorFlow, based on the Leaky Integrate-and-Fire (LIF) response model and Spike-Timing-Dependent Plasticity (STDP). The model simulates an array of neurons with detailed state variables and synaptic dynamics, capable of processing online input sequences. The development of this repository is based on this [work](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0001377).

Author: Kevin Juan Román Rafaele

## Table of Contents
1. Introduction
2. Theoretical Background
3. Model Description
4. Generating Input Sequences
5. Visualization Tools
6. Installation
7. Usage
8. Examples
9. Contributing
10. License
11. References

## Introduction
This project implements a biologically-inspired neuron model to simulate neural dynamics and learning processes. It aims to provide a detailed and flexible framework for studying bio-realistic neuronal behavior and synaptic plasticity using modern machine learning tools.

## Theoretical Background
### Leaky Integrate-and-Fire Model (LIF)
The LIF model is a simplified representation of neuronal activity that captures essential features of spiking neurons. It integrates incoming signals, accounts for the decay of potential over time (leakage), and generates an output spike when the membrane potential exceeds a threshold.

### Spike-Timing-Dependent Plasticity (STDP)
STDP is a biological learning rule observed in the brain, where the timing of spikes from pre- and post-synaptic neurons influences the strength of their connection. If a pre-synaptic neuron fires shortly before a post-synaptic neuron, the connection is strengthened (potentiation). Conversely, if the pre-synaptic neuron fires shortly after the post-synaptic neuron, the connection is weakened (depression).

## Model Description
The model simulates an array of neurons, each with detailed state variables and synaptic mechanisms. The neuron's behavior is governed by the following key equations:

### Membrane Potential Update
$$
um(t+1)=um(t)⋅e−Δtτm+K⋅∑iwi⋅xi(t)um​(t+1)=um​(t)⋅e−τm​Δt​+K⋅∑i​wi​⋅xi​(t)
$$

### Synaptic Potential Update
$$
us(t+1)=us(t)⋅e−Δtτs−K⋅∑iwi⋅xi(t)us​(t+1)=us​(t)⋅e−τs​Δt​−K⋅∑i​wi​⋅xi​(t)
$$

### Total Potential
$$
u(t+1)=um(t+1)+us(t+1)u(t+1)=um​(t+1)+us​(t+1)
$$

### Spike Generation
$$
if u(t+1)>θ and u(t+1)>u(t):if u(t+1)>θ and u(t+1)>u(t):
um(t+1)=−2⋅θum​(t+1)=−2⋅θ
us(t+1)=4⋅θus​(t+1)=4⋅θ
output=1output=1
else:else:
output=0output=0
$$

### Weight Update
$$
if output=1:if output=1:
Wi=Wi+dWp,iWi​=Wi​+dWp,i​
if Wi>1:Wi=1if Wi​>1:Wi​=1
dWp,i=0dWp,i​=0
dWd,i=AddWd,i​=Ad​
$$

### Potentiation and Depression Dynamics
$$
dWp,i(t+1)=dWp,i(t)⋅e−ΔtτpdWp,i​(t+1)=dWp,i​(t)⋅e−τp​Δt​
dWd,i(t+1)=dWd,i(t)⋅e−ΔtτddWd,i​(t+1)=dWd,i​(t)⋅e−τd​Δt​
$$

### Input-Triggered Updates
$$
if xi(t)=1:if xi​(t)=1:
dWp,i=ApdWp,i​=Ap​
Wi=Wi−dWd,iWi​=Wi​−dWd,i​
if Wi<0:Wi=0if Wi​<0:Wi​=0
dWd,i=0dWd,i​=0
$$

## Generating Input Sequences
### Poisson Point Process
The input sequences are generated using a Poisson point process, a stochastic process modeling the occurrence of events happening at a constant average rate, independently of the time since the last event. This is particularly suitable for simulating neural spike trains.

### Function Description
```
my_poisson(p_lambda, last_spike, rate, rate_derivative, rand_1, rand_2):
```
- Generates a spike train based on specified parameters.
- Parameters:
  - p_lambda: Base rate parameter of the Poisson process.
  - last_spike: Tracks time steps since the last spike.
  - rate: Current firing rate of the neuron.
  - rate_derivative: Rate of change of the firing rate.
  - rand_1, rand_2: Random values for variability in spike generation.
- Output: Returns a tuple (spike, last_spike, rate, rate_derivative).

## Visualization Tools
The visualization tools provide comprehensive insights into the neuron model's behavior over time, including:
1. Input Sequence and Potential Evolution:
   - Input Sequence: Scatter plot showing spikes over time.
   - Potential Evolution: Line plot of membrane potential over time.
2. Pattern and Weights:
   - Extended Pattern: Pixelated imshow plot of the extended input pattern.
   - Weights: Bar plot of synaptic weights aligned with the extended pattern.

These visualizations help analyze neuronal dynamics and synaptic adaptations.

## Installation
Clone the repository and install the required dependencies:
```
git clone https://github.com/yourusername/BioRealistic-SRM-STDP-TensorFlow.git
cd BioRealistic-SRM-STDP-TensorFlow
pip install -r requirements.txt
```

## Usage
Load the model and generate input sequences to simulate neuronal behavior. Use the provided visualization tools to analyze the results.
```
from neuron_model import BioRealisticNeuronLayer
from input_sequence import generate_input_sequences
from visualization import plot_visualizations

# Example usage
input_sequences = generate_input_sequences(...)
neuron_layer = BioRealisticNeuronLayer(...)
neuron_layer.simulate(input_sequences)
plot_visualizations(neuron_layer, input_sequences)
```

## Examples
Refer to the examples directory for detailed usage examples, including input sequence generation, neuron layer simulation, and visualization.

## Contributing
Contributions are welcome! Please read the CONTRIBUTING.md file for guidelines on how to contribute to this project.

## License
This project is licensed under the MIT License.

## References
1. [Leaky-Integrate and Fire](https://en.wikipedia.org/wiki/Biological_neuron_model#Leaky_integrate-and-fire)
2. [Spike-Timing Dependent Plasticity](https://en.wikipedia.org/wiki/Spike-timing-dependent_plasticity)
3. [Neuronal Dynamics by. W. Gerstner](https://neuronaldynamics.epfl.ch/online/index.html)
