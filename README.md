# cifar100_pytorch

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

- Channel수의 영향

- epoch와 Batch size간의 상관관계

- Learning rate 조절

- 모델에 깊이, 즉 층을 추가하였을때 accuracy의 

1.Channel수, epoch&Batch size,learning rate조절해가며 학습.
==========================================

epoch, activation function, batchnormalization, batch size, learning rate 등을 모두 동일한 환경으로 하고 convolution 층에서 output channel의 수만 늘렸을 경우

1.batch size=32일때

최대 9%로 ouput channel을 늘려줬을때 정확도가 향상되는 모습 보임.

Batch size=32일때

![image](https://user-images.githubusercontent.com/104436260/178210499-299019de-1cac-41ed-bc7f-69362479df54.png)

Batch size=64일때

![image](https://user-images.githubusercontent.com/104436260/178210646-238c3a0f-ca9e-464e-8b54-89fab9b574af.png)

결론: batch size=64, epoch=8, channel=16,32로 넣어줬을때 39%의 Accuracy로 가장 정확도가 높음 해당 파라미터들을 사용하고, Convolution층을 한층 추가하여 Accuracy 새로 측정

Model layer추가
=============

기존 모델

1.input->convolution1->batch normalization->activation function->max pooling(output channel=16)

2.input->convolution2->batch normalization->activation function->max pooling(output channel=32)

3.data flatten

4.fully connected1->activation function

5.fully connected2->activation function

6.fully connected3

7.output->loss function->back propagation-> optimize

8.accuracy=39%

층 추가 모델1
=======

1.input->convolution1->batch normalization->activation function->max pooling(output channel=16)

2.input->convolution2->batch normalization->activation function->max pooling(output channel=32)

3.input->convolution3->batch normalization->activation function->max pooling(output channel=32)

4.fully connected1->activation function

5.fully connected2->activation function

6.fully connected3

7.output->loss function->back propagation-> optimize

8.accuracy=41%

층 추가 모델2
========

1.input->convolution1->batch normalization->activation function->max pooling(output channel=16)

2.input->convolution2->batch normalization->activation function->max pooling(output channel=32)

3.input->convolution3->batch normalization->activation function->max pooling(output channel=64)

4.fully connected1->activation function

5.fully connected2->activation function

6.fully connected3

7.output->loss function->back propagation-> optimize

8.accuracy=44%

층 추가 모델3
=======

1.input->convolution1->batch normalization->activation function(output channel=32)

2.input->convolution2->batch normalization->activation function->max pooling(output channel=32)

3.input->convolution3->batch normalization->activation function(output channel=64)

4.input->convolution4->batch normalization->activation function->max pooling(output channel=64)

5.input->convolution5->batch normalization->activation function(output channel=128)

6.input->convolution6->batch normalization->activation function->max pooling(output channel=128)

7.fully connected1->batch normalization->activation function

8.fully connected2->batch normalization->activation function

9.fully connected3->batch normalization->activation function

10.fully connected4

11.output->loss function->back propagation-> optimize

12.accuracy=51%

층 추가 모델4
=======

1.input->convolution1->batch normalization->activation function(output channel=32)

2.input->convolution2->batch normalization->activation function->max pooling(output channel=32)

3.input->convolution3->batch normalization->activation function(output channel=64)

4.input->convolution4->batch normalization->activation function->max pooling(output channel=64)

5.input->convolution5->batch normalization->activation function(output channel=128)

6.input->convolution6->batch normalization->activation function->max pooling(output channel=128)

7.fully connected1->activation function

8.fully connected2->activation function

9.fully connected3->activation function

10.fully connected4

11.output->loss function->back propagation-> optimize

12.accuracy=41%

층 추가 모델5
=============

1.input->convolution1->batch normalization->activation function(output channel=32)

2.input->convolution2->batch normalization->activation function->max pooling(output channel=32)

3.input->convolution3->batch normalization->activation function(output channel=64)

4.input->convolution4->batch normalization->activation function(output channel=64)

5.input->convolution4->batch normalization->activation function->Max pooling(output channel=64)

6.input->convolution5->batch normalization->activation function(output channel=128)

7.input->convolution5->batch normalization->activation function(output channel=128)

8.input->convolution6->batch normalization->activation function->max pooling(output channel=128)

7.fully connected1->batch normalization->activation function

8.fully connected2->batch normalization->activation function

9.fully connected3->batch normalization->activation function

10.fully connected4

11.output->loss function->back propagation-> optimize

12.accuracy=51%

최종적으로 모델3과 모델5의 Accuracy값이 동일하게 가장높게나옴-> 모델3층이 더욱 간단한 구조여서 연산과정이 빠르기 때문에 최종 사용 모델은 모델3으로 선정

