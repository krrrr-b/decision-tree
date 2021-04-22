결정 트리(Decision Tree, 의사결정트리, 의사결정나무라고도 함)는 분류(Classification)와 회귀(Regression) 모두 가능한 지도 학습 모델 중 하나입니다.
결정 트리는 스무고개 하듯이 예/아니오 질문을 이어가며 학습합니다.

매, 펭귄, 돌고래, 곰을 구분한다고 생각해봅시다.
매와 펭귄은 날개를 있고, 돌고래와 곰은 날개가 없습니다. 
'날개가 있나요?'라는 질문을 통해 매, 펭귄 / 돌고래, 곰을 나눌 수 있습니다. 
매와 펭귄은 '날 수 있나요?'라는 질문으로 나눌 수 있고, 돌고래와 곰은 '지느러미가 있나요?'라는 질문으로 나눌 수 있습니다. 

아래는 결정 트리를 도식화한 것입니다.

![img1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwlH1u%2FbtqwWZI9Xen%2FkFJDjGSFJAPxhyatC3Xhs0%2Fimg.png)

출처: 텐서 플로우 블로그

이렇게 특정 기준(질문)에 따라 데이터를 구분하는 모델을 결정 트리 모델이라고 합니다. 
한번의 분기 때마다 변수 영역을 두 개로 구분합니다. 

결정 트리에서 질문이나 정답을 담은 네모 상자를 노드(Node)라고 합니다. 
맨 처음 분류 기준 (즉, 첫 질문)을 Root Node라고 하고, 맨 마지막 노드를 Terminal Node 혹은 Leaf Node라고 합니다.

![img2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F96F6N%2FbtqwVglgV2S%2FYTCytd7Z2egbnbJM29MJv1%2Fimg.png)

출처: ratsgo's blog

전체적인 모양이 나무를 뒤짚어 높은 것과 같아서 이름이 Decision Tree입니다.

프로세스
결정 트리 알고리즘의 프로세스를 간단히 알아보겠습니다.

![img3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbGlghI%2FbtqwYFXZzCu%2F0g4cMFuumUkKDYmDfkMdu0%2Fimg.png)

출처: 텐서 플로우 블로그
먼저 위와 같이 데이터를 가장 잘 구분할 수 있는 질문을 기준으로 나눕니다. 

![img4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJSlvg%2FbtqwXHvdrPJ%2FZhikSUKx3SmuYZSz6NGZL1%2Fimg.png)

출처: 텐서 플로우 블로그
나뉜 각 범주에서 또 다시 데이터를 가장 잘 구분할 수 있는 질문을 기준으로 나눕니다. 이를 지나치게 많이 하면 아래와 같이 오버피팅이 됩니다. 결정 트리에 아무 파라미터를 주지 않고 모델링하면 오버피팅이 됩니다. 

![img5](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbUvjhU%2FbtqwYiIK85s%2FoQ3KuTZVk6CgSAQI0VkwW1%2Fimg.png)

출처: 텐서 플로우 블로그

가지치기(Pruning)
오버피팅을 막기 위한 전략으로 가지치기(Pruning)라는 기법이 있습니다. 트리에 가지가 너무 많다면 오버피팅이라 볼 수 있습니다. 가지치기란 나무의 가지를 치는 작업을 말합니다. 즉, 최대 깊이나 터미널 노드의 최대 개수, 혹은 한 노드가 분할하기 위한 최소 데이터 수를 제한하는 것입니다. min_sample_split 파라미터를 조정하여 한 노드에 들어있는 최소 데이터 수를 정해줄 수 있습니다. min_sample_split = 10이면 한 노드에 10개의 데이터가 있다면 그 노드는 더 이상 분기를 하지 않습니다. 또한, max_depth를 통해서 최대 깊이를 지정해줄 수도 있습니다. max_depth = 4이면, 깊이가 4보다 크게 가지를 치지 않습니다. 가지치기는 사전 가지치기와 사후 가지치기가 있지만 sklearn에서는 사전 가지치기만 지원합니다.

알고리즘: 엔트로피(Entropy), 불순도(Impurity)
불순도(Impurity)란 해당 범주 안에 서로 다른 데이터가 얼마나 섞여 있는지를 뜻합니다. 
아래 그림에서 위쪽 범주는 불순도가 낮고, 아래쪽 범주는 불순도가 높습니다. 바꾸어 말하면 위쪽 범주는 순도(Purity)가 높고, 아래쪽 범주는 순도가 낮습니다. 
위쪽 범주는 다 빨간점인데 하나만 파란점이므로 불순도가 낮다고 할 수 있습니다. 

반면 아래쪽 범주는 5개는 파란점, 3개는 빨간점으로 서로 다른 데이터가 많이 섞여 있어 불순도가 높습니다.

![img6](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqLXhZ%2FbtqwWyZl6iV%2FUZnQbf9L5HAFzf6hFfxK71%2Fimg.png)

출처: ratsgo's blog

한 범주에 하나의 데이터만 있다면 불순도가 최소(혹은 순도가 최대)이고, 한 범주 안에 서로 다른 두 데이터가 정확히 반반 있다면 불순도가 최대(혹은 순도가 최소)입니다. 
결정 트리는 불순도를 최소화(혹은 순도를 최대화)하는 방향으로 학습을 진행합니다.

엔트로피(Entropy)는 불순도(Impurity)를 수치적으로 나타낸 척도입니다. 
엔트로피가 높다는 것은 불순도가 높다는 뜻이고, 엔트로피가 낮다는 것은 불순도가 낮다는 뜻입니다. 
엔트로피가 1이면 불순도가 최대입니다. 

즉, 한 범주 안에 서로 다른 데이터가 정확히 반반 있다는 뜻입니다. 
엔트로피가 0이면 불순도는 최소입니다. 
한 범주 안에 하나의 데이터만 있다는 뜻입니다. 엔트로피를 구하는 공식은 아래와 같습니다.

![img7](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpL6pO%2FbtqwVDN1V94%2FTYgn5iFrPTfgdVwZhxVKl1%2Fimg.png)

엔트로피 공식
(Pi = 한 영역 안에 존재하는 데이터 가운데 범주 i에 속하는 데이터의 비율)

참고:
- https://www.youtube.com/watch?v=xki7zQDf74I
- https://www.youtube.com/watch?v=2Rd4AqmLjfU
- https://bkshin.tistory.com/entry/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-4-%EA%B2%B0%EC%A0%95-%ED%8A%B8%EB%A6%ACDecision-Tree?category=1057680
