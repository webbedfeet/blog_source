---
title: Some thoughts on the downsides of current Data Science practice
author: Abhijit Dasgupta
date: '2017-04-19'
categories:
  - Computation
  - Data Science
slug: some-thoughts-on-the-downsides-of-current-data-science-practice
---

Bert Huang has a [nice blog](https://berthuang.wordpress.com/2016/08/03/machine-learnings-poor-fit-for-real-data/) talking about poor results of ML/AI algorithms in "wild" data, which echos some of my experience and thoughts. His conclusions are worth thinking about, IMO.

<blockquote>
1. Big data is complex data. As we go out and collect more data from a finite world, we're necessarily going to start collecting more and more interdependent data. Back when we had hundreds of people in our databases, it was plausible that none of our data examples were socially connected. But when our databases are significant fractions of the world population, we are much farther away from the controlled samples of good laboratory science. This means...

2. Data science as it's currently practiced is essentially bad science. When we take a biased, dependent population of samples and try to generalize a conclusion from it, we need to be fully aware of how flawed our study is. That doesn't mean things we discover using data analytics aren't useful, but they need to be understood through the lens of the bias and complex dependencies present in the training data.

3. Computational methods should be aware of, and take advantage of, known dependencies. Some subfields of data mining and machine learning address this, like structured output learning, graph mining, relational learning, and more. But there is a lot of research progress needed. The data we're mostly interested in nowadays comes from complex phenomena, which means we have to pay for accurate modeling with a little computational and cognitive complexity. How we manage that is a big open problem.
</blockquote>

Specially point 3 is one I've been thinking about a lot recently. Our current frameworks are quite limited in dealing with dependencies and complexity. We've been happy using decades-old methods since they work pretty well on the predictive side as a reasonable approximation to the truth. However, having machines understanding complexity and incorporating it in predictions or understanding is a second-level challenge that can use significant research effort.
