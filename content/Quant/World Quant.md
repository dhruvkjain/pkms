[[Quant/index|Quant/index]]

1,20,000 data fields 

national rounds - mumbai
international rounds - 2025 - singapore

daily max limit (by submitting 1-2 alphas) - 2000
feature that tells you your alpha is +ve score or not - only team members that qualify

main score (unlocks after qualifying) - 
qualifying score - 10,000

don't submit from 19th May to 27th May

after qualifying you are invited to become conditional(background check) research consultant.

there are 300 stocks (300 alpha weights)

we need to generate  alpha matrix to generate alpha weights for each stock on each date
alpha weight - ratio of what amount of your portfolio(total money) you buy on that day in that stock.


World Quant - equity(stocks) long-short market neutral strategy.

US region 
16 datasets

universe - 
- top 3000 - top 3000 market capitalization stocks in terms of there size

delay - 
- delay 1 - uses yesterdays closing 

decay - 
- moving average of the value(5 == last 4 days moving average)

std deviation of returns -> risk
turnover -> average of returns


- Idea: Operating earning yield (OEY) refers to the amount of earnings from operating activities an investor can obtain for each dollar invested. Higher OEY implies higher returns for the same amount of invested capital thus representing an investment opportunity.  
- **Suggested data field:** operating_income, cap, industry
- **Suggested operator**: ts_rank, group_rank, group_mean

examples: https://platform.worldquantbrain.com/learn/documentation/create-alphas/19-alpha-examples

https://www.quantconnect.com/


---

# Finance Basics

**Stock Market**: place where buyers and sellers trade shares/stock of publicly listed companies 
**Share/Stock**: each share/stock represents a small portion of ownership in a company 

**Hedge Firms**: investment vehicles that utilize a range of strategies to seek to generate returns for their investors.

Hedge Firms are different amongst investment companies in the following ways:
- *Leverage*: involves borrowing money that is used to deploy investment ideas
- *Shorting*: when investor borrows and sells shares hoping to buy them back when prices are low.
- *Long position*: buying the stock, and making money when it increase in value
- *Volume*: Volume is the amount of an asset or security that changes hands over some period of time, often over the course of a day. For instance, stock trading volume could refer to the number of shares of a security traded between its daily open and close.

If a company’s stock has high volume, it means that many people are buying and selling the stock.

**Quant Hedge Firms**: firms that use advanced mathematical models, algorithms and statistical techniques for their investment decisions. 

## Alphas 
mathematical models that seek to predict future price movements of various financial instruments.  

Alphas seek to capture an inefficiency in the market which is the applied to a chosen group of stocks called our Alpha Universe.

Alpha is therefore a vector of predicted values of stocks in universe.
each value can change daily and is change is defined by it's direction and magnitude.
direction determine if we should long or short the stock
magnitude represent the proportion of total capital that is allocated to that stock

**Long-Short Neutrality**: we aim to balance estimated returns with downside protection 

**Backtesting**: testing put alpha against past years market. 

**Look Ahead Bias**: pitfall where future information accidentally influences the historical data analysis.

**Performance related metrics**:
- *Sharpe*: measure of risk adjusted returns earned by alphas (higher is better)
- *Turnover*: percentage of capital that alpha trade each day
- *Drawdown*: percentage of the largest loss incurred during a year during backtesting
		return/drawdown > 1 (higher is better)
- *Correlation*: should be low unless we see much higher performance as compared to the correlated alphas
- *Weight*: check if performance is contributed by diverse set of stocks, weight test ensures alpha weight is evenly distributed across stocks and not concentrated on few.
- *Sub-universe*:  check if performance is contributed by diverse set of stocks, sub-universe test check if alpha's performance in the immediate smaller set of tradable stocks, or sub-universe exceeds a required threshold.

Returns is defined by WorldQuant as the return on capital traded:
	*AnnualReturn = AnnualizedPnL / HalfofCapitalTraded*

Profit and Loss (PnL)

Basic Alpha Idea: 
	**Price Reversion**: buy stocks at lower prices expecting it to rise to historical average price and, for the stocks that whose prices are rising we short them expecting the will decline.


# Implementation on BRAIN

**Weights**: market data being a matrix, with each row representing one date and each column representing one stock.

The role of the Alpha expression is to transform the input matrix to an output vector of weights, with each weight corresponding to one of the stocks, where it's value range from -1 to 1, -ve meaning short(sell) and +ve meaning long(buy). 

Once we have got the weights of the stock from the Alpha expression, the next step is to get each day’s profit and loss (PnL).

the amount of money we have got to invest is called the "**book size**". Suppose our book size is 100 USD. So I calculate the money I want to invest in each of the stocks:

money_A = 0.2 * 100 USD = 20 USD Long

money_B = -0.5 * 100 USD = 50 USD Short

money_C = 0.3 * 100 USD = 30 USD Long

> Note: In BRAIN, we use constant book size for all the days, regardless of whether your portfolio makes money or loses money.

This is repeated for each day in the simulation period to calculate and plot the cumulative PnL.

### Results:

A good Alpha tends to ideally have 
- consistently increasing PnL, 
- high Annual Return, 
- and more importantly, few fluctuations in the cumulative profit graph. 

If the standard deviation is low, there tends to be lesser fluctuations in the graph. 
If the graph shows high fluctuations/volatility, despite the returns being high, the Alpha will not be deemed good enough.

Equity long-short market neutral strategy is used commonly by hedge funds, with the goal of minimizing exposure to the market and profit from the changes in the spread between two stocks.

## Datasets and Data Fields:

Datasets are a collection of data fields. 
For example, ‘open price’ and ‘close price’ can be found in the price volume dataset

