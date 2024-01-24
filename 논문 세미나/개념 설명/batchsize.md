

딥러닝의 학습안정화가 잘 안되던 예전에는 batch size를 작게하는게 의외의 효과를 불러왔습니다. batch size가 클수록 많은 train data를 한번에 학습하기 때문에 더욱 정확한 gradient를 계산해서 cost function이 오히려 local minimum으로 수렴 되는 경우가 많았습니다. 그런데 batch size가 작으면 부정확한 gradient가 계산되어 cost function 공간에서 오히려 local minimum을 뛰어넘는 의외의 장점(?)을 가지게 되었습니다.

하지만 요즘엔 Adam optimizer나 batch normalization(BN)등의 기법을 사용함으로써 학습안정화가 정말 잘 되는 네트워크가 많습니다. 따라서 batch size가 커도 local minimum을 잘 지나쳐 global minimum으로 수렴할 수 있게되었습니다. 따라서 **최근에는 batch size를 줄 수 있는만큼 최대한 크게줘야** 좋다고 할 수 있습니다. batch size가 클수록 BN을 더욱 정확하게 계산할 수 있어 BN의 효과를 더욱 잘 누릴 수 있게되고 그렇게 되면 학습속도가 빨라지며 learning rate의 hyper parameter 설정에서 꽤나 자유로워질 수 있게됩니다.

더불어 최근에는 Rectified Adam, Spatial dropout등의 기법이 사용되면서 hyper parameter를 크게 신경쓰지 않아도 좋은 성능으로 모델이 잘 학습된다고 합니다.

![[Pasted image 20231010183417.png]]