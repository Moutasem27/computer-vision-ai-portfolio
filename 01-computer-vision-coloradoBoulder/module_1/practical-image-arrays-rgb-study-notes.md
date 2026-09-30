# Practical Image Array & RGB Image Representation in Excel

## Overview

This lecture extends the basic concepts of image arrays and image matrices by applying them to larger examples in Excel.

The main topics are:

- Converting a long byte image array into an image matrix
- Using image width correctly
- Using Excel's `WRAPROWS` function
- Visualizing byte values as grayscale pixels
- Understanding the effect of an incorrect image width
- Representing a `32 × 32` RGB image
- Understanding why a color image has three values per pixel
- Arranging RGB data for display
- Simulating an LED screen using Excel
- Understanding how individual RGB components visually mix when viewed from a distance

---

# 1. Converting a Byte Image Array into an Image Matrix

A byte image can be represented as a long array of values.

For example:

```text
[0, 0, 0, 0, 0, ...]
```

Suppose we know that the image width is:

```text
32
```

To convert the long array into an image matrix, we need to place **32 values in every row**.

Conceptually:

```text
Byte Image Array
       ↓
Take 32 values
       ↓
First row
       ↓
Take the next 32 values
       ↓
Second row
       ↓
Continue until the array is exhausted
```

Doing this manually is tedious for a large image.

This is one reason programming and spreadsheet functions are useful: they automate repetitive operations.

---

# 2. Using Excel `WRAPROWS`

Excel provides a function called:

```excel
WRAPROWS
```

which can be used to wrap a long sequence of values into rows of a specified size.

For example, if the image width is `32`, the values can be wrapped into rows containing 32 elements.

Conceptually:

```text
Long Array:

1  2  3  4  5  6  7  8  ...

             ↓ WRAPROWS

Row 1: 32 values
Row 2: 32 values
Row 3: 32 values
...
```

This performs the same basic operation that we previously performed manually when converting an image array into an image matrix.

---

# 3. Byte Values as Image Intensities

A byte image uses values in the range:

```text
0 → 255
```

These values can represent grayscale intensity.

A useful interpretation is:

```text
0   → Black
255 → White
```

Values between them represent different shades of gray.

For example:

```text
0    → Black
128  → Gray
255  → White
```

Therefore, when the byte values are arranged into the correct matrix, they can be visualized as a grayscale image.

---

# 4. Visualizing the Image in Excel

Excel can use **conditional formatting** to display the numerical image as different shades.

For example:

```text
Pixel value = 255
        ↓
Bright / white pixel

Pixel value = 128
        ↓
Darker gray pixel

Pixel value = 0
        ↓
Black pixel
```

This allows the spreadsheet to behave like a simple image viewer.

The numerical matrix contains the actual image data, while the cell formatting provides a visual representation of those values.

---

# 5. Why Image Width Matters

The image width is extremely important when converting a 1D image array into a 2D matrix.

For example, suppose the actual image width is:

```text
32
```

If the array is wrapped using:

```text
32 values per row
```

the pixels are placed in their correct positions.

The image can therefore appear as the intended picture.

However, if we use the wrong width, the pixels are placed incorrectly.

---

# 6. Effect of Using the Wrong Width

Suppose the actual image width is:

```text
32
```

but we incorrectly use:

```text
35
```

The values will no longer line up with the original image structure.

The result can look like:

```text
Original:

[ correct image ]

Wrong width:

[ shifted / distorted image ]
```

The entire image may appear to shift or become distorted.

This is a common pattern when an image is reconstructed using an incorrect width.

### Key idea

> **The image width is part of the information needed to correctly reconstruct an image from a 1D array.**

Even though the numerical values themselves have not changed, their spatial arrangement has changed.

---

# 7. Example: 32 × 32 Image

If the image width is:

```text
32
```

and the image is square, then:

```text
Width  = 32
Height = 32
```

Therefore, the image contains:

```text
32 × 32 = 1024 pixels
```

For a grayscale image, each pixel requires one value.

Therefore:

```text
1024 pixels × 1 value
= 1024 values
```

---

# 8. Extending the Example to RGB

A color RGB image requires three values for every pixel:

```text
R = Red
G = Green
B = Blue
```

Therefore, a `32 × 32` RGB image has a conceptual shape of:

```text
32 × 32 × 3
```

The three dimensions represent:

```text
Height × Width × Color Channels
```

So:

```text
32 × 32 × 3
```

means:

```text
32 rows
32 pixels per row
3 values per pixel
```

---

# 9. Number of Values in a 32 × 32 RGB Image

The image contains:

```text
32 × 32 = 1024 pixels
```

Each pixel has three values:

```text
R + G + B
```

Therefore:

```text
1024 × 3 = 3072 values
```

So a `32 × 32` RGB image requires:

```text
3072 numerical values
```

when stored as a flat RGB array.

---

# 10. RGB Ordering

When representing the RGB image array, we need to agree on the order of the color channels.

In this example, the order is:

```text
RGB
```

Therefore, every group of three values represents one pixel:

```text
R  G  B
```

For example:

```text
255, 0, 0
```

represents a red pixel.

The next three values represent the next pixel:

```text
R  G  B
```

and so on.

Therefore:

```text
[R, G, B] [R, G, B] [R, G, B] ...
```

represents consecutive pixels in the image.

---

# 11. Wrapping an RGB Image

For a grayscale `32 × 32` image, we previously needed:

```text
32 values per row
```

However, an RGB image has three values per pixel.

A row contains:

```text
32 pixels × 3 values per pixel
```

Therefore:

```text
32 × 3 = 96 values per row
```

So the RGB array should be wrapped using:

```text
96 values per row
```

Conceptually:

```text
RGB image width = 32 pixels

Each pixel = 3 values

Values per row:
32 × 3 = 96
```

This is why the spreadsheet needs to wrap the data by:

```text
32 × 3
```

rather than simply:

```text
32
```

---

# 12. RGB Image Matrix

The RGB image can therefore be described as:

```text
32 × 32 × 3
```

Each spatial location contains three values:

```text
Pixel (x, y)
     ↓
(R, G, B)
```

A row contains:

```text
(R,G,B) (R,G,B) (R,G,B) ... 
```

for 32 pixels.

Therefore:

```text
32 pixels × 3 values = 96 values
```

per row.

---

# 13. RGB Data as Pixels

Consider a simplified row containing three pixels:

```text
255  0  0
255  255  0
0    255  255
```

We can group the values into:

```text
(255, 0, 0)
(255, 255, 0)
(0, 255, 255)
```

These represent:

```text
Red
Yellow
Cyan
```

The important concept is:

> **Every three consecutive values represent one RGB pixel when the channel order is RGB.**

---

# 14. Simulating an LED Matrix

A display can be thought of as a large matrix of RGB pixels.

Each pixel contains three primary color components:

```text
Red
Green
Blue
```

Each component can have its own intensity.

For example:

```text
RGB = (255, 0, 0)
```

means the red component is at maximum intensity while green and blue are zero.

Similarly:

```text
RGB = (255, 255, 255)
```

means all three components have maximum intensity.

---

# 15. Visualizing RGB Pixels in Excel

Excel can use conditional formatting to color cells based on the RGB values.

A simplified representation is:

```text
R value → controls red component
G value → controls green component
B value → controls blue component
```

The three values together determine the final color of a pixel.

For example:

```text
(255, 0, 0)       → Red
(0, 255, 0)       → Green
(0, 0, 255)       → Blue
(255, 255, 0)     → Yellow
(0, 255, 255)     → Cyan
(255, 0, 255)     → Magenta
(0, 0, 0)         → Black
(255, 255, 255)   → White
```

---

# 16. Why Colors Appear to Mix

When looking very closely at a display, the individual RGB components can be distinguished.

However, when we move farther away from the screen, the individual components become too small to distinguish separately.

They visually blend together.

Conceptually:

```text
Very close:

[R][G][B][R][G][B][R][G][B]

        ↓ move farther away

Individual components become harder to distinguish

        ↓

Colors appear to mix

        ↓

A complete image becomes visible
```

This is similar to how an actual LED display produces colors from many small RGB components.

---

# 17. Excel as a Low-Level Image Simulation

Using Excel makes it possible to see both:

1. The **numerical representation** of the image.
2. The **visual result** produced by those numbers.

This provides a bridge between mathematical/image concepts and actual implementation.

For example:

```text
Numerical Data
      ↓
Array
      ↓
Wrap into rows
      ↓
Image Matrix
      ↓
RGB values
      ↓
Color visualization
      ↓
Displayed image
```

---

# 18. Grayscale vs RGB Representation

## Grayscale

A grayscale pixel requires one value:

```text
Pixel = intensity
```

For a byte image:

```text
0 → 255
```

Example:

```text
128
```

represents a gray intensity.

## RGB

An RGB pixel requires three values:

```text
Pixel = (R, G, B)
```

Example:

```text
(255, 0, 0)
```

represents red.

Therefore:

```text
Grayscale:
1 pixel → 1 value

RGB:
1 pixel → 3 values
```

---

# 19. Comparing Image Representations

| Representation | Values per Pixel | Example |
|---|---:|---|
| Binary | 1 | `0` or `1` |
| Byte grayscale | 1 | `128` |
| RGB | 3 | `(255, 0, 0)` |
| Normalized RGB | 3 | `(1, 0, 0)` |

For a `32 × 32` image:

### Grayscale

```text
32 × 32 × 1
= 1024 values
```

### RGB

```text
32 × 32 × 3
= 3072 values
```

---

# 20. Important Formulas

### Number of pixels

```text
Pixels = Width × Height
```

### Grayscale values

```text
Values = Width × Height
```

### RGB values

```text
Values = Width × Height × 3
```

### RGB values per row

```text
Values per row = Width × 3
```

For a `32 × 32` RGB image:

```text
Pixels:
32 × 32 = 1024

RGB values:
32 × 32 × 3 = 3072

Values per row:
32 × 3 = 96
```

---

# 21. Common Mistake: Incorrect Width

When reconstructing an image from a flat array, using the wrong width causes the image structure to become incorrect.

For example:

```text
Actual width = 32
```

but:

```text
Used width = 35
```

The pixels will be placed at the wrong locations.

This can produce:

- Shifted images
- Distorted images
- Incorrect shapes
- Difficult-to-recognize images

Therefore, always make sure that the width used to reconstruct the matrix matches the original image.

---

# 22. Key Takeaways

### 1. A byte image can be represented as a long array

```text
[0, 0, 12, 50, 128, 255, ...]
```

### 2. The image width determines how the array is wrapped

For width `32`:

```text
32 values per row
```

### 3. Byte values can represent grayscale intensity

```text
0   → Black
128 → Gray
255 → White
```

### 4. Excel can visualize image arrays

Conditional formatting can map numerical values to brightness or color.

### 5. The wrong width produces a distorted image

```text
Correct width → Correct image
Wrong width   → Shifted/distorted image
```

### 6. RGB images require three values per pixel

```text
(R, G, B)
```

### 7. A 32 × 32 RGB image has shape

```text
32 × 32 × 3
```

### 8. Each row of a 32-pixel RGB image contains 96 values

```text
32 × 3 = 96
```

### 9. Every three consecutive values represent one RGB pixel

```text
R G B | R G B | R G B | ...
```

### 10. RGB components visually mix

When the individual RGB components are viewed from farther away, they blend together and appear as the final colors of the image.

---

# 23. Connection to Computer Vision

The practical purpose of these exercises is to understand what happens to an image before higher-level computer vision algorithms operate on it.

The progression is:

```text
Image File
    ↓
Image Array
    ↓
Image Matrix
    ↓
Pixel Representation
    ↓
RGB Channels
    ↓
Image Visualization
    ↓
Image Analysis
    ↓
Machine Learning
    ↓
Neural Networks
    ↓
Advanced Vision Models
```

Understanding this low-level representation is important because later computer vision systems operate on these numerical representations.

The lecture connects these basic concepts to more advanced topics that can follow later, including:

- Neural networks
- Convolutional neural networks
- Transformers
- Vision Transformers
- Multimodal models
- CLIP-style models

---

# Summary

A digital image is ultimately a collection of numerical values.

For a grayscale byte image, each pixel can be represented by one value between:

```text
0 → 255
```

For an RGB image, each pixel contains three values:

```text
R, G, B
```

Therefore, a `32 × 32` RGB image has:

```text
32 × 32 × 3 = 3072 values
```

When reconstructing the image from a flat array, the correct width is essential.

For a `32 × 32` RGB image:

```text
32 pixels per row
×
3 values per pixel
=
96 values per row
```

Excel's `WRAPROWS` function can be used to perform this restructuring automatically.

Finally, the RGB values can be visualized using cell formatting to simulate the behavior of an LED display. When the individual RGB components are viewed from farther away, they visually blend together and produce the colors we perceive in the final image.
