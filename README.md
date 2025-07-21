# Crop Sprayer Drone Dataset

This dataset contains 200+ images of DJI Agras crop sprayer drones taken by private drone operators throughout South America and the Caribbean. The images are classified by generation and drone model, as shown in the table below.

| Gen | Model          | Arms | Rotors | Nozzle Type         | Nozzles | Images |
|-----|----------------|------|--------|---------------------|---------|--------|
| 02  | DJI Agras T16  | 6    | 6      | Pressure (Flat fan) | 8       | 15     |
| 02  | DJI Agras T20  | 6    | 6      | Pressure (Flat fan) | 8       | 44     |
| 03  | DJI Agras T10  | 4    | 4      | Pressure (Flat fan) | 4       | 1      |
| 03  | DJI Agras T30  | 6    | 6      | Pressure (Flat fan) | 16      | 75     |
| 04  | DJI Agras T20P | 4    | 4      | Centrifugal         | 2       | 8      |
| 04  | DJI Agras T40  | 4    | 8      | Centrifugal         | 2       | 67     |
| 05  | DJI Agras T50  | 4    | 8      | Centrifugal         | 2/4     | 17     |

A couple of technical notes:
* The tank size in liters is given in the model name after the letter T, e.g., the T16 has a 16-liter tank, the T30 has a 30-liter tank, and so on. An exception to this rule is the T50, which has a standard tank size of 40 liters and the option to install a 50-liter tank.
* Each rotor is equipped with two propeller blades. Hence, the total number of propeller blades on a drone is twice the number of rotors.

## Purpose

This dataset is obviously too small to train models from scratch, but it is ideal to test fine-tuning methods or few-shot learning methods. Here are a few ideas:
* Combine this dataset with one containing camera drones, i.e., small drones used for photography and videography (e.g., DJI Phantom, Mavic, Inspire, Matrice; Autel EVO; etc.). Fine-tune a model to distinguish crop sprayer drones from camera drones.
* Fine-tune a model to classify drones by nozzle type: flat fan pressure nozzles (T16/T20/T10/T30) vs. centrifugal nozzles (T20P/T40/T50).
* Fine-tune a model to classify by number of arms: 6-arm models (T16/T20/T30) vs. 4-arm models (T10/T20P/T40/T50).

## Data Provenance, Anonymization and ICC Profiles

The majority of the images in this dataset come from WhatsApp group chat conversations and were taken with various smartphone cameras. A small number of images were taken by me using my own smartphone when I worked as a crop spraying services provider.

To ensure anonymization, all faces and identifying information (e.g., logos, truck license plates) were blurred using Gaussian kernels.

Additionally, during metadata cleaning, all Exif metadata (including ICC color profiles) was removed. However, all images were originally captured in sRGB or close-to-sRGB color spaces. As a result, standard image viewers (e.g., Ubuntu's default viewer) render them without visible changes. You can safely assume sRGB when loading the images.

If you are using Python libraries such as PIL, PyTorch, or Keras, you can ensure consistent color handling by explicitly converting images to RGB mode and treating pixel values as standard 0–255 sRGB values.

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

## Contact

For suggestions, questions, or feedback, you can reach me at `luis.i.reyes.castro@gmail.com`. In case you download this dataset from Kaggle, you can find the original repository [here](https://github.com/luis-i-reyes-castro/crop-sprayer-drone).

