---
published : true
title: "[ML Research]optimal policy의 수학적 증명과 Monte Carlo method"
date: 2021-02-26
excerpt: "Q-learning에서 Markov Decision Process, Bellman equation을 통해 optimal policy를 증명하고 MC 방법에 대해 다룹니다."
use_math: true
categories:
  - MLResearch
author : JeongMin Do
---


이전에 sung kim 교수님의 모두의 강화학습을 통해서 간단하게 강화학습의 전체적인 맥락에 대해서 살펴보았는데, 이번에는 강화학습의 보다 심화된 접근을 해보겠습니다.

사실 강화학습은 딥러닝에 비해서 아주 오래된 학문이라고 할 수 있습니다. 따라서 수학적, 통계적 깊이가 생각보다 강한 편이기도 합니다.

앞으로 진행될 강화학습을 다룬 포스트에서는 이러한 이론적 접근에 포커싱하여 진행할 예정입니다. 그리고 조금 부족한 부분이 있다면 추가로 지속해서 채워나갈 예정입니다.

해당 포스트는 유튜버 호펜하임님 채널의 강화학습 관련 강의를 보고 제작되었으며, Q-learning부터 Monte Carlo Method까지 다루겠습니다.

## Q-learning
우선 Q-learning을 개괄적으로 살펴보자면 아래와 같은 다양한 방법론들의 발전 혹은 결합이라고 보시면 될 것 같습니다.

* greedy action: action을 정할 때에, 가장 Q-value가 큰 것으로 결정한다는 방법입니다.(같은 값은 random하게 선택됩니다.)
	* epsilon greedy($\alpha$): exploit하게 선택할 것이냐, exploration하게 선택할 것이냐에 대한 문제를 두 가지를 혼합하여 일부는 greedy하게 선택하고 나머지는 random하게 선택하겠다는 방법으로 마찬가지로 action의 선택에 대한 방법론입니다.
		* decaying epsilon greedy: 앞서 본 epsilon greedy 선택 방법에 대해서 decaying을 적용하겠다는 것으로 senario가 반복됨에 따라 exploration을 줄이겠다는 의미입니다.
* discount factor($\gamma$): 최단 경로를 탐색하기 위한 방법론으로 t번째 Q는 t+1 번째 Q보다 discount factor를 곱한 값으로 설정하여 더 적은 Q-value를 부여하는 방법입니다.

위 방법론들을 통해서 Q는 다음과 같이 업데이트 됩니다.

$ Q(s_t, a_t) = (1 - \alpha) Q(s_t, a_t) + \alpha(R_t + \gamma max_{a_{t+1}} Q(s_{t+1}, a_{t+1})) $

## Markov Decision Process
강화학습의 핵심적인 이론이라고 할 수도 있을 것 같습니다. 강화학습에서 환경과 에이전트의 지속적인 상호작용은 다음과 같은 수학적 양상으로 표현이 가능합니다.

$s_0, a_0, s_1, a_1,s_2, a_2,s_3, a_3,s_4, a_4 …$

따라서, 확률과정론의 수학적 이론에 지배를 받는 상황이라고 할 수 있는데, 여기서 MDP는 현재상태는 이전 상태와 행동으로부터 결정된다는 아이디어 입니다.

즉, 수학적으로 표현하면 $s_{t+1} <= a_t, s_t$ 라고 할 수 있으며, 이에 대한 정리로 다음의 두 식이 성립하게 됩니다.

* $p(a_t \mid s_{t-1}, a_{t-1}, s_{t}) = p(a_t \mid s_t)$
* $p(s_{t+1}\mid s_{t-1}, a_{t-1}, s_t, a_t) = p(s_{t+1}\mid s_t, a_t)$

따라서, 보상(reward)이 최대가 되도록 action을 선택하는 것에 이용되게 됩니다(Maximize Expected Return).
여기서 Return은 아래 식과 같이 표기하도록 합니다.

$G_t = R_t + \gamma R_{t+1} + \gamma^2 R_{t+2} …$

> 이러한 수학적 이론을 토대로 본격적으로 Q-learnning에서의 action 선택 방식이 과연 합리적인가에 대해서 판단할 수 있을 겁니다.

## Expected Return의 표현
앞으로 배우게 될 Bellman Equation은 action을 선택함에 있어 아주 간편하게 해주는 방정식이 될 것입니다.

그러나, 이에 앞서 Expected Return을 새롭게 표현하는 것을 배워야 합니다. 종류는 크게 아래의 두가지로 나뉘게 됩니다.

* State value function: 현재 상태에 따른 가치 = $V(s_t) = \int_{a_t : a_{\infty}} G_t p(a_t : a_{\infty} \mid s_t) d {a_t}: {a_{\infty}}$
* Action value function: 현재상태의 특정 액션에 따른 가치 = $Q(s_t, a_t) = \int_{s_{t+1} : a_{\infty}} G_t p(s_{t+1} : a_{\infty} \mid s_t, a_t) d {s_{t+1}}: {a_{\infty}}$

따라서 $G_t$를 최대화 하는 $p(a_t \mid s_t), p(a_{t+1}\mid s_{t+1})$ 은 모두 optimal policy가 됩니다.

## Bellman Equation
이제 본격적으로 벨만 방정식을 배워보도록 하겠습니다. 이 방정식은 앞서 언급했듯, 우리가 optimal policy를 정하는 과정을 수학적으로 증명하기 위해 쉽게 변형하는 과정이며, 이를 통해 action을 정할 수 있게 됩니다.

사전에 알아야 할 것은 state value function과 action value function은 recursive한 형태로 이루어져 있기 때문에, 서로를 수식으로 표현이 가능하고, 심지어는 자기 자신도 t+1의 스텝을 이용하여 표현이 가능합니다.

따라서 $V(s_t)$ 를 $Q_{s_t, a_t}$로 표현해 봅시다.

$V(s_t)$ 
$= \int_{a_t} \int_{s_{t+1} : a_{\infty}} G_t p(s_{t+1}: a_{\infty} \mid s_t, a_t) d s_{t+1} : a_{\infty} p(a_t \mid s_t) da_t $
$ = \int_{a_t} G_t Q(s_t, a_t) p(a_t \mid s_t) da_t $

이렇게 벨만방정식을 유도할 수 있었습니다. 사실  $V(s_t)$ 를 $Q_{s_t, a_t}$로 표현하는 것 이외에도 총 4가지의 방정식이 도출될 수 있는데, 위에서 본 것의 간단한 적용이라서 생략하도록 하겠습니다.

## optimal policy
최적의 policy를 찾기 위해서는 State value function을 최대로 만들어야 합니다. 따라서 앞서 구한 식 $ V(s_t)= \int_{a_t} G_t Q(s_t, a_t) p(a_t \mid s_t) da_t $가 최대일 때의  policy인 $p(a_t \mid s_t)$를 선택하는 작업이라고 보면 됩니다.

식으로 표현하면 다음과 같습니다.

$argmax_{p(a_t \mid s_t)} \int_{a_t} G_t Q(s_t, a_t) p(a_t \mid s_t) da_t$

여기서 Q는 미리 안다고 가정할 때, Q가 최대일때의 $a_t$만 계속 sampling하는 probability density function을 생각하시면 됩니다. 결국 이러한 매커니즘으로 인해서 greedy한 action 선택이 이루어진다고 보면 되겠습니다.

## Monte Carlo Method
이번엔 몬테카를로 방법에 대해서 간단히 알아보도록 하겠습니다.

아이디어는 N이 충분히 클 때 다음이 성립한다고 가정합니다. 

$E[x] = \int_{x} xp(x) = {1 \over N} \sum_{i =1}^N x_i$

이것을 위 식에 적용해보면 다음과 같은 식이 도출됩니다.

$Q(s_t, a_t) = {1 \over N} \sum_{i = 1}^N G_t^{(N)}$

이러한 방법론은 강력함에도 불구하고 단점이 존재하는데, 너무 많은 경우의 수를 탐색해야 하기에 비용이 많이 든다는 것입니다. 따라서 이를 보완한 TD방법론들이 등장하게 되었습니다.

## Reference 
* [호펜하임 RL 강의](https://www.youtube.com/watch?v=cvctS4xWSaU&list=PL_iJu012NOxehE8fdF9me4TLfbdv3ZW8g)
