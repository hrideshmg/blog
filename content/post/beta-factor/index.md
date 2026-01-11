---
title: 'An Analysis Of Smart-Beta Funds In Indian markets'
description: Why aren't more people talking about these?
slug: smart-beta
date: 2026-01-08
categories: finance
draft: true
#image: cover.png
---
> **Disclaimer:** Do not take anything written in this blog as investment advice, please do your own research before making any financial decisions.

A few months ago, when I started getting into finance, [Zerodha Varsity](https://zerodha.com/varsity/modules/) was my go-to resource. I owe much of my finance/stock-market knowledge to the excellent modules over there.

While I reading through the mutual fund module, one article in particular stood out to me - ["Smart-Beta Funds"](https://zerodha.com/varsity/chapter/smart-beta-funds/). What surprised me wasn't the concept itself, but how little discussion it had compared to other varsity articles. While most modules had hundreds if not thousands of comments, this one had a meagre 25-30.

This lack of attention felt odd, and as someone who likes to dig into things that fly under the radar I looked deeper into them. This blog is an attempt to summarize some of my research into this topic as well as present some of the analysis I've done on these funds.<br> I'll try my best to keep it as beginner friendly as possible so don't worry if you don't know anything about these funds.

## Index Funds

I believe that the best way to understand something is to draw parallels to a concept you already know, which is why I'll compare them to [Index Funds](https://zerodha.com/varsity/chapter/introduction-to-index-funds/). An index fund in short, is simply a fund which tracks a certain index like the Nifty 50 or the sensex, what this means is that hypothetically if your portfolio was entirely consisted of a single index fund, it would move exactly like the index that the fund is tracking.

### Index Funds vs Mutual Funds
Here is something that may surprise a lot of you: Index funds outperform the majority of actively managed funds over the long term. This is almost a universal consensus among most finance-savvy investors but a majority of Indian retailers still prefer to invest in mutual funds regardless.

The cause for this is mainly lower expense ratios that compound over time. Don't just take my word for it though, investing legend Warren Buffet once remarked —
> If a statue is ever erected to honor the person who has done the most for American investors, the handsdown choice should be Jack Bogle.

Jack Bogle being, of course, the inventor of the world’s first index fund (the Vanguard 500 Index Fund). If you're someone who is more data inclined, feel free to take a look at the [SPIVA India](https://www.spglobal.com/spdji/en/documents/spiva/spiva-india-scorecard-mid-year-2025.pdf) reports published by S&P global, which compares the performance of active and passive (index) funds. The 3 and 5 year horizons are of particular interest as 80-90% of active funds see underperformance compared to passive ones.

Index funds represent the purest form of passive investing, but they aren't the end of the spectrum. What if there was a way to tweak certain characteristics of the index? That is where Smart-Beta funds come into play.

## What is a "Smart"-beta fund?

Contrary to its name, "Smart" beta funds aren't actually all that smart, it's more of a marketing term if anything. This style of investing is more commonly referred to as [**factor investing**](https://en.wikipedia.org/wiki/Factor_investing).

Factor based funds sit between active and passive funds. They still have low expense ratios and a pre-defined formula, but unlike index funds, they do not weight stocks by market cap alone. The funds are weighted by certain characteristics called "factors", few examples of factors are:

- Momentum (recent winners)
- Value (cheap stocks)
- Quality (strong balance sheets)
- Low Volatility (consistent share price)

To take momentum as an example, a momentum-factor based fund (like the [Nifty Midcap150 Momentum 50](https://www.niftyindices.com/indices/equity/strategy-indices/nifty-midcap150-momentum-50)) would pick stocks which show the largest recent returns in a specific time period. These funds are usually rebalanced every 6 months or so.

Smart-Beta funds are passive at their core, but they put a spin on the traditional index fund by introducing new formulas to choose which stocks become part of the fund.

### Do Smart-Beta funds Work?
This is probably the question that all of you have been waiting for. While I was doing my research into these funds, I came across this [research article](https://web.archive.org/web/20230806213346/https://www.spglobal.com/spdji/en/documents/education/education-blending-factors-in-your-smart-beta-portfolio.pdf) from S&P global which conducted a data analysis on single-factor funds in the American market.

What they found was that single-factor indices often outperform traditional index funds over the long term. However, they are subject to distinct cycles of underperformance. The article recommends blending different seemingly inversely related factors together to create a more stable source of returns.

However, keep in mind that all of this analysis was done on American markets. I could find no such detailed research conducted on the Indian markets (please do comment down below if you find any!)<br>
So, naturally, I did what any bored computer science student with too much time on his hands would do — I wrote some code to analyze the historical performance myself.

## Digging Into The Data
Now, fair warning! I am not a data scientist or research analyst. So there may be inadvertent errors that may have crept in due to my inexperience. If you wish to proofread it, the colab notebook used for generating the below insights is open-source and available [here](https://github.com/hrideshmg/smart-beta-analysis).
