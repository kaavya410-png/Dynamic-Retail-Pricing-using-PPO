# Reinforcement Learning for Dynamic Retail Pricing: Continuous Market States with Proximal Policy Optimization

## 📌 Project Overview

This project investigates the use of **Proximal Policy Optimization (PPO)** for dynamic retail pricing with continuous market-state information.

The base paper, *A Reinforcement Learning Approach to Dynamic Pricing* by Mullapudi (2025), uses Q-learning with discretized market states. This project explores a PPO-based approach that represents market conditions using continuous numerical features and allows the agent to select continuous price-adjustment actions.

The system uses a simulated pricing environment. A Ridge Regression demand model estimates product demand, while the PPO agent learns pricing actions from the rewards returned by the environment.

> **Important:** This is a simulation-based academic project. Its results should not be interpreted as evidence of real-world retail performance.

## 🎯 Objectives

- Implement a continuous-state reinforcement learning approach for retail pricing.
- Use PPO to learn price adjustments from market information.
- Build a demand model using historical retail data.
- Design a pricing environment with reward calculation and price guardrails.
- Evaluate the learned policy on unseen test periods.
- Compare PPO with simple fixed pricing reference rules.

## 📊 Dataset

- **Dataset:** Retail Price Optimization (public Kaggle dataset)
- **Records used:** 676
- **Products:** 52
- The data contains product-level pricing, sales, competitor and time-related information.
- The data is split chronologically to preserve the order of months and avoid using future observations when training the demand model.

The dataset is used to train a demand model and construct a simulated environment. The PPO agent does not interact with real customers or a live retail system.

## 🧠 Methodology

### 1. Data Preparation
Load the retail pricing data, select the required variables, perform data-quality checks and create chronological training and testing splits.

### 2. Demand Model
A **Ridge Regression** model estimates expected product demand using price and other market-related features. It provides the simulated customer response used by the pricing environment.

### 3. Continuous State Representation
The agent observes ten numerical state features, including:

- Current price relative to the product's average price
- Current price relative to competitor prices
- Competitor price movement
- Seasonal sine and cosine values
- Product rating
- Holiday indicator

Unlike the base paper's discretized state representation, the proposed approach retains continuous numerical values.

### 4. PPO Agent
The PPO agent uses a linear policy and a linear value function:

- The policy selects a price adjustment.
- The value function estimates the expected value of the current state.
- PPO updates the policy using collected interaction experience.

### 5. Action and Price Guardrails
The action is a continuous value between -1 and +1. It is converted into a price adjustment:

`New Price = Current Price × (1 + 0.10 × Action)`

The final price is restricted to within ±20% of the product's average training price.

### 6. Reward Calculation
The environment estimates demand at the adjusted price and calculates profit. A price-stability penalty is also included in the reward to discourage unnecessary price fluctuations.

### 7. Evaluation
The trained agents are evaluated on unseen test months without exploration noise. Five random seeds are used to examine variation between runs.

## ⚙️ Experimental Configuration

| Parameter | Configuration |
|---|---|
| Reinforcement learning algorithm | PPO |
| State representation | Continuous, 10 features |
| Action space | Continuous, -1 to +1 |
| Policy | Linear |
| Value function | Linear |
| Training updates | 80 per seed |
| Sampled episodes per update | 8 |
| Random seeds | 0–4 |
| Unit cost assumption | 60% of average training price |
| Price guardrail | ±20% of average training price |
| Maximum action-based price movement | ±10% |

The unit-cost ratio and guardrails are simulation assumptions, not values directly observed from the dataset.

## 📈 Results

### PPO Test Results (Mean ± Standard Deviation Across Five Seeds)

| Metric | Result |
|---|---:|
| Total simulated profit | 49,795.13 ± 244.59 |
| Profit margin | 44.1% ± 6.8 percentage points |
| Months in which price changed | 92.29% ± 4.32 percentage points |
| Proposals outside guardrails | 38 ± 16.42 per run |
| Mean absolute price change | 6.1% ± 0.6% |

### Comparison with Fixed Reference Rules

| Pricing Rule | Total Simulated Profit | Margin |
|---|---:|---:|
| Hold every price at its average | 51,795.89 | 40.0% |
| PPO agent (five-seed mean) | 49,795.13 | 44.1% |
| Always raise price by 10% each month | 49,253.90 | 48.1% |
| Always cut price by 10% each month | 48,247.99 | 28.1% |

The PPO agent achieved a higher average margin than the average-price reference, but its total simulated profit was lower. Therefore, a higher margin did not translate into higher total profit in this experiment.

## 🔍 Key Findings

- PPO successfully trained and generated continuous price-adjustment decisions.
- The agent changed prices in approximately 92.3% of test months.
- The average profit margin was higher than the fixed average-price reference.
- The fixed average-price reference achieved higher total simulated profit.
- Results varied across random seeds, particularly for profit margin.

## ⚠️ Limitations

- The demand model has weak test performance (R² = -0.110).
- Unit cost, price guardrails and reward penalty settings are simulation assumptions.
- Only five random seeds and 80 PPO updates per seed were used.
- The training curves do not show a consistent upward improvement across all runs.
- The project does not include a Q-learning implementation evaluated in the same simulator.
- Competitor prices are treated as historical inputs and do not react to the agent's decisions.
- The results have not been validated using real stores or customers.

## 🚀 Future Enhancements

- Compare PPO and Q-learning in the same environment with identical conditions.
- Improve the demand model using more data and category-specific demand relationships.
- Increase the number of random seeds and training updates.
- Experiment with a neural-network policy.
- Include competitor reactions and more realistic market behaviour.
- Report confidence intervals and suitable statistical comparisons.
- Validate the approach using real-world retail data.

## 🛠️ Technologies Used

- Python
- NumPy
- Pandas
- Ridge Regression
- Proximal Policy Optimization (PPO)
- Reinforcement Learning
- Matplotlib / data visualisation

## ▶️ Running the Project

The project implementation is provided as a notebook.

1. Open the project notebook in Google Colab or a compatible Jupyter environment.
2. Upload or mount the Retail Price Optimization dataset required by the notebook.
3. Install any dependencies imported by the notebook if they are not already available.
4. Check the dataset path and configuration values.
5. Run the notebook cells in order to prepare the data, train the demand model, construct the environment, train PPO and evaluate the results.

The exact notebook filename and dataset path should be updated here to match the files included in the repository.

## 📁 Suggested Repository Structure

```text
dynamic-retail-pricing-ppo/
│
├── README.md
├── notebooks/
│   └── retail_pricing_ppo.ipynb
├── data/
│   └── (place dataset here if redistribution is permitted)
├── results/
│   ├── ppo_results_per_seed.csv
│   └── ppo_results_summary.csv
└── documentation/
    └── project_documentation.pdf
```

## 📚 Base Paper

Mullapudi (2025), *A Reinforcement Learning Approach to Dynamic Pricing*.

This project uses the paper as its baseline reference and investigates continuous market states and PPO as an alternative approach.

## 👩‍💻 Author

**Kaavya S**  
M.Sc. Data Science  
SASTRA Deemed University  
Academic Year: 2026–2027
