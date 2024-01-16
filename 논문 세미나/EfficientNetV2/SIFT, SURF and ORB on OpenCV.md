

SIFT 방식
- key point detection : 모서리 같이 쉽게 식별이 가능한 고유 영역 찾는것
- key point descriptor extraction : Descriptor는 keypoint에 해당하는 정보이므로 기본적으로 keypoint와 같은 개수로 생성되어지며 실제 유사도를 판별하기 위한 데이터

키포인트 탐지는 DoG(Difference of Gaussian)를 사용한다
DoG는 스택이며 3x3x3 이웃에서 로컬 극값을 찾아 키포인트로 식별한다

![[Pasted image 20231214121653.png]]

SURF 방식
키포인트 탐지를 위해 Dog를 사용하는 대신 상자 필터를 사용함
![[Pasted image 20231214121838.png]]

[[스티칭]]

ORB 방식
**ST and BRIEF**

출처 링크 : https://mikhail-kennerley.medium.com/a-comparison-of-sift-surf-and-orb-on-opencv-59119b9ec3d0

colab : https://colab.research.google.com/drive/1IFC-9QxRzTKoxjdTECoby1B3CamNWFDY?usp=sharing

참고 링크 : https://wjddyd66.github.io/opencv/OpenCV(8)/