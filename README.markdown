# Algo-Trading Prototype with ML and Google Sheets Integration

## Overview
This project is a Python-based algo-trading prototype implemented in a Jupyter Notebook (`AlgoTrading.ipynb`). It fetches stock data for three NIFTY 50 stocks (`INFY.NS`, `TCS.NS`, `HDFCBANK.NS`), generates buy/sell signals using a rule-based strategy (RSI < 30 with 20-DMA > 50-DMA), applies machine learning models (Logistic Regression and Decision Tree) to predict next-day price movements, and logs trade details and portfolio analytics to Google Sheets. The code is designed for modularity and includes error handling, with potential for enhancements like Telegram notifications.

## Objectives
- Fetch daily stock data for `INFY.NS`, `TCS.NS`, and `HDFCBANK.NS` over 6 months using `yfinance`.
- Implement a rule-based trading strategy: buy when RSI < 30 and 20-DMA > 50-DMA, sell when RSI > 70 or 20-DMA < 50-DMA, backtested over 6 months.
- Use Logistic Regression and Decision Tree models to predict next-day price direction (up = buy, down = sell/hold) using RSI, 20-DMA, 50-DMA, and Volume.
- Log trade details (Buy Date, Buy Price, Sell Date, Sell Price, P&L, Win) to per-stock Google Sheets tabs (e.g., `INFY.NS_TRADES`) and summary analytics (Total Trades, Total P&L, Win Ratio) to a `SUMMARY` tab.
- Ensure modular code with robust error handling.
- (Optional) Support Telegram notifications for trade signals (not implemented).

## Features
- **Data Ingestion**: Fetches 6 months of daily OHLCV data for specified stocks using `yfinance` (Cell 3).
- **Trading Strategy**: Generates buy/sell signals based on RSI and DMA conditions, backtested to produce trade logs (assumed via `backtest_strategy`).
- **Backtesting**: Produces trade logs with P&L and win status (Cell 6).
- **ML Models**: Trains Logistic Regression (Cell 5) and Decision Tree (Cell 7) models to predict price movements, reporting accuracy (e.g., 52.17% for `TCS.NS` Logistic Regression).
- **Portfolio Analytics**: Logs total trades, net P&L, and win ratio to Google Sheets (Cells 9 and 10).
- **Google Sheets Integration**: Updates trade details to `<symbol>_TRADES` tabs and summary metrics to a `SUMMARY` tab (Cells 9, 10, 11).
- **Error Handling**: Includes try-except blocks for data fetching, ML training, and Sheets updates (Cells 5, 7, 9, 10, 12).

## Project Structure
```
algo-trading-prototype/
├── AlgoTrading.ipynb         # Jupyter Notebook with full implementation
├── algotradingfree-key.json  # Google Sheets credentials (not committed)
├── requirements.txt          # Dependencies
├── README.md                # This file
```

## Requirements
- Python 3.11+
- Libraries:
  - `yfinance`
  - `pandas`
  - `numpy`
  - `scikit-learn`
  - `gspread`
  - `oauth2client`
- Google Sheets API credentials (`algotradingfree-key.json`)

Install dependencies:
```bash
pip install yfinance pandas numpy scikit-learn gspread oauth2client
```

## Setup
1. **Google Sheets API**:
   - Create a Google Cloud project and enable Google Sheets and Drive APIs.
   - Download service account credentials as `algotradingfree-key.json` and place in the project root.
   - Share the `AlgoTradingResults` Google Sheet with the service account email.
2. **Run the Notebook**:
   - Open `AlgoTrading.ipynb` in Jupyter Notebook or Google Colab.
   - Ensure `algotradingfree-key.json` is uploaded to the runtime environment (e.g., Colab's file system).
   - Execute all cells sequentially to:
     - Install dependencies (Cell 1).
     - Fetch stock data (Cell 3).
     - Run ML models (Cells 5, 7).
     - Log results to Google Sheets (Cells 9, 10, 11).
     - Run the full algo (Cell 12).

## Usage
- **Run the Algo**:
  - Execute the entire notebook or run the `run_algo` function (Cell 12) with:
    ```python
    stock_list = ['INFY.NS', 'TCS.NS', 'HDFCBANK.NS']
    run_algo(stock_list, sheet)
    ```
  - This fetches data, generates signals, backtests, trains ML models, and logs results.
- **Output**:
  - **Console**:
    ```
    ð��� Starting Algo-Trading Run...
    ð��¦ Fetching: INFY.NS
    â�� Done: INFY.NS â�� 2 trades
    ð��¦ Fetching: TCS.NS
    â�� Done: TCS.NS â�� 2 trades
    ð��¦ Fetching: HDFCBANK.NS
    â�� Done: HDFCBANK.NS â�� 1 trades
    ð��� Logistic Regression Prediction Accuracy:
    â�� INFY.NS: Accuracy = 43.48%
    â�� TCS.NS: Accuracy = 52.17%
    â�� HDFCBANK.NS: Accuracy = 60.87%
    ð��³ Decision Tree Prediction Accuracy:
    â�� INFY.NS: Accuracy = 47.83%
    â�� TCS.NS: Accuracy = 60.87%
    â�� HDFCBANK.NS: Accuracy = 39.13%
    ð��¤ Logging results to Google Sheets...
    â�� Updated: INFY.NS_TRADES
    â�� Updated: TCS.NS_TRADES
    â�� Updated: HDFCBANK.NS_TRADES
    â�� Updated: SUMMARY
    ð��� Algo Run Complete. Sheets Updated.
    ```
  - **Google Sheets**:
    - `<symbol>_TRADES` tabs: Trade logs (e.g., Buy Date, Buy Price, Sell Date, Sell Price, P&L, Win).
    - `SUMMARY` tab: Stock, Total Trades, Total P&L, Win Ratio.

## Example Output
- **TCS.NS Trades** (Cell 6):
  ```
  Buy Date: 2025-03-11, Buy Price: 3543.80, Sell Date: 2025-04-29, Sell Price: 3441.91, P&L: -101.89, Win: False
  Buy Date: 2025-05-30, Buy Price: 3432.89, Sell Date: 2025-06-26, Sell Price: 3441.80, P&L: 8.91, Win: True
  ```
- **HDFCBANK.NS Trades** (Cell 6):
  ```
  Buy Date: 2025-03-11, Buy Price: 1667.16, Sell Date: 2025-03-24, Sell Price: 1780.41, P&L: 113.25, Win: True
  ```
- **Summary** (Cell 10):
  - Total Trades: 2 (TCS.NS), 1 (HDFCBANK.NS)
  - Total P&L: -92.98 (TCS.NS), 113.25 (HDFCBANK.NS)
  - Win Ratio: 0.50 (TCS.NS), 1.00 (HDFCBANK.NS)

## Limitations
- **Missing Functions**: The notebook references `add_indicators_manual` and `backtest_strategy`, which are not provided. These are assumed to:
  - Compute RSI, 20-DMA, and 50-DMA.
  - Implement the rule-based strategy (buy: RSI < 30 and 20-DMA > 50-DMA; sell: RSI > 70 or 20-DMA < 50-DMA).
- **Missing Analytics**: Average P&L per trade and max profit/loss are not explicitly computed in the summary.
- **MACD**: Not implemented as an ML feature, despite being mentioned in objectives.
- **Telegram Notifications**: Not implemented, though `python-telegram-bot` is listed in requirements.
- **Data Structure**: Assumes `results` dictionary with `signals` and `trades` DataFrames, which may fail if `backtest_strategy` output is inconsistent.

## Future Improvements
- Implement `add_indicators_manual` to calculate RSI, 20-DMA, 50-DMA, and optionally MACD.
- Implement `backtest_strategy` to ensure consistent signal generation and trade logging.
- Add missing analytics (e.g., average P&L, max profit/loss, Sharpe ratio).
- Incorporate MACD as an ML feature for improved predictions.
- Add Telegram notifications using `python-telegram-bot` for trade signals and errors.
- Include transaction costs and slippage in backtesting.
- Add visualizations (e.g., `matplotlib` for price trends, P&L curves).
- Add unit tests using `pytest` for robustness.

## License
MIT License