# Digital Image Representation in PIL and NumPy

Images are commonly represented in terms of RGB channels and tensors (a stack of matrices). Each matrix in the image tensor corresponds to a specific channel of the image.

- **RGB Image**: Represented as a stack of three matrices (one for Red, Green, and Blue channels).
- **Grayscale Image**: Represented as a single matrix, as there is only one intensity channel.

When loading images from formats like JPEG or PNG, the intensity of each channel typically ranges from 0 to 255. This intensity is stored using an unsigned 8-bit integer (uint8) data type for efficiency.

## Differences between PIL and NumPy Image Representations

- **PIL**:

  - The image is described using width (w), height (h), and the image mode (such as RGB, L, etc.).
- **NumPy**:

  - The image is represented in the form of a 3D array: height (h), width (w), and channels (c). For example, an RGB image would have a shape of (h, w, 3).

## PIL (Pillow)

PIL (or Pillow, its modern fork) is a library used for essential image file operations:

- Opening an image: `Image.open()`
- Saving an image: `pil_image.save()`
- Converting between formats/modes (e.g., RGB to grayscale).
- Inspecting image metadata (mode, size).
- Basic image manipulations, like resizing.
- Converting between PIL Images and NumPy arrays:

  - `Image.fromarray()` converts a NumPy array into a PIL Image.
  - `np.array()` converts a PIL Image to a NumPy array.
