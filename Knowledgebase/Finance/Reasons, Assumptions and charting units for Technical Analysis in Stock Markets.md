# Why technical analysis?
Fundamental analysis dives deep into details, but it makes it domain-specific. For example, when dealing with an agricultural commodity like Coffee or Pepper, the fundamental analysis includes analyzing rainfall, harvest, demand, supply, inventory etc. However, the fundamentals of metal commodities are different, so it is for energy commodities. So every time you choose a commodity, the fundamentals change and you have to spend more time and resources in developing analysis for different domains.

On the other hand, the concept of technical analysis will remain the same irrespective of the asset you are studying because it uses generalized rules, patterns, indices and measures. For example, an indicator such as Moving average convergence divergence (MACD) or Relative strength index (RSI) is used the same way on equity, commodity, or currency. Once you learn to drive a car, you can drive any car.
# Assumptions of technical analysis
1. Markets discount everything - This assumption tells us that all known and unknown information in the public domain is reflected in the latest stock price. For example, an insider could buy the company’s stock in large quantities in anticipation of a good quarterly earnings announcement. While the insider does this secretively, the price reacts, revealing to the technical analyst that something is about to happen in the stock price.

2. The "how" is more important than the "why" - This is an extension of the first assumption. Going with the same example discussed above - the technical analyst would not be interested in questioning **why** the insider bought the stock as long as the technical analyst knows **how** the price reacted to the insider’s action.

3. Price moves in trend - All major moves in the market are an outcome of a trend. The concept of trend is the foundation of technical analysis. For example, the recent upward movement in the NIFTY 50 Index to 18500 from 14750 did not happen overnight. This move happened in a phased manner in over 11 months. Another way to look at it is that once the trend is established, the price moves in the direction of the trend.

4. History tends to repeat itself - In the technical analysis context, the price trend tends to repeat itself. This happens because the market participants consistently react to price movements in remarkably similar ways every time the price moves in a certain direction. For example, in an uptrend, market participants get greedy and want to buy irrespective of the high price. Likewise, market participants want to sell in a downtrend irrespective of the low and unattractive prices. This human reaction has been the same towards stock prices over time, ensuring that history repeats itself.
# OHLC: A simple trend measure
In Technical Analysis, different measures are used to analyze market trends, as a first example take OHLC.
**Open Price** - When the markets open for trading, the first price a trade executes is called the opening price.

**The High Price -** This represents the highest price at which a trade occurred for the given day.

**The Low Price -** This represents the lowest price at which a trade occurred for the given day.

**The Close Price -** This is the most important price because it is the final price at which the market closes for the day. The close indicates the intraday strength and a reference price for the next day. If the close is higher than the open, it is considered a positive day; otherwise negative.

The closing price also shows the market sentiment and serves as a reference point for the next day’s trading. For these reasons, closing is more important than the opening, high or low prices.
# Charting for OHLC
A line chart can be constructed by taking time on the x-axis and share price on the y-axis. A line chart with share price vs time can be used to chart OHLC, but it would require four separate charts, each for open, high, low and close prices. This makes it harder to visualize as we would need to keep track of four things at once. There are two alternatives to a simple line chart.
1. Bar chart
2. Japanese Candlesticks
## Bar chart
A bar chart is a series of straight vertical lines with left and right ticks where each line represents open, high, low and close together. The top end of the vertical line represents high price, the bottom end represents low price. There are two ticks connected to the vertical line where the left tick represents the opening price and the right tick represents the closing price.
```
		high
		|
		|
		|
open----|
		|
		|
		|
		|----close
		|
		|
		|
		low
```
A series of these vertical lines with ticks over time can be used to chart OHLC all in the same graph.

> [!info]  A note about time periods
> Keep in mind that each bar spans a period. For example, if the period of the bar is 15 minutes, then the top and bottom ends of the vertical line represent the highset and lowest price that got traded within those 15 minutes and open and close ticks represent the opening and closing price before and after the 15 minutes. Using this bars of different periods can be used to focus on finer details with 1-5 minute periods or removing noise with periods that span days, weeks or months.

A typical bar chart looks like this:
![sample share price bar chart](https://zerodha.com/varsity/wp-content/uploads/2014/09/M2-Ch3-Chart2.jpg)
## Japanese Candlesticks
The earliest use of candlesticks dates back to the 18th century by a Japanese rice merchant named Homma Munehisa. Sometime around 1980’s a trader named Steve Nison discovered candlesticks, and he introduced the methodology to the rest of the world. He authored the first-ever book on candlesticks titled “Japanese Candlestick Charting Techniques”.

A candlestick chart is made of, well, candlesticks.
![anatomy of a candlestick](https://www.tradingwithrayner.com/wp-content/uploads/2024/06/1.-OHLC-Example.png)

A candlestick has a vertical rectangular body with two wicks sticking out from both ends. Like bar charts, candlesticks also span over periods.

The top end of the upper wick represents highest price of the period.
The bottom end of the lower wick represents lowest price of the period.

Recall that a positive period is when opening price before the period is lower than the closing price after the period and negative period is when opening is higher than the closing price.

If it is a positive period, the lower end of the rectangular body corresponds to the opening price and the upper end corresponds to the closing price. If it is a negative period, then the lower end of the rectangular body corresponds to the closing price and upper end corresponds to the opening price.

Based on the nature of the day, positive or negative, the upper and lower ends of the rectangular body represent different things. To distinguish, the rectangular body is colored a "hot color" like red when the candlestick is over a negative period and it is colored a "cool color" like blue or green when the candlestick is over a positive period.

The positive day candlesticks are called bullish candles, the negative day candlesticks are called bearish candles.

A typical candlestick chart looks like this:
![sample candlestick chart](https://zerodha.com/varsity/wp-content/uploads/2014/09/M2-Ch3-Chart3.jpg)

A candlestick chart is preferred by traders because it allows for inference of candlestick patterns in technical analysis. Also, candlestick patterns come with an inbuilt risk management mechanism.

[[List of Candlestick Patterns in Technical Analysis of Stock Market]]
# guidelines specific to candlesticks patterns
these assumptions are more guidelines than rules, many patterns may violate them in favor of some other specific assumption.
1. Buy strength and sell weakness - Strength is represented by a bullish candle and weakness by a bearish (red) candle. Hence whenever you are buying ensure, it is a blue candle day and whenever you are selling, ensure it’s a red candle day.
2. Be flexible with patterns (quantify and verify) - While the textbook definition of a pattern could state certain criteria, there could be minor variations to the pattern owing to market conditions. So one needs to be a bit flexible. However, one needs to be flexible within limits, and hence it is always required to quantify the flexibility.
3. Look for a prior trend - If you are looking at a bullish pattern, the prior trend should be bearish, and likewise, if you are looking for a bearish pattern, the prior trend should be bullish.
## basic psychological management when using candlesticks
No candlestick pattern can always predict the future with certainity. However, in the case when something goes wrong, it is required to not let emotions take over, one should book a loss with the inbuilt risk management mechanism and move forward.

The risk management mechanism of each candlestick can sometimes also cut your profits. For example, you might put a stoploss according to a candlestick pattern, and that stoploss might get triggered, but then the stock mysteriously starts going, up, in this case, you have booked a loss instead of a profit. But again, this is part of the game and one should move forward instead of making hasty decisions fueled by emotions of loss of monetary value.

Always remember: Once a trade is initiated, you should hold on to it until either the target is hit or the stoploss is breached. If you attempt to do something else before any one of these event triggers, your trade could most likely go bust. So staying on the course of the plan is extremely crucial.