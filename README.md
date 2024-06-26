# Physics-Informed-Neural-Networks-PINN
Welcome to the repository for my research project on Physics-Informed Neural Networks (PINNs) applied to different industrial processes. This project is part of my research internship at Kyoto University's Informatics Lab under the supervision of Dr. Manabu Kano.
## Project Overview
This repository contains the implementation and analysis of PINN-based models for simulating the dynamics of different Chemical, Physical, Mechanical or Electrical processes. Our model leverages both data-driven machine learning techniques and fundamental physical equations to accurately predict key reactor outputs, such as concentration of reactants and temperature profiles, while ensuring adherence to underlying physical laws. This model doesn't employ any realistic control parameters, and hence, is a relatively idealistic model.

To use the code, install all the necessary dependencies by running the following command in the terminal:

`pip install -r requirements.txt`

### PINN for a CSTR
A Continuous Stirred Tank Reactor (CSTR) is a common type of chemical reactor which is typically used for liquid-phase reactions. It is a very basic and simple model.
1. A CSTR operates continuously, with reactants being continuously fed into the reactor and products being continuously removed. This ensures that the reactor operates at a steady state.
2. The contents are continuously stirred with an agitator, ensuring uniform composition and temperature throughout the reactor.
3. In the given problem, the chemical reaction occurring within the CSTR is a first-order reaction with respect to the concentration of the reactant. The reaction rate depends on the concentration of the reactant and follows the Arrhenius temperature dependence, meaning the reaction rate increases exponentially with temperature. 

#### Problem Statement - As of now, I’ve used the same problem statement as given in the Appendix C of the book Process Control, by Sir Thomas E. Marlin. The objective is to raise the temperature of a chemical reactor to 395.3 K without exceeding this temperature by adjusting the coolant flow.


The system is liquid in the tank, and the reaction parameters are as follows:
1. Flow Rate (F): 1 m3/min
2. Volume (V): 1 m3
3. Initial Concentration of Reactant (CA0): 2.0 kmol/m3
4. Initial Temperature (T0): 323 K
5. Specific Heat Capacity of Liquid (Cp): 1 cal/(g·K)
6. Density of Liquid (ρ): 106 g/m3
7. Reaction Rate Constant (k0): 1.0 × 1010 min−1
8. Activation Energy divided by Gas Constant (E/R):
8330.1 K
9. Heat of Reaction (ΔH_rxn): -130 × 106 cal/kmol
10. Coolant Inlet Temperature (Tcin): 365 K
11. Steady-State Coolant Flow Rate (Fc)ₛ: 15 m3/min
12. Specific Heat Capacity of Coolant (Cp_c): 1 cal/(g·K)
13. Density of Coolant (ρ_c): 106 g/m3
14. Heat Transfer Coefficient (a): 1.678 × 106 (cal/min)/(K)
15. Coolant Flow Exponent (b): 0.5

The steady-state values of the dependent variables are given as:
1. Steady-State Temperature (Tₛ): 394 K
2. Steady-State Concentration of Reactant (CAₛ): 0.265
kmol/m3

A step change in the coolant flow of -1 m3/min is applied to analyze its effect on the system.
The reactor equations are provided in the 'utility' folder, with the symbols having their usual meanings.

##### Simulation Setup: The provided CSTR simulation was set up to run for 10 Sec with a step change in coolant flow applied at t = 0. Time step for the simulation was chosen to be sufficiently small to capture the dynamics of the system accurately.

##### Step Change: A step change of -1 m3/min in the coolant flow rate was introduced to observe its effect on the reactor temperature and concentration of reactants.
Based on this simulation, a generated a CSV file which was later used in training and testing my PINN model.

#### Input Variables:
1. F2: Flow rate of the reactor (m3/min)
2. T1: Inlet temperature of the reactor (K)
3. CA1: Initial concentration of species A (kmol/m3)
4. F1: Flow rate (m3/min)
5. Fc1: Coolant flow rate (m3/min)
6. Tc1: Coolant inlet temperature (K)
7. Time: Time (min)
8. Volume (m3)

#### Output Variables:
1. CA: Concentration of species A (kmol/m3)
2. T: Temperature of the reactor (K) 
3. Tc_out: Outlet temperature of the coolant (K)

#### Model Structure:
1. Input Layer: The input layer consists of 8 nodes corresponding to the input variables.
2. Hidden Layers: The model includes 3 hidden layers with 128 hidden nodes and activation
function - Tanh.
3. Output Layer: The output layer consists of 3 nodes corresponding to the output variables.

I've created 3 different variations of the PINN model, considering overfitting and L2_regularization.

#### Loss Function - The loss function in a PINN model is a combination of the data loss and the physics-informed loss. The data loss used in the model is the Mean Squared Error (MSE) between the predicted and true values. The physics-informed loss is based on the differential equations governing the system.

#### Reference Paper used:
1. As suggested by my supervisor, I found and read a paper about the implementation of PINN models on different chemical processes
2. The paper I referenced is - “Optimal temperature trajectory for tubular reactor using physics informed neural networks” by Rahul Patel, Sharad Bhartiya, Ravindra Gudi https://www.sciencedirect.com/science/article/pii/S0959152423000823#sec3.2
3. This paper implemented a similar approach in finding out the temperature trajectory for a tubular reactor based on the input variables using PINN
4. I also referenced the code they used and what losses they incorporated

#### Results
The best model - PINN3 gave the following results:
1. Training: MSE - 0.0018, R2 - 99.64%
2. Testing: MSE - 0.0004, R2 - 99.95%

#### Conclusion
The results of our best-performing model, PINN3, demonstrated exceptional accuracy and reliability in simulating the dynamics of the Continuous Stirred Tank Reactor (CSTR).
These metrics indicate that the PINN3 model successfully captured the complex relationships within the CSTR system, achieving high fidelity to both training and testing data. The low MSE values suggest that the predicted outputs are very close to the actual values, while the high R² values demonstrate that the model explains almost all of the variability in the data.

The graphical results, which provide a visual comparison of true vs. predicted values and scatter plots of the model’s performance, are uploaded in the 'results' folder. These visualizations further confirm the accuracy and robustness of the model.

Overall, these results are quite promising. They highlight the potential of Physics-Informed Neural Networks (PINNs) to effectively integrate physical laws into machine learning models, enhancing their predictive capabilities while reducing the need for extensive datasets.