## Paper

http://blog.mobile.codalism.com/research/papers/wmz10_tois.pdf

<br><br>

## Paper Topic

두 검색/추천 결과의 유사도를 비교할 때, 결과 set 의 단순한 비교가 아닌 ranking 을 고려하여 비교 평가하는 새로운 방법 제시

기존에도 비슷한 목적의 평가 방식은 많이 있었으나, 해당 논문은 보다 현실적인 indefinite ranking 을 비교하려고 시도함이 특징

<br><br>

## Ideation

### 일반적인 Ranked-List 의 특징은 어떤 것이 있을까?
- imcomplete, 현실적으로 세상 모든 item 이 결과에 나올 수는 없음  
- top-weighted, 상위 rank 에 있는 item 이 비교적 더 중요함  
- indefinite, 사용자는 전체 결과 중 일부 item 만 스크롤하여 봄, 모든 depth 를 살펴보진 않음

<br>

### 이를 통해 사전에 전제해볼 수 있는 내용은?
- 상위 rank 유사도에 더 높은 가중치  
- item 을 어느 rank 까지 볼 것이냐는 임의로 정하지 말고, 데이터에 따라 일관되게 선정
- indefinite 성질을 훼손하지 않는 최소한의 데이터 조작 및 실험 가정 필요,  
  즉 데이터 본연의 특성을 가능한 유지해야 다양한 상황에 보편적으로 적용할 수 있을 것

<br>

### 수학적으로 어떻게 모델링할 수 있을지

rank = 1 부터 ranking depth 가 깊어짐에 따라, 사용자가 각 rank 에서의 두 검색/추천 결과를 비교하는 상황이라고 생각한다.

먼저, 각 rank 까지 살펴봤을 때 거기까지의 유사도 (논문에서는 overlap) 은 다음과 같이 계산해본다.


사용자의 persistence, 즉 둘을 비교하면서 얼마나 깊은 depth 까지 보게 될까의 정도를 실험 파라미터 $p$ 로 한다.    
좀 더 구체적으로 얘기하면 이미 앞서 중복된 결과를 보고도, 다음 rank 결과를 기대하며 depth 를 좀 더 살펴보는 것이다.

사용자가 특정 depth 에서 탐색을 중단할지 말지의 수학적 모델은 이러한 persistence 값이 $p$ 인 베르누이 분포이다.

$$ \begin{cases}
P(stop) = p \\
P(continue) = 1-p \\
  \end{cases} (n = 1)$$


<br><br>

## 






 

