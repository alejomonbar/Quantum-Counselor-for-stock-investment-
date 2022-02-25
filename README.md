# Introduction


# Quantum Counselor for Portfolio Investment
The Quantum Counselor for portfolio investment is a tool with two main objectives: forecasting the trend of assets price and optimizing portfolio returns both using quantum computing techniques. For the case of the forecasting method, we use a hybrid method for a Quantum neural network (**QNN**) that combines a deep learning model of classical LSTM layers with quantum layers. For the case of portfolio optimization, we convert the optimization problem into a Quadratic unconstrained binary optimization (**QUBO**) problem and using the quantum algorithms of Quantum Approximate Optimization Algorithm (**QAOA**) and the variational quantum eigensolver (**VQE**) solve the problem. Additionally, we use the classical solver CPLEX for comparison with the other two methods. Both tools are deeply connected because the forecasted price of the different assets is used for the optimization protfolio cost function construction.

<center><img src="./Images/Diagram2.png" width="900"></center>

### Requirements

- qiskit version:  0.19.2
- qiskit_optimization version:  0.3.1
- docplex version:  2.22.213

# Outline

1. Stocks forecasting using a QNN


2. Portfolio Optimization.


3. Stocks Selection.


4. Model XS (3 Stocks, 2 periods), QAOA and VQE with SPSA and COBYLA classical optimizers.


5. Model S  (5 Stocks, 3 periods), QAOA and VQE with SPSA and COBYLA classical optimizers.


6. Model M  (7 Stocks, 3 periods), QAOA and VQE with SPSA and COBYLA classical optimizers.


7. A novel approach for the Portfolio Optimization


8. References

# 1. Stocks forecasting using a QNN

One of the principal requirements for portfolio optimization is the ability to predict the price trend *P_{n,t}* for *N* assets during some periods of time. In this work, our first objective is to show the capabilities of a quantum neural network (QNN) to predict the trend of a set of stocks. Specifically, we select 8 stocks from 8 conglomerates based on the dataset of Xu et al. [[2]](https://aclanthology.org/P18-1183/): 

- Basic Materials: TOTAL S.A. "TOT"

- Consumer Goods: Appel Inc. "AAPL"

- Healthcare: AbbVie Inc. "ABBV"

- Services: Wall-Mart Stores Inc. "WMT"

- Utilites: Duke energy corporation "DUK"

- Financial: HSBS Holding pcl "HSBC"

- Industrial Goods: ABB Ltd. "ABB"

- Technology: China Mobile Limited "CHL"

The information comes from Sep 2012 to Sep 2017 with daily Technical information of Open, High, Low, Close, Adj Close, and Volume for the stocks price.

<figure>
<img src="./Images/Stocks.png" width="1000"> 
<figcaption align = "center"><b>Fig.1 - Stocks trend for the 8 conglomerates. We used the closed price as the prediction parameter.</b></figcaption>
</figure>
<br>
<br>


From here, we use a **QNN** to predict the trend in the price of the stock. The results of the forecasting are stored in **stocks_forecasting**

We design a quantum circuit  for encoding all the values in the data set, using 30 data per instance with this proposal we reduce to an output of 10.

<img src="./Images/QNN-ea.png" width="900"> 

The ansatz for the QNN follows the structure of the figure below structure, and using for the model 2 layers of this ansatz.





we design two proposal of solutions with this quantum layer.

<center><img src="./Images/models.png" width="800"></center>


Using the minimum  parameters for predict the stock.

<center><img src="./Images/models_minimal.png" width="800"></center>


One result of the  classical minimal  model


<center><img src="./Images/pred_min_8.png " width="800"></center>


One result of the  hybrid minimal  model


<center><img src="./Images/pred_min_8_q.png " width="800"></center>


Error between Classic and our  Hybrid proposal
<center><img src="./Images/error_models.png" width="800"></center>



Using more parameters to obtain the less error for all the stocks.

<center><img src="./Images/Stock_predictions.png" width="800"></center>




# 2. Portfolio optimization

The results of three portfolio cases are presented in this section: 

- XS model with 3 stocks and 2 periods of time
- S model with 5 stocks and 3 periods of time
- M model with 7 stocks and 3 periods of time.

For portfolio optimization, we use the modern portfolio theory where we is wanted to maximize the return of an investment while keeping the risk of losing money low. We based or cost function in the work of Mugel et al. [[1]](https://doi.org/10.1103/PhysRevResearch.4.013006) where the cost function is described by:

<center><img src="./Images/Cost-function.png" width="800"></center>

The equation shown above is encoded using the function *Model* from **docplex** a classical library from IBM for optimization problems. Next, using the *qiskit_optimization* function *QuadraticProgramToQubo*, the problem is translated to the **QUBO** representation. The **QUBO** is the input to the functions **QAOA** and **VQE** from **qiskit**. They translate the problem to the needed *Hamiltonian* and solve it using a hybrid model. In our case, we test two classical solvers the Simultaneous Perturbation Stochastic Approximation (**SPSA**) and Constrained Optimization BY Linear Approximation (**COBYLA**).

*Fig. 2* shows the training of the S -model and the S -NEW model (explained in **Section 3**) using VQE and QAOA. Here, it is observed that the VQE has a smoother way to come to the optimal point for both cases COBYLA and SPSA while the QAOA for 1 repetition is not comming to the optimal point contrary to the 2 repetitions where the optimal is found.

<figure>
<img src="./Images/Training.png" width="1000"> 
<figcaption align = "center"><b>Fig.2 - Cost function optimization using two quantum algorithms VQE and QAOA with classical optimizers SPSA and COBYLA. The red results are for the model S while the green for a novel approach explained in **Section 3** a</b></figcaption>
</figure>
<br>
<br>

## Proposal 
This project uses different methods and techniques of Quantum computing and Quantum machine learning as:

- QNN,
- VQE,
- QAOA.

and classical methods as  ANN, LSTM, Optimizers (Cobyla) 

<center><img src="./Images/methodology.png" width="800"></center>


For the case of the Hibryd LSTM we use proposal to design:

<center><img src="./Images/proposal_diagrams.png" width="800"></center>

# Prelimineries result 

## using the first layer as  QNN
<center><img src="./Images/pred_1.png" width="800"></center>


## using the last layer as  QNN
<center><img src="./Images/pred_f.png" width="800"></center>

## mock data for the portfolio part
<center><img src="./Images/proposal_1.png" width="800"></center>


# Progress

Positive results for each module separately, next step, connect the results of each block and compare the cases of each with their respective classical equivalence.

# References 
[1] Mugel, S., Kuchkovsky, C., Sánchez, E., Fernández-Lorenzo, S., Luis-Hita, J., Lizaso, E., & Orús, R. (2022). Dynamic portfolio optimization with real datasets using quantum processors and quantum-inspired tensor networks. Physical Review Research, 4(1), 1–13. https://doi.org/10.1103/PhysRevResearch.4.013006.


[2] Chen, Samuel & Yoo, Shinjae & Fang, Yao-Lung. (2020). Quantum Long Short-Term Memory. 


[3] Bausch, Johannes. (2020). Recurrent Quantum Neural Networks. 


[4] Verdon, Guillaume & Broughton, Michael & Mcclean, Jarrod & Sung, Kevin & Babbush, Ryan & Jiang, Zhang & Neven, Hartmut & Mohseni, M.. (2019). Learning to learn with quantum neural networks via classical neural networks. 
