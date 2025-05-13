# Image Tensors in PyTorch

In PyTorch, tensors are n-dimensional arrays, similar to NumPy arrays but with additional features that make them ideal for deep learning:

- **Accelerated Computation**: PyTorch tensors can be run on GPUs or TPUs, greatly improving execution speed for large-scale computations.

- **Automatic Differentiation**: Unlike NumPy, PyTorch supports automatic gradient computation, which is essential for training neural networks via backpropagation.

## Backpropagation & Computation Graph

- During training, the **derivative of the loss function w\.r.t. model weights** is computed to update the weights — this is how learning happens.
- PyTorch builds a **computation graph** dynamically, tracking all operations (e.g., addition, multiplication, subtraction, etc.) on tensors to enable gradient calculation.

## Image Tensor Dimensions

- PyTorch expects image tensors in the format **(N, C, H, W)**:

  - **N**: Batch size
  - **C**: Number of channels
  - **H**: Image height
  - **W**: Image width
    This is often referred to as the **"channels-first"** format.

## Useful Tensor Operations

- **`ToImage()`**: Converts images from NumPy format (H, W, C) to PyTorch tensor format (C, H, W).
- **`permute()`**: Used to rearrange tensor dimensions — e.g., convert (C, H, W) to (H, W, C) for plotting.
- **`unsqueeze(0)`**: Adds a batch dimension to tensors, converting (C, H, W) to (1, C, H, W).
- **`.cpu()` or `.to('cpu')`**: Moves tensor to the CPU — necessary for visualization since libraries like Matplotlib don't work with GPU tensors.

## Numerical stability

Numerical stability refers to how well an algorithm or computation handles rounding errors, large or small values, and precision limitations without producing incorrect or extreme results (like infinity, NaN, or zero due to underflow).

In deep learning and numerical computing, instability can lead to:

- Exploding or vanishing gradients
- Overflow/underflow in exponential functions
- Loss of precision in floating-point arithmetic
- Training failures or non-convergence

### Common Sources of Instability

1. **Exponentiation (e.g., `e^x`)**
   Large values of `x` (e.g., 255 in image data) lead to overflow: `e^255` → ∞.
   ➜ **Solution**: Normalize inputs (e.g., divide pixel values by 255).

2. **Softmax and Log-Sum-Exp**
   Softmax involves exponentials:

   $$
   \text{softmax}(x_i) = \frac{e^{x_i}}{\sum_j e^{x_j}}
   $$

   Large inputs can cause overflow in `e^x`.
   ➜ **Solution**: Use a numerically stable softmax by subtracting the max value:

   $$
   \text{softmax}(x_i) = \frac{e^{x_i - \max(x)}}{\sum_j e^{x_j - \max(x)}}
   $$

3. **Vanishing/Exploding Gradients**
   During backpropagation, gradients may become too small (→ no learning) or too large (→ unstable updates).
   ➜ **Solution**: Use proper weight initialization, normalization (like BatchNorm), and gradient clipping.

4. **Floating-point Precision Limits**
   Operations on very small or very large numbers can lead to rounding errors.
   ➜ **Solution**: Use appropriate data types (`float32` or `float64` when necessary).

### How to Improve Numerical Stability

- Normalize inputs (common for image data: `x / 255.0`)
- Use stable formulations of functions (e.g., log-sum-exp trick)
- Clip gradients to prevent exploding gradients
- Choose appropriate activation functions (e.g., ReLU avoids vanishing gradient better than sigmoid/tanh)
- Track loss and gradient values during training to catch instabilities early

### Numerical Stability & Normalization

- **Normalization** helps avoid issues like overflow or infinite values during computation.

  - Example: computing `e^255` gives infinity.
  - Normalize pixel values by dividing by 255 → `[255, 204, 170] → [1.0, 0.8, 0.67]`.

```python
# We convert the input to float and rescale it to
# 0,1 to avoid overflow and get stable operations
transform = transforms.Compose([
transforms.ToImage(),
transforms.ToDType(torch.float32, scale=True)])
# This transform would also work with a NumPy array
image_tensor = transform(pil_image)
# Prints (1.0, 0.0)
print(image_tensor.max(), image_tensor.min())
```

## Gradient Tracking

- When creating tensors for learning, set **`requires_grad=True`** to enable PyTorch to track operations and compute gradients automatically.

PyTorch's architecture, especially in modules like `nn.Conv2d`, relies on this tensor structure and gradient tracking for building and training deep learning models.
