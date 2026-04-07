# Python Quiz: Statistical Sampling and Confidence Intervals

---

## Task 1: Select a Random Sample

**Goal:** Select a random sample of 40 individuals from the population and assign the created numpy array to the variable `sample`.
```python
numpy.random.seed(12)
sample = numpy.random.choice(dBP, 40)
```

**Explanation:**
- `numpy.random.seed(12)` sets the random seed for **reproducibility**
- `numpy.random.choice(dBP, 40)` randomly selects **40 values** from the population
- Results are stored in the `sample` variable

**Why use a seed?**
- Ensures the same random numbers are generated each time
- Important for reproducible research

---

## Task 2: Calculate the Sample Mean

**Goal:** Calculate the sample mean and assign the value to the variable `sample_mean`.
```python
sample_mean = sample.mean()
```

**Explanation:**
- `.mean()` calculates the **average** of all values in the sample
- This is our **point estimate** of the population mean

**What is the sample mean?**
- The average value in our sample
- Used to estimate the true population mean

---

## Task 3: Calculate the Standard Deviation

**Goal:** Calculate the standard deviation of the sample and assign the value to the variable `sample_std`.
```python
sample_std = sample.std(ddof=1)
```

**Explanation:**
- `.std(ddof=1)` calculates the **sample standard deviation**
- `ddof=1` uses **Bessel's correction** (divides by n-1 instead of n)

**Why ddof=1?**
- `ddof=1` provides an **unbiased estimate** of population standard deviation
- Without it, we would underestimate the true population variance
- `ddof` = Delta Degrees of Freedom

**Key difference:**
- Population std: divide by n
- Sample std: divide by n-1

---

## Task 4: Calculate the Standard Error

**Goal:** Calculate the standard error of the sample and assign the value to the variable `standard_error`.
```python
standard_error = sample_std / numpy.sqrt(40)
```

**Explanation:**
- Standard error = sample std / √(sample size)
- Measures the **variability of the sample mean**
- Formula: SE = s / √n

**What is standard error?**
- Estimates how much the sample mean varies from the true population mean
- Smaller SE = more precise estimate
- Larger sample size = smaller SE

---

## Task 5: Calculate the Confidence Coefficient

**Goal:** Calculate the confidence coefficient for a 95% confidence level, given the sample size and degrees of freedom, and assign the value to the variable `c`.
```python
# Degrees of freedom is sample size minus 1 (40 - 1 = 39)
c = stats.t.ppf(0.975, 39)
```

**Explanation:**
- `stats.t.ppf()` finds the **t-distribution critical value**
- `0.975` represents the 97.5th percentile (for two-tailed 95% CI)
- `39` is the degrees of freedom (n - 1 = 40 - 1)

**Why 0.975?**
- For 95% confidence, we have 2.5% in each tail
- Lower tail: 0.025 (2.5%)
- Upper tail: 0.975 (97.5%)

**Degrees of freedom:**
- df = n - 1
- df = 40 - 1 = 39

---

## Task 6: Calculate the Lower Bound

**Goal:** Calculate the lower bound of the confidence interval for the sample mean and assign the result to the variable `lower_bound`.
```python
lower_bound = sample_mean - c * standard_error
```

**Explanation:**
- Lower bound = mean - (critical value × standard error)
- Represents the **lower limit** of the 95% confidence interval

**Formula:**
- Lower bound = x̄ - t* × SE
- Where:
  - x̄ = sample mean
  - t* = critical t-value
  - SE = standard error

---

## Task 7: Calculate the Upper Bound

**Goal:** Calculate the upper bound of the confidence interval for the sample mean and assign the result to the variable `upper_bound`.
```python
upper_bound = sample_mean + c * standard_error
```

**Explanation:**
- Upper bound = mean + (critical value × standard error)
- Represents the **upper limit** of the 95% confidence interval

**Formula:**
- Upper bound = x̄ + t* × SE

**Interpreting the CI:**
- We are **95% confident** the true population mean lies between `lower_bound` and `upper_bound`
- Does NOT mean there's a 95% probability the mean is in this range
- Means: if we repeated this process 100 times, ~95 intervals would contain the true mean

---

## Task 8: Create Bootstrapped Sample Means

**Goal:** Create an array of 1000 bootstrapped sample means (using the sample size of 40) and assign the array to the variable `bootstrapped_means` and sort the values in ascending order.
```python
bootstrapped_means = [numpy.random.choice(sample, 40, replace=True).mean() for _ in range(1000)]
bootstrapped_means.sort()
```

**Explanation:**
- **List comprehension** creates 1000 bootstrapped means
- `numpy.random.choice(sample, 40, replace=True)` samples **with replacement**
- `.mean()` calculates the mean of each bootstrap sample
- `.sort()` arranges values in **ascending order**

**What is bootstrapping?**
- Resampling technique that samples **with replacement**
- Creates many samples from the original sample
- Used to estimate sampling distribution

**Why replace=True?**
- Allows the same value to be selected multiple times
- Essential for bootstrapping method

---

## Task 9: Show the 2.5th Percentile

**Goal:** Show the bootstrapped value representing the 2.5th percentile.
```python
# The 25th value in a 0-indexed list of 1000 items represents the 2.5% cutoff
bootstrapped_means[24]
```

**Explanation:**
- Index `[24]` gives the **25th value** (0-indexed)
- 25 out of 1000 = 2.5%
- Represents the **lower bound** of the bootstrap confidence interval

**Why index 24?**
- Lists are **0-indexed** (start at 0, not 1)
- 2.5% of 1000 = 25 values
- 25th value is at index 24

**Calculation:**
- 0.025 × 1000 = 25
- Index = 25 - 1 = 24

---

## Task 10: Show the 97.5th Percentile

**Goal:** Show the value bootstrapped representing the 97.5th percentile.
```python
# The 975th value in a 0-indexed list of 1000 items represents the 97.5% cutoff
bootstrapped_means[974]
```

**Explanation:**
- Index `[974]` gives the **975th value** (0-indexed)
- 975 out of 1000 = 97.5%
- Represents the **upper bound** of the bootstrap confidence interval

**Why index 974?**
- 97.5% of 1000 = 975 values
- 975th value is at index 974

**Calculation:**
- 0.975 × 1000 = 975
- Index = 975 - 1 = 974

**Bootstrap CI:**
- Lower bound: `bootstrapped_means[24]`
- Upper bound: `bootstrapped_means[974]`
- This gives a **95% bootstrap confidence interval**

---

## Complete Workflow Example
```python
import numpy
import scipy.stats as stats

# Step 1: Set seed and select sample
numpy.random.seed(12)
sample = numpy.random.choice(dBP, 40)

# Step 2: Calculate sample statistics
sample_mean = sample.mean()
sample_std = sample.std(ddof=1)
standard_error = sample_std / numpy.sqrt(40)

# Step 3: Calculate traditional confidence interval
c = stats.t.ppf(0.975, 39)
lower_bound = sample_mean - c * standard_error
upper_bound = sample_mean + c * standard_error

print(f"Traditional 95% CI: ({lower_bound:.2f}, {upper_bound:.2f})")

# Step 4: Calculate bootstrap confidence interval
bootstrapped_means = [numpy.random.choice(sample, 40, replace=True).mean() 
                      for _ in range(1000)]
bootstrapped_means.sort()

boot_lower = bootstrapped_means[24]
boot_upper = bootstrapped_means[974]

print(f"Bootstrap 95% CI: ({boot_lower:.2f}, {boot_upper:.2f})")
```

---

## Quick Reference: Key Concepts

### Sample Statistics

| Statistic | Formula | Code |
|-----------|---------|------|
| Sample Mean | x̄ = Σx / n | `sample.mean()` |
| Sample Std Dev | s = √[Σ(x-x̄)² / (n-1)] | `sample.std(ddof=1)` |
| Standard Error | SE = s / √n | `sample_std / numpy.sqrt(n)` |

### Confidence Intervals

| Component | Formula | Code |
|-----------|---------|------|
| Critical Value (t) | t* at α/2, df=n-1 | `stats.t.ppf(0.975, n-1)` |
| Lower Bound | x̄ - t* × SE | `mean - c * se` |
| Upper Bound | x̄ + t* × SE | `mean + c * se` |

### Bootstrapping

| Step | Description | Code |
|------|-------------|------|
| Resample | Sample with replacement | `numpy.random.choice(sample, n, replace=True)` |
| Calculate Statistic | Mean of bootstrap sample | `.mean()` |
| Repeat | Do this many times | `for _ in range(1000)` |
| Percentiles | 2.5th and 97.5th | `sorted_boots[24]`, `sorted_boots[974]` |

---

## Key Formulas

### Standard Error

$$SE = \frac{s}{\sqrt{n}}$$

### Confidence Interval

$$CI = \bar{x} \pm t^* \times SE$$

### Bootstrap CI

$$CI_{boot} = [\hat{\theta}_{0.025},\ \hat{\theta}_{0.975}]$$
