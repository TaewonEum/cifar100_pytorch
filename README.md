# cifar100_pytorch~7/15업데이트 예정

CIFAR100 image classification in Pytorch
=========================

개발환경: Jupyter lab

목표 accuracy=50%

분석 Flow
=============

1.Input data(32x32x3 color image) 불러오기 및 정규화

2.Built Train Model

-Convolution layer(first) 정의: nn.Module Method 상속받아 사용함

nn.Conv2d(input_channel=3, output_channel, filter_size) 첫번째 input channel=3으로 고정, output channel은 6,16,32 등 세밀한 학습을 위해서 점차적으로 늘려갈 예정

-Batch Normalization layer 추가

학습속도 개선(learning rate를 크게 설정), 가중치 초깃값 선택의 의존성 줄임, 과적합 위험을 줄임(Drop out기능 대체), Gradient Vanishing(기울기 소실문제) 등을 위해 추가
입력분포를 균일화.

-Activation Function 적용

데이터에 비선형성을 추가하기 위하여, 딥러닝에서 자주 사용하는 Activation Function ReLU 함수를 적용함. 데이터에 비선형성을 추가하여 더 복잡한 분류문제 해결 가능

-Maxpooling적용

2x2 size, stride=2인 Maxpooling 적용, pooling layer를 통해 이미지의 특징을 더 부각시키고 정보를 유지하며, 이미지의 크기는 더 줄일 수 있음.

-Convolution layer(second)=위의 Convolution layer 1층 과정을 한번더 반복해서 적용해줌

-Fully Connected layer(완전 연결 계층)

이미지별 Class에 속할 확률을 계산해주는 층. 3차원 이미지 데이터를 1차원으로 펼쳐주고 Batch normalization->activation function적용해줌

-Test data

이후에 학습한 모델을 활용하여 Test data에 적용하여 최종 Accuracy 계산


연구목적
=========

1.Channel수의 영향

2.epoch와 Batch size간의 상관관계

3.Learning rate 조절

4.모델에 깊이, 즉 층을 추가하였을때 모델 

1.Channel수와 모델 Accuracy와의 상관관계
==========================================

epoch, activation function, batchnormalization, batch size, learning rate 등을 모두 동일한 환경으로 하고 convolution 층에서 output channel의 수만 늘렸을 경우

1.batch size=32일때

최대 9%로 ouput channel을 늘려줬을때 정확도가 향상되는 모습 보임.

Batch size=32일때

![image](https://user-images.githubusercontent.com/104436260/178190359-ba9df277-5478-4f5d-8ce9-89857cd7f348.png)

Batch size=64일때

![image](https://user-images.githubusercontent.com/104436260/178200654-bea70c33-efbf-4f54-b1db-c45d7243e3ea.png)

결론: batch size=64, epoch=8일때 39%의 Accuracy로 가장 정확도가 높음 해당 파라미터들을 사용하고, Convolution층을 한층 추가하여 Accuracy 새로 측정

