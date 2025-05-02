# Case Study: Robinhood AUC, Net Deposits, and the Shift Towards Active Trading

## Abstract

This case study analyzes the relationship between Robinhood's reported Assets Under Custody (AUC), monthly Net Deposits, and the SPDR S&P 500 ETF Trust (SPY) performance from January 2022 to March 2025. Visualization reveals AUC closely tracks SPY, driven by market valuation. However, Net Deposits exhibit distinct patterns, including lagged recovery post-downturn and acceleration during rallies, suggesting evolving retail investor sentiment. Notably, recent increases in Net Deposits, coinciding with platform enhancements like futures trading, hint at a potential shift towards a more active user base. This evolution could imply greater resilience in Net Deposits and trading revenue for Robinhood, even during market downturns, compared to reliance solely on passive, buy-and-hold investors.

## Background

* **Robinhood Markets, Inc.:** A prominent financial services platform offering commission-free trading, attracting a large retail investor base.
* **Assets Under Custody (AUC):** Total market value of client assets held on the platform, influenced by market performance and fund flows.
* **Net Deposits:** Monthly net flow of customer funds (Deposits - Withdrawals), indicating active capital allocation decisions.
* **SPDR S&P 500 ETF Trust (SPY):** Benchmark for U.S. stock market performance.
* **Platform Evolution:** Robinhood has expanded its offerings beyond basic stock trading to include options, cryptocurrencies, retirement accounts, and more recently, futures trading, catering to a broader range of investor/trader profiles.

Understanding how market trends, active fund flows, and platform changes interact is crucial for assessing Robinhood's growth trajectory and revenue stability.

## Objective

To analyze the interplay between Robinhood's AUC, Net Deposits, and SPY performance, using the provided data (Jan 2022 - Mar 2025), and to hypothesize how platform evolution, particularly the introduction of products appealing to active traders (like futures), might influence future Net Deposit trends and platform resilience.

## Data & Methodology

### Data Sources & Collection

* **SPY Data:** Daily historical closing prices for the SPDR S&P 500 ETF (SPY) were obtained (e.g., from historical financial data providers like Nasdaq). Raw data included columns like Date, Open, High, Low, Close, Volume.
* **Robinhood Data:** Monthly operational metrics reports, specifically containing Assets Under Custody (AUC) and Net Deposit figures, were sourced directly from Robinhood's Investor Relations website. These reports were available as `PDF` documents.

### Timeframe

* January 2022 – March 2025 (as per provided dataset)

### Variables

* Robinhood AUC (Billion USD)
* Robinhood Net Deposits (Billion USD)
* SPY Closing Price (USD) - Month-End

### Data Processing & Tools

* **SPY Data Cleaning:** Initial SPY historical data was processed using `Microsoft Excel` to remove unnecessary columns (Volume, Open, High, Low), keeping only the Date and Close price columns needed for the analysis.
* **SPY Month-End Extraction:** The cleaned daily SPY data was then processed using `Google BigQuery` (as detailed in the SQL query below) to select only the closing price for the last trading day of each month within the specified timeframe.
* **Robinhood Data Extraction:** `Google Gemini` was employed to scrape and extract the required monthly AUC and Net Deposit figures directly from the downloaded `PDF` reports from Robinhood's Investor Relations site.
* **Data Consolidation & Merging:** The extracted Robinhood data and the processed month-end SPY data were imported into `Google Sheets` for final validation and merging into a single dataset aligned by month.
* **Visualization:** A multi-axis line chart was created to effectively display variables with significantly different scales (AUC, Net Deposits, SPY Price). This was achieved using:
    * **HTML and JavaScript:** The foundation for the interactive chart.
    * **`Chart.js` library:** Provided the core charting functionality (configured as a `line` chart).
    * **`chartjs-adapter-date-fns`:** Used alongside `Chart.js` for correct handling and formatting of time-series data (dates) on the x-axis.
    * **Multiple Axes Configuration:** Three distinct Y-axes were defined within the `Chart.js` `scales` options to handle the different data scales and units:
        * `yAuc`: Primary left axis (Blue line) for AUC (Billion USD).
        * `yDeposits`: Secondary left axis (Green dashed line) for Net Deposits (Billion USD).
        * `ySpy`: Right axis (Red line) for SPY Closing Price (USD).
    * **Embedded Data:** The consolidated monthly data points for Date, AUC, Net Deposits, and SPY Price were embedded directly into the JavaScript code as an array of objects.
    * **Custom Styling:** Chart appearance (colors, labels, tooltips, grid lines) was customized using `Chart.js` configuration options.

### Analytical Approach

* **Trend analysis and interpretation:** The multi-axis line chart was visually analyzed to identify correlations, divergences, and specific patterns in the relationship between the three variables over time. Focus was placed on Net Deposit behavior relative to market direction (SPY) and overall asset value (AUC).

### Actual SQL Query Used for SPY Data (BigQuery)

The following Google BigQuery SQL query was used on the cleaned data (containing only Date and Close) to extract the month-end data points for SPY from the `firs-bg-project.datesspy.datespy` table for the period January 2022 to March 2025:

```sql
SELECT *
FROM `firs-bg-project.datesspy.datespy`
WHERE DATE(date) = LAST_DAY(DATE(date), MONTH) -- Selects only rows where the date is the last day of its month
  AND DATE(date) >= '2022-01-01'               -- Start date filter
  AND DATE(date) <= '2025-03-31';              -- End date filter (inclusive of the last day of March 2025)
```
![image](https://github.com/user-attachments/assets/1721381a-e2e3-4c56-be28-de366a920238)

## Analysis & Findings (Based on the Chart)

The multi-axis line chart reveals several key dynamics:

* **`AUC` & `SPY` Correlation:** `AUC` (blue line, primary left axis) shows a strong visual correlation with `SPY` Price (red line, right axis). Peaks and troughs largely coincide, confirming that market valuation is a primary driver of `AUC` fluctuations.

* **Net Deposits vs. Market Momentum:** `Net Deposits` (green dashed line, secondary left axis) display a more complex relationship:
    * *Lagging Recovery (Mid-2022 to Mid-2023):* While `SPY` began recovering in late 2022/early 2023, `Net Deposits` remained relatively flat and low (around `$1.0B` - `$1.6B`), suggesting retail investors hesitated to add significant new capital immediately following the downturn.
    * *Accelerated Inflows (Late 2023 - 2024):* As the market rally gained strong momentum (`SPY` climbing steadily), `Net Deposits` saw a significant uptick, frequently exceeding `$3.5B` and peaking near `$5B` - `$7.6B` in late 2024/early 2025 in this dataset. This suggests increased confidence or FOMO driving capital inflows during strong bull phases.
    * *Sensitivity to Volatility (e.g., Sep 2024):* The dip in `Net Deposits` during September 2024, despite a high closing `SPY` price, highlights that net flows can react negatively to perceived short-term risk or volatility, even if the overall market trend remains positive.

* **Recent Deposit Strength:** The data shows particularly strong `Net Deposits` in late 2024 and early 2025, reaching levels significantly higher than during the 2023 recovery phase, even as `SPY` experienced some volatility.

## Discussion: Platform Evolution & Future Implications

The observed patterns in `Net Deposits`, particularly the recent strength, invite consideration of factors beyond simple market sentiment. Robinhood's strategic expansion into products favored by more active traders, such as options and futures, could be influencing these flows.

### Attracting Active Traders

Features like futures trading appeal to users interested in short-term market movements, hedging, or leveraging volatility, rather than just long-term investing. This potentially diversifies Robinhood's user base beyond traditional buy-and-hold investors.

### Potential for Increased Engagement

Active traders typically engage with the platform more frequently, generating trading volume irrespective of overall market direction (up or down).

### Hypothesis on Net Deposits & Revenue Resilience

If Robinhood successfully attracts and retains a larger cohort of active traders, `Net Deposits` might become less solely dependent on positive market momentum. Active traders may deposit funds to capitalize on volatility, potentially leading to more consistent, or even counter-cyclical, net inflows compared to periods dominated by passive investing sentiment.

This shift could enhance Robinhood's revenue resilience. Trading-based revenue (like Payment for Order Flow or PFOF, commissions on crypto/futures) is driven by activity volume. Increased activity from traders, even during market downturns or sideways markets where passive investors might pause deposits, could provide a more stable revenue stream compared to relying heavily on `AUC`-based fees or interest on idle cash balances which are more sensitive to market levels and interest rates. The strong deposit numbers seen towards the end of the dataset, coinciding with these product rollouts, could be an early indicator of this trend.

### Caveats

This remains a hypothesis based on interpreting the chart alongside platform strategy. Confirmation would require more granular data on user segments, trading volumes per product, and analysis over different market cycles. The provided data doesn't definitively prove this shift, but the `Net Deposit` behavior, especially recent strength, aligns plausibly with the potential impact of attracting more active users.

## Conclusion

While Robinhood's `AUC` remains tightly coupled with S&P 500 performance, `Net Deposits` reveal a dynamic picture of investor behavior influenced by market sentiment, momentum, and volatility. The analysis suggests a potential evolution in Robinhood's user base, possibly driven by the introduction of products like futures trading that appeal to more active participants. If this trend continues, it could lead to `Net Deposit` patterns that are less strictly correlated with simple market direction and potentially enhance the resilience of Robinhood's trading revenues, particularly during periods of market uncertainty or downturns. Further analysis with segmented user data would be needed to validate the extent of this shift.




