# Image Representation Using Mathematical Functions

## Overview

This lecture introduces mathematical functions as a compact alternative to raw pixel arrays for describing digital images.

The main topics are:

- Comparing pixel array representations to functional image representations
- Defining basic 2D image functions ($f(x, y) = x$ and $f(x, y) = y$)
- Combining coordinate variables ($f(x, y) = x + y$)
- Linear combinations of coordinates ($a \cdot x + b \cdot y$)
- High-dimensional pixel arrays vs. low-dimensional functional representations
- Defining geometric shapes using conditional/piecewise functions
- Basic image arithmetic (scalar addition, negation, scalar multiplication)
- Coordinate transformations (axis flipping)
- Linear combination of two images
- Applying binary masks to images
- Dimensionality reduction and squeezed functional representations

---

# 1. Comparing Pixel Arrays and Functions

In previous discussions, digital images were represented as 2D matrices of discrete pixel intensity values.

An alternative approach is to describe the pixel intensity at any given spatial location $(x, y)$ using a continuous or discrete mathematical function $f(x, y)$.

Conceptually:

```text
Pixel Representation:
Store explicit intensity value for every (x, y) location.

Functional Representation:
Store a function f(x, y) that computes the intensity for any (x, y) location.
```

---

# 2. Basic Single-Variable 2D Image Functions

Consider a $5 \times 5$ image matrix centered around the origin $(0,0)$.

### Example 1: $f(x, y) = x$

In this function, the intensity value at any location depends strictly on its $x$-coordinate, ignoring $y$:

$$f(x, y) = x$$

Evaluating across a $5 \times 5$ grid where $x \in \{-2, -1, 0, 1, 2\}$ and $y \in \{-2, -1, 0, 1, 2\}$:

$$\begin{bmatrix}
-2 & -1 & 0 & 1 & 2 \\
-2 & -1 & 0 & 1 & 2 \\
-2 & -1 & 0 & 1 & 2 \\
-2 & -1 & 0 & 1 & 2 \\
-2 & -1 & 0 & 1 & 2
\end{bmatrix}$$

Intensity increases horizontally from left to right.

---

### Example 2: $f(x, y) = y$

In this function, the intensity value at any location depends strictly on its $y$-coordinate, ignoring $x$:

$$f(x, y) = y$$

Evaluating across the same $5 \times 5$ grid:

$$\begin{bmatrix}
2 & 2 & 2 & 2 & 2 \\
1 & 1 & 1 & 1 & 1 \\
0 & 0 & 0 & 0 & 0 \\
-1 & -1 & -1 & -1 & -1 \\
-2 & -2 & -2 & -2 & -2
\end{bmatrix}$$

Intensity varies vertically along the $y$-axis.

---

# 3. Combining Coordinate Variables

We can combine $x$ and $y$ variables into a single functional expression.

### Sum of Coordinates: $f(x, y) = x + y$

Evaluating $f(x, y) = x + y$ on the $5 \times 5$ grid:

$$\begin{bmatrix}
0 & 1 & 2 & 3 & 4 \\
-1 & 0 & 1 & 2 & 3 \\
-2 & -1 & 0 & 1 & 2 \\
-3 & -2 & -1 & 0 & 1 \\
-4 & -3 & -2 & -1 & 0
\end{bmatrix}$$

This produces a diagonal gradient pattern across the image grid.

---

# 4. Dimensionality: Raw Pixels vs. Function Parameters

There is a fundamental tradeoff in image representation between storing raw pixel data and using functional formulations.

```text
Raw Pixel Spectrum                        Functional Spectrum
------------------                        -------------------
High Dimensionality                        Low Dimensionality
Requires storage for every pixel           Requires only functional parameters
5x5 Image = 25 dimensions                  f(x, y) = a*x + b*y -> 2 parameters (a, b)
1000x1000 Image = 1,000,000 dimensions     1,000,000 values vs. 2 parameters
```

### Key Takeaway

> **Functional representation acts as an extreme form of dimensionality reduction by capturing underlying spatial patterns with minimal parameters.**

---

# 5. Representing Geometric Shapes Using Piecewise Functions

Complex spatial structures, such as a solid square, can be represented using conditional or piecewise functions.

### Solid Square Function

To define a solid $3 \times 3$ square of intensity $1$ centered at $(0,0)$ on a $5 \times 5$ grid:

$$f(x, y) = \begin{cases} 1 & \text{if } |x| \le 1 \text{ and } |y| \le 1 \\ 0 & \text{otherwise} \end{cases}$$

Matrix Evaluation:

$$\begin{bmatrix}
0 & 0 & 0 & 0 & 0 \\
0 & 1 & 1 & 1 & 0 \\
0 & 1 & 1 & 1 & 0 \\
0 & 1 & 1 & 1 & 0 \\
0 & 0 & 0 & 0 & 0
\end{bmatrix}$$

In software implementations, piecewise functional definitions correspond directly to conditional `if-else` control structures.

---

# 6. Basic Image Operations

### 1. Brightness Adjustment (Scalar Addition)

Adding a constant scalar $C$ to an image function increases overall intensity:

$$g(x, y) = f(x, y) + C$$

If $C = 10$, every spatial location is shifted by $10$:
- A pixel with intensity $4$ becomes $14$.
- Background pixels with intensity $0$ become $10$.

---

### 2. Image Inversion (Negation)

Negating an image function flips its intensity values:

$$g(x, y) = -f(x, y)$$

- An intensity of $3$ becomes $-3$.
- An intensity of $0$ remains $0$.

---

### 3. Contrast Scaling (Scalar Multiplication)

Multiplying an image function by a scalar factor $k$ scales the local intensity proportionally:

$$g(x, y) = k \cdot f(x, y)$$

If $k = 3$:
- Intensity $3$ becomes $9$.
- Intensity $2$ becomes $6$.
- Background zeros remain $0$.

---

# 7. Coordinate Transformations (Axis Flipping)

By substituting spatial coordinate indices $u$ and $v$ for $y$ and $x$, spatial transformations (e.g., flipping) can be performed.

```text
Original Axes (x, y)  --->  Transformed Axes (u, v)
```

### Case 1: Vertical Flip ($u = -y$, $v = x$)

Flipping $y$ relative to the horizontal axis $y = 0$ flips the resulting image upside down.

---

### Case 2: Dual-Axis Flip ($u = -y$, $v = -x$)

Negating both coordinate axes flips the image across both the vertical and horizontal axes, equivalent to a $180^\circ$ rotation.

---

# 8. Linear Combination of Two Images

Two images $f(x, y)$ and $g(x, y)$ can be blended together using a linear combination:

$$h(x, y) = a \cdot f(x, y) + b \cdot g(x, y)$$

To constrain energy and prevent intensity overflow, coefficients are normalized such that:

$$a + b = 1$$

### Calculation Example

Given $a = 0.7$ and $b = 0.3$:

1. **Location A:** $f = 2$, $g = 8$
   $$h = (0.7 \times 2) + (0.3 \times 8) = 1.4 + 2.4 = 3.8$$

2. **Location B:** $f = 4$, $g = 8$
   $$h = (0.7 \times 4) + (0.3 \times 8) = 2.8 + 2.4 = 5.2$$

3. **Location C:** $f = 6$, $g = 100$
   $$h = (0.7 \times 6) + (0.3 \times 100) = 4.2 + 30.0 = 34.2$$

---

# 9. Masking (Element-Wise Multiplication)

Image masking selectively preserves specific regions of an input image $f(x, y)$ using a binary or intensity mask $m(x, y)$.

$$h(x, y) = f(x, y) \odot m(x, y)$$

Where $\odot$ represents element-wise (Hadamard) multiplication rather than standard matrix multiplication.

```text
Input Image f(x,y)       Binary Mask m(x,y)       Masked Output h(x,y)
    [ 2  3 ]         x       [ 1  1 ]         =       [ 2  3 ]
    [ 4  5 ]                 [ 0  1 ]                 [ 0  5 ]
```

- Locations where $m(x, y) = 1$ preserve the original image pixel intensity.
- Locations where $m(x, y) = 0$ zero out the image intensity.

---

# 10. Summary Table of Functional Operations

| Operation | Equation | Visual / Functional Effect |
|---|---|---|
| Single Variable $x$ | $f(x, y) = x$ | Horizontal intensity gradient |
| Single Variable $y$ | $f(x, y) = y$ | Vertical intensity gradient |
| Coordinate Addition | $f(x, y) = x + y$ | Diagonal intensity gradient |
| Piecewise Shape | $f(x, y) = \text{if } \|x\|, \|y\| \le 1 \text{ then } 1 \text{ else } 0$ | Solid rectangular/square region |
| Brightness Shift | $g(x, y) = f(x, y) + C$ | Uniform brightness increase/decrease |
| Inversion | $g(x, y) = -f(x, y)$ | Negated pixel intensities |
| Contrast Scaling | $g(x, y) = k \cdot f(x, y)$ | Proportional intensity scaling |
| Axis Flip | $g(u, v) = f(-y, x)$ | Spatial re-orientation / inversion |
| Linear Blend | $h(x, y) = a \cdot f(x,y) + b \cdot g(x,y)$ | Weighted image blending |
| Masking | $h(x, y) = f(x, y) \odot m(x, y)$ | Element-wise region filtering |

---

# Key Takeaways

1. **Functional Abstraction:** An image can be viewed either as an explicit grid of stored pixel intensities or as a continuous/discrete function $f(x, y)$ mapping spatial coordinates to intensity values.
2. **Extreme Compression:** Storing function parameters (e.g., $a \cdot x + b \cdot y$) rather than raw pixel matrices provides significant dimensionality reduction, compressing megapixel images down to a few coefficients.
3. **Geometric Definitions:** Geometric shapes are expressed mathematically using conditional ranges or piecewise equations.
4. **Spatial Operations:** Traditional image processing steps—such as brightness shifts, contrast scaling, spatial transformation, linear blending, and masking—can be modeled directly as continuous operations on function space.