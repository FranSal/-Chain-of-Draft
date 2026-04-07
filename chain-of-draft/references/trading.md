# Trading / Financial Calculations — CoD Few-Shot Examples

Use these for position sizing, risk calculations, P&L, expectancy, SQN, and other quantitative trading computations.

## Few-shot set: Position sizing (pick 2-3)

### Example 1: Fixed fractional position sizing
Q: Account equity is $50,000. Risk per trade is 2%. Stop loss is $500 per contract. How many contracts should I trade?
A: 50000×0.02=1000 risk$; 1000/500=2 contracts. #### 2

### Example 2: Forex lot sizing
Q: I have €10,000 equity, risk 1% per trade. Entry at 1.0850, stop at 1.0800 on EUR/USD. Standard lot = 100,000 units. What lot size?
A: Risk=100€; SL=50 pips; 100/50=2€/pip; 2/10=0.2 lots. #### 0.2

### Example 3: Crypto position size
Q: Portfolio $25,000. Max risk 1.5%. BTC entry at $60,000, stop at $57,000. How many BTC to buy?
A: 25000×0.015=375$risk; 60000-57000=3000$/BTC risk; 375/3000=0.125 BTC. #### 0.125

## Few-shot set: System metrics (pick 2-3)

### Example 4: Expectancy
Q: A system has 60% win rate, average win $200, average loss $150. What is the expectancy per trade?
A: 0.6×200=120; 0.4×150=60; 120-60=60. #### $60

### Example 5: SQN (System Quality Number)
Q: A system has expectancy of 0.45R with standard deviation of 1.2R over 100 trades. What is the SQN?
A: SQN=√100×(0.45/1.2); 10×0.375=3.75. #### 3.75

### Example 6: Risk-reward ratio
Q: Entry at $100, stop at $95, target at $115. What is the risk-reward ratio?
A: Risk=100-95=5; Reward=115-100=15; R:R=15/5=3:1. #### 3:1

## Few-shot set: P&L calculations (pick 2-3)

### Example 7: Futures P&L
Q: Bought 3 ES contracts at 4500. Sold at 4520. Tick value is $12.50 per point. What is the profit?
A: 4520-4500=20pts; 20×12.50=250$/contract; 250×3=750. #### $750

### Example 8: Portfolio return
Q: Started with $100,000. After 12 months, account is at $118,000. What is the annual return percentage?
A: (118000-100000)/100000=0.18=18%. #### 18%

### Example 9: Drawdown
Q: Account peak was $50,000. Current equity is $42,000. What is the current drawdown percentage?
A: (50000-42000)/50000=0.16=16%. #### 16%

## Usage notes

- Trading calculations are inherently multi-step — perfect for CoD
- Always show the critical multiplication/division chain
- Use $ or % symbols to keep units clear in minimal space
- For Van Tharp-style R-multiples, express everything in R units
- Position sizing follows a universal pattern: risk$ / risk_per_unit = units
- SQN formula: √N × (mean_R / std_R) — show substitution in one line
