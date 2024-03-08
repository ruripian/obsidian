안녕하세요 이번에
EfficientNetV2
Smaller Models and Faster Training에 대해 발표하게 된
소프트웨어 대학원 컴퓨터공학과 한수호입니다

- next
먼저 모델에 대한 간단한 소개를 하려고 하는데요
이제는 나온지 꽤 된 chat gpt 같은걸 보면 성능이 되게 뛰어나잖아요 그런데 이런 모델들을
학습하는 과정에서 보면 학습 자체에 많은 시간과
막대한 컴퓨터 파워가 필요하다는게 알려져있고 이로 인해서 뭐 재학습을 한다거나 모델 개선을
하기 위해서 실험을 하기가 상당히 부담스러운 환경을 가지고 있습니다.
이 EfficientNetV2 같은 경우에 가장 중점을 두고 있는 부분이 
학습 하는데에 걸리는 시간과 파라미터 수 
즉 학습 효율성에 대해 포커스를 두고 있는 모델입니다

- next
그래서 다음 표를 보시면 imagenet을 사용해서 비교한 것인데요 
훈련 시간은 tpu-core 32개로 측정을 하였다고 합니다.
원래 있던 efficientnet 
여기서는 efficientnet v1이라고 지칭을 하는데
이것보다 최대 6.8배 정도 적은 prameter를 사용했고
5배에서 11배 더 빠른 학습 시간을 보여주고 있는것을 확인 할 수 있습니다.

- next
관련연구로는 세가지를 가져왔습니다

첫번째는 Training and parameter efficiency입니다
화면에 보이는 것처럼 Resnet / TResnet / EfficientnetX
이런 모델들의 지향점이 훈련된 모델의 GPU 추론 속도,정확도를 올리는 것이라면
EfiicientnetV2는 훈련 속도와 parameter의 효율성에 있다고 말하고 있습니다
이걸 얻기 위해 정확도를 포기하는게 아니라, 비슷한 정확도를 가지면서
훈련을 할 때 시간적으로 효율적인 모델을 지향하는 것입니다

- next
두번째는 progressive training과 mix and match 입니다
Gan에서는 training의 setting이나 network를 동적으로 바꾸는 방법을 사용을 합니다
그리고 mix and match에서는 서로 다른 이미지 크기를 무작위로 [[샘플링]]을 합니다
두 방법 같은 경우에 훈련 속도를 향상 시키지만 정확도가 떨어지는 문제가 있습니다.
본 논문에서는 이렇게 정확도가 떨어지는 이유가 
이미지 크기가 작을 때는 regularization을 약하게 써야하고
이미지 크기가 클 때는 regularization을 강하게 써야하는데
이미지 크기와 무관하게 regularization를 사용을 하기 때문에 일어나는 현상이고
이를 개선하면 훈련 속도와 정확성을 둘 다 높일 수 있다고 합니다.

- next
그리고 마지막으로 curriculum learning기법을 부분적으로 사용했다고 합니다
curriculum learning은
쉬운 예제에 대해서 학습을 시작하고
이후에는 더 어려운 샘플을 주면서 학습에 대한 강화를 하는것을 말합니다
그래서 위에 있는 쉬운 예제는 정확한 모양의 정사각형이나 원같은 도형을 모아둔 easy sample이고
아래에 있는 도형은 타원이나 직사각형같은 도형들이 있는데 이 easy sample을 이용해서 학습하고 난 다음에 제공하는 어려운 샘플입니다.
이 방법을 기반으로 더 많은 regularization를 추가해서
학습 난이도를 증가시키는 방법을 채택했다고 합니다.

- next
그러면 이 논문에 대해서 자세히 다루기 전에 지금 제가 다루고 있는게 efficientnetv2 잖아요
간단하게 efficientnetv1이 뭐였는지만 소개하고 넘어가겠습니다
efficientnet같은 경우에는 모델의 width, depth, resolution을 조절하는 모델이고요
이걸 직접 사람이 찾는게 아니라 nas를 이용한 모델입니다
이 모델에 대한 자세한 설명은 저번에 발표를 이미 하셨기 때문에 생략하고 넘어가도록 하겠습니다

- next
그래서 여기서 어떤 부분이 개선이 되었냐
첫 번째는 학습할 때 사용하는 image size입니다
두 번째는 모델을 구성하는 depthwise convolution 블럭이 있는데 여기에 포함된
depthwise 연산을 개선하였고요
세 번째는 방금 보여드린 compound scaling을 하는 방식에 대해서 개선을 했다고 합니다
이 부분에 대해서 자세하게 이야기를 해보도록 하겠습니다

- next
먼저 Training with very large image sizes / is slow 라고 여기에서 말을 했는데
말 그대로 큰 이미지로 훈련하면 느리다고 합니다.
EfficientNet V1에서 가장 작은 모델인 B0의 image size는 224 by 224
가장 큰 모델인 B7의 image size는 600 by 600으로 사용을 하고 있습니다.
훈련에 사용하는 gpu의 메모리가 한정적이기 때문에
이미지 사이즈가 커지면 배치사이즈가 작아질수밖에 없고 
[[batchsize]] 가 작으면 훈련 속도는 느려지게 됩니다.

이런 문제를 FixEfficientNet이라는 이전에 있었던 논문에서는 
훈련에 사용하는 이미지 크기를 작게 설정함으로써 해결을 하였는데요
그렇게 설정할 수 있는 근거로  training할때 찾고자 하는 객체를 crop해서 큰 크기로 resize를 하면 
testdata에서 이미지안에 그 객체가 차지하는 비율에서 차이가 나기 때문에
training 할 때 input size를 작게 주는게 더 높은 효율을 보인다라고 이야기를 하였습니다 

본 논문에서는 이 근거를 바탕으로 progressive Learning 이라는 방법을 사용하였습니다.
이것도 기존에 있었던 방식인데요 이미지의 크기를 작은 상태에서
점차 크게 늘려가면서 학습을 진행하는 방식입니다.
기존에 있던 progressive learning을 그대로 사용하면 정확도가 감소하는 문제가 있는데요
이걸 이미지 크기마다 각자 다른 강도의 regularization을 적용하여 해결하였습니다

아래 표는 이미지 크기마다 각자 다른 강도의 regulariztion을 적용한 실험 결과인데
크기가 가장 작은 이미지의 경우 regularization 강도가 약할때 정확도가 높았고
크기가 가장 큰 경우 regularization의 강도를 강하게 가져갔을때 정확도가 가장 높은것을 확인할 수 있었습니다.
여기서는 이 방법을 progressive learning with adaptive regularization 이라고 하였습니다.

- next
본 논문에서 사용한 regularization은 세가지가 있는데요 
[[dropout]], [[RandAugment]] , [[mixup]]입니다
오른쪽 그림을 보면 adaptive regularization 을 적용하여 작은 이미지부터 큰 이미지의 순서로 모델을 학습하였으며, 이미지의 크기에 비례하는 regularization 강도를 설정해준 것을 볼 수 있습니다.   EfficientNetV2에서는 적절한 강도의 regularization을 이미지 크기에 따라 적용함으로써 progressive learning 과정에서 떨어진 정확도를 보정해주었습니다.

- next
이러한 방법은 EfficientNetV2에서만 특별하게 작용하는 것이 아니라 
기존에 있었던 모델인 EfficientNetV1이나 Resnet에서도 효과적임을 볼 수 있습니다.
여기 표를 보시면 정확도에서도 소폭 증가를 하였고 훈련시간이 최소 29퍼센트 최대 65퍼센트까지 감소한것을 볼 수 있습니다.

- next
두번째는 Equally scaling up every stage is sub-optimal이라고 합니다.
여기서 compound scaling을 할 때 모든 단계의 모델을 동일하게 키우는것이 비효율적일 것이라
생각해서 진행했다고 합니다.

그림을 보시면 제일 왼쪽에 있는게 아무런 scaling을 하지 않은 baseline이라고 가정했을 때
가운데에는 적절한 값을 찾아서 모든 단계에서 모델을 키운 efficientnetv1입니다
제일 오른쪽 그림은 적절한 값을 찾아서 모델을 키우지만
처음 단계보다 마지막 부분에 가중치를 더 둔 efficientnetv2의 모습입니다.
이런 방법을 사용했을때 더 효율적인 모델이 된다고 합니다.

- next
마지막으로 Depthwise convolutions are slow in early layers입니다

[[Depthwise_convolution]] 은 MobileNet V1에서 처음 소개된 연산인데
3by3 convolution 연산의 메모리와 연산량을 줄이기 위해 제안되었습니다.
입력 데이터의 각 채널에 깊이가 1인 kernel을 이용하여 convolution 연산을 실행하고 그 결과를 다시 이어 붙여주는 형식입니다.
EfficientNet V1 모델은 depthwise convolution을 이용한 MBConv 블럭으로 구성되어 있습니다.

그런데 3by3 convolution 연산이 가장 자주 이용되는 연산이여서 gpu의 여러 라이브러리들에서
최적화가 잘 되어있습니다. 이 때문에 depthwise 3x3 convolution 연산을 했을 때
오히려 overhead가 나서 EfficientNet V2 모델에서는
MBConv 블럭의 depthwise 3x3 convolution 연산을
그냥 3x3 convolution 연산으로 바꾼 Fused-MBConv 블럭을 이용하였습니다.

오른쪽 1번 표를 보면 MBConv 블럭만 이용했을 때보다 Fused-MBConv 블럭을 이용했을 때, parameter 수와 FLOPs 측면에서는 손해를 보지만
학습의 정확도와 속도 측면에서는 오히려 효율적임을 알 수 있습니다
그리고 모든 스테이지를 fused mbconv 블록을 사용하는 것보다 1~3 stage만 사용하는게
더 효율적인 것도 볼 수 있었습니다
그리고 모델을 구성하기 위해서 NAS를 사용했는데
보상 함수를 정확도와 학습 시간, parameter 개수로 구성해서 2번표와
같이 모델을 구성할 수 있었다고 합니다.

- next
Training입니다
data는 ImageNet2012와 ImageNet21k를 사용했고요
progressive learning with adaptive regularization 을 위해 350 epochs를 87 epochs씩 네 단계로 나누고, 이미지 크기와 regularization의 강도를 단계마다 일정하게 증가해주며 학습하였습니다

제어 변수는 small,midium,large모델에서 image size는 동일하게 사용하고
random augmentation,mixup, dropout rate의 최대값을 조정하여 사용하였습니다

- next
결과로는 다음 표와 같이 보여집니다
convolution 및 transformer 기반의 모델들과 비교해도 빠르고 정확하고 EfficientNetV1을
제외하면 모델 크기에서도 효율적인 것을 확인 할 수 있었습니다.
EfficientNetV2-Medium 모델은 비슷한 정확도를 가진 EfficientNet-B7에 비해서
학습 과정에서 11배나 빠른 속도인것을 볼 수 있었습니다.

- next
결론입니다
모델의 지향점이 단순히 정확도 뿐만 아니라 학습하는데의 효율성에도 있어야 한다 라고 하는것 같고
사실 이 지향점이 실제로 과제를 하게 되면 저희가 시간이 무한하게 제공이 되는게 아니기 때문에 되게 데이터 양이 많아서 학습이 오래 걸리고 오래 걸리다 보면 할 수 있는 작업이 제한적이게 되는데
중간에 뭐 데이터 전처리를 추가를 하거나, augmentation을 추가하거나 해보면서 시행착오를 거치고 싶어도 못하는 경우가 많은데
그런 부분에서 다른 모델을 실험할 때 제가 만약에 모델을 뜯어 고칠 능력만 있으면
여기서 제안한 progressive learning with adaptive regularization을 사용하면 진짜 획기적으로 속도를 빠르게 가질 수 있어서 되게 좋은 접근 방법이라고 생각이 됩니다.


//////////
근데 아까전에 mb convolution 이 오버헤드가 나서
모든 스테이지를 fused mbconv 블록을 사용하는게 아니라 