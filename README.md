# crop-sprayer-drone

## DJI Agras Crop Sprayer Drone Dataset

This dataset contains 200+ images to DJI Agras T16 & T20, T10 & T30, T20P & T40, and T50 crop sprayer drones, taken by private drone operators throughout South America and the Caribean. The images are classified by generation and drone model.

---

## 🖼️ Image Color and ICC Profiles

This dataset originally contained images from various smartphone cameras. To ensure anonymization and remove sensitive metadata, all Exif metadata — including ICC color profiles — has been stripped. All images were originally captured in sRGB or close-to-sRGB color spaces, so standard image viewers (e.g., Ubuntu's default viewer) rendered them without visible changes after profile deletion, and you can safely assume sRGB when loading the images. If using Python libraries such as Keras, PyTorch, or PIL, you can ensure consistent color handling by explicitly converting images to RGB mode and treating pixel values as standard 0–255 sRGB values.

### Examples of Safe Image Loading

Using PIL (standalone)
```python
from PIL import Image
img = Image.open("path/to/image.jpg").convert("RGB")  # Force sRGB interpretation
img_array = np.array(img) / 255.0  # Normalize if needed
```

Using PyTorch with torchvision
```python
from torchvision import transforms
from PIL import Image

transform = transforms.Compose([
    transforms.ConvertImageDtype(torch.float),
    transforms.ToTensor(),  # Converts to [0, 1] and permutes (H, W, C) to (C, H, W)
])

img = Image.open("path/to/image.jpg").convert("RGB")
tensor = transform(img)
```

Using Keras
```python
from tensorflow.keras.preprocessing.image import load_img, img_to_array

# Load image in RGB mode (do not resize unless required)
img = load_img("path/to/image.jpg", color_mode='rgb')
img_array = img_to_array(img) / 255.0  # Normalize if required by your model
```

