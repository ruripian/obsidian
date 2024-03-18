# 자돈사 물/사료 이상 탐지 프로젝트
이 프로젝트는 양돈 분야에서 물과 사료의 효율적인 관리를 위해 무인으로 밥그릇 내의 사료와 물의 이상을 탐지하기 위해 제작되었습니다. 돈사의 천장에 설치된 CCTV 영상 데이터를 활용하여 semantic segmentation 모델을 사용하여 밥그릇의 위치를 식별하고, 밥그릇 내부의 물이 넘치거나 사료가 부족한 상황을 탐지합니다.

## 사용법
학습에 필요한 하이퍼 파라미터는 hyperparameters.yaml 에서 변경할 수 있습니다.
```yaml
batch_size: 4
epochs: 100
device: cuda  # 'cuda' if torch.cuda.is_available() else 'cpu'
patch_size: 240
patch_stride: 120
num_workers: 0
num_classes: 4
class_names:
  - box
  - feed
  - water
loss_func: dice
train_data_rate: 0.8
model_names:
  - deeplabv3_resnet101
```

main.py 를 이용하여 학습을 시작 할 수 있습니다
```bash
python main.py
```

predection.py를 이용하여 추론을  `-model` 옵션을 사용하여 모델 이름을 지정할 수 있습니다.
```bash
python predection.py -model deeplabv3_resnet101
```


## 학습 데이터

프로젝트에서 사용되는 학습을 위한 라벨 데이터의 형식은 png며 구성은 다음과 같습니다
1. **물 (water):**
   - 물이 있는 영역을 나타내는 라벨 데이터
   - data : (255,0,0)
2. **사료 (feed):**
   - 사료가 있는 영역을 나타내는 라벨 데이터
   - data : (255,0,255)
3. **밥그릇 (box):**
   - 밥그릇의 위치를 나타내는 라벨 데이터
   - data : (255,255,0)
![2023_04_23_23_09_17_label](/uploads/de085f0063e1acdf2b207cdd3e0d666f/2023_04_23_23_09_17_label.png)
## 참고사항

- 라벨링은 labelme를 이용하여 진행하였습니다.
