# Continue: Racket의 웹 어플리케이션 ([원문](https://docs.racket-lang.org/continue/index.html))

동적 웹 어플리케이션(dynamic web applications)은 어떻게 만드는 걸까요? 이 튜토리얼에서는, Racket으로 어떻게 동적 웹 어플리케이션을 만드는지 보여줍니다. 우리는 어떻게 웹 서버를 시작하고, 동적 웹 컨텐트(dynamic web content)를 생성하고, 사용자와 상호 작용을 하는지 설명합니다. 예시는 간단한 웹 저널이 -- 블로그 -- 될 것입니다.

이 튜토리얼은 어떻게 구조를 사용하는지, 고차함수(higher-order functions), 그리고 약간의 mutation을 알기 위해 <em><a href="http://htdp.org/">How to Design Programs</a></em>를 충분히 읽은 학생들을 대상으로 작성되었습니다.

---

## 목차
1. [시작하기](#시작하기)

---

## 시작하기

---
