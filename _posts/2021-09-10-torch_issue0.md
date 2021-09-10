---
title: "[pytorch] requires_grad=False로 해도 학습이 되는 경우"
classes: wide
use_math: true
categories:
- Pytorch
tags:
- pytorch
- requires_grad
---

연구를 하던 중에 어떤 파라미터의 업데이트를 중단하기 위해 requires_grad를 False로 설정해도 계속해서 weight가 업데이트 되는 이슈가 있었다. 

예를 들어 파라미터 $A$와 파라미터 $B$가 있을 때

전체 $epoch$의 처음 절반은 $A$를 학습하고 이후 절반은 $B$를 학습하고자 할 때

초기 설정을 A.requires_grad = True, B.requires_grad = False로 설정하고, 

중간에 다시 A.requires_grad = False, B.requires_grad = True로 설정하였다. 

하지만 A가 이후에도 계속해서 업데이트 되는 문제가 발생하였다.

디버깅을 통해 A의 gradient를 출력해보니 None이 아닌 0.0이 출력되었다. 

[참고링크](https://stackoverflow.com/questions/53159427/pytorch-freeze-weights-and-update-param-groups) 를 통해 원인에 대해 명확하게 이해하진 못했지만 해결방안은 찾을 수 있었다.

A.requires_grad = False를 할 때 동시에 A.grad = None으로 설정해주면 해결이 되었다.

why????

생각나는 원인은 

1. weight_decay에 의한 업데이트
2. requires_grad = False로 두어 gradient 추척은 되지 않지만 이전의 gradient가 zero_grad()를 통해 0.0으로 되어 1번과 같은 다른 원인들에 의해 업데이트

정도가 있는 것 같다. 