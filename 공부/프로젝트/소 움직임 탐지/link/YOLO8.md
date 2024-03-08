
## YOLO5 모델 구조
![[Pasted image 20240123130830.png | 450x350]]

## YOLO8 모델 구조
![[Pasted image 20240123132901.png ]]

### YOLO5와의 차이점
> 1. Replace the C3 module with the C2f module
> 
> 2. Replace the first 6x6 Conv with 3x3 Conv in the Backbone
> 
> 3. Delete two Convs (No.10 and No.14 in the YOLOv5 config)
> 
> 4. Replace the first 1x1 Conv with 3x3 Conv in the Bottleneck
> 
> 5. Use decoupled head and delete the objectness branch

### Anchor Free Detection
- 미리 입력된 Anchor Box를 사용하지 않고, 객체의 center를 직접 예측하는 방법
- Bbox 예측의 수를 줄여서 NMS(Non-Maximum Suppression)를 가속화 할 수 있음
	- cf. NMS : 가장 스코어가 높은 박스만 남기고 나머지를 제거하는 기법

### YOLOv5의 대표 Augmentation - Mosaic 기법 일부 제거
- 무한 증강은 성능이 저하되는 것을 발견
- 학습이 종료되기 10 epoch 이전에 Augmentation 종료

