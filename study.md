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
Can manipulate both representations independently to produce new, perceptually meaningful images.   
Generate images that mix the content and style representation from two different source images.   
2개의 이미지를 이용해 content와 style representation을 섞어 이미지를 생산한다.   
Global arrangement of the original photograph is preserved, the colours and local structures that compose the global scenery are provied by the artwork.   
Original photograph의 전체적인 배열이 유지되고, 전체적인 배경을 구성하는 색상과 local structures는 작품에 의해 제공된다.   

