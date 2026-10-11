import numpy as np
import matplotlib.pyplot as plt

width, height = 900, 600

# Give each pixel a complex-number coordinate
x = np.linspace(-2.2, 0.8, width)
y = np.linspace(-1.2, 1.2, height)
X, Y = np.meshgrid(x, y)
C = X + 1j * Y

Z = np.zeros_like(C)
escape_steps = np.zeros(C.shape, dtype=int)
still_bounded = np.ones(C.shape, dtype=bool)

max_iterations = 100

for step in range(max_iterations):
    Z[still_bounded] = Z[still_bounded] ** 2 + C[still_bounded]
    escaped_now = np.abs(Z) > 2
    newly_escaped = escaped_now & still_bounded
    escape_steps[newly_escaped] = step
    still_bounded[newly_escaped] = False

plt.figure(figsize=(10, 7))
plt.imshow(
    escape_steps,
    extent=[x.min(), x.max(), y.min(), y.max()],
    origin="lower",
    cmap="magma"
)
plt.xlabel("Real part")
plt.ylabel("Imaginary part")
plt.title("The Mandelbrot Set")
plt.show()

escape_steps = np.full(C.shape, max_iterations, dtype=int)
escape_steps[newly_escaped] = step
escape_steps[newly_escaped] = step + 1
