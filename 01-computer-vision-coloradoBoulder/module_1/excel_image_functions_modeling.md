# Excel Implementation & Modeling of Image Functions

## Overview

This guide details how to model, evaluate, and visualize image functions in Microsoft Excel prior to scaling up implementations to framework-level code (e.g., PyTorch).

Key topics include:
- Strategy for scaling learning: Hand calculation ($5 \times 5$) $\to$ Excel modeling ($10 \times 10$ to $20 \times 20$) $\to$ PyTorch implementation.
- Utilizing relative vs. absolute cell references (`$`) to build parameterized 2D spatial coordinate grids.
- Excel conditional formatting for color-scaling spatial intensity patterns.
- Modeling analytical image functions: Linear gradients, distance metrics ($L_1$, $L_2$), division with numerical stabilization ($\epsilon$), and 2D Gaussian distributions.
- Array formulas for element-wise operations, scalar shifts, parameterized linear combinations, and binary masking.

---

# 1. Excel Cell Referencing Strategy for 2D Grids

When building spatial coordinate grids in Excel, copying formulas without proper locking leads to unwanted shifting due to **relative referencing**. Constructing $f(x, y)$ functions across a 2D matrix requires strategic **absolute cell referencing** using the `$` anchor symbol.

```text
       Column H  Column I  Column J  ...
Row 22   x = -2    x = -1    x = 0   ...  <-- Fix Row index ($22)
Row 23   y = -2
Row 24   y = -1                           <-- Fix Column index ($B)
...