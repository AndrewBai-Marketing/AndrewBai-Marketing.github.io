---
layout: page
title: Machine Learning Learns Bayes
description: Joint with Thomas Wiemann and Sanjog Misra
coauthors: <a href="https://thomaswiemann.com/">Thomas Wiemann</a> and <a href="https://sanjogmisra.com/">Sanjog Misra</a>
importance: 1
category: research
---

## Abstract

In a panel data setting of CPG purchases, Hierarchical Bayes choice models compress each household's full sequence of trip level decisions into a low-dimensional vector of posterior coefficients, an embedding of the complete purchase history. We ask whether modern ML policy learners can inherit this information advantage by using those embeddings as inputs. To test this, we develop a cluster-robust double/debiased machine learning estimator of per-customer profit and apply it to the IRI mayonnaise panel across eight pricing policies. While standalone Bayes outperforms ML on ad-hoc features, ML methods supplied with Bayesian embeddings match or exceed its profitability. Our results underscore the power of model driven embeddings for personalized pricing.
