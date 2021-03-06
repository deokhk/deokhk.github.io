﻿---
layout: post
title: 나만의 Pytorch Tutorial - 1
comment: true
---
## 시작하게 된 이유
 이번 여름방학 중 학부생 연구참여를 수행하게 되었다. 자연어처리에 관심이 있는 나는 해당 분야의 Lab에서 연구를 진행하게 되었다.  이번 여름방학의 연구 목표는 다음과 같다. 

> BERT 기반의 QA task 논문과 최신 코드를 읽고 내 프로젝트에 적용해 볼 수 있고, 간단한 Network는 Pytorch로 구현이 가능한 정도의 Level.
> 
 나는 머신러닝 및 자연어처리 분야에 대해 몇몇 수업을 들었기 때문에 이론적인 지식은 어느 정도 가지고 있지만, 딥러닝 프레임워크에 대해서는 별로 경험이 없다. 이전에 과제연구를 수행하는 과정에서 Keras를 사용해본것만이 전부이다. 이에 이번 여름방학 중 Pytorch를 공부하기로 하였고 그 과정에서 내가 느끼기에 중요하다고 판단되는 것을 초심자의 마음으로 이곳에 작성하고자 한다. 해당 문서는 [Pytorch 공식 튜토리얼](https://tutorials.pytorch.kr/beginner/blitz/tensor_tutorial.html#sphx-glr-beginner-blitz-tensor-tutorial-py)을 기반으로 작성되었다.
 ## Pytorch에서 Tensor란 무엇인가
Pytorch에서의 Tensor란 Numpy에서의 ndarray와 유사하다. 이러한 Tensor는 Numpy 배열로 변환하거나 그 반대도 가능하다. 차이점은 Tensor는 GPU를 이용한 연산 가속이 가능하다는 점이다. 
우선 필요한 Module을 import하자.

    from __future__ import print_function
    import torch
    

우선 Data로부터 Tensor를 만드는 방법부터 알아보자. 우리는 아래의 원소들로 채워진 2X2 행렬을 만들 수 있다.

    x = torch.tensor([[1, 2, 3],[4, 5, 6]])
    print(x)
결과:

    tensor([[1, 2, 3], 
            [4, 5, 6]])

 이러한 행렬의 크기는 다음과 같은 방법으로 구할 수 있다.
 

    print(x.size())
  결과:
  

    torch.Size([2, 3])
   우리는 size()를 통해 해당 matrix가 2X3 matrix임을 확인할 수 있다. 그렇다면 특정 size의 matrix는 어떻게 생성할 수 있을까? 
   

    #초기화되지 않은 2x3 행렬 생성
	x = torch.empty(2,3)
	print(x)

	#무작위 값으로 초기화된 2x3 행렬 생성
	y = torch.randn(2,3)
	print(y)

	#0으로 초기화된 2x3 행렬 생성
	z = torch.zeros(2,3)
	print(z)

	#0으로 초기화되었고 data type이 long인 2x3 행렬 생성
	l = torch.zeros(2,3, dtype=torch.long)
	print(l)
결과:

    tensor([[4.9484e-36, 0.0000e+00, 1.6675e-43],    
            [5.6052e-44, 1.6816e-43, 6.1657e-44]]) 
    tensor([[ 0.9699, 1.1269, 0.6148], 
            [ 1.0897, -0.4911, -0.3662]]) 
    tensor([[0., 0., 0.], 
            [0., 0., 0.]]) 
    tensor([[0, 0, 0], 
            [0, 0, 0]])

## Tensor와 관련된 연산
행렬 간의 연산을 하다 보면 연산 대상 행렬간의 차원을 맞추어 줘야 되는 경우가 종종 발생한다. flattening을 수행해야 되는 경우도 있다. 
tensor의 모양(shape)이나 크기(size)를 변환시키고 싶은 경우에는 __view()__ 를 사용한다.

    x = torch.randn(6, 6)
	y = x.view(36)
	z = x.view(-1, 12) # -1은 다른 차원들을 사용하여 유추한다.

	print(x.size())
	print(y.size())
	print(z.size())
결과:

    torch.Size([6, 6]) 
    torch.Size([36]) 
    torch.Size([3, 12])
위에서 말했던 대로 Pytorch의 Tensor는 Numpy 배열과 서로 변환이 가능하다. 또한 cpu상에 존재하는 pytorch tensor와 numpy 배열은 저장 공간을 공유한다. 

	x = torch.ones(4, 4)
	y = x.numpy() # x tensor를 numpy 배열로 변환
	z = torch.from_numpy(y) # y numpy 배열을 tensor로 변환

	print(x)
	print(y)
	print(z)

결과:

	tensor([[1., 1., 1., 1.], 
	        [1., 1., 1., 1.], 
	        [1., 1., 1., 1.], 
	        [1., 1., 1., 1.]]) 
	[[1. 1. 1. 1.] 
	[1. 1. 1. 1.] 
	[1. 1. 1. 1.] 
	[1. 1. 1. 1.]] 
	tensor([[1., 1., 1., 1.], 
	       [1., 1., 1., 1.], 
	       [1., 1., 1., 1.], 
	       [1., 1., 1., 1.]])

아래와 같은 연산을 통해 cpu상의 pytorch tensor와 numpy배열은 저장공간을 공유한다는 것을 확인할 수 있다.

	x.add_(1) # x의 모든 원소에 1을 더함

	print(x)
	print(y)
	print(z)
결과:

	tensor([[2., 2., 2., 2.], 
	        [2., 2., 2., 2.], 
	        [2., 2., 2., 2.], 
	        [2., 2., 2., 2.]]) 
	[[2. 2. 2. 2.] 
	 [2. 2. 2. 2.] 
	 [2. 2. 2. 2.] 
	 [2. 2. 2. 2.]] 
	tensor([[2., 2., 2., 2.], 
	        [2., 2., 2., 2.], 
	        [2., 2., 2., 2.], 
	        [2., 2., 2., 2.]])

## Tensor를 GPU로 옮기기
Tensor의 장점은 GPU를 이용하여 연산가속을 할 수 있다는 점이다. 
```Python
x = torch.zeros(2,2)

if torch.cuda.is_available():
	y = torch.ones((2,2), device="cuda") # GPU상에 직접적으로 Tensor 생성하기
	print(y)

	x = x.to("cuda") # CPU상의 Tensor를 GPU로 옮기기
	print(x)
	
	z = 2*y	
	print(z)
```
결과:

    tensor([[1., 1.], 
            [1., 1.]], device='cuda:0') 
    tensor([[0., 0.], 
            [0., 0.]], device='cuda:0') 
    tensor([[2., 2.], 
            [2., 2.]], device='cuda:0')



