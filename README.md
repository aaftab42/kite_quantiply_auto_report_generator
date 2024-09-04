# Welcome to My GitHub Repository!

Hello! I'm new to GitHub and still Transitioning from an entrepreneurial background to a data analyst and developer. This repository contains a program I’ve developed for processing and analyzing trading data. As I’m still learning and evolving in this space, the code may have some quirks or flaws. I plan to update this repository regularly, and any feedback or suggestions would be greatly appreciated.

## Project Overview

This project is specifically designed for traders, particularly those involved in high-frequency trading in the derivatives market of the Indian stock market. It integrates with Kite from Zerodha and Quantiply, which provides algorithmic trading services. Users can create and run their trading algorithms using these platforms.

### Problem Statement

- created this project to address a significant issue faced by traders when placing orders through Quantiply. Many traders use LIMIT orders, which capture the current price at the time of order creation. However, in the fast-paced trading environment where nanoseconds matter, if a LIMIT order doesn't execute immediately, the algorithm converts it into a market order. The trade then executes after a time delay set by the trader.

- In this process, Zerodha records the price at which the order was initially fired, but when generating trade reports from Zerodha, this crucial value is missing; only the average execution price is provided. To accurately measure slippage and assess risk on large positions, it's essential to track the price at the time the order was fired. Quantiply also doesn't offer this data, which is why I developed this program—to capture the strike price when the order was fired, allowing for detailed insights into slippage and better risk management.


## Functionality

The code performs the following tasks:

1. **Data Retrieval and Processing**:
   - **Kite Data**: Retrieves order data from Zerodha's Kite platform.
   - **Quantiply Data**: Downloads and processes trading reports from Quantiply.

2. **Data Matching and Transformation**:
   - Matches and merges data from Kite and Quantiply.
   - Formats and calculates profit and loss (P&L) for each trade.

3. **Data Export**:
   - Exports the processed data to Google Sheets for further analysis.

### Code Overview

1. **Kite Data Retrieval** (`kite_data()`):
   - Retrieves order data from Kite. 
   - **Note**: Replace `USERNAME` and `PASSWORD` placeholders with your actual login credentials.

2. **Entry and Exit Data Formatting** (`entry_exit_data()`):
   - Formats the order data for analysis.
   - **Note**: This function processes the raw data to distinguish between entry and exit orders.

3. **Quantiply Data Retrieval** (`quantiply_data()`):
   - Downloads and reads trading reports from Quantiply.
   - **Note**: Ensure that the download directory is correctly set up.

4. **Data Matching** (`order_data()`):
   - Matches Kite data with Quantiply data and calculates profit/loss.
   - **Note**: The `STRATEGY` field in `final_df` will be populated based on the algorithm names from Quantiply.

5. **Google Sheets Export** (`google_sheet_export()`):
   - Exports the final processed DataFrame to Google Sheets.
   - **Note**: Replace placeholders in the `SERVICE_ACCOUNT_INFO` dictionary with your actual Google API credentials.

### Sample Output

## Raw Data

The `raw_df` table contains the raw order data for trades executed on a specific day. The columns include details such as status, strike price, order type, and time of execution.

| Status    | Strike                     | Type | Quantity   | Price | Avg. Price | Trigger Price | Order Type | Product | Order ID         | Exchange Order ID | Time                 |
|-----------|----------------------------|------|------------|-------|------------|---------------|------------|---------|------------------|--------------------|----------------------|
| CANCELLED | FINNIFTY2490324200CE NFO  | SELL | 0 / 800    | 0.05  | 0          | 0             | LIMIT      | NRML    | 240903802031529 | 1000000271624992   | 2024-09-03 15:00:00 |
| COMPLETE  | FINNIFTY2490323150PE NFO   | SELL | 800 / 800  | 0.05  | 0.05       | 0             | LIMIT      | NRML    | 240903802031532 | 2100000252486831   | 2024-09-03 15:00:00 |
| COMPLETE  | FINNIFTY2490323850PE NFO   | BUY  | 800 / 800  | 0.05  | 0.05       | 0             | LIMIT      | NRML    | 240903801826423 | 2000000235138067   | 2024-09-03 15:00:00 |
| COMPLETE  | FINNIFTY2490323850CE NFO   | BUY  | 800 / 800  | 44.05 | 42.1609375 | 42.8          | LIMIT      | NRML    | 240903801827474 | 1000000250563101   | 2024-09-03 15:00:00 |
| COMPLETE  | FINNIFTY2490323850CE NFO   | SELL | 800 / 800  | 20.75 | 21.4       | 21.4          | LIMIT      | NRML    | 240903801825566 | 1000000250244627   | 2024-09-03 10:00:00 |
| COMPLETE  | FINNIFTY2490323850PE NFO   | SELL | 800 / 800  | 0     | 8.3515625  | 0             | MARKET     | NRML    | 240903801825586 | 2000000235080861   | 2024-09-03 10:00:00 |
| COMPLETE  | FINNIFTY2490323650CE NFO   | BUY  | 800 / 800  | 90.6  | 89.4       | 88            | LIMIT      | NRML    | 240903801310157 | 1000000144370086   | 2024-09-03 10:00:00 |
| COMPLETE  | FINNIFTY2490323650PE NFO   | BUY  | 800 / 800  | 79.9  | 78.95      | 77.6          | LIMIT      | NRML    | 240903801310156 | 2000000140821932   | 2024-09-03 10:00:00 |
| COMPLETE  | FINNIFTY2490323650CE NFO   | SELL | 800 / 800  | 42.8  | 44.003125  | 0             | LIMIT      | NRML    | 240903801309894 | 1000000144345348   | 2024-09-03 10:00:00 |
| COMPLETE  | FINNIFTY2490323650PE NFO   | SELL | 800 / 800  | 37.9  | 38.8       | 0             | LIMIT      | NRML    | 240903801309893 | 2000000140800367   | 2024-09-03 10:00:00 |
| COMPLETE  | FINNIFTY2490323150PE NFO   | BUY  | 800 / 800  | 0.25  | 0.2        | 0             | LIMIT      | NRML    | 240903801303980 | 2100000140402853   | 2024-09-03 10:00:00 |
| COMPLETE  | FINNIFTY2490324200CE NFO   | BUY  | 800 / 800  | 0.3   | 0.3        | 0             | LIMIT      | NRML    | 240903801303979 | 1000000143519809   | 2024-09-03 12:00:00 |


## Final Data

The `final_df` table provides the consolidated trading results after processing the raw data. It includes information about the trading strategy, order execution, and performance.

| STRATEGY  | INDEX             | SYSTEM | EPRODUCTTYPE | EDATETIME           | ESTRIKE | EOPTION | ETYPE | EPRICE | ETRIGGER | EAVERAGE  | EORDERTYPE | ESTATUS    | EQTY | EXDATETIME          | EXSTRIKE | EXOPTION | EXTYPE | EXPRICE | EXTRIGGER | EXAVERAGE  | EXORDERTYPE | EXSTATUS   | EXQTY | PL       | CREATED_AT                       | SYMBOL               |
|-----------|-------------------|--------|--------------|---------------------|---------|---------|-------|--------|----------|-----------|------------|------------|------|---------------------|----------|----------|-------|---------|-----------|------------|-------------|------------|------|----------|----------------------------------|----------------------|
| FIN HEDGE | NIFTY FIN SERVICE | ALGO   | NRML         | 2024-09-03 11:00:00 | 23150   | PE      | BUY   | 0.25   | 0.0      | 0.200000  | LIMIT      | COMPLETE   | 800  | 2024-09-03 15:00:00 | 23150    | PE       | SELL  | 0.05    | 0.0       | 0.050000   | LIMIT       | COMPLETE   | 800  | -120.00  | 2024-09-03 20:02:51.865627       | FINNIFTY2490323150PE  |
| FIN HEDGE | NIFTY FIN SERVICE | ALGO   | NRML         | 2024-09-03 10:00:00 | 24200   | CE      | BUY   | 0.30   | 0.0      | 0.300000  | LIMIT      | COMPLETE   | 800  | 2024-09-03 15:32:44 | 24200    | CE       | SELL  | 0.05    | 0.0       | 0.000000   | LIMIT       | CANCELLED  | 800  | -240.00  | 2024-09-03 20:02:51.865627       | FINNIFTY2490324200CE  |
| FIN SAMM  | NIFTY FIN SERVICE | ALGO   | NRML         | 2024-09-03 11:00:00 | 23650   | CE      | SELL  | 42.80  | 0.0      | 44.003125 | LIMIT      | COMPLETE   | 800  | 2024-09-03 14:00:00 | 23650    | CE       | BUY   | 90.60   | 88.0      | 89.400000  | LIMIT       | COMPLETE   | 800  | -36317.50| 2024-09-03 20:02:51.865627       | FINNIFTY2490323650CE  |
| FIN SAMM  | NIFTY FIN SERVICE | ALGO   | NRML         | 2024-09-03 10:00:00 | 23650   | PE      | SELL  | 37.90  | 0.0      | 38.800000 | LIMIT      | COMPLETE   | 800  | 2024-09-03 12:00:00 | 23650    | PE       | BUY   | 79.90   | 77.6      | 78.950000  | LIMIT       | COMPLETE   | 800  | -32120.00| 2024-09-03 20:02:51.865627       | FINNIFTY2490323650PE  |
| FIN AIFF  | NIFTY FIN SERVICE | ALGO   | NRML         | 2024-09-03 11:00:00 | 23850   | PE      | SELL  | 0.00   | 8.351562 | 0.000000  | MARKET     | COMPLETE   | 800  | 2024-09-03 11:00:00 | 23850    | PE       | BUY   | 0.05    | 0.0       | 0.050000   | LIMIT       | COMPLETE   | 800  | 6649.50  | 2024-09-03 20:02:51.865627       | FINNIFTY2490323850PE  |
| FIN AIFF  | NIFTY FIN SERVICE | ALGO   | NRML         | 2024-09-03 11:00:00 | 23850   | CE      | SELL  | 21.40  | 0.0      | 21.400000 | LIMIT      | COMPLETE   | 800  | 2024-09-03 12:00:00 | 23850    | CE       | BUY   | 44.05   | 42.8      | 42.160938  | LIMIT       | COMPLETE   | 800  | -16643.50| 2024-09-03 20:02:51.865627       | FINNIFTY2490323850CE  


- **Consolidated Data**: Combines entry and exit orders into a single column, facilitating easier data analysis.
- **Trigger Price Evaluation**: Helps in evaluating slippage for market and limit orders, which is essential for adjusting trading strategies.

### Contact

Feel free to reach out if you have any questions or suggestions:

- LinkedIn: [aaftab42](https://www.linkedin.com/in/aaftab42)
- Email: [aaftabahmed42@gmail.com](mailto:aaftabahmed42@gmail.com)

Thank you for checking out my project!

