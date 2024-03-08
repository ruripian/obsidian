High-Resolusion Representation Learning for Visual Recognition에 대해 발표하게 된
소프트웨어 대학원 컴퓨터공학과 한수호입니다

**hrnet은 제목 그대로 고해상도 정보를 이용하여 학습을 하는 모델입니다**
이러한 고해상도 정보가 human pose estimation, semantic segmentaation, object detection과 같이 위치 정보를 중요하게 여기는 작업에서 고해상도 정보를 유지하는게 중요하다고 생각이 되어 이러한 모델을 만들게 되었다고 합니다.

**기존에 있는 resnet 아니면 vgg넷 같은 경우에는**
다음 그림처럼 원본 이미지에서 낮은 해상도로 가면서 학습을 하고, 이후에 고해상도 이미지로 복원을 하는 방법을 사용하는데,  HRnet에서는 아래 그림처럼 고해상도 정보를 유지하는것을 목표로 합니다.


**관련 연구로는 2가지를 가져왔습니다**
첫번째는 Learning low-resolution representation입니다
FCN에서 fully connected layer를 제외해서 저해상도 표현을 학습을 하기 위해 사용했습니다

두번째는 High resolution representation recovering그리고 maintating입니다
이거같은 경우에는 Upsampling을 통해 High-resolution을 복원하는 방식입니다
앞에서 학습한 저해상도 표현을 다시 업샘플링해서 고해상도로 복원하기 위해 사용했습니다
그리고 Maintaining은 DenseNet,GridNet등에 사용되었지만 언제 사용해야 하는지에 대한 설계가 명확하지 않다 이런식으로 앞선 논문들에서의 사용이 저


**다음은 메쏘드입니다.**


hrnet은 다음과 같은 구조를 가지게 됩니다.
모든 resoluition에서 input을 받고 그것들이 전부 연결된 구조를 가지고 있는 것을 확인할 수 있습니다. 여기서 이제 low->high 로 가는것과 high에서 low로 가는 경우가 있습니다

먼저 low에서 high로 가는 경우는 bilinear로 키워준 후에 1 by 1 convolution을 적용합니다 
high -> low로 가는 경우는 stride 2를 놓고 3 by 3인 convolution을 적용을 해가지고 크기를 줄여주는 방법을 사용합니다.

그리고 위 그림에서 고해상도 이미지의 기준은 원본 이미지의 8분의 1사이즈 or 4분의 1사이즈로 정한다고 합니다. 아마 원본 이미지를 그대로 사용하는 경우에 메모리 사용량과 연산량이 압도적으로 커지기 때문에 먼저 downsampling을 하는것으로 보여집니다


**HRNET은 크게 3가지로 분류가 되어있는데요**
HRNETV1은 high resolution representation만 사용해서 출력값을 만들어내고 나머지 저해상도 정보들은 무시합니다.
HRNETV2에서는 low resolution output을 전부 high resolution으로 맞추고 합치게 됩니다.
HRNETV2p는 HRNETV2에서 만든 고해상도 정보를 다운 샘플링해서 멀티레벨 representation을 만들게 됩니다.


**그러면 왜 한가지 방법만 사용을 하는게 아니라 버전별로 나누어서 설명을 하냐면**
각각의 버전마다 강점을 보이는 테스크가 나뉘어져 있기 때문이라고 합니다.
HRNetV1은 human pose estimation,
V2는 Sementic segmentation
V2p는 Object detection 에서 강점을 가지고 있다고 합니다

**그래서 먼저 HRnetv1을 사용한 결과를 확인해보았습니다**
데이터는 coco dataset을 사용했고요
전체적으로 다른 모델에 비해 파라미터 수 대비 ap값이 높게 나오는것을 볼 수 있었고요

오른쪽 아래 그래프 같은 경우에는 hrnet이 고해상도 정보를 넣었을때의 장점을 보여주기 위해서 한 실험으로 보입니다
그래서 이미지 크기를 원래 사용했던 원본같은 경우에는 파란색
그리고 두배 줄인것은 주황색
또 4배 줄인거는 노랑색 이런식으로 크기를 줄였을때의 ap값의 변화를 보여주면서 해상도를 높게 가져가는게 결과값이 훨씬 더 좋게 나온다는것을 보여주기 위해서 분리를 해서 따로 실험을 했습니다.

**다음은 semantic segmentation입니다**
semantic segmentation은 HRnetv2를 이용해서 진행을 했고요 pascal context데이터와 cityscapes 데이터를 사용을 했는데 이때도 역시 hrnetv2를 mIou값이 높은것을 확인할 수 있었고요
추가적으로 OCR을 붙여서 사용을 했을때 mIou값이 증가함을 보여줬습니다 

그리고 마지막으로 hrnetv2p입니다
오브젝트 디텍션에서의 결과값인데요 resnet이랑 비교해서 성능을 찍은 표인데
자체로는 결과값들을 보면 막 그렇게까지 차이가 나지 않아서 크게 사용을 할 이유가 없어보이지만 resnet보다 더 데이터가 가벼워서 좋다 이런 이야기는 있었습니다.


결론적으로 hrnet은  학습하는 동안 고해상도 정보를 계속 유지를 하면서 병렬적으로 데이터를 쌓아가는 모델입니다.


그런데 이제 각 버전마다 v1은 포즈디텍션 v2는 segmentation v2p는 object dection 이런식으로 특성화된 작업에 맞추어서 설계된 모델이기 때문에 하고자 하는 작업이 무엇인지에 대한 세밀한 분석이 이루어져야 한다고 생각됩니다.

그리고 어디서 가장 많이 사용하는지에 대해서도 찾아봤는데 찾고자 하는 데이터가 전체 사진 대비 비율적으로 작은 정보를 가지고 있는 위성영상 같은 분야에서 성능이 좋기때문에 사용을 했었다라고 합니다.
지금은 이제 다른 모델들도 많이 나와서 다른거 사용을 하는 것 같지만

**디스커션**
각 모델이 뛰어난 성능이 부분이 정해져 있는데 논문 내에서는 항상 경험적인 결과다 라고 기술을 해놓았기 때문에 왜 그렇게 되었냐에 대해서 확인을 못하는게 아쉽고,
경험적인것이기 때문에 여기서 제시한 뭐 v1이 pose 디텍션에 좋다 v2가 세그멘테이션에 좋다 이런걸 실제 사용할때 한번 더 검증을 해봐야하지 않나 생각이 듭니다.

이상으로 hrnet에 대한 발표를 마치도록 하겠습니다
