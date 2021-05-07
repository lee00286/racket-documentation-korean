# 1.1 평가 모델 (Evaluation Model)

Racket 평가(evaluation)는 수식을 단순화하여 값을 얻는 것으로 볼 수 있습니다. 예를 들어, 초등학생이 아래의 식을 단순화하듯이

```
1 + 1 = 2
```

Racket 평가는 아래와 같이 단순화합니다

```
(+ 1 1) → 2
```

화살표 `→`는 `=`를 대체하여 평가가 수식을 단순한 수식이 되도록 함을 강조합니다. 특히 숫자 2와 같은 값은 더이상 단순화 될 수 없는 식으로 단순화됩니다.

---

<h2><a id="sub-expression">1.1.1 하위 식의 평가와 후속문 (Sub-expression Evaluation and Continuations)</a></h2>

일부 단순화는 한 번 이상의 과정을 필요로 합니다. 예를 들어:

<pre>
(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29">-</a> 4 (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1)) → (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29">-</a> 4 2) → 2
</pre>

[값](https://docs.racket-lang.org/reference/eval-model.html#%28tech._value%29)이 아닌 식은 언제나 두 부분으로 분할될 수 있습니다: 한 번의 단순화가 필요한 <em>redex("reducible expression")</em>와 redex를 둘러싸는 부분을 평가하는 <em>후속문(continuation)</em>이 있습니다. <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29">-</a> 4 (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1))</code>에서는, redex는 <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1)</code>이며 후속문은 <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29">-</a> 4 [])</code>인데, 여기서 `[]`는 단순화 된 [redex](https://docs.racket-lang.org/reference/eval-model.html#%28tech._redex%29)에 해당합니다. 즉, 후속문은 [값](https://docs.racket-lang.org/reference/eval-model.html#%28tech._value%29)으로 단순화 된 [redex](https://docs.racket-lang.org/reference/eval-model.html#%28tech._redex%29)를 어떻게 "이어가는가"에 대한 것입니다.

일부 식을 평가하기 전에, 일부 혹은 모든 하위 식(sub-expression)이 먼저 평가되어야 합니다. 예를 들어, <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29">-</a> 4 (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1))</code>에서는 <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1)</code>가 단순화 되기 전까지 <code><a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29">-</a></code>를 평가할 수 없습니다. 그러므로, 각 구문 형식의 설명(specification)은 하위 식을 평가하는 방법과 식을 단순화하기 위해 결과를 결합하는 방법을 명시합니다.

식의 *dynamic* extent는 [redex](https://docs.racket-lang.org/reference/eval-model.html#%28tech._redex%29)를 포함하는 식의 연속적인 평가 과정입니다.

---
