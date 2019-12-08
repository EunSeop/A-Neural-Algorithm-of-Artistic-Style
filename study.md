https://arxiv.org/abs/1508.06576

Separate and recombine content and style.
Use CNN

## Obtain Content
Input image is transformed to representations that increasingly care about the actual content of the image.
Can directly visualize information each layer.(by reconstructing the image only from the feature maps in that layer)
Higher layers capture high-level content.
Lower layer reproduce exact pixel value of the original image
 -> Use higher layers as a Content representation


## Obtain Style
Use feature space originally designed to capture texture information.
This feature space is built on top of the filter responses in each layer in network.
Feature correlation - correlation between the different filter response over the spatial extent of the feature map.
 -> Including Feature Correlation of multiple layers


## Reconstructing image from style feature(Style representation)
Capture its general appearance in terms of colour and localised structures.
Size and complexity of local image increases along the hierarchy.(multi-scale representation)