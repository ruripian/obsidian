
## 전제 조건
- 카메라가 정확한 위치에 고정되어 있을 것
- 카메라의 겹치는 영역이 존재 할 것
- 동일한 카메라를 사용할 것 (세팅 차이)
## 해결해야할 문제

연산 속도, 효율성 : ORB
매칭 정확도 : SIFT,BRISK

- 속도 문제
StitcHD https://www.youtube.com/watch?v=3in4e_yYOIA
https://www.youtube.com/watch?v=IDzFvqRb40Y

![[Pasted image 20231214135041.png|800]]

- 이미지 합성에서의 cuda 사용 :  
	- https://answers.opencv.org/question/234209/cudacuts-for-image-texture-synthesis/
	- 카메라는 위치와 방향이 고정되기 때문에 미리 카메라의 합성 방향은 고정이 가능함
- 정확도 문제
	- 사용 알고리즘 차이
		- [[SIFT, SURF and ORB on OpenCV]]


- git 구현 링크
- https://github.com/creimers/real-time-image-stitching/tree/5c8920def8df0ca051b60e95bb6c18f1101adbfd - 가로 행렬 이미지 스티칭


https://study.marearts.com/2011/08/two-image-mosaic-paranoma-based-on-sift.html
