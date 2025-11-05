# Option-Pricing-BS-MonteCarlo

Interactive Python application for pricing exotic options using Black-Scholes analytical formulas and Monte Carlo simulations.

## 📋 Description

This project provides an interactive web application built with **Streamlit** for pricing three types of options:

- **European Options** (Call & Put) - Pricing using Black-Scholes and Monte Carlo
- **Asian Options** - Monte Carlo pricing with arithmetic averaging
- **Tunnel Options** - Monte Carlo pricing with multiple barriers

The application allows real-time visualization of diffusion processes, Monte Carlo convergence, and confidence intervals.

## 🚀 Features

### European Options
- Analytical pricing with Black-Scholes formula
- Numerical pricing with Monte Carlo simulation
- Comparison between both methods
- Geometric Brownian Motion visualization
- Convergence plot with confidence intervals

### Asian Options
- Asian call based on arithmetic average
- Payoff calculated on the ratio of means over two periods
- Monte Carlo simulation with fine time discretization
- Convergence analysis and error estimation

### Tunnel Options
- Payoff structure with two barriers (K₁ and K₂)
- Tiered returns based on barrier levels:
  - 1% return when price is between barriers
  - 2% return when price exceeds upper barrier
  - Plus final call option component
- Quarterly time steps simulation
- Visual barrier representation

### Common Features
- **Interactive parameter controls**: Real-time adjustment of S₀, K, T, σ, μ, r
- **Confidence intervals**: Customizable confidence levels
- **Convergence visualization**: Dynamic plots showing simulation convergence
- **Diffusion process plots**: Visual representation of price paths
- **Error estimation**: Statistical error bounds for Monte Carlo results

## 📦 Installation

### Prerequisites

```bash
Python 3.8+
```

### Clone the repository

```bash
git clone https://github.com/yourusername/Option-Pricing-BS-MonteCarlo.git
cd Option-Pricing-BS-MonteCarlo
```

### Install dependencies

```bash
pip install -r requirements.txt
```

**requirements.txt**:
```txt
streamlit
numpy
scipy
matplotlib
```

## 💻 Usage

### Launch the application

```bash
streamlit run app.py
```

The application will open in your default web browser at `http://localhost:8501`.

### Using the Interface

1. **Select Option Type**: Use the sidebar radio buttons to choose between European, Tunnel, or Asian options
2. **Configure Parameters**: Adjust the parameters in the sidebar:
   - `S₀`: Current stock price
   - `K`: Strike price (or K₁, K₂ for Tunnel options)
   - `T`: Time to maturity (years)
   - `σ`: Volatility
   - `μ`: Drift parameter
   - `r`: Risk-free interest rate
   - Confidence Interval level
   - Number of simulations (N)
3. **Run Simulation**: Click "Lancer les Simulations" button to execute
4. **Analyze Results**: View pricing results, convergence plots, and diffusion processes

### Example Parameters

**European Call**:
- S₀ = 100, K = 105, T = 2.0, σ = 0.15, μ = 0.015, r = 0.01, N = 1000

**Asian Call**:
- S₀ = 100, K = 1.05, T = 2.0, σ = 0.15, μ = 0.015, r = 0.01, N = 1000

**Tunnel Option**:
- S₀ = 100, K₁ = 0.9, K₂ = 1.10, T = 2.0, σ = 0.15, μ = 0.015, r = 0.01, N = 1000

## 📊 Mathematical Framework

### Black-Scholes Model

The Black-Scholes model assumes the stock price follows the stochastic differential equation:

$$dS_t = S_t(\mu dt + \sigma dW_t)$$

Where:
- `μ` is the drift
- `σ` is the volatility
- `W_t` is a standard Brownian motion

The analytical solution is:

$$S_t = S_0 \exp\left(\mu t - \frac{\sigma^2}{2}t + \sigma W_t\right)$$

**European Call Price (Black-Scholes)**:
```
C = S₀N(d₁) - Ke^(-rT)N(d₂)
```

**European Put Price (Black-Scholes)**:
```
P = Ke^(-rT)N(-d₂) - S₀N(-d₁)
```

Where:
```
d₁ = [ln(S₀/K) + (r + σ²/2)T] / (σ√T)
d₂ = d₁ - σ√T
```

### Monte Carlo Simulation

The Monte Carlo approach simulates `N` price paths and estimates the option price as:

$$\text{Price} = \frac{1}{N} \sum_{i=1}^{N} \text{Payoff}(S_T^{(i)})$$

**Geometric Brownian Motion Discretization**:
```
S(t+dt) = S(t) × exp[(μ - σ²/2)dt + σ√dt × Z]
```
Where `Z ~ N(0,1)` is a standard normal random variable.

### Asian Option Payoff

The Asian call payoff is based on the ratio of arithmetic means over two periods:

$$\text{Payoff} = \max\left(\frac{\text{mean}(S_{61:120})}{\text{mean}(S_{1:60})} - K, 0\right)$$

The time discretization uses 252 trading days per year, with observations split into two periods.

### Tunnel Option Payoff

The Tunnel option has a complex payoff structure with quarterly observations:

$$P = \sum_{i=1}^{T-1} \begin{cases}
\frac{S_i}{S_0} \times 0.01 & \text{if } K_1 < S_i < K_2 \\
\frac{S_i}{S_0} \times 0.02 & \text{if } S_i \geq K_2 \\
0 & \text{otherwise}
\end{cases} + \max\left(\frac{S_T}{S_0} - K_2, 0\right)$$

## 🗂️ Project Structure

```
Option-Pricing-BS-MonteCarlo/
│
├── app.py                    # Main application (single file)
├── requirements.txt          # Python dependencies
├── README.md                # This file
└── LICENSE                  # License file
```

## 🔧 Key Functions

### European Options
- `d_j()`: Calculate d₁ and d₂ for Black-Scholes formula
- `vanilla_call_price()`: Black-Scholes call pricing
- `vanilla_put_price()`: Black-Scholes put pricing
- `simulate_gbm()`: Geometric Brownian Motion path simulation
- `process()`: Vectorized multi-path generation
- `payoff_call()` / `payoff_put()`: Payoff calculations
- `simulate_monte_carlo()`: Monte Carlo option pricing
- `convergence_mc()`: Convergence analysis with confidence intervals

### Asian Options
- `generate_t_as()`: Time grid with two observation periods
- `simulate_gbm_as()`: GBM simulation for Asian options
- `process_as()`: Vectorized path generation
- `payoff_as()`: Asian option payoff (ratio of means)
- `simulate_montecarlo_as()`: Monte Carlo Asian option pricing
- `convergence_mc_as()`: Convergence with error tracking

### Tunnel Options
- `generate_t_Tunnel()`: Quarterly time discretization
- `simulate_gbm_tunnel()`: GBM for tunnel options
- `process_tunnel()`: Vectorized path generation
- `payoff_Tunnel()`: Multi-level barrier payoff calculation
- `simulate_montecarlo_Tunnel()`: Monte Carlo tunnel option pricing
- `convergence_mc_Tunnel()`: Convergence analysis

### Streamlit Pages
- `euro_option_page()`: European option interface
- `asiatique_option_page()`: Asian option interface
- `tunnel_option_page()`: Tunnel option interface
- `main()`: Application entry point with page navigation

## 📈 Convergence and Error Analysis

The application computes confidence intervals using:

$$\text{CI} = \bar{X} \pm z_\alpha \frac{s}{\sqrt{n}}$$

Where:
- `X̄` is the sample mean
- `z_α` is the standard normal quantile for confidence level
- `s` is the sample standard deviation
- `n` is the number of simulations

Error is estimated as: `Error = (Upper_Bound - Lower_Bound) / 2`

## 🎯 Example Results

**European Call** (S₀=100, K=105, T=2, r=0.01, σ=0.15):
- Black-Scholes: ~8.02
- Monte Carlo (1000 sims): ~8.01 ± 0.15
- Convergence achieved with increasing N

**Asian Call** (S₀=100, K=1.05, T=2, σ=0.15, μ=0.015):
- Monte Carlo (1000 sims): Price depends on path evolution
- Error decreases with O(1/√N)

**Tunnel Option** (S₀=100, K₁=0.9, K₂=1.10):
- Complex payoff with barrier-dependent returns
- Visual representation shows price paths relative to barriers

## 🔬 Validation

The implementation validates Monte Carlo results against Black-Scholes analytical solutions for European options. The convergence plots demonstrate:
- Mean converges to theoretical price
- Confidence interval narrows with more simulations
- Standard error follows theoretical O(1/√N) rate

## 🚀 Future Enhancements

Potential improvements:
- Additional exotic options (Barrier, Lookback, Digital)
- Greeks calculation and visualization
- Implied volatility solver
- Variance reduction techniques (Antithetic variables, Control variates)
- Performance optimization with NumPy vectorization
- Export results to CSV/Excel
- Historical data integration
- Multi-language support

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/NewFeature`)
3. Commit your changes (`git commit -m 'Add NewFeature'`)
4. Push to the branch (`git push origin feature/NewFeature`)
5. Open a Pull Request

## 📚 References

1. Black, F., & Scholes, M. (1973). "The Pricing of Options and Corporate Liabilities", *Journal of Political Economy*, 81(3), 637-654
2. Hull, J. C. (2018). *Options, Futures, and Other Derivatives* (10th Edition), Pearson
3. Glasserman, P. (2003). *Monte Carlo Methods in Financial Engineering*, Springer
4. Shreve, S. E. (2004). *Stochastic Calculus for Finance II: Continuous-Time Models*, Springer

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Your Name**
- GitHub: [@yourusername](https://github.com/yourusername)

## 🙏 Acknowledgments

- Black-Scholes formula for European option benchmarking
- Streamlit framework for interactive web application
- NumPy and SciPy for numerical computations
