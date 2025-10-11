---
layout: post
title: Exploratory Data Analysis of Anscombe's Quartet
subtitle: Notebook and Report
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [eda, python, data-science, anscombe]
author: Zebunnisa Kanji
---

# Abstract
   This project explores Anscombe’s Quartet, a set of four small datasets that share nearly identical summary statistics but look very different when plotted. Using Python, I calculated the mean, variance, correlation, regression line, and coefficient of determination (R²) for each dataset. Although these statistical values are almost the same, the visualizations reveal distinct patterns — one linear, one curved, one with an outlier, and with a single influential point.

The analysis highlights the importance of Exploratory Data Analysis (EDA) and the principles of clear visualization. By applying Edward Tufte’s ideas, such as maximizing data-to-ink ratio and removing chartjunk, I created plots that communicate the data in this way.
# Introduction
    Francis Anscombe created four datasets, now known as Anscombe’s Quartet, to demonstrate the importance of visualizing data. Each dataset contains eleven pairs of x and y values. When analyzed statistically, all four sets have almost identical means, variances, correlations, and linear regression lines — yet when plotted, they show dramatically different patterns.
    The purpose of Exploratory Data Analysis (EDA) is to look beyond simple summary statistics and use graphs to uncover relationships, trends, and outliers that numbers alone cannot show. This project applies EDA techniques to Anscombe’s Quartet using Python, calculating descriptive statistics and creating scatter plots to compare each dataset. The goal is to understand how visualization reveals the deeper structure of data and why clear, ethical data presentation is essential in science and engineering.
# Data
The datasets are stored in `anscombe.csv` and contain columns: `dataset`, `x`, `y`. Each dataset (I–IV) has 11 data points. A preview of the first few rows:

| dataset | x  | y    |
|---------|----|------|
| I       | 10 | 8.04 |
| I       | 8  | 6.95 |
| I       | 13 | 7.58 |
| I       | 9  | 8.81 |
| I       | 11 | 8.33 |

# Visualizations
[interactive visualizations avaible in anscombeZK.ipynb]

## Scatterplots with Regression Lines
!(assets/img/anscombe_quartet_labeled.png)

## Overlaid
!(assets/img/anscombe_overlaid.png)

## Residual Plots
!(assets/img/anscombe_quartet_residual.png)

## Violin/Box Plots
!(assets/img/anscombe_boxplots_fixed.png)
!(assets/img/anscombe_violinplots_fixed.png)
