# Study Guide: Computer Vision & Image Representation in Excel

---

## 1. Core Concept: Image Representation Continuum
In computer vision, images can be viewed along a spectrum of dimensionality:
* **Highest Dimension (Pixel Matrix):** Representing an image pixel-by-pixel as an explicit 2D grid/array of intensity values.
* **Lowest Dimension (Functional Representation):** Representing an image continuously as a mathematical function $f(x, y)$, where pixel intensity is generated dynamically based on spatial coordinates $(x, y)$.

> **Goal of Classical & Modern CV:** Bridging raw pixel data with implicit functional representations to enable tasks like feature extraction, image generation, and classification.

---

## 2. Essential Excel Techniques for Matrix Modeling
Before coding in PyTorch or C++, Excel serves as an intermediate modeling tool for $10 \times 20$ to $25 \times 25$ grid sizes.

### Absolute vs. Relative Referencing (`$`)
When evaluating $f(x, y)$ on a 2D grid, spatial coordinates ($x$ and $y$) reside in specific header rows and columns.
* **Fixing Rows (for $x$-coordinates):** Fix the row number using `G$22` so that copying vertically does not shift the selected $x$-coordinate row.
* **Fixing Columns (for $y$-coordinates):** Fix the column letter using `$B33` so that copying horizontally does not shift the selected $y$-coordinate column.
* **Fixing Parameters (Scalars):** Fix both row and column `$C$2` for static global parameters (e.g., $\sigma$ or $\alpha$).

### Efficient Grid Operations
* **Paste Special Formulas:** Select the full $M \times N$ grid $\rightarrow$ **Paste Special ($\text{Alt}+\text{E}+\text{S}+\text{F}$)** $\rightarrow$ **Formulas**.
* **Color Scales:** Use Conditional Formatting Color Scales (e.g., Red-Yellow-Green or Grayscale) to visualize numeric spatial patterns visually.

---

## 3. Synthetic Image Functions $f(x, y)$

### A. Linear Functions & Spatial Orientation
Linear combinations of spatial coordinates produce directional gradient patterns across the image grid.

| Function Formula | Formula Pattern / Behavior | Geometric Pattern |
| :--- | :--- | :--- |
| $f(x, y) = x + y$ | Equal weight on both axes | Diagonal gradient (Top-Left to Bottom-Right) |
| $f(x, y) = x - y$ | Subtraction flips gradient direction | Diagonal gradient (Bottom-Left to Top-Right) |
| $f(x, y) = 2x + 5y$ | $y$ changes $2.5\times$ faster than $x$ | Flatter, steeper diagonal gradient along the $y$-axis |

### B. Division Functions & Epsilon Regularization
* **Function:** $f(x, y) = \frac{x}{y}$
* **Issue:** Division by zero occurs along the axis where $y = 0$, causing `#DIV/0!` errors.
* **Solution (Epsilon Regularization):** Introduce a small numerical stability term $\epsilon > 0$:
  $$f(x, y) = \frac{x}{y + \epsilon} \quad (\text{e.g., } \epsilon = 0.01)$$

### C. Distance Metrics & Norm Spaces

#### $L_1$ Norm (Manhattan / Diamond Distance)
$$f(x, y) = |x| + |y|$$
* **Excel Implementation:** `= ABS(x) + ABS(y)`
* **Visual Output:** Diamond-shaped isosurfaces centered at the origin.

#### $L_2$ Norm (Euclidean / Circular Distance)
$$f(x, y) = \sqrt{x^2 + y^2}$$
* **Excel Implementation:** `= SQRT(x^2 + y^2)`
* **Visual Output:** Concentric circular isosurfaces centered at the origin.

---

## 4. Analytical Probability Functions: 2D Gaussian

### Mathematical Definition
A 2D Gaussian distribution represents a localized smooth intensity spot:

$$f(x, y) = \frac{1}{2\pi \sigma^2} \exp\left( -\frac{x^2 + y^2}{2\sigma^2} \right)$$

Where:
* $\sigma$ = Standard deviation (controls the blur/spread of the Gaussian kernel).
* $\sigma^2$ = Variance.

### Verification via Discrete Integration
For any continuous probability density function:

$$\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(x, y) \, dx \, dy = 1$$

In a discrete image matrix, we verify this property by summing all pixel intensity values across the 2D grid:

$$\sum_{x} \sum_{y} f(x, y) \approx 1.0$$

* If the discrete sum $\approx 1$, the continuous Gaussian calculation and normalization constant $\frac{1}{2\pi \sigma^2}$ are correctly implemented.

---

## 5. Modern Image Operations using Dynamic Array Formulas

Instead of cell-by-cell copying, modern spreadsheet and array engines perform operations on entire image matrices at once.

### A. Scalar Operations (Brightness Control)
Applying scalar addition or subtraction adjusts global image illumination:
$$I_{\text{out}} = F \pm c$$
* **Excel Array Syntax:** Select destination range $\rightarrow$ `= F_range + 10`
* **Effect:** Positive constants brighten the image (saturating high values); negative constants darken the image.

### B. Parameterized Linear Combination (Image Blending)
Linear blending combines two images $F$ and $G$ using a mixing parameter $\alpha \in [0, 1]$:

$$I_{\text{out}} = \alpha F + (1 - \alpha) G$$

* **Excel Array Syntax:** `= alpha * F_range + (1 - alpha) * G_range`
* **Behavior:**
  * When $\alpha = 1.0 \rightarrow$ Output is purely Image $F$.
  * When $\alpha = 0.0 \rightarrow$ Output is purely Image $G$.
  * When $\alpha = 0.5 \rightarrow$ Equal 50/50 blend of both images.

### C. Element-Wise Multiplication (Masking)
Masking applies a binary or grayscale spatial filter $M$ to an image $F$:

$$I_{\text{out}} = F \odot M$$

* **Excel Array Syntax:** `= F_range * M_range`
* **Effect:** Pixels where $M(x,y) = 1$ pass through untouched; pixels where $M(x,y) = 0$ are suppressed to zero (black).

---

## 6. Self-Assessment Review Questions

1. **Why is absolute row-referencing used for $x$ and absolute column-referencing used for $y$ when generating $2\text{D}$ functions?**
2. **What geometric shapes do $L_1$ and $L_2$ norm functions generate when plotted as grayscale images?**
3. **What is the purpose of adding an $\epsilon$ term to functions like $f(x, y) = \frac{x}{y}$?**
4. **How do array formulas improve efficiency over standard relative-cell copying when blending images?**
5. **How does changing the value of $\sigma$ affect the visual output and pixel sum of a 2D Gaussian image?**
