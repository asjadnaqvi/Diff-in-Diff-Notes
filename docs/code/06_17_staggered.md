---
layout: default
title: did_multiplegt_dyn
parent: Stata code
nav_order: 16
mathjax: true
image: "../../../assets/images/DiD.png"
---

# staggered 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Notes

- Based on: [Roth and Sant'Anna 2023](https://www.journals.uchicago.edu/doi/abs/10.1086/726581)
- Program version (if available): version 0.7.1 24Sep2024 
- Last checked: Nov 2024

## Installation

```stata
ssc install staggered, replace
```

Take a look at the help file:

```stata
help staggered
```

## Test the command

Please make sure that you generate the data using the script given [here](https://asjadnaqvi.github.io/DiD/docs/code/06_03_data/) 

Let's try the basic `staggered` command:


```stata
staggered Y, i(id) t(t)  g(gvar) estimand(eventstudy) eventTime(-10/10)
```

and we get this output:

```stata
(warning: e(V) is a diagonal matrix of SEs, not a full vcov matrix)

Staggered Treatment Effect Estimate                      Number of obs = 1,800

------------------------------------------------------------------------------
             |              Adjusted
             | Coefficient  std. err.      z    P>|z|     [95% conf. interval]
-------------+----------------------------------------------------------------
    gvar -10 |   9.304067   .4996601    18.62   0.000     8.324751    10.28338
          -9 |   9.787764   .6209413    15.76   0.000     8.570741    11.00479
          -8 |   10.01082   .5978471    16.74   0.000     8.839057    11.18257
          -7 |   9.569863   .5851203    16.36   0.000     8.423048    10.71668
          -6 |    9.87556   .6564452    15.04   0.000     8.588951    11.16217
          -5 |   10.13594   .5682056    17.84   0.000     9.022282    11.24961
          -4 |   8.770024   .4246769    20.65   0.000     7.937672    9.602375
          -3 |    8.64266   .3522466    24.54   0.000     7.952269    9.333051
          -2 |   9.083506   .3091394    29.38   0.000     8.477604    9.689408
          -1 |   9.249542   .3714874    24.90   0.000      8.52144    9.977644
           0 |  -.0439985   .2793774    -0.16   0.875    -.5915681    .5035711
           1 |   7.357605   1.382526     5.32   0.000     4.647904    10.06731
           2 |   13.78912   1.325263    10.40   0.000     11.19165    16.38658
           3 |   19.75595    1.33892    14.76   0.000     17.13171    22.38018
           4 |   25.76031   1.372833    18.76   0.000      23.0696    28.45101
           5 |   32.51529   1.378378    23.59   0.000     29.81371    35.21686
           6 |   39.04744   1.445284    27.02   0.000     36.21473    41.88014
           7 |   44.62738   1.330505    33.54   0.000     42.01964    47.23512
           8 |   50.92396   1.321695    38.53   0.000     48.33349    53.51444
           9 |   57.47601   1.415283    40.61   0.000     54.70211    60.24991
          10 |   63.62182   1.306207    48.71   0.000      61.0617    66.18193
------------------------------------------------------------------------------


```

Graph is generated following the process described [here](https://github.com/mcaceresb/stata-staggered):

```stata
tempname CI b
mata st_matrix("`CI'", st_matrix("r(table)")[5::6, .])
mata st_matrix("`b'",  st_matrix("e(b)"))
coefplot matrix(`b'), ci(`CI') vertical yline(0)
```

<img src="../../../assets/images/staggered_1.png" width="100%">
