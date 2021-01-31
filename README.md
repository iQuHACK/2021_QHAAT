# Quantum Autoencoder for Quantum Data Compression

## Introduction
 Quantum computing have been showing its tremendous advantages in many tasks, where it is slow or even impossible for classical algorithms to be employed, ranging from cryptography to quantum simulation. For example, once we have sufficiently powerful quantum computers, Shor's algorithm will factorize large numbers in minutes. Quantum simulators for chemistry have also been shown to be capable of efficiently calculating molecular energies for small system. In our NISQ area, however, the limited amount of quantum computing resources may prevent us from achieving desired results. Therefore, it is essential to find methods to compress quantum data, saving up resources needed. Our Quantum Autoencoder (QAE) project is based on the model proposed by Romero et al. \cite{}. We implement a quantum-classical hybrid procedure to perform training QAE, then we demonstrate its performance on MNIST handwritten digits database.
## Model  
  \\\Add figure 1 of the original paper here. 
  
  In classical AE, we apply the encoder, a deep NN, to compress an (n + k) bits into an n bits. The NN weights are then trained on training set so as to minmize the loss, which is usually defined by the Euclidean distance between the input vector and the output reconstructed vector. On the other hand, operations in the quantum world are reversible, so the encoder gives us (n + k) qubits, which means k qubits are “trash state”. Then, to reconstruct the original state, we discard this trash state and add in “fresh” k qubits as fixed “reference state”. [...explanation to be added] The cost function is defined by averaged fidelity between "trash state" and "reference state"
  
  \\\add the equation
 
## Training: Hybrid quantum-classical procedure
  As in many state-of-the-art quantum algorithms, we implement a hybrid procedure. 
  The steps in a single iterations are:
   1. Prepare the input state |ψi⟩ and the reference state (we choose to use \ket{0})
   2. Apply the encoder Up⃗.
   3. Measure the cost function C2(p⃗), which is the fi- delity between the trash state and the reference state via a SWAP test.
  On classical computer, perform minimization of log10(1− C2(p⃗)) over p⃗ until convergence. As suggested by the authors \cite{}, we use Basin- Hopping algorithms with L-BFGS-B optimizer.
  
## Demonstrations
  We use Qiskit to implement the circuit.
  We use scipy library to optimize parameters.
  Results:
  
  \\add figures.
  
  
## Discussion
  Compression rate still small?
  Encoder circuit still not universal?
  Try higher dimensional data in the future?
  
## References
  @article{Romero_2017,
   title={Quantum autoencoders for efficient compression of quantum data},
   volume={2},
   ISSN={2058-9565},
   url={http://dx.doi.org/10.1088/2058-9565/aa8072},
   DOI={10.1088/2058-9565/aa8072},
   number={4},
   journal={Quantum Science and Technology},
   publisher={IOP Publishing},
   author={Romero, Jonathan and Olson, Jonathan P and Aspuru-Guzik, Alan},
   year={2017},
   month={Aug},
   pages={045001}
}

