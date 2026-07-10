## Hi, I'm Marwin 👋

[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?&style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/marwin-alexander-steiner/)
[![Substack](https://img.shields.io/badge/Substack-FF6719?style=for-the-badge&logo=substack&logoColor=white)](https://substack.com/@marwin628124)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:marwin.steiner@gmail.com)
[![HKJC API](https://img.shields.io/badge/🐎%20HKJC%20Racing%20API-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://hkjc-web.vercel.app/)

I'm a finance graduate from [Bayes Business School](https://www.bayes.city.ac.uk/) (BSc Investment & Financial Risk Management, Top Decile) with experience in data engineering, systematic trading, and derivatives research. If it's convex and complex, I'm interested!

Right now, I'm building data and execution infrastructure for systematic trading in C++ and continuing my research in the relative-value volatility space.

Currently forward-testing three quirky volatility statistical arbitrage edges in index vol. All of them (combined) lead to market-neutral returns.

### Previous roles:
- Data Engineer at Swiss Re
- Summer Intern at Swiss Life Asset Managers
- Spring Insight at Barclays

### What I'm thinking about

A few threads I'm currently pulling on:

- **Building volatility surfaces.** In my bachelor's thesis, I focused on volatility statistical arbitrage. To define fair value in the $\mathbb{Q}$ domain, you need an IV parametrisation. The tooling I built for this is now [`pysvi`](https://github.com/marwinsteiner/pysvi) [![PyPI Downloads](https://static.pepy.tech/personalized-badge/svi-py?period=total&units=INTERNATIONAL_SYSTEM&left_color=BLACK&right_color=GREEN&left_text=downloads)](https://pepy.tech/projects/svi-py), an open-source Python library on PyPI. 
- **Event-driven & prediction markets.** I've been building automated trading infrastructure for [Polymarket](https://github.com/marwinsteiner/polymarket-bot). In my view, prediction markets are one of the most interesting laboratories for testing probabilistic reasoning under uncertainty.
- **Longhand.** I did not like existing $\LaTeX$ editors, so I created my own research workspace: [Longhand.dev](https://www.longhand.dev)
- **HKJC API.** As a transient interest, I wrote a scraper for and strung an API around the Hong Kong Jockey Club website, allowing retrieval of historic race and form data because I did not want to pay for existing services. The goal was to use machine learning to identify consistent race winners. This research is not publicly available.

### Paper Abstracts
Title: Mean Reversion in the Intraday Implied Volatility Surface of S&P 500 Options

We study intraday S&P 500 index option implied volatilities using an extended stochastic
volatility-inspired parametrisation, recalibrated jointly across all expiries at 60-second inter
vals over 63 trading days. Two experiments are conducted. The first decomposes the recon
structed implied volatility surface via functional principal component analysis, fits a vector
autoregression to the resulting factor scores, and tests whether the surface-level forecast can
outperform a random walk. The second measures the basis between each quoted implied
volatility and the fitted surface, tests for serial dependence, and assesses the economic scale
of the resulting deviations against the bid–ask spread.

### Toolbox

![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=C%2B%2B&logoColor=white)
![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/-React-61DAFB?style=flat-square&logo=react&logoColor=black)
![PySpark](https://img.shields.io/badge/-PySpark-E25A1C?style=flat-square&logo=apachespark&logoColor=white)
![SQL](https://img.shields.io/badge/-SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)
![Node.js](https://img.shields.io/badge/-Node.js-339933?style=flat-square&logo=node.js&logoColor=white)
![Palantir Foundry](https://img.shields.io/badge/-Palantir_Foundry-101113?style=flat-square&logo=palantir&logoColor=white)
