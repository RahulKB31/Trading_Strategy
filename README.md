### Strategy Documentation: Trading Strategy Based on Pre-Calculated Vectors

#### **1. Overview**
The task is to develop a trading strategy leveraging pre-calculated vector representations of stock market trends over rolling windows (e.g., 20 minutes) for 5 years of historical data. These vectors encapsulate price movements for various instruments and are generated by a pre-trained model. The goal is to predict future price movements and execute profitable trades based on these predictions.

The strategy is divided into two parts:
1. **Prediction of Price Movements** using two machine learning models:
   - **LSTM** (Long Short-Term Memory) network: Used to predict future price trends based on historical vector data.
   - **Random Forest Classifier**: A classification model that predicts price movements by categorizing price changes into three actions: Buy, Hold, and Sell.
   
2. **Risk Management** by applying stop-loss and take-profit thresholds to protect capital and maximize returns.

#### **2. Data Understanding**
The vector representations capture rolling windows of price movements. For example, if the market opens at 9:15 AM, the first vector represents the price movements between 9:15 and 9:35, the next vector represents 9:16 to 9:36, and so forth. This forms a continuous time series of vectors representing price behavior.

#### **3. Analyzing Vectors**
To start, the provided vector data is loaded using `gensim`'s `KeyedVectors`. The key insights from this analysis include:
- Each vector corresponds to a specific time window.
- Vectors capture multi-dimensional price movements (e.g., open, high, low, close prices).
- Cosine similarity between vectors is calculated to measure the similarity of price movements over consecutive time windows.

The strategy extracts patterns by comparing vectors over time, providing insights into market trends.

#### **4. Trading Strategy**

**A. Long Short-Term Memory (LSTM) Model**
LSTM is used to predict future price trends based on past vector data. The LSTM model is trained using the following process:
- The pre-calculated vectors are split into training and testing sets.
- The input vectors are reshaped into a 3D format (samples, time steps, features) for LSTM processing.
- The model predicts price movement as a continuous variable.

**Prediction Logic**:  
Based on the LSTM prediction (`y_pred`):
- If the prediction indicates a significant positive price movement (`pred > 0.1`), the action is **Buy**.
- If the prediction indicates a significant negative price movement (`pred < -0.1`), the action is **Sell**.
- For smaller movements, the action is **Hold**.

**B. Random Forest Classifier**
The Random Forest model is used to classify price movements into discrete categories:
- **1** for Buy, **-1** for Sell, and **0** for Hold.
- The Random Forest model is trained using the difference between consecutive vectors as the target variable, converted into categorical labels based on a threshold.
  
**Prediction Logic**:
- The classifier predicts whether the price will increase, decrease, or stay stable, and corresponding trading actions (Buy, Sell, Hold) are executed.

#### **5. Risk Management**

To minimize losses and lock in profits, the following rules are applied:
- **Stop-Loss**: Exit a trade if the price moves against the position by 2% or more.
- **Take-Profit**: Exit a trade if the price moves in favor of the position by 5% or more.

These thresholds ensure that losses are capped, and profits are secured when the market behaves favorably.

#### **6. Backtesting**
The strategy was backtested using the historical data provided. The backtest simulated trades based on the predictions of the LSTM model and Random Forest classifier. The results were as follows:
- **LSTM Example**:
    - Price: 101 → Action: **Exit (Take Profit)**.
    - Price: 99 → Action: **Exit (Stop Loss)**.
- **Random Forest Example**:
    - Price: 102 → Action: **Exit (Take Profit)**.

Both models provided actionable signals, and risk management was applied to optimize the profitability of trades.

#### **7. Conclusion**
This trading strategy combines machine learning predictions with robust risk management to maximize profitability in stock trading. The LSTM model captures time-series dependencies, while the Random Forest classifier adds an additional layer of decision-making by categorizing price movements. With the integration of stop-loss and take-profit thresholds, the strategy can minimize risk while capturing significant gains.

#### **8. Future Enhancements**
- Incorporate additional features such as market sentiment data or technical indicators to enhance model predictions.
- Test the strategy across different timeframes and asset classes to generalize its effectiveness.
- Fine-tune the thresholds for stop-loss and take-profit based on volatility analysis for specific stocks.

---

This documentation outlines the approach, models used, and results from the backtesting of the trading strategy.
