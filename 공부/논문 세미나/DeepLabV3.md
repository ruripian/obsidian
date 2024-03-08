안녕하세요 이번에 DeepLabv3 플러스에 대해 발표하게 된
소프트웨어 대학원 컴퓨터공학과 한수호입니다
- next
먼저 모델에 대한 간단한 소개를 하려고 하는데요

- next
DeepLabv3는 semantic segmentation을 하기 위한 모델입니다.
semantic segmentation은 화면에 보이는 것처럼 객체의 정보와 그 위치를 판별하기 위한 방법인데요

- next
semantic segmentation 성능 개선을 하기 위해서 Deeplabv3 플러스 에서는
Spatial pyramid pooling과 encoder-decoder구조의 장점을 합쳐서 이용했다고 하는데
그 부분에 대해서 이야기 해보려고 합니다

- next
관련 연구로는 세가지를 가져왔습니다

- next
첫번째는 Atrous Spatial Pyramid Pooling 입니다
이걸 사용하는 이유로는 input 이미지에 대해서
멀티 스케일 정보를 얻기 위해 사용한다고 합니다.
먼저 위 그림에 있는 Atrous Convolution 대해 이야기를 해보면
convolution layer에 dilation rate를 추가한 방법인데요
왼쪽 그림의 경우 dilation rate=1일때는 그대로 3 by 3 연산을 하고
dilation rate=2일때는 파라미터 자체는 원래 있던 9개만 사용하지만
5 by 5 kernal이 연산하는 영역과 동일한 범위를 연산할 수 있습니다.
비어있는 공간에 데이터는 단순히 0이 들어가게 됩니다.

이를 이용한 Atrous spatial pyramid pooling은 각기 다른 dilation rate를 가진
atrous pooling layer를 중첩하여 multi scale에 더 잘 반응할 수 있도록 한 방법입니다
그래서 그림 오른쪽을 보시면 각기 다른 rate를 가진 레이어들을 동시에 연산을 하고 concat해서 사용하는것을 볼 수 있습니다.

- next
두번째는 인코더-디코더입니다

기존에는 biliner upsampling을 해서 prediction을 얻었는데 

논문에서는 encoder-decoder 구조를 사용하기 때문에
위에서 추출한 정보를 가지고 segmentation을 진행할 때,
물체의 경계가 모호하지 않도록 공간적인 정보를 최대한 유지할 수 있다고 합니다.

논문에서는 encoder의 output값인 feature map이
encoding할때의 convolution으로 인해서
물체 경계와 관련된 정보들을 잃게 된다고 합니다.
논문에서는 이런 정보를 feature map을 더 정교하게 만들면 되지 않나라고 생각을 했었는데
GPU 메모리가 한정되어 있기 때문에
feature map의 크기는 input image의 크기보다 8배이상 더 작아야 되는 제약사항이 있고

그래서 이 논문에서는 피쳐맵을 더 정교하게 만드는게 아닌 인코더 디코더 구조를 사용했고DeepLabv3를 인코더 모듈로 사용하고
새로운 간단한 디코더 모듈을 추가해서 더 성능을 높일 수 있었다고 합니다

- next
마지막으로 depthwise separable convolution 입니다
논문에서는 depthwise separable convolution을 활용하기 때문에
정확도와 속도 사이의 trade-off를 해결할 수 있다고 합니다

본 논문에서는 semantic segmentation에서 사용하기에 적합하고
속도와 정확도 두가지 측면에서 모두 이점을 가지도록
기존에 있었던 Xception model에 depthwise separable convolution을 적용했다고 합니다.

- next
그래서 위의 방법들을 어떻게 적용하였는지에 대해 말해보려고 합니다

- next
첫번째로  Depthwise separable convolution 입니다

Depthwise separable convolution은 제일 왼쪽 위에 있는 depthwise convolution과
pointwise convolution의 연속된 구조로 구성이 되어있고, 이것을 사용하게 되면 
연산량을 굉장히 줄일 수 있다고 합니다

특히, depthwise convolution은 각각 channel에 대해 [[spatial correlation]]에 대한 연산을 수행하고pointwise convolution은 depthwise convolution의 결과를 합치는 channel correlation에 대한 연산을 수행합니다

이 연산에 대해 보면
아래에 있는 왼쪽 그림은 일반적인 convolution 연산이고
오른쪽은 depth wise convoultion 연산입니다
두개의 차이를 보면 Depth-wise Convolution은 한 번 통과하면 하나로 병합되지 않고
(R, G, B)가 각각 Feature Map이 됩니다.

- next
그래서 그 각각의 Feature Map에 대해서 pointwise convoulution을 하게 되는데
이런 방법을 사용하면
기존의 convoulution 연산의 연산량은 c곱하기 k제곱 곱하기 m이지만
이 방법은 c곱하기 k제곱 더하기 m으로
output 채널수의 영향을 상대적으로 덜 받게되어서 연산량이 줄어들게 됩니다

그래서 논문에서는 이를 사용하여 연산 복잡도를 감소시켰다고 합니다.

- next
두번째로 DeepLab v3 as encoder 입니다
DeepLab v3는 다음과 같은 구조를 가지고 있습니다
atrous convolution을 이용해서 임의의 해상도 feature들을 추출하는 구조를 가지고 있습니다.
여기서 나온 output 피쳐를 가지고 디코더의 입력값으로 넣게 됩니다

세번째로 Proposed decoder입니다
다음 그림에서 위에 있는 인코더 부분이 방금 보여드렸던 deeplabv3 고요 

DeepLabv3에서는 encoder feature들을 단순히 16배 bilinear upsample을 수행하는데
이런 방법은 object segmentation details를 복원하지 못한다고 합니다.

그래서 논문에서는 다음 그림과 같은 간단한 decoder 구조를 제안했습니다.
Encoder에서 나온 encoder feature를 4배 bilinear upsample을 해주고
그 다음 backbone network 중간에서 나온 feature maps(Low-Level Features)을 1x1 convolution을 적용하여 channel을 줄입니다.
이 둘을 합친 다음 3x3 convolution layers를 거친 후 마지막 1x1 convolution을 적용해서 나온
output을 원래 input size로 bilinear upsample(4배)을 해줍니다
단순히 크기로 보면 둘 다 16배를 더 키워주는 작업이지만
디코더를 사용했을때가 object segmentation details에 대한 정보를 
더 많이 담을 수 있다고 합니다

- next
마지막으로 Modified Aligned Xception 입니다
논문에서는 Xception 모델이 classification에서 빠른 연산과 함께 좋은 성능을 보여주었기 때문에
backbone network로 사용했다고 합니다
왼쪽에 있는게 Xception모델이고 오른쪽이 modified aligned Xception인데
변경점으로는 max pooling 연산은 depthwise separable convolution 연산으로 바꾸고
3x3 depthwise convolution 뒤에 batch normalization과 ReLU를 추가했다고 합니다

- next
실험 부분입니다

- next
첫번째로는 decoder design choices입니다
decoder의 구조를 결정하기 위해서 resnet101 - backbone을 인코더로 사용해서 실험을 진행했습니다

논문에서의 고려 사항이 두가지가 있었는데
첫번째는 Encoder module에서 나오는 low-level feature map에 적용하는
1x1 convolution channel 수고요

두번째는 Decoder 마지막에 사용되는 3x3 convolution을 구조를 어떻게 해야할지 고민했다고 하는데요
이것에 대해서는 각각 실험을 통해서 1x1 convolution channel은 48 channel로 사용하고 
3x3 convolution 구조는 3x3, 256 convolution을 2번 적용하는게 가장 좋은것을 확인했습니다

- next
두번째로는 ResNet-101 을 Network Backbone 으로 했을때의 실험결과입니다

실험 결과로는 
output stride=8 즉 denser feature map을 사용하는 것과 multi-scale inputs는 성능을 향상시킨다.

Flip을 적용 했을때는 조금의 성능향상이 있긴 하지만 연산 복잡도가 두배나 증가해서 적합하지 않음을 발견했습니다

그리고 Decoder를 사용할 때 적은 연산량의 증가로 높은 성능 향상을 가져온다

마지막으로 Training시 output stride가 32인 경우
이때가 atrous convolution을 사용하지 않은 경우라고 합니다
이때 복잡도는 줄어들지만 성능 또한 함께 떨어지는것을 확인했습니다

- next
세번째로는 Xception as Network Backbone 입니다

기본적으로 resnet backbone을 사용했을때와 비슷한 결과를 보여줬고요
추가적으로 coco나 jft 데이터로 pre-train한 모델을 사용할 때 성능이 향상되는것을 확인했습니다

- next
네번째로는 improvement along object boundaries 입니다
왼쪽 위에 그래프를 보면 decoder를 사용했을때가 Bilinear upsample 방식을 사용했을때보다
성능이 더 좋은것을 알 수 있고 오른쪽 그림을 봐도 경계선 식별에서 더 성능이 좋은 모습을 볼 수 있습니다

- next
결론입니다

- next
결론적으로 추가적인 구조 변경도 있긴 했지만 DeepLabv3의 구조에서 prediction 을 단순하게 Bilinear upsample 방식으로 얻는게 아니라
decoder 방식을 사용했을때 성능 향상이 있다는것을 보여준 모델입니다

개인적으로는 실험 부분에서 Decoder Design Choices with resnet 101 파트가 있었는데
디코더 구조를 결정할때 resnet 101으로만 실험하는게 아니라 논문에서 제시한
Xception backbone으로 실험했을때 구조가 달라질지, 달라진다면 성능에서의
유의미한 변화가 있을지에 대해 궁금하긴 합니다

- next
넵 이상으로 발표를 마치겠습니다 
질문 있으시면 편하게 질문 주시면 감사하겠습니다