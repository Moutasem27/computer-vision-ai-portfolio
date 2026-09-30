# Lecture Transcripts: Image Representation & Processing in Excel

---

## Transcript 1: Introduction to Image Functions & Absolute Referencing in Excel

Now, let's go to the Excel example. How can we practically calculate some of this by writing some code? When we deal with slightly larger scale, there is a scale of $5 \times 5$, I'd like to work it by hand; a scale with about $10 \times 20$, I'd like to do in Excel to model my calculation; and then once I feel confident about my calculation, I implement in PyTorch. So this is my preferred method of scaling up your learning. 

So let me repeat some of the examples I showed you before, but I'm going to use black ink this time, because it's going to be quite colorful, this example I prepared for you. So let me just do the black ink. 

As a review, what if we talked about this $f(x) = x$, $f(x, y) = y$? We talked about the three images in the previous example by hand. How can we code this up? The third image is $x + y$.

Now, I'm going to select an image, so let me define this location as a particular cell. I want to just set $x$, so that's it. Then I copy and paste to the other location; it seems to be fine. I copy over here, also fine. But then I realize it's not working here. What's going on? 

When we examine that, you realize that by default, when you copy and paste formulas, it uses relative position and relative references. So you're going to keep the relative cell relative to your location by the same distance, which is about four down. In this case, we don't want it to do that; we need to use **absolute reference**. This is an Excel technique we have to use quite a lot in this class, so I think this is a good way to introduce it.

What I'd like to fix is all my $x$ values to this particular column—this column is where my $x$'s are. We want to fix the row to row 22. So what I want to do is fix this using a dollar sign (`$`) to say that I want to fix row 22, as 22 is something I don't want to change. But column `G`, I'm happy to change from column `G` to column `H`. 

So now we have my function. Now I copy and paste this across. This is about 9 to 81 times—I could do it 81 times, but I don't want to do that by hand. So I bring out my mouse to select the whole area. Then I paste using **Paste Special -> Formulas**. 

---

### Scaling Up & Formatting Patterns

Now I'm going to repeat similar logic, but I'll do it a lot faster this time. What I need to fix for $y$ is the column. So when I write the equation, I select $y$, but I don't want the column to change. I place a dollar sign (`$`) before the column letter. Every time I press `F2`, I can see the cell being highlighted, showing which square it depends on. Now I select the entire region and paste formulas to get my image function.

Lastly, I combine the two concepts: $f(x, y) = x + y$. Not only do I have to fix $y$, but I also fix $x$. I use absolute references strategically: fix row 22 for $x$ and fix column `Z` (or column reference) for $y$. 

To visualize this, I can add a **Color Scale** (conditional formatting). The high positive values are green and low values are red. It makes it much easier to visualize that $f(x,y) = x + y$ forms a diagonal pattern.

---

### Linear Functions & Slopes

Let's do another function: $f(x, y) = x - y$.
1. Select $x$ (e.g., row 51) and $y$ (e.g., column `B`).
2. Fix $x$'s row (`$51`) and $y$'s column (`$B`).
3. Copy and Paste Special Formulas across the $21 \times 21$ space.

Comparing $x + y$ and $x - y$, you can see that $x + y$ goes diagonally one way, and $x - y$ flips the diagonal direction.

Next, let's add coefficients:
$$f(x, y) = 2x + 5y$$

When we apply the color scale to $2x + 5y$, the gradient becomes flatter because the function changes much faster in $y$ ($5y$) and slower in $x$ ($2x$).

---

### Handling Division by Zero with Epsilon

What about $f(x, y) = \frac{x}{y}$?

If we write `= X / Y` and copy it across, we get division by zero errors (`#DIV/0!`) wherever $y = 0$. 

In practice, when implementing this, we add a small epsilon ($\epsilon$) to the denominator:
$$f(x, y) = \frac{x}{y + 0.01}$$

Now, at $y = 0$, the value gets large but does not blow up into an error.

---

## Transcript 2: Vector Norms ($L_1$ and $L_2$ Norm Images)

### $L_1$ Norm Space

Let's define an image that represents $L_1$ space:
$$f(x, y) = |x| + |y|$$

1. Pick any cell and enter `= ABS(x) + ABS(y)`.
2. Apply appropriate absolute column/row references.
3. Copy and paste formulas across the region.
4. Apply a color scale.

The resulting pattern clearly forms a **diamond shape**.

---

### $L_2$ Norm Space

Now let's do $L_2$ norm space:
$$f(x, y) = \sqrt{x^2 + y^2}$$

1. Enter `= SQRT(x^2 + y^2)`.
2. Fix the reference rows and columns.
3. Copy and paste formulas across the grid.
4. Apply conditional color formatting.

The resulting visualization shows **concentric circles**, which matches the geometric definition of distance/circles.

---

## Transcript 3: 2D Gaussian Distribution Image

Now let's implement a 2D Gaussian distribution:
$$f(x, y) = \frac{1}{2\pi \sigma^2} \exp\left( -\frac{x^2 + y^2}{2\sigma^2} \right)$$

### Setting Up Parameters:
- Set $\sigma = 1$ (or $0.5$).
- Pre-compute $2\sigma^2$ in a helper cell.

### Formula Implementation:
1. In Excel, write: `= EXP(-(x^2 + y^2) / (2 * sigma_sq)) / (2 * PI() * sigma_sq)`.
2. Fix references to the parameter cells using absolute references (`$`).
3. Copy and paste formulas across the grid.

### Verification (Discrete Integration):
A fundamental property of a probability density function like the 2D Gaussian is that its integral equals 1. In this discrete grid, we can perform a double summation:
$$\sum_{x} \sum_{y} f(x, y) \approx 1$$

Using `= SUM(region)` on our matrix confirms the total sum equals $1$. You can now interactively adjust $\sigma$ to see how the Gaussian light spot expands or contracts in real time.

---

## Transcript 4: Array Formulas, Linear Combinations & Masking

Now let's talk about applying function operations to images using **Array Formulas**. 

Suppose we have an image $F$ representing a computer monitor.

### 1. Scalar Subtraction / Addition ($F \pm c$)
Instead of copying a formula cell by cell, we can use an array formula:
1. Select the entire output grid.
2. Type `= F_array - 10` (or select a parameter cell $A$).
3. Excel applies the scalar operation to every single element in the 2D array simultaneously.

Adding a positive constant increases brightness (eventually saturating), while subtracting darkens the image.

---

### 2. Parameterized Linear Combination of Images
Given two images $F$ and $G$, we can construct a linear blend parameterized by $\alpha \in [0, 1]$:
$$I_{\text{out}} = \alpha F + (1 - \alpha) G$$

Using array formulas:
1. Define $\alpha$ in a parameter cell (e.g., `0.5`).
2. Write `= alpha * F_array + (1 - alpha) * G_array`.
3. Adjusting $\alpha$ smoothly transitions between image $F$ (e.g., computer) and image $G$ (e.g., question mark).

---

### 3. Element-wise Image Masking
Masking is an element-wise multiplication between an image $F$ and a binary/grayscale mask $M$:
$$I_{\text{out}} = F \odot M$$

In Excel:
1. Select the destination array.
2. Type `= F_array * M_array`.
3. Press enter to perform element-wise array multiplication.

Changing values in the mask $M$ updates the masked image output in real-time.

---

## Summary & Conclusion

In these exercises, we explored two extremes of image representation:
1. **Highest Dimension:** Representing an image pixel by pixel as raw numerical arrays.
2. **Lowest Dimension:** Representing an image purely as a mathematical function $f(x, y)$.

Later in the course, classical image analysis and deep learning methods focus on bridging these extremes—learning underlying functional representations from raw pixel data to perform tasks like recognition, segmentation, and generative modeling.
