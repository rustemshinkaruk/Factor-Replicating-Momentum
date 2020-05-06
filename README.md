# Factor-Replicating-Momentum

I replicated Mometum factor from Fama French paper and used some visualitation tools to depict the performance compared to S&p500 on the graph. Results at the end.

Problem 1.

Cleaning steps:

I downloaded 8 variables specified in the assignment from CRSP from Jan 1926 to Dec 2018. I used monthly data.

I restrict the sample to common shares (share codes 10 and 11) and to securities traded in the New York Stock Exchange, American Stock Exchange, or the Nasdaq Stock Exchange (exchange codes 1, 2, and 3).

I take absolute value of prices because there are some negative prices. (they are negative because price was not available at that date and average of bid ask is taken with minus sign) so it is reasonable to take absolute values of them.

Then I assign zero return to the month when company was created. It is reasonable because if you buy company at the date of creation your return will be zero on that day if we assume price to be constant that day. This assumption can be argued but having the limited data this is best what I can think of.

I take into account the delisting return. If return is missing on the delisting date, I insert delisting return instead and calculate the price for delisting date as previous price multiplied by one plus delisting return. If the previous price is not available, I exclude the entire row.  If both delisting returns and returns are not missing then I use for returns (1+ret)*(1+deltisting return)-1.

After I accounted for missing prices and delisting returns where it was possible (i.e. delisting return and previous price was available) I remove all rows with either missing prices, returns or shares outstanding. The reason for removing them is that there are periods when there are no data for several months in a row and it becomes hard to reasonably interpolate the data. 


Output calculations:

By the time I start my output calculations I don't have any missing values in data set.

As described in the paper I further delete all firms that does not fall under the following description:” We require that a firm have a valid share price and number of shares as of the formation date and that there be a minimum of eight monthly returns over the past 11 months, skipping the most recent month, which is our formation period. Following convention and CRSP availability, all prices are closing prices, and all returns are from close to close.”

The ranking returns are calculated as following:

![alt text](https://github.com/rustemshinkaruk/Factor-Replicating-Momentum/blob/master/table_1.png)

Using the following formula:

![alt text](https://github.com/rustemshinkaruk/Factor-Replicating-Momentum/blob/master/table_2.png)

If we will look at the figure above then the ranking period returns are the cumulative returns from close of the last trading day of April 2008 through the last trading day of March 2009. Note that, consistent with the literature, there is a one month gap between the end of the ranking period and the start of the holding period.

I calculate Market value by multiplying shares outstanding by price. 

Then I find total market capitalization for each month by summing market capitalizations of each individual firms for a particular month and lag it by 1 month.

Problem 2.

Construction of deciles based on Fama and French momentum:

As of close of the final trading day of each month, firms are ranked on their cumulative return from 12 months before to one month before the formation date. 

All firms meeting the data requirements are placed into one of ten decile portfolios on the basis of their cumulative returns over the ranking period. However, the portfolio
breakpoints are based on NYSE firms only. That is, the breakpoints are set so that
there are an equal number of NYSE firms in each of the 10 portfolios.12 The firms
with the highest ranking period returns go into portfolio 10 the Winner" decile
portfolio and those with the lowest go into portfolio 1, the Loser" decile. We also
evaluate the returns for a zero investment Winner-Minus-Loser (WML) portfolio which I introduce later in problem 4, which is the difference of the Winner and Loser portfolio each period.

Construction of deciles based on Daniel and Moskowitz (2016) momentum:
The procedure is the same as for Fama and French apart from one thing: here we calculate deciles based on all New York Stock Exchange, American Stock Exchange, or the Nasdaq Stock Exchange (exchange codes 1, 2, and 3). So we don’t require in this case that every decile has equal number of firms from NYSE.

Problem 3.

For each year and month I calculate 10 deciles portfolio returns for both Fama and French and Daniel and Moskowitz. I described in problem 2 how the breakpoints of each differs. 

The portfolio returns are formed in the following way:
Again returning to the picture from the previous problem the holding period returns of the decile portfolios are the value-weighted returns of the firms in the portfolio over the one month holding period from the closing price last trading day in April through the last trading day of May. Given the monthly formation process, portfolio membership does not change within month, except in the case of delisting. This means that, except for dividends, cash payouts, and delistings, the portfolios are buy and hold portfolios. 

I find weights by dividing each firm's market capitalization lagged by 1 period by total market capitalization lagged by 1 period for each firm for each month. Then to find value weighted return in period t I use weights that were calculated using t-1 market capitalization. I multiply weights by return. Then I find sum of weights multiplied by returns for each month for each decile portfolio.

The risk free rate series is the one-month Treasury bill rate that I include in output table and use it in order to calculate excess returns. The data is obtained from Fama and French website.

My output table spans the period from 1927 to 2018.

Problem 4.

In this problem I try to replicate Table 1 in Daniel and Moskowitz (2016), except for certain rows.

![alt text](https://github.com/rustemshinkaruk/Factor-Replicating-Momentum/blob/master/table_3.png)

My replication (and it spans period from 1927/01 to 2013/03):

![alt text](https://github.com/rustemshinkaruk/Factor-Replicating-Momentum/blob/master/table_4.png)

I report the following  statistics: annualized mean, annualized standard deviation, annualized sharpe ratio, skewness, and excess kurtosis

I used monthly data to calculate all statistics that are presented below. I used all sample data that I have in calculations of kurtosis and skewness. To calculate all statistics I used R built in functions: mean(), sd(),skewness(),kurtosis(). Sharpe ratio was calculated as mean(excess return)/sd(excess return)

I annualized using the convention where I multiply mean by 12 and standard deviation by square root of 12. 

I calculate the first row as a mean return of the portfolios for a particular decile from 1 to 10 and special column which is WML zero investment Winner-Minus-Loser (WML) portfolio, which is the difference of the Winner and Loser portfolio each period. This is equal weighted mean. 

I calculate the second row as a standard deviation of portfolio returns for a particular decile from 1 to 10 and special column which is WML zero investment Winner-Minus-Loser (WML) portfolio.

Problem 5.

I present the correlation table that shows in the first row the correlation between my replication and Daniel and Moskowitz portfolio returns taken from their website. The second row present the correlation between my replication and Fama and French portfolio returns taken from their website. 

![alt text](https://github.com/rustemshinkaruk/Factor-Replicating-Momentum/blob/master/table_5.png)

To calculate correlation I used built in R function cor(). The sample period is from 1927 to 2018.



Problem 6.

I retrieved the data for Levered Momentum strategy for the last decade. We can see that since crisis it didn’t beat the market in most of the time. 


![alt text](https://github.com/rustemshinkaruk/Factor-Replicating-Momentum/blob/master/table_6.jpg)



Problem 7.

Momentum strategies tend to crash during crisis (i.e. when volatility of market increases tremendously). And now with a lot of macroeconomic indicators showing slowdown of growth, Trade War with China,  the Fed’s interest hike it is not the best time to go for momentum. Also based on the data I provide in problem 6 it hasn’t worked since last crisis very well so I have no sound reason to go for momentum. I would say it depleted itself as more and more investors scrutinize it and it is not a good hedge during bad times.





