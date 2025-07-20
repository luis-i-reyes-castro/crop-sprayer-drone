# Crop Sprayer Drone Dataset

This dataset contains 200+ images of DJI Agras crop sprayer drones taken by private drone operators throughout South America and the Caribbean. The images are classified by generation and drone model as shown in the table below.

| Generation | Model          | Arms | Rotors | Nozzle Type               | Num of Nozzles |
|------------|----------------|------|--------|---------------------------|----------------|
| 02         | DJI Agras T16  | 6    | 6      | Flat Fan Pressure Nozzles | 8              |
| 02         | DJI Agras T20  | 6    | 6      | Flat Fan Pressure Nozzles | 8              |
| 03         | DJI Agras T10  | 4    | 4      | Flat Fan Pressure Nozzles | 4              |
| 03         | DJI Agras T30  | 6    | 6      | Flat Fan Pressure Nozzles | 16             |
| 04         | DJI Agras T20P | 4    | 4      | Centrifugal Nozzles       | 2              |
| 04         | DJI Agras T40  | 4    | 8      | Centrifugal Nozzles       | 2              |
| 05         | DJI Agras T50  | 4    | 8      | Centrifugal Nozzles       | 2/4            |

## Anonymization and ICC Profiles

This dataset originally contained images taken from various smartphone cameras. To ensure anonymization all faces and identifying information (logos, truck license plates, etc.) have been blurred using Gaussian kernels. Furthermore, during the metadata cleaning process, all Exif metadata, including ICC color profiles, was deleted. Nevertheless, all images were originally captured in sRGB or close-to-sRGB color spaces, so standard image viewers (e.g., Ubuntu's default viewer) rendered them without visible changes after profile deletion, and you can safely assume sRGB when loading the images. If using Python libraries such as Keras, PyTorch, or PIL, you can ensure consistent color handling by explicitly converting images to RGB mode and treating pixel values as standard 0–255 sRGB values.

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
