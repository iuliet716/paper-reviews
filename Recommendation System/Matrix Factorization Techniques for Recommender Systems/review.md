## Papers

matrix factorization techniques for recommender systems  
[https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf](https://datajobs.com/data-science-repo/Recommender-Systems-%5BNetflix%5D.pdf)

#### Other References
Collaborative Filtering for Implicit Feedback Datasets  
[http://yifanhu.net/PUB/cf.pdf](http://yifanhu.net/PUB/cf.pdf)

<br>

## Latent Factor Model

관찰되지 않은, 현재 데이터만으로는 알 수 없는 latent factor 에 대한 모델링

<br>

<img width="763" alt="Untitled" src="https://github.com/iuliet716/paper-reviews/assets/62701789/6ddfa336-57fe-42ce-968b-28f761f50e8c">

<br><br>

위 그림처럼 User, Item 을 벡터 공간에 나타낸다면

User 벡터와 Item 벡터 가 얼마나 유사한지, 즉 dot product 를 계산하여 

해당 Item 에 대한 User 의 rating 을 정의할 수 있음

이 때, **현재로서는 알 수 없는 rating 값 일부를 예측하기 위한 모델링**이 필요

(e.g. 아직 평가하지 않은 영화에 대한 평점)

이를 구현하기 전에 User-Item 의 $(U \times I)$ 행렬을 먼저 분해한 뒤 진행 (Matrix Factorization)

<br>

# Matrix Factorization

![Untitled (1)](https://github.com/iuliet716/paper-reviews/assets/62701789/5625ede9-38da-4a1f-8ebe-abdd428ca03c)

<br><br>

User-Item $(U \times I)$ = User-Feature $(U \times k)$ $\times$ Feature-Item $(k \times I)$

User_i 의 Item_i 에 대한 예측 rating 은 User_i 벡터와 Item_i 벡터의 내적

<br>
<img width="131" alt="Untitled (2)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/ff4ec961-ca07-4fe9-b008-3a36c023a2b9">
<br><br>

하지만, 이를 그대로 사용하는 것은 data sparsity 문제로 인한 overfitting 가능성이 존재

따라서, 아래와 같이 정규화된 objective function 을 사용

<img width="443" alt="Untitled (3)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/6f4aa5b8-fbb8-4f72-9e4a-1096d6401565">

<br>

($\lambda$ 값은 hyperparameter, 튜닝 관련 참고 논문 : https://proceedings.neurips.cc/paper_files/paper/2007/file/d7322ed717dedf1eb4e6e52a37ea7bcd-Paper.pdf)

<br>

# Learning Algorithm

논문에서는 학습을 위해 다음 2가지 방법을 제안

## Stochastic Gradient Descent (SGD)

각 training case 의 prediction error 를 계산한 후, 파라미터를 최적화

<img width="179" alt="Untitled (4)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/817fa0e8-d415-4e8d-b136-657df5c5cab5">

<img width="278" alt="Untitled (5)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/9d9399e6-d736-47a5-84cc-5cd91fc60d42">


## ALS (****Alternating Least Squares)****

SGD 는 구현은 쉽고 빠르나, 각 single case 를 살펴보므로 

sparse data 일 경우에는 그다지 좋은 방법이 아님

따라서, **implicit feedback 중심의 데이터에서는 ALS 가 SGD 보다 더 이점이 있음**

unknown parameter 인 $\large p_{u}$, $\large q_{i}$ 를 하나씩 fix 해두고 

fix 되지 않은 한 쪽은 least-squre 방식으로 최적화

이를 번갈아가며 반복하여 objective function 을 수렴시킨다.

<br>

# Others

## Adding Biases

User-Item Bias = Avg. Rating + User deviation + Item deviation  
<img width="196" alt="Untitled (6)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/9af89897-e824-4dea-bb8c-97b04c85fcc8">

<br>

User-Item Rating = User-Item dot product + User-Item Bias  
<img width="256" alt="Untitled (7)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/46d05269-26a2-4726-8172-7ade8cf1d45e">

<br>

Objective function  
<img width="424" alt="Untitled (8)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/b9abe36f-1b41-4e7c-ae33-417894aca219">

## Additional Input Sources

데이터에 추가적인 implicit data 를 incorporate 하도록 구현한 수식

$cf)$ boolean implicit data 표현 예시  
<img width="194" alt="Untitled (9)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/5eeea159-65b6-4482-9998-067e49397958">

예를 들어, 정규화된 $\large x_{i}$ 와 보통의 $\large y_{a}$ 를 추가하는 경우  
<img width="517" alt="Untitled (10)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/8647eda0-5f58-4807-850a-8e9d006bf66d">

## Temporal Dynamics

주어진 데이터(e.g. 평점, 인기도) 는 시간에 따라 변화 가능

이를 동적으로 반영하고자 할 경우  
<img width="354" alt="Untitled (11)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/8c0419fe-bd72-4081-903d-418cffd92659">

## Inputs with Varying Confidence Levels

rating 의 신뢰성을 반영하기 위한 방안

one-time 보다는 recurring event 를 사용하면 유저의 실제 선호도를 더 잘 반영할 수 있음

c_ui = User-Item confidence-level weight  
<img width="311" alt="Untitled (12)" src="https://github.com/iuliet716/paper-reviews/assets/62701789/0edb6526-0d4f-4563-92fc-4ce7795e4728">

# Conclusion

- **Collaborative Filtering 이므로 cold-start 문제 존재**

- **Others 항목에 표시된 것처럼 서비스에서 요구하는 다양한 항목을 반영 가능**

- **실제 적용 시, 실 데이터를 보고 어떻게 적용할 것인지 잘 판단하는 것이 중요**
  - **featrue 를 어떻게 정의할 것인지 생각 필요**
  - **로그 데이터 수가 얼마나 준비되어 있는지 확인 필요**
