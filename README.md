# Bond-Returns

# Bond Returns: Interest Rate Risk Analysis

*"The essence of investment management is the management of risks, not the management of returns"* - Benjamin Graham

A comprehensive quantitative analysis of bond portfolio risk management, focusing on interest rate sensitivity measures and statistical risk factor decomposition using real market data.

## 📊 Project Overview

This project implements and analyzes various interest rate risk metrics for fixed-income portfolios, combining traditional bond mathematics with modern statistical techniques. The analysis covers duration, convexity, key rate sensitivities, and principal component analysis of bond index returns.

### Key Features

- **Classical Bond Risk Metrics**: Duration, modified duration, and convexity calculations
- **Key Rate Analysis**: DV01 and key rate shift (KR01) decomposition
- **Portfolio Strategy Comparison**: Barbell vs. Bullet strategy analysis
- **Statistical Risk Factors**: PCA-based factor extraction from bond index returns
- **Real Market Data**: Analysis of 33 BofA Merrill Lynch bond indexes (1998-2025)

## 🛠️ Installation & Setup

### Requirements

```bash
pip install pandas numpy matplotlib scikit-learn statsmodels
```

### Data Dependencies

This project requires access to:
- **FRED API**: For bond index data and Treasury rates
- **Custom Libraries**: 
  - `finds.readers.Alfred`: FRED data interface
  - `finds.recipes.bond_price`: Bond pricing utilities

### API Setup

Create a `secret.py` file with your FRED API credentials:
```python
credentials = {
    'fred': {
        'api_key': 'your_fred_api_key_here'
    }
}
```

Get your free FRED API key at: https://research.stlouisfed.org/docs/api/api_key.html

## 📈 Analysis Components

### 1. Interest Rate Sensitivity Measures

#### Duration & Convexity
- **Macaulay Duration**: Weighted average time to receive cash flows
- **Modified Duration**: Price sensitivity to yield changes
- **Convexity**: Curvature adjustment for large rate movements

#### Key Rate Analysis
- **DV01**: Dollar value impact of 1 basis point rate change
- **Key Rate Shifts**: Decomposition into 2Y, 5Y, and 10Y rate sensitivities
- **KR01**: Partial sensitivities to specific maturity buckets

### 2. Portfolio Strategies

#### Barbell vs. Bullet Comparison
- **Barbell Strategy**: Short + Long maturity bonds
- **Bullet Strategy**: Medium-term focused bonds
- **Convexity Arbitrage**: Exploiting positive convexity benefits

Example results for duration-matched portfolios:
- Both strategies achieve 8.18 years duration
- Barbell strategy delivers 36% higher convexity (107.4 vs 78.9)

### 3. Statistical Risk Factor Analysis

#### Principal Component Analysis
- **33 Bond Indexes**: Comprehensive market coverage
- **4 Main Factors**: Explaining 87.6% of return variance
- **Factor Interpretation**: Level, slope, twist, and credit spread components

#### Risk Factor Mapping
- **PC1 (59.3% variance)**: Level and credit risk
- **PC2 (18.3% variance)**: Yield curve slope
- **PC3 (7.6% variance)**: Credit spread dynamics  
- **PC4 (2.3% variance)**: Yield curve twist

## 📊 Key Results

### Bond Risk Metrics Example
| Bond | Duration | Convexity | Strategy |
|------|----------|-----------|----------|
| 5Y 2% Coupon | 4.68 | 24.82 | Barbell Component |
| 10Y 4% Coupon | 8.18 | 78.90 | Bullet Strategy |
| 20Y 6% Coupon | 12.62 | 212.46 | Barbell Component |

### Factor Model Performance
- **Level Factor**: R² = 54.7% (driven by credit and level changes)
- **Slope Factor**: R² = 48.6% (negative correlation with level)
- **Credit Factor**: R² = 4.8% (specialized credit risk)
- **Twist Factor**: R² = 2.4% (yield curve curvature changes)

## 🔧 Usage Examples

### Basic Bond Analytics
```python
from finds.recipes import bond_price

# Calculate bond price and duration
bond_params = dict(coupon=0.04, m=2, n=20, par=100, yields=0.04)
price = bond_price(**bond_params)
duration = modified_duration(**bond_params, price=price)
convexity = modified_convexity(**bond_params, price=price)
```

### Key Rate Sensitivity Analysis
```python
# Generate key rate shift patterns
KR = [2, 5, 10]  # Key rate maturities
# Calculate DV01 decomposition across maturity spectrum
```

### Statistical Factor Analysis
```python
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

# Extract principal components from bond returns
pipe = Pipeline([('scaler', StandardScaler()), ('pca', PCA())])
factors = pipe.fit_transform(bond_returns)
```

## 📁 Project Structure

```
bond-returns/
├── README.md
├── bond_analysis.ipynb          # Main analysis notebook
├── secret.py                    # API credentials (not in repo)
├── requirements.txt
└── data/
    └── bond_indexes.csv         # Cached bond index data
```

## 📚 Theoretical Background

### Duration Mathematics
For a bond with continuous compounding:

Duration = -1/P × dP/dy = Σ(t × CFₜ × e^(-y×t)) / P

Where:
- P = Bond price
- y = Yield
- CFₜ = Cash flow at time t

### Convexity Benefits
Convexity provides asymmetric returns:
- **Rate Decreases**: Greater price appreciation than duration predicts
- **Rate Increases**: Smaller price depreciation than duration predicts
- **Parallel Shifts**: Positive convexity always benefits bondholders

## 🎯 Applications

This analysis framework supports:
- **Portfolio Risk Management**: Quantifying interest rate exposure
- **Hedging Strategies**: Matching duration and key rate sensitivities  
- **Relative Value Trading**: Identifying convexity arbitrage opportunities
- **Factor Investing**: Understanding systematic risk drivers
- **Stress Testing**: Modeling portfolio behavior under rate scenarios

## 📊 Data Sources

- **Bond Indexes**: BofA Merrill Lynch Total Return Indexes (33 series)
- **Interest Rates**: U.S. Treasury constant maturity rates (2Y, 5Y, 10Y)
- **Credit Spreads**: BAA corporate bond yields
- **Time Period**: January 1998 - February 2025
- **Frequency**: Daily returns and rate changes

## 🤝 Contributing

Contributions are welcome! Areas for enhancement:
- Additional risk metrics (VaR, Expected Shortfall)
- International bond market analysis
- Credit risk modeling extensions
- Real-time data integration
- Interactive visualization dashboard

## 📄 License

This project is available under the MIT License. See LICENSE file for details.

## 🔗 References

- Hull, J. "Options, Futures, and Other Derivatives"
- Tuckman, B. & Serrat, A. "Fixed Income Securities"  
- Federal Reserve Economic Data (FRED)
- BofA Merrill Lynch Bond Index Methodology

---

*Built with Python, pandas, scikit-learn, and financial market expertise*
