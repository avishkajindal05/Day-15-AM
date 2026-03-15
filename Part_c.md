# Probability & Statistics – Answers

## Q1 — Base Rate Fallacy (Medical Test Example)

The **base rate fallacy** occurs when people ignore the **prior probability (base rate)** of an event and focus only on the accuracy of a test.

### Example

Suppose:

- A disease affects **1 in 10,000 people**  
  - Prevalence = **0.01%**
- A medical test is **99% accurate**
  - **Sensitivity (true positive rate)** = 99%
  - **Specificity (true negative rate)** = 99%

Now imagine testing **1,000,000 people**.

### Step 1: People who actually have the disease

Disease prevalence:

```

1 / 10,000

```

Out of 1,000,000 people:

```

1,000,000 × 0.0001 = 100 people have the disease

```

Test results for these 100 people:

- True positives = 99  
- False negatives = 1

### Step 2: People without the disease

```

1,000,000 - 100 = 999,900 people

```

False positive rate = **1%**

```

999,900 × 0.01 ≈ 9,999 false positives

```

### Step 3: Compare results

| Result Type | Count |
|--------------|------|
| True positives | 99 |
| False positives | 9,999 |

Total positive tests:

```

99 + 9,999 = 10,098

```

Probability that a positive test actually means disease:

```

99 / 10,098 ≈ 0.98%

````

### Conclusion

Even with a **99% accurate test**, **most positive results are false positives** because the **disease is extremely rare**.

This happens because:

- The **base rate (prevalence)** is very small
- Even a small false positive rate produces **many incorrect positives**

This is the **base rate fallacy**: ignoring how rare the event actually is.

---

# Q2 — Coding: Simulating the Central Limit Theorem

The **Central Limit Theorem (CLT)** states that the **sampling distribution of the sample mean approaches a normal distribution** as the sample size increases, regardless of the underlying distribution (as long as variance is finite).

Below is a Python implementation that works for **any scipy.stats distribution**.

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

def simulate_clt(distribution, params, n_samples=30, n_simulations=10000):
    """
    Simulates the Central Limit Theorem.

    Parameters
    ----------
    distribution : scipy.stats distribution object
        Example: stats.expon, stats.uniform, stats.gamma

    params : dict
        Parameters for the distribution
        Example: {"scale": 1}

    n_samples : int
        Number of samples per simulation

    n_simulations : int
        Number of repeated simulations
    """

    sample_means = []

    # Generate simulations
    for _ in range(n_simulations):
        samples = distribution.rvs(size=n_samples, **params)
        sample_means.append(np.mean(samples))

    sample_means = np.array(sample_means)

    # Theoretical mean and variance
    dist_mean = distribution.mean(**params)
    dist_var = distribution.var(**params)

    clt_mean = dist_mean
    clt_std = np.sqrt(dist_var / n_samples)

    # Plot histogram
    plt.figure(figsize=(8,5))
    plt.hist(sample_means, bins=50, density=True, alpha=0.6, label="Simulated sample means")

    # Overlay theoretical normal distribution
    x = np.linspace(min(sample_means), max(sample_means), 500)
    normal_pdf = stats.norm.pdf(x, clt_mean, clt_std)

    plt.plot(x, normal_pdf, 'r', lw=2, label="Normal approximation")

    plt.title("Central Limit Theorem Simulation")
    plt.xlabel("Sample Mean")
    plt.ylabel("Density")
    plt.legend()

    plt.show()

    return sample_means
````

### Example Usage

```python
simulate_clt(stats.expon, {"scale":1}, n_samples=30, n_simulations=10000)
```

This will:

1. Draw samples from an **exponential distribution**
2. Compute **sample means**
3. Plot the **histogram of means**
4. Overlay the **theoretical normal distribution predicted by CLT**

---

# Q3 — Exponential Distribution and Misleading Mean

Customer purchase amounts often follow an **exponential distribution** because:

* Most purchases are **small**
* A few purchases are **very large**

This produces a **right-skewed distribution**.

### Problem with Using the Mean

The **mean purchase value** can be misleading because:

1. **Heavy right tail**

   * A few very large purchases inflate the mean.

2. **Most customers spend less than the mean**

   * Investors may believe customers spend more than they actually do.

3. **High variance**

   * Mean becomes unstable when extreme purchases occur.

Example:

| Purchase Values |
| --------------- |
| 5               |
| 6               |
| 7               |
| 8               |
| 300             |

Mean:

```
(5 + 6 + 7 + 8 + 300) / 5 = 65.2
```

But **most customers spend under 10**.

### Better Metrics to Show Investors

Instead of just the mean, present:

#### 1. Median Purchase Amount

More robust to extreme values.

Example:

```
Median = 7
```

Better represents the **typical customer**.

---

#### 2. Percentiles

Example:

| Percentile | Value                 |
| ---------- | --------------------- |
| 50th       | Median purchase       |
| 75th       | High typical purchase |
| 90th       | Premium buyers        |

This shows the **distribution of spending behavior**.

---

#### 3. Histogram / Distribution Plot

Visualizing the distribution shows:

* Majority of small purchases
* Long tail of large purchases

---

#### 4. Revenue Concentration

Example metric:

```
Top 10% of customers generate 60% of revenue
```

This helps investors understand **customer value segmentation**.

---

### Conclusion

The **mean alone hides the true structure of customer spending** in skewed distributions like the exponential.

A better investor report includes:

* Median
* Percentiles
* Distribution plots
* Revenue concentration metrics

These give a **much more accurate picture of customer behavior and revenue risk**.

`
