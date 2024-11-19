# UTS-PENGOLAHAN-CITRA-DIGITAL
Nama : Syahrul Alvian Chusnan
NIM :23422044

from PIL import Image
import numpy as np

# Load the image
image = Image.open('gambar.jpg')  # Replace 'input.jpg' with your image path
image = image.convert('RGB')  # Ensure it's in RGB format
width, height = image.size

# Convert image to grayscale
gray_image = image.convert('L')
gray_pixels = np.array(gray_image)

# Compute histogram
histogram = np.zeros(256, dtype=int)
for pixel_value in gray_pixels.flatten():
    histogram[pixel_value] += 1

# Compute cumulative distribution function (CDF)
cdf = np.cumsum(histogram)
cdf_normalized = (cdf - cdf.min()) * 255 / (cdf.max() - cdf.min())
cdf_normalized = cdf_normalized.astype('uint8')

# Apply the equalization
equalized_pixels = cdf_normalized[gray_pixels]

# Convert back to an image
equalized_image = Image.fromarray(equalized_pixels)

# Save and display results
equalized_image.save('output.jpg')  # Replace 'output.jpg' with your output file name
equalized_image.show()

# Print the original and equalized histograms
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 5))

# Original Histogram
plt.subplot(1, 2, 1)
plt.title("Original Histogram")
plt.hist(gray_pixels.flatten(), bins=256, range=(0, 255), color='gray')
plt.xlabel("Pixel Intensity")
plt.ylabel("Frequency")

# Equalized Histogram
plt.subplot(1, 2, 2)
plt.title("Equalized Histogram")
plt.hist(equalized_pixels.flatten(), bins=256, range=(0, 255), color='gray')
plt.xlabel("Pixel Intensity")
plt.ylabel("Frequency")

plt.tight_layout()
plt.show()
