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

<h2><a id="tail-position">1.1.2 꼬리 위치 (Tail Position)</a></h2>

Redex <code><em>expr1</em></code>의 [후속문](https://docs.racket-lang.org/reference/eval-model.html#%28tech._continuation%29)이 <code><em>expr1</em></code>을 둘러싸는 식 <code><em>expr2</em></code>의 [후속문](https://docs.racket-lang.org/reference/eval-model.html#%28tech._continuation%29)과 같을 때, 식 <code><em>expr1</em></code>는 <code><em>expr2</em></code>에 대하여 <em>꼬리 위치(tail position)</em>에 있다고 말합니다.

예를 들어, 식 <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1)</code>는 <a href="#tail-position.html">꼬리 위치</a>에 대하여 꼬리 위치에 있지 <em>않습니다</em>. 설명하자면, 어떠한 [후속문](https://docs.racket-lang.org/reference/eval-model.html#%28tech._continuation%29) <code><em>C</em></code>의 <code>[]</code>를 <code><em>expr</em></code>으로 대체할 때 <code><em>C</em>[<em>expr</em>]</code>라고 표기합니다.

<pre>
<em>C</em>[(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29">-</a> 4 (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1))] → <em>C</em>[(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29">-</a> 4 2)]
</pre>

위의 경우에는, <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1)</code>를 단순화하는 [후속문](https://docs.racket-lang.org/reference/eval-model.html#%28tech._continuation%29)은 <code><em>C</em></code>가 아닌 <code><em>C</em>[(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29">-</a> 4 [])]</code>입니다. 첫 문단에서 명시한 조건이 충족되지 않았습니다.

반면, <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1)</code>는 <code>(<a href="https://docs.racket-lang.org/reference/if.html#%28form._%28%28quote._~23~25kernel%29._if%29%29">if</a> (<a href="https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._zero~3f%29%29">zero?</a> 0) (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1) 3)</code>에 대하여 <a href="#tail-position.html">꼬리 위치</a>에 있습니다. 왜냐하면, 모든 후속문 <code><em>C</em></code>의 경우,

<pre>
<em>C</em>[(<a href="https://docs.racket-lang.org/reference/if.html#%28form._%28%28quote._~23~25kernel%29._if%29%29">if</a> (<a href="https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._zero~3f%29%29">zero?</a> 0) (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1) 3)] → <em>C</em>[(<a href="https://docs.racket-lang.org/reference/if.html#%28form._%28%28quote._~23~25kernel%29._if%29%29">if</a> #t (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1) 3)] → <em>C</em>[(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 1)]
</pre>

위의 경우에는 첫 문단에서 명시한 조건이 충족됩니다. 이 단순화 시퀀스(sequence)의 과정은 <code><a href="https://docs.racket-lang.org/reference/if.html#%28form._%28%28quote._~23~25kernel%29._if%29%29">if</a></code>의 정의에 의해 진행되며, [후속문](https://docs.racket-lang.org/reference/eval-model.html#%28tech._continuation%29) <code><em>C</em></code>에 의존하지 않습니다. <code><a href="https://docs.racket-lang.org/reference/if.html#%28form._%28%28quote._~23~25kernel%29._if%29%29">if</a></code> 형식의 "then" 브랜치(branch)는 언제나 <code><a href="https://docs.racket-lang.org/reference/if.html#%28form._%28%28quote._~23~25kernel%29._if%29%29">if</a></code> 형식에 대하여 꼬리 위치에 있습니다. <code><a href="https://docs.racket-lang.org/reference/if.html#%28form._%28%28quote._~23~25kernel%29._if%29%29">if</a></code>와 `#f`는 비슷한 단순화 규칙을 가지므로, <code><a href="https://docs.racket-lang.org/reference/if.html#%28form._%28%28quote._~23~25kernel%29._if%29%29">if</a></code> 형식의 "else" 브랜치 또한 <a href="#tail-position.html">꼬리 위치</a>에 있습니다.

<a href="#tail-position.html">꼬리 위치</a>의 사양(specification)은 컴퓨팅 할 때(computation)의 점근적 공간 소비(asymptotic space consumption)를 보장합니다. 보통, <a href="#tail-position.html">꼬리 위치</a>의 사양은 <code><a href="https://docs.racket-lang.org/reference/if.html#%28form._%28%28quote._~23~25kernel%29._if%29%29">if</a></code>와 같은 각 구문 형식(syntactic form)의 설명을 동반합니다.

---

<h2><a id="multiple-return">1.1.3 많은 리턴 값 (Multiple Return Values)</a></h2>

Racket 프로시저(procedure)는 많은 인수를 받아들이므로 이와 균형을 맞추기 위해 *많은* 값을 평가할 수 있습니다.

대부분의 [후속문](https://docs.racket-lang.org/reference/eval-model.html#%28tech._continuation%29)은 특정 개수의 결과 <a href="#sub-expression">값</a>을 기대합니다. 개수 상관없이 결과 값을 받아들이는 [후속문](https://docs.racket-lang.org/reference/eval-model.html#%28tech._continuation%29)도 존재합니다. 사실, <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> [] 1)</code>와 같은 대부분의 [후속문](https://docs.racket-lang.org/reference/eval-model.html#%28tech._continuation%29)은 하나의 값을 기대합니다. 함수 <code>(<a href="https://docs.racket-lang.org/reference/let.html#%28form._%28%28quote._~23~25kernel%29._let-values%29%29">let-values</a> ([(x y) []]) <em>expr</em>)</code>는 두 개의 결과 값을 기대합니다; 첫 번째 결과는 <code><em>expr</em></code>에서 `x`를 대체하며, 두 번째 결과는 <code><em>expr</em></code>에서 `y`를 대체합니다. 함수 <code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> [] (+ 1 2))</code>는 결과를 무시하므로, 개수 상관 없이 결과를 받아들입니다.

보통, 구문 형식의 사양은 형식과 하위 식에서 만들어지는 <a href="#sub-expression">값</a>의 개수를 나타냅니다. 또한, 일부 프로시저(특히 <a href="#sub-expression">값</a>)는 다수의 <a href="#sub-expression">값</a>을 생성하며, 일부 프로시저(특히 [call-with-values](https://docs.racket-lang.org/reference/values.html#%28def._%28%28quote._~23~25kernel%29._call-with-values%29%29))는 내부에 특정 개수의 <a href="#sub-expression">값</a>을 받아들이는 후속문을 만듭니다.

---
