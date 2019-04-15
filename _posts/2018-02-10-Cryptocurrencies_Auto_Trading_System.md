---
layout: post
title: "Cryptocurrencies Auto Trading System"
date: 2018-02-10
excerpt: "Tracking Price Trend and Auto Trading"
project: true
visible: false
tag:
- Cryptocurrency
- Auto Trading
commnets: false
---

![autotrade](https://user-images.githubusercontent.com/8471958/50430613-498ee280-0908-11e9-8638-967cf265a487.png)

<center>Visualization Cryptocurrencies price trend and transaction logs</center>

## Project Description

For a long time, people try to analyze and predict the stock price. Based on this analysis and prediction, they make the transaction of stock automatically. Unfortunately, this process doesn't prove stable ability, also needs lots of time and a lot of money to improve. 

Early 2018, in Korea, cryptocurrencies, including Bitcoin, was booming. Because of that, many people jumped in the cryptocurrency trading market, and many exchange companies are launched. The price of cryptocurrencies has skyrocketed and the Korean government didn't prepare that situation yet. 

In such conditions, I guess that cryptocurrency trading market looks like the general stock market, honestly more drastic, therefore it seems appropriate to test the auto trading system. I developed a simple auto trading system using the API for Korbit, one of the Korean cryptocurrency exchange company and Python. Moreover, I made a single web page to check price trend and transaction logs.

For this project, using the following skills:

![python](https://img.shields.io/badge/python-green.svg?logo=python&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![bitcoin](https://img.shields.io/badge/bitcoin-green.svg?logo=bitcoin&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![D3](https://img.shields.io/badge/d3.js-green.svg?logo=d3.js&style=for-the-badge&colorB=AAAAAA){:class="img-badge"}

---

## Trading Algorithms

Trading is comprised of 2 simple actions, buy and sell. The most important point is the answer when we buy or sell. At first, I made the algorithm with simple and basic logic. When the price is higher than I bought, an auto trading system(ATS) try to sell. When the price is lower than I sell, ATS try to buy. However, everyone knows this trading algorithm doesn't work well. So I improved this ATS's trading algorithm step by step.

The followings are conditions that I consider to make buy and sell decision.

* ATS status(ordered, wait)

  - ordered: There is an order that is not completed.
  - wait: There is no order. ATS is waiting for the decision next action.

* ATS policy(buy, sell)
  - ATS has their own trading policy. ATS status has only two types(buy, sell).
  - This policy is changed when the new transaction occurs.
* Latest trading price

  - Latest trading price is an important criterion to decide the action.
* Waiting count
  - Trading algorithm executes every 5 minutes.
  - Waiting for count increase when there is no action.
* Profit Range
  - The margin between the current price and the latest trading price
  - Actual using value to decide the action is the value that this profit range is divided by latest trading price(Profit range ratio).
* Reference price
  - The default reference price is the current price and the price is always changing.
  - The reference price is 1.5% higher or lower than the previous stage's reference price.
    - If the reference price is 3% higher than the current price, reference price increases 1.5%
    - If the reference price is 3% lower than the current price, reference price decreases 1.5%
* Reference policy(wait, buy, sell)
  - The default policy is 'wait'. However, reference policy is changed when the reference price is not changed and waiting for count lower than 36.
  - Reference policy is changed by waiting count, the gap between the latest trading price and reference price(Base point) and the gap between the current price and reference price(Current point)

    | Waiting Count | Base point - Current Point | Reference Policy |
    | :-----------: | :------------------------: | :--------------: |
    |   under 48    |       from 0 to 0.5        |       sell       |
    |   under 48    |       from -0.5 to 0       |       buy        |
    |   under 60    |       from 0.5 to 1        |       sell       |
    |   under 60    |      from -1 to -0.5       |       buy        |
    |       -       |           over 1           |       sell       |
    |       -       |          under -1          |       buy        |

Using these conditions, the trading algorithm makes a decision to buy, sell or wait.

1. Calculate the reference price and reference policy

2. When ATS status is 'wait' and ATS policy is same to reference policy, ATS make a transaction by ATS policy. If the action occurs,
   - Update latest trading price and policy. 
   - Change ATS status to 'ordered'.
   - Initialize waiting count
3. When ATS status is 'wait' and ATS policy isn't same to reference policy, check the profit range.

    | ATS policy | Profit range ratio | waiting count | Decision |
    | :--------: | :----------------: | :-----------: | :------: |
    |    buy     |     under -1.5     |   under 12    |   buy    |
    |    buy     |     under -1.0     |   under 24    |   buy    |
    |    buy     |     under -0.5     |   under 36    |   buy    |
    |    buy     |         -          |    over 36    |   buy    |
    |    sell    |      over 1.5      |   under 12    |   sell   |
    |    sell    |       over 1       |   under 24    |   sell   |
    |    sell    |      over 0.5      |   under 36    |   sell   |
    |    sell    |      over 0.3      |    over 36    |   sell   |

## Achievement

The trading algorithm that is applied in this ATS is so simple to predict proper price or timing of the action. This algorithm is only considered bottleneck of trading complement and ratio of price fluctuation. This means that this algorithm works well when the market situation is steadily on the upswing. However, if the market situations drop suddenly, this algorithm will make the bad decision continuously.

In fact, I invested the Ethereum, one of the cryptocurrencies, for two months using this ATS. In the first 3 weeks, I have earned 30% of my first investment. However, the Korean government announced the new policy and regulation related to cryptocurrencies, and the price dropped to the ground. This algorithm cannot respond to the situation like this, I almost lost the balance that I entered to use this test.
