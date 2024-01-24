
### Regularization 기법 중 하나

Drop-out은 서로 연결된 연결망(layer)에서 0부터 1 사이의 확률로 뉴런을 제거(drop)하는 기법입니다. 예를 들어, 위의 **_그림 1_** 과 같이 drop-out rate가 0.5라고 가정하겠습니다. Drop-out 이전에 4개의 뉴런끼리 모두 연결되어 있는 전결합 계층(Fully Connected Layer)에서 4개의 뉴런 각각은 0.5의 확률로 제거될지 말지 랜덤하게 결정됩니다. 위의 예시에서는 2개가 제거된 것을 알 수 있습니다. 즉, 꺼지는 뉴런의 종류와 개수는 오로지 랜덤하게 drop-out rate에 따라 결정됩니다. Drop-out Rate는 하이퍼 파라미터이며 일반적으로 0.5로 설정합니다.


Overfitting을 방지하기 위해 사용됩니다.
![[Pasted image 20231010181101.png]]