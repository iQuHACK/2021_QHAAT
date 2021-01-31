# Quantum Autoencoder for Quantum Data Compression 

## Introduction
 Quantum computing have been showing its tremendous advantages in many tasks, where it is slow or even impossible for classical algorithms to be employed, ranging from cryptography to quantum simulation. For example, once we have sufficiently powerful quantum computers, Shor's algorithm will factorize large numbers in minutes. Quantum simulators for chemistry have also been shown to be capable of efficiently calculating molecular energies for small system. In our NISQ area, however, the limited amount of quantum computing resources may prevent us from achieving desired results. Therefore, it is essential to find methods to compress quantum data, saving up resources needed. Our Quantum Autoencoder (QAE) project is based on the model proposed by Romero et al. [[1]](https://iopscience.iop.org/article/10.1088/2058-9565/aa8072/meta). We implement a quantum-classical hybrid procedure to perform training QAE, then we demonstrate its performance on MNIST handwritten digits database.
 
## Model  
  <img src="QAE_model.jpg" width=400px></img>
  
  *Picture adapted from original paper. <img src="https://render.githubusercontent.com/render/math?math=\rho"> is an (n+k) input. The top k (3 in this picture) qubits are discarded after encoding, then a 'fresh' k-qubit reference state is added.*
  
  In classical AE, we apply the encoder, a deep NN, to compress an (n + k) bits into an n bits. The NN weights are then trained on training set so as to minmize the loss, which is usually defined by the Euclidean distance between the input vector and the output reconstructed vector. On the other hand, operations in the quantum world are reversible, so the encoder gives us (n + k) qubits, which means k qubits are *trash state*. Then, to reconstruct the original state, we discard this trash state and add in “fresh” k qubits as a **fixed** *reference state*.
  ### Schematic diagram of QAE
  
  <img src="schematic_circuit.png" width=600px></img> 
    
  Encoder circuit proposed by [[1]](https://iopscience.iop.org/article/10.1088/2058-9565/aa8072/meta), consisting of general single qubit rotations whose parameters are to be learned.
  
  <img src="proposed_encoder.png" width=600px></img> 
  
  ### Objective function
  
  The objective function is defined by the fidelity between trash state and reference state. We measure this quantity by performing the swap test. 
  
  Below is an example of our implmentation, where the encoder compress 4-qubit data into 2-qubit data. The top qubit is for swap test measurement, the next 2 qubits are reference state, the remaining 4 qubits are input state. As this for training only, we do not include the decoder. In the swap test [[2]](https://en.wikipedia.org/wiki/Swap_test), the desired fidelity between trash and reference state is given by 
               <img src="https://render.githubusercontent.com/render/math?math=\F=\sqrt{2p_0-1} ">, where <img src="https://render.githubusercontent.com/render/math?math=p_0"> is the probabiliry of measuring 0. In our simulation, we can obtain <img src="https://render.githubusercontent.com/render/math?math=p_0"> by two different methods: sampling with multiple shots (more realistic but less efficient) or computing analytically with qiskit supported functions (efficient for demonstration purpose).
 
  <img src="our_circuit.png" width=800px></img> 

## Training procedure: Hybrid quantum-classical model
  As in many state-of-the-art quantum algorithms, we implement a hybrid procedure. 
  The steps in a single iterations are:
   1. Prepare the input state <img src="https://render.githubusercontent.com/render/math?math=\| \psi\rangle"> and the reference state (we choose to use <img src="https://render.githubusercontent.com/render/math?math=\| 0\rangle">)
   2. Apply the encoder <img src="https://render.githubusercontent.com/render/math?math=\U^{\vec{p}}">.
   3. Measure the cost function, which is the fidelity between the trash state and the reference state via a Swap test.
   
  On classical computer, perform minimization of loss function over parameters <img src="https://render.githubusercontent.com/render/math?math=\vec{p}"> until convergence. As suggested by the authors of [[1]](https://iopscience.iop.org/article/10.1088/2058-9565/aa8072/meta), we use Basin-Hopping algorithm with L-BFGS-B optimizer, provided by python library Scipy.
  
## Performance
  #### Compressing H3 eigenstates: 
  We train the model on two datasets: H3 molecular orbitals and MNIST handwritten digits.
  
  
 
  #### Compressing MNIST handwritten digits: compression ratio 8:6
  We first pre-process the dataset into 16x16 blackwhite images, then encode the 256 pixels into 8-qubit states. We use 2-qubit reference state, hence compressing 8 qubits to 6 qubits.
  
  **Results:** At the time of writing, the training on batch of 90 images is being run and the current fidelity is 0.83. We expect the fidelity can gets higher and we plan to render the reconstructed images to compare with the original images.
  
  
  
  Due to time limitation, we was not able to run simulation on IonQ cloud.
## Discussion and future work
  When applying this method in practice, one must consider resource limitations. Since we need to perform multiple shots for swap test, we will need to prepare multiple copies of input state and reference state, where preparing the latter is apparently easier than the former.
  Furthermore, we believe our encoder circuit used in this model need to be improved. Particularly, we want to design a more powerful circuit, for example, consisting of general d-qubit gates, where d<img src="https://render.githubusercontent.com/render/math?math=\geq">2. Note that, as long as d is small, the number of trainable parameters is polynomial in terms of input dimension. We also want to try higher dimensional data and higher compression rate when we have time in future.
  
## References
  [1] J. Romero, J. P. Olson, and A. Aspuru-Guzik, “Quantumautoencoders for efficient compression of quantum data,” Quantum Science and Technology, vol. 2, p. 045001, Aug2017.
  [2] Swap Test, Wikipedia. https://en.wikipedia.org/wiki/Swap_test

