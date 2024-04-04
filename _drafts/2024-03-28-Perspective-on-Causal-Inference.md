---
title: "Perspective on Causal Inference"
layout: post
date: 2024-03-28
categories: idea
---

{% newthought 'For when' %} we'd like to know cause and effect. <!--more--> Causal inference is an umbrella term describing approaches to uncover the root causes of the outcomes we observe --- in contrast to [hypothesis testing](https://datascienceperspectives.xyz/2024/03/04/Perspective-on-Hypothesis-Tests.html), which only tries to distinguish real outcomes from random ones.

### Why should a business care about causal inference?

Businesses make decisions. We hope that these decisions have consequence; otherwise, what's the point of doing anything at all? Henry Ford, on the now-famous "[Five-Dollar Day](https://www.thehenryford.org/explore/blog/fords-five-dollar-day/)", decided to _double_ employee compensation in order to combat high employee turnover. {% sidenote 'One' "Specifically, the raise came in the form of a bonus that made up the difference between $5 and the employees wage. [According to Tim Worstall of the Adam Smith Institute, there were also condititions that made the bonus less accessible for immigrants, women, and men who had working-class wives](https://www.adamsmith.org/blog/tax-spending/on-henry-ford-and-his-5-a-day#:~:text=We%20might%20also%20note%20that%20he%20wasn%27t%20in%20fact%20paying%20higher%20wages%3A%20he%20was%20offering%20a%20bonus%2C%20a%20highly%20conditional%20bonus%3A)" %} The decision paid off, at least by the standards of history books and stock markets.

Hindsight offers many laudable examples of good decision making in business (see also: [survivorship bias](https://en.wikipedia.org/wiki/Survivorship_bias#:~:text=did%20not%20survive.-,During,-World%20War%20II)). Much less is offered to the business that needs to make a decision _now_, in a world of infinite potential outcomes.

{% maincolumn 'assets/img/causalinf_gtrends.png' '[From Google Trends](https://trends.google.com/trends/explore?date=all&geo=US&q=data%20driven&hl=en)' %}
