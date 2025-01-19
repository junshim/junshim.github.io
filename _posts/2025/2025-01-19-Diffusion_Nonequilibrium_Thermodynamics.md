---
title: "[Paper Review] Deep Unsupervised Learning using Nonequilibrium Thermodynamics"
categories:
  - Paper Review
tags:
  - Review
  - Machine Learning
  - Diffusion Model

last_modified_at: 2025-01-19T21:00:00+09:00
toc: true
---

# Paper Info
Titile: Deep Unsupervised Learning using Nonequilibrium Thermodynamics
Publisher: ICML'15

# 읽은 이유
최근 Diffusion Model이 각광 받고 있는데 그것의 첫 주자로 뽑히는 논문이라 읽어봄

# Research Objectives
Develop a generative modeling approach that achieves both flexibility and computational tractability by leveraging concepts from nonequilibrium statistical physics.  
This involves designing a forward diffusion process to systematically and gradually destroy the structure in data distributions, followed by learning a reverse diffusion process that restores the original structure.  

# Summary

## Key Idea
### Forward Trajectory

The forward diffusion process systematically converts the data distribution $q(x_0)$ into a simple, tractable distribution $\pi(x_T)$ through a series of iterative steps. The key equations defining the forward trajectory are:

1. **Forward Step**:
   $$
   q(x_t | x_{t-1}) = T_\pi(x_t | x_{t-1}; \beta_t)
   $$
   Here, $T_\pi$ represents the diffusion kernel, and $\beta_t$ is the diffusion rate controlling the amount of noise added at each step.

2. **Full Forward Trajectory**:
   The joint distribution over all time steps is expressed as:
   $$
   q(x_{0:T}) = q(x_0) \prod_{t=1}^T q(x_t | x_{t-1})
   $$

3. **Gaussian Diffusion Kernel**:
   When Gaussian noise is used for diffusion, each step follows:
   $$
   q(x_t | x_{t-1}) = \mathcal{N}(x_t; \sqrt{1 - \beta_t} \cdot x_{t-1}, \beta_t \cdot I)
   $$
   - $\sqrt{1 - \beta_t} \cdot x_{t-1}$: Scales the previous state to retain some of its structure.
   - $\beta_t \cdot I$: Adds Gaussian noise to the state, increasing randomness.

#### Intuition
The forward trajectory gradually destroys the structure in the data by scaling the original values and adding noise at each step. As $t \to T$, the data distribution converges to a simple, isotropic Gaussian distribution $\pi(x_T)$.

### Reverse Trajectory

The reverse diffusion process reconstructs the data structure by progressively removing noise from the simple distribution $\pi(x_T)$, ultimately recovering the original data distribution $q(x_0)$. The key equations defining the reverse trajectory are:

1. **Reverse Step**:
   $$
   p(x_{t-1} | x_t) = T_\pi(x_{t-1} | x_t; \theta_t)
   $$
   Here, $T_\pi$ represents the reverse diffusion kernel, and $\theta_t$ are the learned parameters (e.g., mean and variance) for each step.

2. **Full Reverse Trajectory**:
   The joint distribution for the reverse process is given by:
   $$
   p(x_{0:T}) = \pi(x_T) \prod_{t=1}^T p(x_{t-1} | x_t)
   $$

3. **Gaussian Reverse Kernel**:
   When Gaussian noise is used for diffusion, the reverse step involves sampling from:
   $$
   p(x_{t-1} | x_t) = \mathcal{N}(x_{t-1}; f_\mu(x_t, t), f_\Sigma(x_t, t))
   $$
   - $f_\mu(x_t, t)$: The predicted mean for $x_{t-1}$, learned during training.
   - $f_\Sigma(x_t, t)$: The predicted variance (or uncertainty), also learned during training.

#### Intuition
The reverse trajectory starts from a simple distribution $\pi(x_T)$ (e.g., isotropic Gaussian) and progressively removes noise to recover the structure of the original data distribution $q(x_0)$. By learning the parameters $f_\mu(x_t, t)$ and $f_\Sigma(x_t, t)$, the model can accurately reconstruct complex data distributions from noisy representations.

### Learning Process

The learning process ensures that the reverse diffusion model accurately approximates the forward diffusion process by minimizing the discrepancy between the two. The key steps in the learning process are:

1. **Training Objective**:
   The training aims to maximize the log-likelihood of the data distribution $q(x_0)$, which is approximated by maximizing its lower bound $K$:
   $$
   K = -\sum_{t=2}^T \int dx(0)dx(t)q(x(0),x(t)) \cdot D_{KL}(q(x(t-1)|x(t),x(0)) \| p(x(t-1)|x(t)))
   $$
   - $q(x_{t-1} | x_t, x_0)$: The conditional distribution derived from the forward process.
   - $p(x_{t-1} | x_t)$: The learned reverse conditional distribution.

2. **Loss Function**:
   Building on the training objective defined above, the actual optimization minimizes the KL divergence between the forward-derived conditional distribution $q(x_{t-1} | x_t, x_0)$ and the learned reverse distribution $p(x_{t-1} | x_t)$ at every time step. The total loss is given by:
   $$
   L = \sum_{t=1}^T D_{KL}(q(x_{t-1} | x_t, x_0) \| p(x_{t-1} | x_t))
   $$
   - $q(x_{t-1} | x_t, x_0)$: The conditional distribution from the forward process, acting as the ground truth.
   - $p(x_{t-1} | x_t)$: The reverse process distribution, parameterized by the model.

3. **Forward Process**:
   The primary goal of the forward process is to compute the conditional distribution $q(x_{t-1} | x_t, x_0)$, which is utilized in the loss function for training.

   - The conditional distribution $q(x_{t-1} | x_t, x_0)$ can be expressed as:
     $$
     q(x_{t-1} | x_t, x_0) \propto q(x_t | x_{t-1}) \cdot q(x_{t-1} | x_0)
     $$

   - **Key Components**:
     - **Transition between consecutive steps**:
       $$
       q(x_t | x_{t-1}) = \mathcal{N}(x_t; \sqrt{1 - \beta_t} \cdot x_{t-1}, \beta_t \cdot I)
       $$
       This describes how noise is added at each forward step, where $\beta_t$ is the noise intensity at time step $t$.
     - **Prior distribution of $x_{t-1}$ conditioned on the original data $x_0$**:
       $$
       q(x_{t-1} | x_0) = \mathcal{N}(x_{t-1}; \sqrt{\bar{\alpha}_{t-1}} \cdot x_0, (1 - \bar{\alpha}_{t-1}) \cdot I)
       $$
       This represents the prior distribution of $x_{t-1}$ given the original data $x_0$, where:
       $$
       \bar{\alpha}_{t-1} = \prod_{s=1}^{t-1} (1 - \beta_s)
       $$

   - **Gaussian-Based Calculation**:
     By leveraging the Gaussian properties of the forward process, $q(x_{t-1} | x_t, x_0)$ is calculated as:
     $$
     q(x_{t-1} | x_t, x_0) = \mathcal{N}(x_{t-1}; \tilde{\mu}_t(x_t, x_0), \tilde{\beta}_t \cdot I)
     $$
     where the mean $\tilde{\mu}_t(x_t, x_0)$ and variance $\tilde{\beta}_t$ are given by:
     $$
     \tilde{\mu}_t(x_t, x_0) = \frac{\sqrt{\bar{\alpha}_{t-1}} \cdot \beta_t}{1 - \bar{\alpha}_t} \cdot x_0 + \frac{\sqrt{1 - \beta_t} \cdot (1 - \bar{\alpha}_{t-1})}{1 - \bar{\alpha}_t} \cdot x_t
     $$
     $$
     \tilde{\beta}_t = \frac{\beta_t \cdot (1 - \bar{\alpha}_{t-1})}{1 - \bar{\alpha}_t}
     $$

   - This calculation provides an efficient and mathematically tractable framework for obtaining $q(x_{t-1} | x_t, x_0)$, which serves as the ground truth for optimizing the reverse process.

4. **Reverse Process**:
   The reverse process aims to approximate the forward-derived conditional distribution $q(x_{t-1} | x_t, x_0)$ by learning the reverse conditional distribution $p(x_{t-1} | x_t)$. This approximation is achieved through a parameterized model $f$, such as a Convolutional Neural Network (CNN), which predicts the parameters of $p(x_{t-1} | x_t)$.

   - The reverse distribution is modeled as:
     $$
     p(x_{t-1} | x_t) = \mathcal{N}(x_{t-1}; f_\mu(x_t, t), f_\Sigma(x_t, t))
     $$
     where:
     - $f_\mu(x_t, t)$: The predicted mean, learned by the model $f$.
     - $f_\Sigma(x_t, t)$: The predicted variance (or predefined uncertainty).

   - **Key Components**:
     - **Model Prediction**:
       The model $f$, such as a CNN, takes the noisy data $x_t$ and the time step $t$ as inputs and predicts the parameters of the reverse distribution $p(x_{t-1} | x_t)$. Specifically:
       - **Input**:
         - $x_t$: The noisy data at time step $t$.
         - $t$: The current time step, which is often encoded via positional embeddings to capture temporal information.
       - **Output**:
         - $f_\mu(x_t, t)$: The predicted mean for $x_{t-1}$.
         - $f_\Sigma(x_t, t)$: The predicted variance (or predefined uncertainty) for $x_{t-1}$.

     - **Reverse Sampling**:
       Using the predicted reverse distribution $p(x_{t-1} | x_t)$, a sample $x_{t-1}$ is drawn:
       $$
       x_{t-1} \sim \mathcal{N}(f_\mu(x_t, t), f_\Sigma(x_t, t)).
       $$

   - **Connection to Loss**:
     The reverse process is trained by minimizing the KL divergence between the learned reverse distribution $p(x_{t-1} | x_t)$ and the forward-derived ground truth $q(x_{t-1} | x_t, x_0)$, as defined in the loss function:
     $$
     L = \sum_{t=1}^T D_{KL}(q(x_{t-1} | x_t, x_0) \| p(x_{t-1} | x_t)).
     $$
     This ensures that the reverse process effectively approximates the forward process in reverse, allowing accurate reconstruction of the original data $x_0$.

5. **Learning Process in Practice**:

    **Note**: The following description outlines a practical approach based on general principles of probabilistic modeling and diffusion models. These details are not explicitly discussed in the original paper and are inferred as a possible implementation for training.

    In the practical implementation of the learning process, the theoretical distributions $q(x_{t-1} | x_t, x_0)$ and $p(x_{t-1} | x_t)$ are approximated using sampled values from the forward process and the reverse model's predictions. The steps are as follows:

    - **Forward Process**:
        - Starting with the original data $x_0$, noise is iteratively added based on a fixed noise schedule $\beta_t$, resulting in progressively noisier samples $x_1, x_2, \dots, x_T$:
            $$
            q(x_t | x_{t-1}) = \mathcal{N}(x_t; \sqrt{1 - \beta_t} \cdot x_{t-1}, \beta_t \cdot I).
            $$
        - These sampled noisy data $x_t$ act as the representation of $q(x_{t-1} | x_t, x_0)$ during training, as the theoretical distribution is not directly used.

    - **Reverse Process**:
        - A model $f$, such as a CNN, predicts the parameters of the reverse conditional distribution $p(x_{t-1} | x_t)$ for each step $t$:
            $$
            p(x_{t-1} | x_t) = \mathcal{N}(x_{t-1}; f_\mu(x_t, t), f_\Sigma(x_t, t)).
            $$
        - The inputs and outputs during this process are:
            - **Input**:
            - $x_t$: Noisy data at step $t$.
            - $t$: Current time step, encoded as positional embeddings.
            - **Output**:
            - $f_\mu(x_t, t)$: Predicted mean for $x_{t-1}$.
            - $f_\Sigma(x_t, t)$: Predicted variance (or fixed uncertainty).
        - Using these predictions, a sample $x_{t-1}$ is generated:
            $$
            x_{t-1} \sim \mathcal{N}(f_\mu(x_t, t), f_\Sigma(x_t, t)).
            $$

    - **Loss Calculation**:
        - The goal is to minimize the KL divergence $D_{KL}(q(x_{t-1} | x_t, x_0) \| p(x_{t-1} | x_t))$. However, in practice:
            - The sampled $x_{t-1}$ and $x_t$ from the forward process are treated as ground truth.
            - The loss is computed as the Mean Squared Error (MSE) between $x_{t-1}$ and the predicted mean $f_\mu(x_t, t)$:
            $$
            L_t = \mathbb{E}_{x_t, x_{t-1}, x_0} \left[ \| x_{t-1} - f_\mu(x_t, t) \|^2 \right].
            $$
            - The total loss across all time steps is:
            $$
            L = \sum_{t=1}^T L_t.
            $$

    - **Conclusion**:
        - The forward process provides noisy samples $x_t$ and $x_{t-1}$, which are used as approximations of $q(x_{t-1} | x_t, x_0)$ during training.
        - The reverse model $f$ learns to predict $f_\mu(x_t, t)$ to match the sampled $x_{t-1}$, minimizing the MSE-based loss.
        - This approach ensures the reverse process can accurately reconstruct $x_0$ from $x_T$ during inference.

# Note for Remembering
