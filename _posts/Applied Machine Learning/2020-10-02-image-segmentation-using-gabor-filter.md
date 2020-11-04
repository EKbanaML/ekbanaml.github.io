---
title: "Image Texture Segmentation Using Gabor Filter"
excerpt_separator: "<!--more-->"
last_modified_at: 2020-10-02T16:20:02-05:00
categories:
  - Applied Machine Learning
tags:
  - Texture Segmentation
  - Gabor Filter
author: ashuta
---

**Gabor Filters** are known best to depict the mammalian multi-channel approach of vision for interpreting and segmenting textures. This article shows an implementation of gabor filters in python using sklearn and other libraries to  segment textures within an image. We will take an example of a slab image and segment the textures within the image using gabor filter. Our implementation shows three steps: pre-processing, gabor filter and post-processing.
<p>Now lets begin with the required imports.</p>

```
from PIL import Image, ImageEnhance
import numpy as np
import image_slicer
from PIL import ImageFilter
import colorsys
from skimage.filters import gabor, gaussian
from IPython.display import display
from matplotlib.pyplot import imshow
from pywt import dwt2
import pickle
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler
%matplotlib inline
```

First, we will get the example image and display it.


```python
image1 = 'slab.jpg'
image = Image.open(image1).convert('RGB')
image_size = image.size
print(image_size)
display(image)
```

    (550, 550)



![Output Image]({{ site.url }}{{ site.baseurl }}/assets/images/gabor_filter/index.png)


## Pre-processing

Enhance the color and brightness of the image. Then convert the image to gray scale.


```
converter = ImageEnhance.Color(image)
image = converter.enhance(0.5) #enchance color by a factor of 0.5
image = enhance_brightness(image)
# convert to grayscale
image = image.convert('L')
display(image)


def enhance_brightness(image):
    """
    :param image: unenhanced image
    :return: Image with enhanced brightness. The new brightness is within the range of [0.3, 1] if the original
    brightness is greater than 0.1 else it is set to 0.1
    """
    mean_brightness = get_brightness(image)
    a, b = [0.3, 1]
    if mean_brightness<0.1:
        a = 0.1
    min_, max_ = [0, 1]
    new_brightness = (b - a) * (mean_brightness - min_) / (max_ - min_) + a

    brightness_factor = new_brightness/mean_brightness
    enhancer = ImageEnhance.Brightness(image)
    enhanced_image = enhancer.enhance(brightness_factor)
    return enhanced_image


def get_brightness(image):
    """
    :param image: unenhanced image
    :return: mean brightness of the image
    """
    brightness = []
    pixel_values = list(image.getdata())
    for values in pixel_values:
        R, G, B = values
        bright = np.sqrt(0.299 * R ** 2 + 0.587 * G ** 2 + 0.114 * B ** 2) / 255
        brightness.append(bright)

    return np.mean(brightness)
```

![Output Image]({{ site.url }}{{ site.baseurl }}/assets/images/gabor_filter/output_6_0.png)

## Applying Gabor Filter

While applying gabor filters, we have to consider the following three parameters:
<ol>
<li>bandwidth</li>
<li>frequency</li>
<li>orientation</li>
</ol>

**Bandwidth**:
Narrower bandwidth is desirable because they make finer distinction in texture. In this implementation, we have determined the value of bandwidth using energy density of image. The higher the energy density, the lesser the bandwidth so that more textured areas do not overpower less textured areas.


```
pixels = np.asarray(image, dtype="int32")
energy_density = get_energy_density(pixels)
# get fixed bandwidth using energy density
bandwidth = abs(0.4*energy_density - 0.5)

def get_image_energy(pixels):
    """
    :param pixels: image array
    :return: Energy content of the image
    """
    _, (cH, cV, cD) = dwt2(pixels.T, 'db1')
    energy = (cH ** 2 + cV ** 2 + cD ** 2).sum() / pixels.size
    return energy


def get_energy_density(pixels):
    """
    :param pixels: image array
    :param size: size of the image
    :return: Energy density of the image based on its size
    """
    energy = get_image_energy(pixels)
    energy_density = energy / (pixels.shape[0]*pixels.shape[1])
    return round(energy_density*100,5)
```

**Theta**:
Theta represents the orientation of the gabor filter. In this implementation, we have used six orientations from 0 degree to 150 degree with a seperation of 30 degree each. Instead of this, we can also take an orientation seperation of 45 degree but, this would not produce as good results.

**Frequency**:
According to this, for an orientation seperation of 30 degree, the total number of gabor filters required is: `6log2(Nc/2)` where, `Nc` is the width of the image. The frequencies that should be used are:


1. $$\sqrt{2}$$ ,  2$$\sqrt{2}$$ , 4$$\sqrt{2}$$  ...  Nc/4$$\sqrt{2} $$


However, in case of our image, we have experimentally chosen the following set of frequencies:

1$$\sqrt{2}$$  , 1+$$\sqrt{2}$$  , 2+$$\sqrt{2}$$  , 2$$\sqrt{2}$$

You can visualize the result of each gabor filter to decide for yourself.

Hence we have a total of 24 gabor filters.

The gabor filter gives a complex response. So we take the magnitude of the response for further processing.


```
magnitude_dict = {}
for theta in np.arange(0, np.pi, np.pi / 6):
    for freq in np.array([1.4142135623730951, 2.414213562373095, 2.8284271247461903, 3.414213562373095]):
        filt_real, filt_imag = gabor(image, frequency=freq, bandwidth=bandwidth, theta=theta)
        # get magnitude response
        magnitude = get_magnitude([filt_real, filt_imag])
        ''' uncomment the lines below to visualize each magnitude response '''
        # im = Image.fromarray(magnitude.reshape(image_size)).convert('L')
        # display(im)
        magnitude_dict[(theta, freq)] = magnitude.reshape(image.size)


def get_magnitude(response):
    """
    :param response: original gabor response in the form: [real_part, imag_part]
    :return: the magnitude response for the input gabor response
    """
    magnitude = np.array([np.sqrt(response[0][i][j]**2+response[1][i][j]**2)
                        for i in range(len(response[0])) for j in range(len(response[0][i]))])
    return magnitude
```

## Post-processing

Now we have obtained the magnitude of 24 gabor filters. The next step is to smooth and reduce the magnitude array. We can apply gaussian smoothing to each of the magnitude response and then apply PCA to reduce the dimensions.


```python
# apply gaussian smoothing
gabor_mag = []
for key, values in magnitude_dict.items():
    # the value of sigma is chosen to be half of the applied frequency
    sigma = 0.5*key[1]
    smoothed = gaussian(values, sigma = sigma)
    gabor_mag.append(smoothed)
gabor_mag = np.array(gabor_mag)

# reshape so that we can apply PCA
value = gabor_mag.reshape((-1, image_size[0]*image_size[1]))

# get dimensionally reduced image
pcaed = apply_pca(value.T).astype(np.uint8)
result = pcaed.reshape((image_size[0], image_size[1]))
result_im = Image.fromarray(result, mode='L')

def apply_pca(array):
    """
    :param array: array of shape pXd
    :return: reduced and transformed array of shape dX1
    """
    # apply dimensionality reduction to the input array
    standardized_data = StandardScaler().fit_transform(array)
    pca = PCA(n_components=1)
    pca.fit(standardized_data)
    transformed_data = pca.transform(standardized_data)
    return transformed_data
```

And here's the result


```python
display(result_im)
```


![Output Image]({{ site.url }}{{ site.baseurl }}/assets/images/gabor_filter/output_17_0.png)



You can further experiment with orientation, bandwidth and frequency values to get more suitable results.


## References
[1] [https://www.mathworks.com/help/images/texture-segmentation-using-gabor-filters.html](https://www.mathworks.com/help/images/texture-segmentation-using-gabor-filters.html)

[2] [https://pdfs.semanticscholar.org/a53b/78ff23daf515a344d47f4848e1f2528b3074.pdf](https://pdfs.semanticscholar.org/a53b/78ff23daf515a344d47f4848e1f2528b3074.pdf)
