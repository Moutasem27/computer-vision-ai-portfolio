# Image Data Representation

## Overview

In computer vision, images are ultimately represented as numerical data that a computer can store and process.

This lecture covers several common image representations:

- Image arrays
- Image matrices
- Binary images
- Byte arrays
- Byte image matrices
- Double arrays
- Double image matrices
- Normalized double arrays
- RGB color values
- RGB image arrays
- LED matrix representation
- How image data can eventually be processed by algorithms and neural networks

> **An image is represented by numbers, and the arrangement and meaning of those numbers determines how the image is interpreted.**

---

# 1. Image Arrays

In computer vision, an image is typically loaded from a file into the computer's memory as an **array of numbers**.

For example, consider a very small image containing six values:

```text
[4, 3, 2, 5, 6, 7]
```

At this stage, this is simply an array of numbers. We still need to tell the computer how these values should be arranged into an image.

For a simple grayscale image, the image can be represented as a **2D matrix**.

---

# 2. Converting an Image Array into an Image Matrix

To convert a 1D image array into a 2D image matrix, we need to know the **width of the image**.

For example:

```text
Image array:

[4, 3, 2, 5, 6, 7]
```

If the image width is `2`, we place two values in each row:

```text
4  3
2  5
6  7
```

Therefore:

```text
Image width = 2
Image height = 3
```

The resulting image matrix is:

```text
[ [4, 3],
  [2, 5],
  [6, 7] ]
```

### Why is the width important?

The computer needs to know when one row ends and the next row begins.

Conceptually:

```text
Take values from the array
        ↓
Place width number of values in each row
        ↓
Start a new row
        ↓
Continue until all values are used
```

## Example: Width = 3

Using the same array:

```text
[4, 3, 2, 5, 6, 7]
```

If the width is `3`:

```text
4  3  2
5  6  7
```

Therefore:

```text
Image width  = 3
Image height = 2
```

The same array can produce different image dimensions depending on the known width.

---

# 3. Binary Images

A **binary image** uses only two possible values:

```text
0
1
```

Binary images are often produced by applying a **condition, threshold, or filter** to an image.

For example, consider:

```text
4  3
2  5
6  7
```

Suppose we want to keep only values that are:

```text
> 3
```

We evaluate every pixel:

| Value | Greater than 3? | Binary value |
|------:|:---------------:|-------------:|
| 4 | Yes | 1 |
| 3 | No | 0 |
| 2 | No | 0 |
| 5 | Yes | 1 |
| 6 | Yes | 1 |
| 7 | Yes | 1 |

The resulting binary image is:

```text
1  0
0  1
1  1
```

### General idea

A binary image can be generated using a condition:

```text
if pixel > threshold:
    pixel = 1
else:
    pixel = 0
```

Binary images are useful for separating objects or regions from their background.

---

# 4. Byte Arrays

Instead of using arbitrary numerical values, we can specify the **data type** of the array.

A common type is a **byte**.

## What is a byte?

A byte contains:

```text
8 bits
```

An unsigned byte can represent:

```text
0 → 255
```

This comes from:

```text
2^8 = 256
```

Since the values start at `0`:

```text
0 → 255
```

There are 256 possible values.

### Byte range

```text
Minimum = 0
Maximum = 255
```

Therefore, a byte image can contain values such as:

```text
0
42
128
200
255
```

but cannot contain:

```text
-1
300
```

when using an unsigned byte representation.

---

# 5. Byte Image Matrix

A byte array can be rearranged into an image matrix using the image width.

For example, suppose we have:

```text
[255, 0, 128, 42, 0, 30]
```

If the image width is `3`:

```text
255   0   128
42    0    30
```

Therefore:

```text
Byte image matrix:

[ [255, 0, 128],
  [42,  0, 30] ]
```

Every value represents the intensity of one pixel.

---

# 6. Double Arrays

Another common numerical representation is a **double** (double-precision floating-point number).

Unlike a byte, a double can represent:

- Positive numbers
- Negative numbers
- Decimal values
- Very small numbers
- Very large numbers

A simplified example of a double array could contain:

```text
[-0.05, -0.3, 4305, -3.4, 0, 3.2]
```

The important difference is that doubles are not restricted to:

```text
0 → 255
```

like an unsigned byte.

They can represent values across a much larger numerical range.

---

# 7. Double Image Matrix

Just like with a byte array, a double array can be rearranged into a 2D image matrix.

For example:

```text
[-0.05, -0.3, 4305, -3.4, 0, 3.2]
```

With an image width of `3`:

```text
-0.05   -0.3   4305
-3.4     0      3.2
```

The important concept is that the **same array-to-matrix conversion process** applies regardless of whether the values are bytes, integers, or doubles.

---

# 8. Normalized Double Arrays

Sometimes using the full range of a double is unnecessary.

Instead, we can **normalize** values so that they fall within:

```text
0 → 1
```

A normalized double array may look like:

```text
[0, 0.4, 0.3, 0.2, 0.1, 0.3]
```

The key properties are:

```text
Minimum = 0
Maximum = 1
```

There are no negative values in a normalized representation.

## Why normalize?

Normalization is commonly useful when values have a meaningful interpretation between `0` and `1`.

For example, **probability maps** commonly contain values in this range:

```text
0 ≤ probability ≤ 1
```

For example:

```text
0.0 → 0% probability
0.5 → 50% probability
1.0 → 100% probability
```

A normalized array can also be rearranged into a matrix if the image width is known.

---

# 9. RGB Color Representation

A color image requires more information than a grayscale image.

The most common color representation is:

```text
RGB
```

RGB stands for:

- **R** = Red
- **G** = Green
- **B** = Blue

Each pixel contains three values:

```text
(R, G, B)
```

When using bytes, each channel has a range of:

```text
0 → 255
```

---

# 10. Basic RGB Colors

## Red

Red has maximum intensity in the red channel and zero intensity in the other channels:

```text
RGB = (255, 0, 0)
```

## Green

```text
RGB = (0, 255, 0)
```

## Blue

```text
RGB = (0, 0, 255)
```

## White

All three channels have maximum intensity:

```text
RGB = (255, 255, 255)
```

## Black

All three channels have zero intensity:

```text
RGB = (0, 0, 0)
```

---

# 11. Normalized RGB Values

RGB values can also be represented using normalized floating-point values between `0` and `1`.

The byte range:

```text
0 → 255
```

becomes:

```text
0.0 → 1.0
```

### Examples

| Color | Byte RGB | Normalized RGB |
|---|---|---|
| Red | `(255, 0, 0)` | `(1, 0, 0)` |
| Green | `(0, 255, 0)` | `(0, 1, 0)` |
| Blue | `(0, 0, 255)` | `(0, 0, 1)` |
| White | `(255, 255, 255)` | `(1, 1, 1)` |
| Black | `(0, 0, 0)` | `(0, 0, 0)` |

---

# 12. Mixing RGB Channels

By combining different RGB channels, we can produce additional colors.

## Yellow

Yellow is produced by combining maximum red and green:

```text
RGB = (255, 255, 0)
```

Normalized:

```text
(1, 1, 0)
```

## Cyan

Cyan is produced by combining green and blue:

```text
RGB = (0, 255, 255)
```

Normalized:

```text
(0, 1, 1)
```

## Magenta

Magenta is produced by combining red and blue:

```text
RGB = (255, 0, 255)
```

Normalized:

```text
(1, 0, 1)
```

---

# 13. RGB Image Arrays

For a grayscale image, one value can represent one pixel.

For example:

```text
[4, 3, 2, 5]
```

Each number represents one pixel.

However, a color pixel requires **three values**:

```text
(R, G, B)
```

Therefore:

> A color image with `N` pixels requires `3N` values when represented using RGB.

## Example: Four-Pixel RGB Image

Suppose we have four pixels:

```text
Red
Yellow
Cyan
Black
```

Their RGB values are:

```text
Red    = (255, 0, 0)
Yellow = (255, 255, 0)
Cyan   = (0, 255, 255)
Black  = (0, 0, 0)
```

The image array becomes:

```text
[
    255, 0, 0,
    255, 255, 0,
    0, 255, 255,
    0, 0, 0
]
```

There are:

```text
4 pixels × 3 values per pixel = 12 values
```

---

# 14. RGB Channel Ordering

When storing RGB values, we must agree on the order of the channels.

The most common ordering is:

```text
RGB
```

meaning:

```text
Red → Green → Blue
```

For example:

```text
255, 0, 0
```

means red.

However, some libraries use a different order.

For example, **OpenCV commonly uses BGR ordering** rather than RGB.

Therefore, when working with image-processing libraries, it is important to know which channel order the library expects.

### RGB

```text
(R, G, B)
```

### BGR

```text
(B, G, R)
```

Using the wrong channel order can cause colors to appear incorrectly.

---

# 15. RGB Image Matrix

Once the RGB array has been created, the values can be arranged into the image's spatial structure.

For example, if we have four pixels:

```text
Red     Yellow
Cyan    Black
```

The image can be conceptually represented as:

```text
[ Red     Yellow ]
[ Cyan    Black  ]
```

But internally, each pixel contains three channel values.

For example:

```text
Red    = (255, 0, 0)
Yellow = (255, 255, 0)
Cyan   = (0, 255, 255)
Black  = (0, 0, 0)
```

---

# 16. How Screens Display RGB Images

A screen is made from a very large number of tiny pixels.

Each pixel contains three smaller light sources:

```text
Red LED
Green LED
Blue LED
```

These three components can be controlled independently.

By changing their intensity, the screen can produce different colors.

## Example: Red Pixel

```text
Red   = ON
Green = OFF
Blue  = OFF
```

## Example: Yellow Pixel

Yellow is produced by combining red and green:

```text
Red   = ON
Green = ON
Blue  = OFF
```

## Example: Cyan Pixel

Cyan is produced by combining green and blue:

```text
Red   = OFF
Green = ON
Blue  = ON
```

## Example: Black Pixel

For black:

```text
Red   = OFF
Green = OFF
Blue  = OFF
```

---

# 17. LED Matrix Concept

A screen can be thought of as a huge matrix of RGB LED pixels.

For example, a very small image might contain:

```text
┌───────────────┐
│ RED   YELLOW  │
│ CYAN  BLACK   │
└───────────────┘
```

Each visible pixel is actually composed of three smaller light sources.

Modern screens contain millions of these individual pixels.

For example:

```text
1 megapixel ≈ 1,000,000 pixels
```

If every pixel has three RGB components:

```text
1,000,000 × 3
= 3,000,000
```

individual color components are being controlled.

When viewed from a normal distance, these tiny components blend together and appear as a single colored image.

---

# 18. From Image File to Computer Vision

The overall process can be thought of as:

```text
Image File
    ↓
Array of Values
    ↓
Image Matrix
    ↓
RGB Channels (for color images)
    ↓
Image Processing
    ↓
Feature Extraction / Analysis
    ↓
Machine Learning / Neural Network
```

When an image is loaded into a computer vision system, its data is stored in memory as numerical values.

The program then interprets those values according to:

- Image width
- Image height
- Number of channels
- Data type
- Channel ordering
- Pixel values

---

# 19. Image Processing as Matrix Operations

Once the image is represented as a matrix, many image-processing operations can be performed using loops and mathematical operations.

Conceptually, processing an image involves going through:

```text
for each row:
    for each column:
        process the pixel
```

For example:

```python
for y in range(height):
    for x in range(width):
        process(image[y][x])
```

For a color image, the pixel may contain three values:

```text
image[y][x] = (R, G, B)
```

This representation allows algorithms to analyze the image, calculate features, and eventually pass the information into machine-learning or neural-network models.

---

# 20. Key Takeaways

### Image Array

An image can initially be stored as a 1D array of numerical values.

```text
[4, 3, 2, 5, 6, 7]
```

### Image Matrix

The array can be rearranged into rows and columns when the image width is known.

```text
4  3
2  5
6  7
```

### Binary Image

Contains two values, usually:

```text
0 and 1
```

Often produced using a threshold or condition.

### Byte Image

Uses an 8-bit value:

```text
0 → 255
```

### Double Image

Uses floating-point values and can represent positive, negative, very small, and very large numbers.

### Normalized Image

Values are restricted to:

```text
0 → 1
```

This is especially useful for probabilities and probability maps.

### RGB Image

Each pixel contains three values:

```text
(R, G, B)
```

For byte RGB:

```text
0 → 255
```

For normalized RGB:

```text
0 → 1
```

### RGB Channel Ordering

The order matters:

```text
RGB
```

is different from:

```text
BGR
```

Some libraries, such as OpenCV, commonly use BGR.

### LED Matrix

Each screen pixel can be thought of as three independently controlled light sources:

```text
Red + Green + Blue
```

Their intensities are mixed to produce the final visible color.

---

# 21. Important Terms

| Term | Meaning |
|---|---|
| **Image Array** | Numerical array containing image data |
| **Image Matrix** | 2D arrangement of image values |
| **Binary Image** | Image using two values, usually 0 and 1 |
| **Byte** | 8-bit data type |
| **Byte Range** | 0–255 for an unsigned byte |
| **Double** | Floating-point numerical data type |
| **Normalized Value** | Value scaled to a defined range, commonly 0–1 |
| **RGB** | Red, Green, Blue color representation |
| **Channel** | One component of a color image |
| **Pixel** | Smallest individual element of an image |
| **BGR** | Blue, Green, Red channel ordering |
| **LED Matrix** | Conceptual representation of pixels as RGB light components |
| **Threshold** | Condition used to classify pixel values |
| **Probability Map** | Image where pixel values represent probabilities |

---

# Summary

The fundamental idea behind digital images is that **images are data**.

A computer does not directly understand an image as humans do. Instead, it receives numerical values representing pixels.

A simple grayscale image can be represented using one value per pixel:

```text
Pixel → 1 value
```

A color RGB image requires three values per pixel:

```text
Pixel → R + G + B
```

These values can be stored using different numerical representations such as:

```text
Binary → 0 or 1
Byte   → 0 to 255
Double → Large numerical range
Normalized Double → 0 to 1
```

Once the image is represented numerically, computer vision algorithms can manipulate the pixels, extract features, analyze the image, and eventually use the resulting information with machine-learning and neural-network models.
