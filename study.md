---
Preview in vscode: Ctrl+Shift+v
---
# Summary style transfer paper

[paper](https://arxiv.org/abs/1508.06576)  
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

Multi-scale representation  
Capture its general appearance in terms of colour and localised structures.  
Size and complexity of local image increases along the hierarchy.(multi-scale representation)  
Can manipulate both representations independently to produce new, perceptually meaningful images.  
Generate images that mix the content and style representation from two different source images.  
2개의 이미지를 이용해 content와 style representation을 섞어 이미지를 생산한다.  
Global arrangement of the original photograph is preserved, the colours and local structures that compose the global scenery are provied by the artwork.  
Original photograph의 전체적인 배열이 유지되고, 전체적인 배경을 구성하는 색상과 local structures는 작품에 의해 제공된다.  

## 효과 정도

Style representation은 Multi-scale representation으로 Multiple-layer로 구성되어있다.  
lower layers를 적용하면 style이 locally 적용되어 다른 사진처럼 보이게 된다.?(잘 적용이 안된걸 이야기 하는듯)  
Higher layers 적용하면 local image structures가 점점 더 넓게 적용되어 부드럽고 continuous한 시각효과를 나타낸다. 그래서 Highest layers에 적용했을때가 제일 괜찮아보인다.  
Contents와 Style 완전히 분리하는건 불가능하다. 두 이미지 합성할 때 완벽하게 될 순 없다.  
그래도 이미지 합성할 때 사용한 loss function보면 잘 분리가 됐다.(완벽하게는 못했지만 그래도 어느정도 성과가 나왔다는 말인 듯)  
Style과 Content의 정도를 조절 할 수도 있다.  
Style에 강조를 두면 예술작품 효과를 잘 적용하지만 사진내용은 보기 힘들다.  
Content에 강조를 두면 사진내용은 잘 보이지만 예술효과는 잘 적용이 안된다.  
그래서 각 이미지 짝별로 trade-off를 정해서 시각효과를 조절 할 수 있다.

## 과거의 Style transfer

이전의 style transfer 작업들은 손글씨나 얼굴인식에 비해 덜 복잡한 작업처럼 다뤄졌다.  
이는 non-parametric technique을 바로 이미지 pixel representation에 적용해서 그런 것이다.  
기존 비슷한 작업을 computer vision분야에서는 photorealistic rendering이라고 하는데 여기서는 style transfer 하기 위해 texture transfer를 사용한다.  
그래서 이 논문에서는 Deep Neural Network를 object recognition에 학습해서 더 고품질의 이미지를 만들어낸다.  

## 과거로 부터 배운 것

Deep Neural Network가 object recognition에 사용되는건 예전 연구중 예술작품이 언제 만들어졌는지 구분하는 style recognition분야에서 쓰였었다.  
거기에선 content representation이라고 하는 network activation 위에 학습되는 방식을 사용했다.  
그래서 우린 stationary feature space를 만들어서 하는게 style classification에서 더 좋은 성능을 낼거라 생각했다. 이것이 우리가 말했던 style representation이다.  

## Methods(구현 상세 설명)

지금까지 설명한 모형은 VGG-Network와 CNN이라는 인간수준의 object detection성능을 가진 모형을 기반으로 만들어졌다.  
16 convolution, 5 pooling layers of the 19layer VGG-Network을 사용했다.  
Fully connected layers를 사용하지 않았다.  
이미지 합성 분야에서 max-pooling 작업을 average pooling 으로 대체하면 grdient flow를 improve하고 appealing results를 약간 더 얻을 수 있다.  
네트워크 안 각 레이어는 non-linear filter bank로 레이어 포지션에 따라 복잡성이 올라가는 형식이다.  
주어진 입력 데이터 x가 cnn을 통해 각 레이어의 filter response로 encoded 되기 때문이다.  
각 다른 레이어에서 encoded된 이미지 정보를 시각화 하기위해서 우리는 gradient descent 를 오리지널 이미지의 feature response와 매치되는 다른 이미지를 찾기 위해 white noise image에 적용했다.  
p는 original image, x는 generated image, Pl과 Fl은 각각 레이어l에서의 feature representation일때, 두 feature representations의 squared-error loss는 아래와 같다.  
![eq1](img/eq1.PNG)  
The derivative of this loss with respect to the activations in layer l equals  
![eq2](img/eq2.PNG)  
이미지 x에 대한 기울기는 standard error backpropagation으로 계산할 수 있다.  
그래서 우리는 initially random image x를 바꿀수있다. cnn 특정 레이어에서 original image p와 동일한 response를 만들때까지.  
각 레이어 별 CNN response의 top에 style representation을 만들었다. 이건 다른 filter response의 correlation을 계산하는것이다. 이는 input image의 공간확장을 바꿔줄거라 기대하는것이다?  
Feature correlation은 레이어 l에서 vectorized feature map i,j의 inner product  
![아직사진안올림](img/eq3.PNG)\
주어진 이미지의 스타일에 맞는 texture 생성하기위해 white noise에 gradient decent를 사용했다. 이는 original image의 style representation에 맞는 이미지를 찾기위해서 사용했다.\
이건 original image의 Gram matrix와 생성될 이미지의 Gram matrix의 mean-squared distance를 최소화 하는 방식으로 진행되었다.\
