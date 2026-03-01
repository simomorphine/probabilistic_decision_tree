# Probabilistic Decision Tree: A First-Principles Framework for Exploration-Exploitation

> *"Observing a reward and learning from a reward are not the same thing."*


---

## Overview

The exploration-exploitation dilemma stands as one of the central problems in sequential decision-making under uncertainty. An agent must balance two competing objectives: exploiting current knowledge to maximize immediate reward, versus exploring uncertain options to gain information that might improve future decisions.

Despite decades of research, exploration strategies are often presented as algorithmic recipes — ε-greedy, softmax, UCB, Thompson Sampling — with little unifying theory.

**We ask: can exploration be derived from first principles rather than postulated?**

In this work, we develop a complete framework based on a simple observation: at each decision point, an agent faces nested uncertainty that can be represented as a probabilistic tree:

1. **Action uncertainty** — Should I follow my current policy or deviate?
2. **Reward uncertainty** — What reward will I receive?
3. **Information uncertainty** — Will this experience update my beliefs?

By formalizing this tree structure and analyzing the resulting optimization problem, we derive exploration as a rational consequence of valuing both immediate rewards and future information.

---

## The Core Idea

Most bandit algorithms conflate two distinct quantities:

| Quantity | What it measures |
|---|---|
| **Reward** $R$ | What the agent gains |
| **Information** $I$ | What the agent learns |

The Probabilistic Decision Tree treats these as **separate random variables** at separate levels of the tree. The key insight is that the joint distribution $P(A, R, I \mid \theta)$ — not just $P(R \mid A)$ — is the minimal sufficient structure for rational decision-making.

This separation allows the framework to explicitly represent:

- **Informative failures** — negative reward, high information gain
- **Uninformative successes** — positive reward, low information gain
- **Exploration bias** — exploration actions are structurally more informative than exploitation

---

## Results

### Stationary Environments

| Difficulty | Gap | Winner | Elwardi Standard | Thompson Sampling | UCB1 |
|---|---|---|---|---|---|
| Easy | 0.15 | Thompson Sampling | 107.61 | **31.37** | 129.56 |
| Medium | 0.08 | Thompson Sampling | 76.96 | **55.73** | 137.45 |
| Hard | 0.02 | **Elwardi Standard** | **43.75** | 47.50 | 65.42 |

When the environment is stable and the signal is clear, Thompson Sampling is hard to beat. When the gap is nearly indistinguishable (0.02), explicit information modeling matters.

### Non-Stationary Environments

| Scenario | Winner | Elwardi Disc γ=0.95 | Elwardi Disc γ=0.99 | Thompson Sampling |
|---|---|---|---|---|
| Abrupt Shift | **Elwardi Disc γ=0.95** | **42.34** | 66.47 | 123.78 |
| Gradual Drift | **Elwardi Disc γ=0.99** | 31.68 | **29.26** | 51.84 |

**3× better than Thompson Sampling on abrupt shifts. Nearly 2× better on gradual drift.**

The pattern is consistent: in stable environments, classical methods are strong. In changing environments — which describes most real environments — explicitly separating reward from information gain makes a structural difference.

---

## Mathematical Framework

### The Three-Level Tree

$$P(A, R, I \mid \theta) = P(A) \cdot P(R \mid A, \theta) \cdot P(I \mid A, R, \theta)$$

**Level 1 — Action Selection:**

$$A \sim \begin{cases} \pi_{\text{current}} & \text{w.p. } \alpha \\ \pi_{\text{explore}} & \text{w.p. } 1-\alpha \end{cases}$$

**Level 2 — Reward Realization:**

$$P(R = +r \mid A=a, \theta) = p_a(\theta)$$

**Level 3 — Information Gain:**

$$P(I=1 \mid A=a, R=r, \theta) = q_{a,r}(\theta)$$


---

## Algorithms

| Algorithm | Description | Best For |
|---|---|---|
| `ElwardiStandard` | Base algorithm with explicit information-reward separation | Stationary, hard environments |
| `ElwardiDiscounted γ=0.95` | Aggressive discounting of past observations | Abrupt shifts |
| `ElwardiDiscounted γ=0.99` | Gentle discounting of past observations | Gradual drift |
| `ElwardiSurprise` | Surprise-based information estimation | Exploratory use |


---

## Theoretical Background

This algorithm is the first working implementation of **Humble Systems Theory** — a framework that models any information processing system as optimizing goals under energy and informational cost constraints. The full theory is developed in:

- *Beyond Energy: A New Geometry for Information Processing Systems* — Mohamed Elwardi, 2025
- *Bitopological Structure of Equilibrium in Humble Systems Theory* — Mohamed Elwardi, 2025

The core claim: systems that fail to account for the cost of information — not just the cost of action — are systematically miscalibrated. The bandit results above are the first empirical evidence.


---

## License

MIT — use it, build on it, tell me what you find.

---

## Author

**Mohamed Elwardi** — Math teacher, accidental theorist.
Morocco 🇲🇦

📧 [Mohamedelwardi@yahoo.com](mailto:Mohamedelwardi@yahoo.com)
🔗 [LinkedIn](www.linkedin.com/in/mohamed-elwardi-🇵🇸-721b8b197)

---


*$\delta > 0$, always and necessarily.*
