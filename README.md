# Algorithmic Trading Strategies - Long-Short Backtesting

A simple Python project demonstrating momentum-based long-short trading strategies with backtesting on Indian and US markets.

## 🎯 What Are Long-Short Strategies?

Unlike traditional "buy and hold" strategies that only profit when prices go up, **long-short strategies are ALWAYS in the market**:

- **Long Position (+1)**: When trend is up → Profit from rising prices
- **Short Position (-1)**: When trend is down → Profit from falling prices

Think of it like this: You're not just riding the wave up—you're also catching the wave down! 🏄‍♂️

## 📊 Strategies Included

### 1. 12-Period SMA Long-Short Strategy (NIFTY 50)
- **Ticker**: ^NSEI (NIFTY 50 Index)
- **Timeframe**: 5-minute intervals, 50 days
- **Logic**: 
  - Long when Close > 12-period SMA
  - Short when Close < 12-period SMA
- **Type**: Medium-speed trend following

### 2. 5-Period EMA Long-Short Strategy (TCS)
- **Ticker**: TCS.NS (Tata Consultancy Services)
- **Timeframe**: 5-minute intervals, 50 days
- **Logic**: 
  - Long when Close > 5-period EMA
  - Short when Close < 5-period EMA
- **Type**: Fast momentum strategy

## 🚀 Quick Start

### Prerequisites
```bash
pip install pandas yfinance numpy matplotlib
```

### Running the Strategies

**12-SMA Long-Short (NIFTY):**
```bash
python sma_long_short.py
```

**5-EMA Long-Short (TCS):**
```bash
python ema_long_short.py
```

## 📈 How These Strategies Work

### 12-Period SMA Strategy (Slower & Steadier)

**Step 1:** Calculate the 12-period Simple Moving Average
- Takes average of last 12 closing prices
- Smooths out short-term noise
- Shows the broader trend direction

**Step 2:** Generate Signals
- Price crosses **ABOVE** SMA → Go **LONG** (bet on price going up)
- Price crosses **BELOW** SMA → Go **SHORT** (bet on price going down)

**Step 3:** Hold Position Until Next Signal
- Stay long until price drops below SMA
- Stay short until price rises above SMA
- Always positioned—never sitting in cash!

**Real Example:**
```
9:30 AM - NIFTY: 21,500 | SMA: 21,450 → LONG position
10:00 AM - NIFTY: 21,480 | SMA: 21,470 → Still LONG
10:30 AM - NIFTY: 21,420 | SMA: 21,460 → SHORT position (reversed!)
11:00 AM - NIFTY: 21,380 | SMA: 21,450 → Still SHORT (profiting from drop!)
```

### 5-Period EMA Strategy (Faster & More Aggressive)

**Step 1:** Calculate 5-period Exponential Moving Average
- Like SMA but gives MORE weight to recent prices
- Reacts faster to price changes
- Catches trends earlier (but also gets faked out more)

**Step 2:** Generate Signals (Same Logic, Faster Response)
- Price crosses **ABOVE** EMA → Go **LONG**
- Price crosses **BELOW** EMA → Go **SHORT**

**Step 3:** Trade Frequently
- With 5-period EMA, you'll trade 2-3x more than 12-SMA
- Catches trends quickly but also whipsaws in choppy markets

**Key Difference from 12-SMA:**
- 5-EMA = Hyperactive trader (more trades, faster reactions)
- 12-SMA = Patient trader (fewer trades, waits for confirmation)

## 🔍 What the Code Shows You

### 1. Buy & Hold Return
What if you just bought on Day 1 and held till Day 50?
- Baseline to compare against
- Only profits from upward movement

### 2. Long-Short Strategy Return
What if you followed the long-short signals for 50 days?
- Can profit from BOTH up and down movements
- Shows if the strategy beats simple buy & hold

### 3. Outperformance
How much better (or worse) the strategy did vs buy & hold

### 4. Number of Trades
Total times we switched positions
- 5-EMA typically trades 2-3x more than 12-SMA

### 5. Position Distribution
Shows how much time spent in each position:
- Long periods: Betting on price increase
- Short periods: Betting on price decrease

### 6. Visual Performance Chart
A graph comparing cumulative returns of both strategies over time

## 💡 Long-Short vs Long-Only: What's the Difference?

### Long-Only Strategy (Conservative)
```
Price > SMA → Position = +1 (Long)
Price < SMA → Position = 0 (Cash/Out)
```
- Only profits when market goes up
- Sits in cash during downtrends

### Long-Short Strategy (Aggressive)
```
Price > SMA → Position = +1 (Long)
Price < SMA → Position = -1 (Short)
```
- Profits from BOTH up and down movements
- Always in the market

**Simple Analogy:**
- **Long-Only** = Umbrella (protects you from rain, but you're just standing still)
- **Long-Short** = Sailboat (you adjust sails to move WITH the wind, whichever way it blows!)

## 🎓 What You'll Learn

- How to download stock data using yfinance
- How to calculate Simple Moving Average (SMA)
- How to calculate Exponential Moving Average (EMA)
- How to implement long-short trading logic
- How to backtest trading strategies
- How to visualize strategy performance
- Difference between momentum and trend-following strategies

## 📊 When Do These Strategies Work Best?

### 12-SMA Long-Short Works Well:
✅ Clear trending markets (strong up or down movements)  
✅ Medium volatility markets  
✅ Liquid instruments like NIFTY  

### 5-EMA Long-Short Works Well:
✅ High volatility with clear momentum  
✅ Strong directional moves  
✅ Very liquid stocks like TCS  

### Both Strategies Struggle:
❌ Sideways/choppy markets (no clear direction)  
❌ Around major news events (unpredictable spikes)  

## 📁 Repository Structure

```
├── README.md
├── sma_long_short.py          # 12-SMA strategy on NIFTY
├── ema_long_short.py           # 5-EMA strategy on TCS
├── requirements.txt
└── .gitignore
```

## 🛠️ Customization Guide

### Change the Ticker
```python
# Instead of NIFTY
df = yf.download('^NSEI', ...)

# Try other indices/stocks
df = yf.download('^NSEBANK', ...)  # Bank NIFTY
df = yf.download('RELIANCE.NS', ...)  # Reliance
df = yf.download('AAPL', ...)  # Apple
```

### Adjust Time Period
```python
# Instead of 50 days
start1 = end1 - pd.Timedelta(days=100)  # 100 days
start1 = end1 - pd.Timedelta(days=365)  # 1 year
```

### Change Interval
```python
# Instead of 5-minute
interval='1m'   # 1 minute (very fast)
interval='15m'  # 15 minutes
interval='1h'   # 1 hour
interval='1d'   # Daily
```

### Adjust Period Length
```python
# For SMA strategy
sma = 20  # Slower, fewer trades
sma = 8   # Faster, more trades

# For EMA strategy
ema = 10  # Slightly slower than 5
ema = 3   # Super fast!
```

### Make It Long-Only (Remove Shorting)
```python
# Change this line:
df['position'] = np.where(df['Close'] > df['sma'], 1, -1)

# To this:
df['position'] = np.where(df['Close'] > df['sma'], 1, 0)
```

## 🔧 Key Coding Concepts Used

### 1. Data Manipulation with Pandas
- Loading data from yfinance
- Creating new columns
- Calculating rolling averages

### 2. NumPy for Calculations
- `np.where()` for conditional logic
- Percentage change calculations
- Cumulative product for returns

### 3. Matplotlib for Visualization
- Plotting strategy performance
- Comparing multiple strategies on one chart

### 4. Financial Concepts
- Moving averages (SMA vs EMA)
- Position sizing (+1, -1, 0)
- Cumulative returns calculation
- Backtesting methodology

## 🤝 Contributing

Feel free to:
- Add new indicators (RSI, MACD, Bollinger Bands)
- Test on different stocks/indices
- Improve visualizations
- Add more performance metrics
- Experiment with different parameters

## ⚖️ Disclaimer

**FOR EDUCATIONAL PURPOSES ONLY**

This project is designed to teach:
- Python programming for finance
- Backtesting concepts
- Trading strategy logic

This is NOT:
- Financial advice
- A guaranteed money-making system
- Ready for live trading

Always do your own research before making any investment decisions!

## 👨‍💻 Author

[Prince Saini]

---

**⭐ If this helped you learn algorithmic trading concepts, please star the repo!**
