
## Regularization 기법 중 하나

### Introduction

Data augmentation은 딥러닝 모델의 성닝을 올리는데 적합하지만 전문성과 domain에 대한 사전지식이 필요하다.

이를 해결하기 위해 NAS로 적절한 parmeter를 찾는 방법이 제안 되었으나 하지만 아직도 많은 연산량과 복잡도를 갖고 있다는 것이 사실이다.

본 논문에서는 model size와 dataset size에 상관 없는 전략을 제시했다. Separate search가 필요없으며 RandAugment라고 부른다.

확 줄어든 parameter space는 매우 간단한 grid search만으로 충분한 data augmentation policy를 찾아내며 최고의 성능을 보여준다.

![[Pasted image 20231010182027.png]]

```python
class RandAugment:
    def __init__(self, n, m):
        self.n = n
        self.m = m      # [0, 10]
        self.augment_list = augment_list()

    def __call__(self, img):
        ops = random.choices(self.augment_list, k=self.n)
        for op, minval, maxval in ops:
            val = (float(self.m) / 30) * float(maxval - minval) + minval
            img = op(img, val)

        return img
```


14개중 몇가지를 고를지 선택하는 N과 0~30사이의 magitude(크기) M 두가지의 parameter를 가짐
![[Pasted image 20231010181901.png]]