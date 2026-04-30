# Probability and Mathematical Statistics Calculator

Self-contained, single-file web application for probability theory and
mathematical statistics coursework. No build step, no external runtime
dependencies. The whole app lives in `index.html` and runs entirely in the
browser using vanilla JavaScript and HTML5 Canvas.

## What it does

The app contains 19 calculators grouped into six sections in the sidebar:

**Combinatorics**
- Factorial n!
- Combinations C(n,k) and arrangements A(n,k)
- Bernoulli single-trial probability

**Standard normal helpers**
- Density phi(x) of N(0,1)
- Laplace function Phi(x) (the Russian convention: integral from 0 to x)
- Inverse Laplace function

**Discrete distributions**
- Binomial table P(X=k) with mean, variance, and a verification sum
- Poisson table with lambda = n*p auto-derivation
- Hypergeometric table
- Geometric distribution Geom(p)

**Continuous distributions**
- Normal N(mu, sigma) with PDF and CDF curves
- Exponential Exp(lambda)
- Uniform U(a, b)
- Gamma(k, theta)
- Beta(alpha, beta)

**Hypothesis testing**
- Student's t (three sub-tasks: confidence probability, t-coefficient,
  one- and two-sided critical points)
- Chi-square (p-value from chi-squared, critical chi-squared from alpha)
- Fisher-Snedecor F-distribution critical value

**Sample statistics**
- Descriptive statistics with three input modes: raw values, discrete
  series (x_i, n_i), and grouped interval series (a_i, b_i, n_i). Computes
  mean, median, multimodal modes, biased and unbiased variance, both
  standard deviations, quartiles, IQR, coefficient of variation, skewness,
  and excess kurtosis. For grouped intervals, mode and median are computed
  via the standard textbook formulas (Gmurman / mathprofi convention).
  Three plots: frequency polygon, histogram (Sturges' rule by default),
  empirical CDF F*(x) as a step function.
- Pearson chi-squared goodness-of-fit test against six distributions
  (normal, uniform, exponential, Poisson, binomial, geometric).
  Parameters can be estimated from the sample (method of moments) or
  set manually. Two methods of computing expected frequencies:
  textbook convention (density at midpoint, no tail group, soft tail
  merging) or strict normalisation (CDF integral with tails extended
  to plus/minus infinity, hard cascading merge). Output includes the
  full computation table, observed-vs-expected chart, and the verdict
  with p-value.

## Input formats

The descriptive-statistics and Pearson sections accept data in any of
these forms, mixed:

- comma- or space-separated: `12, 14, 16, 12`
- Python-style list: `[1, 2, 3]`
- semicolons: `1; 2; 3`
- one value per line
- comma as decimal separator (`1,5`) or dot (`1.5`)

A grid of editable cells mirrors the textarea; rows can be added with
`+` and removed with the cross icon next to each row.

## Visualisation

All charts are drawn on the HTML5 Canvas without any plotting library.
Bar charts for discrete distributions, line charts with optional
markers for continuous distributions, shaded critical regions for
hypothesis tests, step functions for empirical CDFs, histograms with
overlaid theoretical density curves for the Pearson test.

Light and dark themes are switchable from the header; the choice
persists in `localStorage`. All chart colours come from CSS variables,
so charts re-render correctly when the theme changes.

## Math implementation

All special functions are implemented from scratch in JavaScript:

- erf via Abramowitz and Stegun 7.1.26 approximation
- log-gamma via the Lanczos series
- regularised lower and upper incomplete gamma (series and continued
  fraction)
- regularised incomplete beta via Lentz continued fraction
- inverse standard normal via the Beasley-Springer-Moro algorithm
- inverse chi-squared, t, F, gamma, and beta via Newton-Raphson on
  the corresponding CDF, with Wilson-Hilferty (chi-squared) or
  normal approximations as starting points

Every Excel function used in the original `terver.xls` reference
spreadsheet is re-implemented and verified against textbook tables.

## Usage

Open `index.html` in any modern browser. Everything runs locally;
no network access is required after the page loads (Google Fonts is
fetched once for Roboto, with a system sans-serif fallback).

For deployment, the file can be served as static content from any
host: GitHub Pages, Cloudflare Pages, Netlify, an S3 bucket, or any
plain HTTP server. There is no backend.

The whole UI and styles are wrapped in a single `.terver-app` container,
so the markup can also be embedded inside another page (for example, a
Tilda block) without leaking CSS into the host page.

## Credits

The original calculator structure and the choice of formulas follow
the workbook by A. Yemelin (mathprofi.ru), which itself is based on
the standard Russian-language textbook by V. E. Gmurman, "Probability
Theory and Mathematical Statistics". Notations, parameter estimators,
and test conventions follow that tradition (sample standard deviation
sigma_v rather than the unbiased s for Pearson's test; midpoint density
approximation for theoretical frequencies; soft tail merging in the
default mode).
