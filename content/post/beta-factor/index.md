---
title: 'What Are Smart-Beta Funds, and Do They Work in India?'
description: A 10-year, data-driven look at factor investing across Indian indices
slug: smart-beta
date: 2026-01-08
categories: finance
draft: false
image: cover1.jpg
---
> **Disclaimer:** Do not take anything written in this blog as investment advice, please do your own research before making any financial decisions.

A few months ago, when I started getting into finance, [Zerodha Varsity](https://zerodha.com/varsity/) was my go-to resource. I owe much of my finance/stock-market knowledge to the excellent modules over there.

While I reading through the mutual fund module, one article in particular stood out to me - ["Smart-Beta Funds"](https://zerodha.com/varsity/chapter/smart-beta-funds/). What surprised me wasn't the concept itself, but how little discussion it had compared to other varsity articles. While most modules had hundreds if not thousands of comments, this one had a meagre 25-30.

This lack of attention felt odd, and as someone who likes to dig into things that fly under the radar I decided to look deeper into them. This blog is an attempt to summarize my learnings and present some of the analysis I've done on these funds. I'll try my best to keep it as beginner friendly as possible so don't worry if you don't know anything about these funds.

## Index Funds

One of the best ways to understand something is to draw parallels to an already known concept, which is why I'll compare Smart-Beta funds to [Index Funds](https://zerodha.com/varsity/chapter/introduction-to-index-funds/). An index fund is simply a fund which tracks a certain index like the Nifty 50 or the Sensex, what this means is that if your entire portfolio was composed of a single index fund, it would move almost exactly like the index that it is tracking.

### Index Funds vs Mutual Funds
Here is something that may surprise a lot of you: Index funds outperform the majority of actively managed funds over the long term. This is almost a universal consensus among most finance-savvy investors. One of the primary causes for this being lower expense ratios that compound over time. 

Don't just take my word for it though, investing legend Warren Buffet once remarked —
> "If a statue is ever erected to honor the person who has done the most for American investors, the handsdown choice should be Jack Bogle."

Jack Bogle being, of course, the inventor of the world’s first index fund (the Vanguard 500 Index Fund). <br>If you're someone who is more data inclined, feel free to take a look at the [SPIVA India](https://web.archive.org/web/20251009172652/https://www.spglobal.com/spdji/en/documents/spiva/spiva-india-scorecard-mid-year-2025.pdf) reports published by S&P global, which compares the performance of active and passive (index) funds. The 3 and 5-year horizons are of particular interest as 80-90% of active funds see underperformance compared to passive ones.

Index funds represent the purest form of passive investing, but they don't represent the totality of the spectrum. What if there was a way to tweak certain characteristics of the index? That is where  the magic of Smart-Beta funds come into play.

## What is a "Smart"-beta fund?

Contrary to its name, "Smart" beta funds aren't actually all that smart, it's more of a marketing term if anything. This style of investing is more commonly referred to as [**factor investing**](https://en.wikipedia.org/wiki/Factor_investing).

Factor based funds sit between active and passive funds. They still have low expense ratios and a pre-defined formula, but unlike index funds, they do not weight stocks by market cap alone. The funds are weighted by certain characteristics called "factors", few examples of factors are:

- Momentum (recent winners)
- Value (cheap stocks)
- Quality (strong balance sheets)
- Low Volatility (consistent share price)

To take momentum as an example, a momentum-factor based fund (like the [Nifty Midcap150 Momentum 50](https://web.archive.org/web/20251207192845/https://niftyindices.com/indices/equity/strategy-indices/nifty-midcap150-momentum-50)) would pick stocks which show the largest recent returns in a specific time period. These funds are usually rebalanced every 6 months or so.

Smart-Beta funds are passive at their core, but they put a spin on the traditional index fund by using specific formulas to decide which stocks become part of the fund.

### Do Smart-Beta funds Work?
This is probably the question that all of you are most interested in. While I was doing my research into these funds, I came across this [research article](https://web.archive.org/web/20230806213346/https://www.spglobal.com/spdji/en/documents/education/education-blending-factors-in-your-smart-beta-portfolio.pdf) from S&P global which conducted a data analysis on single-factor funds in the US market.

What they found was that single-factor indices often outperform traditional index funds over the long term. However, they are subject to distinct cycles of underperformance. The article recommends blending different seemingly inversely related factors together to create a more stable source of returns.

However, keep in mind that all of this analysis was done on U.S. markets. I could find no such detailed research conducted on the Indian markets (please do comment down below if you find any)

So, naturally, I did what any bored computer science student with too much time on their hands would do — I wrote some code to analyze the historical performance myself :)

## Digging Into The Data
Now, fair warning! I'm not a data scientist or a research analyst. So there may be some errors that have crept in due to my inexperience. If you wish to proofread it, the colab notebook used for generating the below insights is open-source and available [here](https://github.com/hrideshmg/smart-beta-analysis).

One important thing to note is that **past performance are not indicative of future returns**. That said, I still do think that historical data is a useful tool, this article wouldn't exist otherwise ;)

### Source
The stock market data for the different indices was obtained from NSE's website. I've used the past 10 years stock data from `01-01-2015` to `25-04-2025`. The choice of 10 years was, to be honest, completely arbitrary, it just felt like a good enough sample space when I ran the program.

I had randomly chosen the following few indices for comparison:
- Nifty 50 (Benchmark)
- Gold (Benchmark)
- Nifty Midcap 150 (Benchmark)
- Nifty Alpha 50
- Nifty 50 Low Volatility
- Nifty Midcap 150 Quality
- Nifty Midcap 150 Momentum
- Nifty 200 Momentum
- Nifty 200 Quality
- Nifty 200 Value
- Nifty 200 Alpha
- Nifty Smallcap 250 Quality

If you're annoyed by the lack of diversity in same-sized factor indices (like the absence of a Nifty 150 Value), you're not alone. NSE doesn’t recognize these as official indices, so they simply don’t exist. As for why they don't exist, your guess is as good as mine.

### Methodology
I've chosen to compare the indices based on the following few metrics:
- **CAGR:** Average yearly growth rate of the index.
- **Rolling Returns:** Overlapping returns calculated over 1-year, 3-year and 5-year periods.
- **Sharpe Ratio:** Volatility adjusted returns.
- **Max Drawdown:** Largest percentage drop from a peak to a trough in 10 years.
- **Annual Volatility:** How much the index tends to fluctuate in a year.

In addition to the above, I've also measured how "in-sync" these indices move with respect to each other through a correlation matrix. I got inspiration to do this from the S&P report, which found that in U.S. markets certain indices are inversely correlated, meaning that combining them could yield better stability.

Of all these metrics, the two most important ones are the Sharpe ratio and [rolling returns](https://zerodha.com/varsity/chapter/rolling-returns/).

#### Sharpe? What's that?
If you're a seasoned investor, you've probably seen most of the metrics that I have listed above — except maybe the **Sharpe ratio**. Though its more commonly used by quant traders, I believe that its an effective way to compare returns while accounting for risk.

The Sharpe ratio, simply put tells you how much extra return an investment delivers for the risk you take to get it. A higher ratio is better (more return for each unit of risk).

If you're interested in more details, I'd recommend checking out this excellent video:
{{<youtube 9HD6xo2iO1g >}}

## Results

### Key Metrics

The table below compares the key metrics I mentioned earlier across the benchmark and smart-beta indices over the past 10 years. It is sorted according the Sharpe ratio.

<div class="table-scroll">
<table>
  <thead>
    <tr>
      <th>Index</th>
      <th>CAGR (%)</th>
      <th>Max Drawdown (%)</th>
      <th>Annual Volatility (%)</th>
      <th>Sharpe Ratio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Nifty Midcap 150 Momentum</td><td>0.20</td><td>-39.61</td><td>0.19</td><td>1.02</td>
    </tr>
    <tr>
      <td>Nifty Midcap 150</td><td>0.11</td><td>-38.67</td><td>0.19</td><td>0.97</td>
    </tr>
    <tr>
      <td>Nifty 50 Low Volatility</td><td>0.12</td><td>-29.33</td><td>0.14</td><td>0.91</td>
    </tr>
    <tr>
      <td>Nifty 200 Alpha</td><td>0.18</td><td>-37.50</td><td>0.21</td><td>0.84</td>
    </tr>
    <tr>
      <td>Nifty Smallcap 250 Quality</td><td>0.15</td><td>-51.91</td><td>0.19</td><td>0.81</td>
    </tr>
    <tr>
      <td>Nifty 200 Momentum</td><td>0.16</td><td>-34.21</td><td>0.20</td><td>0.80</td>
    </tr>
    <tr>
      <td>Nifty Midcap 150 Quality</td><td>0.12</td><td>-36.26</td><td>0.16</td><td>0.79</td>
    </tr>
    <tr>
      <td>Nifty Alpha 50</td><td>0.17</td><td>-40.67</td><td>0.22</td><td>0.78</td>
    </tr>
    <tr>
      <td>Nifty 200 Quality</td><td>0.10</td><td>-29.51</td><td>0.15</td><td>0.70</td>
    </tr>
    <tr>
      <td>Gold</td><td>0.09</td><td>-22.00</td><td>0.14</td><td>0.67</td>
    </tr>
    <tr>
      <td>Nifty 50</td><td>0.10</td><td>-38.44</td><td>0.17</td><td>0.63</td>
    </tr>
    <tr>
      <td>Nifty 200 Value</td><td>0.10</td><td>-60.74</td><td>0.23</td><td>0.46</td>
    </tr>
  </tbody>
</table>
</div>

### Rolling Returns
Relying too heavily on 10-year metrics can be misleading. In practice, investors rarely think in such long, fixed timeframes. What matters far more are the returns one can reasonably expect over shorter horizons, such as 1-year, 3-year, or 5-year periods.

This is precisely what [rolling returns](https://zerodha.com/varsity/chapter/rolling-returns/) helps us to calculate:

#### 1-Year

<div class="table-scroll">
<table>
  <thead>
    <tr>
      <th>Index</th>
      <th>Median (%)</th>
      <th>Max (%)</th>
      <th>Min (%)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Nifty Midcap 150</td><td>24.80</td><td>111.92</td><td>-34.44</td>
    </tr>
    <tr>
      <td>Nifty Midcap 150 Momentum</td><td>20.33</td><td>117.42</td><td>-25.76</td>
    </tr>
    <tr>
      <td>Nifty Alpha 50</td><td>17.82</td><td>155.47</td><td>-29.71</td>
    </tr>
    <tr>
      <td>Nifty Smallcap 250 Quality</td><td>17.38</td><td>126.86</td><td>-40.91</td>
    </tr>
    <tr>
      <td>Nifty 200 Alpha</td><td>16.89</td><td>94.24</td><td>-28.06</td>
    </tr>
    <tr>
      <td>Nifty 200 Momentum</td><td>16.26</td><td>83.07</td><td>-26.50</td>
    </tr>
    <tr>
      <td>Nifty 200 Value</td><td>15.33</td><td>122.79</td><td>-49.22</td>
    </tr>
    <tr>
      <td>Nifty Midcap 150 Quality</td><td>14.49</td><td>95.27</td><td>-29.52</td>
    </tr>
    <tr>
      <td>Nifty 50 Low Volatility</td><td>11.86</td><td>73.76</td><td>-22.34</td>
    </tr>
    <tr>
      <td>Nifty 50</td><td>11.79</td><td>94.67</td><td>-33.40</td>
    </tr>
    <tr>
      <td>Nifty 200 Quality</td><td>10.19</td><td>72.80</td><td>-26.04</td>
    </tr>
    <tr>
      <td>Gold</td><td>6.91</td><td>46.57</td><td>-16.30</td>
    </tr>
  </tbody>
</table>
</div>

<img src="year1.png">

#### 3-Year

<div class="table-scroll">
<table>
  <thead>
    <tr>
      <th>Index</th>
      <th>Median (%)</th>
      <th>Max (%)</th>
      <th>Min (%)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Nifty Midcap 150</td><td>26.15</td><td>39.89</td><td>13.87</td>
    </tr>
    <tr>
      <td>Nifty Midcap 150 Momentum</td><td>22.23</td><td>44.36</td><td>-3.11</td>
    </tr>
    <tr>
      <td>Nifty Alpha 50</td><td>20.81</td><td>46.82</td><td>-4.85</td>
    </tr>
    <tr>
      <td>Nifty 200 Alpha</td><td>19.58</td><td>34.96</td><td>-2.37</td>
    </tr>
    <tr>
      <td>Nifty 200 Momentum</td><td>19.02</td><td>31.10</td><td>0.11</td>
    </tr>
    <tr>
      <td>Nifty Smallcap 250 Quality</td><td>18.47</td><td>46.97</td><td>-14.18</td>
    </tr>
    <tr>
      <td>Nifty Midcap 150 Quality</td><td>15.17</td><td>28.59</td><td>-3.40</td>
    </tr>
    <tr>
      <td>Nifty 50 Low Volatility</td><td>13.51</td><td>25.71</td><td>-2.51</td>
    </tr>
    <tr>
      <td>Nifty 50</td><td>13.16</td><td>31.64</td><td>-5.25</td>
    </tr>
    <tr>
      <td>Nifty 200 Quality</td><td>12.47</td><td>25.03</td><td>-1.40</td>
    </tr>
    <tr>
      <td>Nifty 200 Value</td><td>9.90</td><td>47.50</td><td>-22.32</td>
    </tr>
    <tr>
      <td>Gold</td><td>7.22</td><td>20.28</td><td>-2.53</td>
    </tr>
  </tbody>
</table>
</div>

<img src="year3.png">

#### 5-Year
<div class="table-scroll">
<table>
  <thead>
    <tr>
      <th>Index</th>
      <th>Median (%)</th>
      <th>Max (%)</th>
      <th>Min (%)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Nifty Midcap 150</td><td>26.85</td><td>36.90</td><td>21.45</td>
    </tr>
    <tr>
      <td>Nifty Alpha 50</td><td>22.74</td><td>39.54</td><td>0.54</td>
    </tr>
    <tr>
      <td>Nifty Midcap 150 Momentum</td><td>21.48</td><td>39.99</td><td>3.87</td>
    </tr>
    <tr>
      <td>Nifty 200 Alpha</td><td>18.83</td><td>33.76</td><td>4.83</td>
    </tr>
    <tr>
      <td>Nifty 200 Momentum</td><td>18.21</td><td>29.99</td><td>5.38</td>
    </tr>
    <tr>
      <td>Nifty Smallcap 250 Quality</td><td>15.74</td><td>37.87</td><td>-3.01</td>
    </tr>
    <tr>
      <td>Nifty Midcap 150 Quality</td><td>14.75</td><td>24.11</td><td>1.78</td>
    </tr>
    <tr>
      <td>Nifty 50</td><td>13.25</td><td>24.57</td><td>-2.79</td>
    </tr>
    <tr>
      <td>Nifty 50 Low Volatility</td><td>13.12</td><td>24.09</td><td>1.43</td>
    </tr>
    <tr>
      <td>Nifty 200 Quality</td><td>13.11</td><td>21.08</td><td>-0.48</td>
    </tr>
    <tr>
      <td>Gold</td><td>8.49</td><td>14.92</td><td>3.19</td>
    </tr>
    <tr>
      <td>Nifty 200 Value</td><td>6.98</td><td>43.25</td><td>-12.51</td>
    </tr>
  </tbody>
</table>
</div>

<img src="year5.png">

### Correlation Matrix

<div class="table-scroll">
  <style>
  .correlation-matrix {
    font-size: 0.85em;
    background-color: #1a1a1a;
    color: #e0e0e0;
  }
  .correlation-matrix th {
    background-color: #2a2a2a;
    font-weight: 600;
    text-align: center;
    padding: 8px 4px;
    font-size: 0.75em;
    border: 1px solid #3a3a3a;
  }
  .correlation-matrix td {
    text-align: center;
    padding: 6px 4px;
    border: 1px solid #3a3a3a;
  }
  .correlation-matrix tbody tr td:first-child {
    font-weight: 600;
    text-align: left;
    background-color: #2a2a2a;
  }
  .corr-high { background-color: #1a3a2a; color: #90ee90; }
  .corr-med { background-color: #3a3a1a; color: #ffd700; }
  .corr-low { background-color: #3a1a1a; color: #ff6b6b; }
  .corr-diag { background-color: #2a2a2a; font-weight: 600; color: #b0b0b0; }
</style>
<table class="correlation-matrix">
  <thead>
    <tr>
      <th></th>
      <th>Nifty 50</th>
      <th>Alpha 50</th>
      <th>Low Vol</th>
      <th>MC150 Qual</th>
      <th>MC150 Mom</th>
      <th>200 Mom</th>
      <th>200 Qual</th>
      <th>200 Val</th>
      <th>200 Alpha</th>
      <th>SC250 Qual</th>
      <th>MC 150</th>
      <th>Gold</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Nifty 50</td><td class="corr-diag">1.00</td><td class="corr-med">0.78</td><td class="corr-high">0.90</td><td class="corr-med">0.82</td><td class="corr-med">0.80</td><td class="corr-high">0.86</td><td class="corr-high">0.86</td><td class="corr-med">0.79</td><td class="corr-high">0.83</td><td class="corr-med">0.75</td><td class="corr-high">0.84</td><td class="corr-low">0.04</td>
    </tr>
    <tr>
      <td>Nifty Alpha 50</td><td class="corr-med">0.78</td><td class="corr-diag">1.00</td><td class="corr-med">0.81</td><td class="corr-high">0.88</td><td class="corr-high">0.94</td><td class="corr-high">0.90</td><td class="corr-med">0.78</td><td class="corr-med">0.81</td><td class="corr-high">0.93</td><td class="corr-high">0.88</td><td class="corr-high">0.93</td><td class="corr-low">0.06</td>
    </tr>
    <tr>
      <td>Nifty 50 Low Vol</td><td class="corr-high">0.90</td><td class="corr-med">0.81</td><td class="corr-diag">1.00</td><td class="corr-high">0.88</td><td class="corr-high">0.84</td><td class="corr-high">0.87</td><td class="corr-high">0.94</td><td class="corr-med">0.78</td><td class="corr-high">0.85</td><td class="corr-med">0.79</td><td class="corr-high">0.88</td><td class="corr-low">0.05</td>
    </tr>
    <tr>
      <td>Nifty MC150 Quality</td><td class="corr-med">0.82</td><td class="corr-high">0.88</td><td class="corr-high">0.88</td><td class="corr-diag">1.00</td><td class="corr-high">0.92</td><td class="corr-high">0.86</td><td class="corr-high">0.86</td><td class="corr-med">0.79</td><td class="corr-high">0.88</td><td class="corr-high">0.89</td><td class="corr-high">0.95</td><td class="corr-low">0.05</td>
    </tr>
    <tr>
      <td>Nifty MC150 Momentum</td><td class="corr-med">0.80</td><td class="corr-high">0.94</td><td class="corr-high">0.84</td><td class="corr-high">0.92</td><td class="corr-diag">1.00</td><td class="corr-high">0.92</td><td class="corr-med">0.81</td><td class="corr-med">0.81</td><td class="corr-high">0.93</td><td class="corr-high">0.87</td><td class="corr-high">0.96</td><td class="corr-low">0.07</td>
    </tr>
    <tr>
      <td>Nifty 200 Momentum</td><td class="corr-high">0.86</td><td class="corr-high">0.90</td><td class="corr-high">0.87</td><td class="corr-high">0.86</td><td class="corr-high">0.92</td><td class="corr-diag">1.00</td><td class="corr-high">0.84</td><td class="corr-med">0.82</td><td class="corr-high">0.97</td><td class="corr-med">0.81</td><td class="corr-high">0.89</td><td class="corr-low">0.06</td>
    </tr>
    <tr>
      <td>Nifty 200 Quality</td><td class="corr-high">0.86</td><td class="corr-med">0.78</td><td class="corr-high">0.94</td><td class="corr-high">0.86</td><td class="corr-med">0.81</td><td class="corr-high">0.84</td><td class="corr-diag">1.00</td><td class="corr-med">0.72</td><td class="corr-high">0.84</td><td class="corr-med">0.75</td><td class="corr-high">0.83</td><td class="corr-low">0.06</td>
    </tr>
    <tr>
      <td>Nifty 200 Value</td><td class="corr-med">0.79</td><td class="corr-med">0.81</td><td class="corr-med">0.78</td><td class="corr-med">0.79</td><td class="corr-med">0.81</td><td class="corr-med">0.82</td><td class="corr-med">0.72</td><td class="corr-diag">1.00</td><td class="corr-high">0.83</td><td class="corr-med">0.78</td><td class="corr-high">0.86</td><td class="corr-low">0.08</td>
    </tr>
    <tr>
      <td>Nifty 200 Alpha</td><td class="corr-high">0.83</td><td class="corr-high">0.93</td><td class="corr-high">0.85</td><td class="corr-high">0.88</td><td class="corr-high">0.93</td><td class="corr-high">0.97</td><td class="corr-high">0.84</td><td class="corr-high">0.83</td><td class="corr-diag">1.00</td><td class="corr-high">0.83</td><td class="corr-high">0.91</td><td class="corr-low">0.06</td>
    </tr>
    <tr>
      <td>Nifty SC250 Quality</td><td class="corr-med">0.75</td><td class="corr-high">0.88</td><td class="corr-med">0.79</td><td class="corr-high">0.89</td><td class="corr-high">0.87</td><td class="corr-med">0.81</td><td class="corr-med">0.75</td><td class="corr-med">0.78</td><td class="corr-high">0.83</td><td class="corr-diag">1.00</td><td class="corr-high">0.91</td><td class="corr-low">0.04</td>
    </tr>
    <tr>
      <td>Nifty Midcap 150</td><td class="corr-high">0.84</td><td class="corr-high">0.93</td><td class="corr-high">0.88</td><td class="corr-high">0.95</td><td class="corr-high">0.96</td><td class="corr-high">0.89</td><td class="corr-high">0.83</td><td class="corr-high">0.86</td><td class="corr-high">0.91</td><td class="corr-high">0.91</td><td class="corr-diag">1.00</td><td class="corr-low">0.07</td>
    </tr>
    <tr>
      <td>Gold</td><td class="corr-low">0.04</td><td class="corr-low">0.06</td><td class="corr-low">0.05</td><td class="corr-low">0.05</td><td class="corr-low">0.07</td><td class="corr-low">0.06</td><td class="corr-low">0.06</td><td class="corr-low">0.08</td><td class="corr-low">0.06</td><td class="corr-low">0.04</td><td class="corr-low">0.07</td><td class="corr-diag">1.00</td>
    </tr>
  </tbody>
</table>
</div>

## Conclusion
While I started this analysis to understand the performance of smart-beta funds, I was more surprised by how much better the Nifty Midcap 150 performs compared to the Nifty 50 for only a modest increase in volatility.

Another interesting observation was how highly correlated all of these indices are, a lot of them move almost in-sync. This is in direct contrast to the U.S. market in which some factors even exhibit negative correlations.<br>
The only instrument that had a low correlation was gold, interestingly though, even it did not have a negative correlation. Which may cast some doubt on the claim that gold can be reliably used as a hedge for portfolios.

I've purposefully avoided drawing any conclusions about individual indices, because ultimately, I think that's a call you should make yourself :)<br>

Hope you found this  useful, or at the very least sparked some new ideas!
