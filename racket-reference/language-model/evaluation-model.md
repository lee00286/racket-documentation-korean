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

<h2><a id="top-level">1.1.4 최상위 수준의 변수 (Top-Level Variables)</a></h2>

아래의 값이 주어졌다고 가정합니다

```
x = 10
```

그러면 대수학을 배운 학생은 `x + 1`을 아래처럼 단순화합니다:

```
x + 1 = 10 + 1 = 11
```

Racket도 평가 과정에서 거의 비슷하게 작동하는데, 요구만 있으면 [최상위 수준의 변수(top-level variables)](https://docs.racket-lang.org/reference/eval-model.html#%28tech._top._level._variable%29)([변수와 장소](https://docs.racket-lang.org/reference/eval-model.html#%28part._vars-and-locs%29)를 확인해주십시오)를 대체용으로 사용할 수 있습니다. 예를 들어, 아래의 정의가 주어졌다고 가정합니다

<pre>
(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 10)
</pre>

그러면

<pre>
(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> x 1) → (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 10 1) → 11
</pre>

Racket에서는, 정의가 생성되는 방식은 정의가 사용되는 방식만큼 중요합니다. 그러므로 Racket 평가는 정의와 현재 식 모두를 놓치지 않으며, <code><a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a></code>과 같은 평가 형식에 대응하여 정의들을 확장합니다.

그러면, 각 평가 과정은 현재 정의들과 프로그램을 새로운 정의들과 프로그램으로 변형합니다. <code><a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a></code>가 정의들 안으로 움직일 수 있게 되기 전에, 식(<code>define</code>의 오른편)이 <a href="#sub-expression">값</a>으로 단순화되어야 합니다. (<code>define</code>의 왼편은 식이 아니므로, 평가되지 않습니다.)

&nbsp;&nbsp;&nbsp;&nbsp;defined:<br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 9 1)) (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> x 1))</code><br>
→ defined:<br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 10) (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> x 1))</code><br>
→ defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 10)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/void.html#%28def._%28%28quote._~23~25kernel%29._void%29%29">void</a>) (<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> x 1))</code><br>
→ defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 10)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> x 1)</code><br>
→ defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 10)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 10 1)</code><br>
→ defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 10)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>11</code>

<code><a href="https://docs.racket-lang.org/reference/set_.html#%28form._%28%28quote._~23~25kernel%29._set%21%29%29">set!</a></code>를 사용하면, 프로그램에 존재하는 [최상위 수준의 변수](https://docs.racket-lang.org/reference/eval-model.html#%28tech._top._level._variable%29)와 관련된 값을 바꿀 수 있게 됩니다:

&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 10)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/set_.html#%28form._%28%28quote._~23~25kernel%29._set%21%29%29">set!</a> x 8) x)</code><br>
→ defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 8)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/void.html#%28def._%28%28quote._~23~25kernel%29._void%29%29">void</a>) x)</code><br>
→ defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 8)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>x</code><br>
→ defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x 8)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate: <code>8</code>

---

<h2><a id="object-imperative">1.1.5 객체와 명령형 업데이트 (Objects and Imperative Update)</a></h2>

[최상위 수준의 변수](https://docs.racket-lang.org/reference/eval-model.html#%28tech._top._level._variable%29)의 업데이트를 위한 <code><a href="https://docs.racket-lang.org/reference/set_.html#%28form._%28%28quote._~23~25kernel%29._set%21%29%29">set!</a></code> 외에도, 혼합 자료 구조(compound data structure)의 요소(element)를 수정할 수 있도록 허용하는 다양한 프로시져가 있습니다. 예를 들어, <code><a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-set%21%29%29">vector-set!</a></code>는 벡터(vector)의 내용물을 수정합니다.

이러한 데이터의 수정을 설명하기 위해서는, 수식의 결과물인 <a href="#sub-expression">값</a>과 값이 언급한 데이터를 가진 <em>객체</em>들을 구분해야 합니다.

몇 종류의 <a href="#object-imperative">객체</a>는 booleans, <code>(<a href="https://docs.racket-lang.org/reference/void.html#%28def._%28%28quote._~23~25kernel%29._void%29%29">void</a>)</code>, 작고 정확한 정수(integers)와 같은 값의 역할을 할 수 있습니다. 그러나, 일반적으로, <a href="#sub-expression">값</a>은 어딘가 다른 곳에 저장된 <a href="#object-imperative">객체</a>를 말합니다. 예를 들어, <a href="#sub-expression">값</a>은 첫 번째 슬롯에 `10`이라는 값을 가지는 특정한 벡터를 말합니다. 만약 <a href="#object-imperative">객체</a>가 하나의 <a href="#sub-expression">값</a>을 통해 수정되었다면, 그 <a href="#object-imperative">객체</a>를 참조하는 모든 <a href="#sub-expression">값</a>을 통해 수정 내용을 확인할 수 있습니다.

평가 모델(evaluation model)에서는, 정의 세트(definition set)과 마찬가지로 평가의 각 단계마다 <a href="#object-imperative">객체</a> 세트를 가져와야 합니다. <code><a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a></code>처럼 <a href="#object-imperative">객체</a>를 만드는 연산은 <a href="#object-imperative">객체</a> 세트에 추가됩니다:

&nbsp;&nbsp;&nbsp;&nbsp;objects:<br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 10 20))</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y x)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-set%21%29%29">vector-set!</a> x 0 11)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 10 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y x)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-set%21%29%29">vector-set!</a> x 0 11)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 10 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/void.html#%28def._%28%28quote._~23~25kernel%29._void%29%29">void</a>)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y x)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-set%21%29%29">vector-set!</a> x 0 11)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 10 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y x)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-set%21%29%29">vector-set!</a> x 0 11)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 10 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y &lto1&gt)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-set%21%29%29">vector-set!</a> x 0 11)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 10 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
<code style="margin-left:5.5em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/void.html#%28def._%28%28quote._~23~25kernel%29._void%29%29">void</a>)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-set%21%29%29">vector-set!</a> x 0 11)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 10 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
<code style="margin-left:5.5em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-set%21%29%29">vector-set!</a> x 0 11)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 10 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
<code style="margin-left:5.5em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-set%21%29%29">vector-set!</a> &lto1&gt 0 11)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 11 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
<code style="margin-left:5.5em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/begin.html#%28form._%28%28quote._~23~25kernel%29._begin%29%29">begin</a> (<a href="https://docs.racket-lang.org/reference/void.html#%28def._%28%28quote._~23~25kernel%29._void%29%29">void</a>)</code><br>
<code style="margin-left:10em">(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 11 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
<code style="margin-left:5.5em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> y 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 11 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
<code style="margin-left:5.5em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector-ref%29%29">vector-ref</a> &lto1&gt 0))</code><br>

→ objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 11 20))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
<code style="margin-left:5.5em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> y &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>11</code><br>

[최상위 수준의 변수](https://docs.racket-lang.org/reference/eval-model.html#%28tech._top._level._variable%29)와 객체 참조(object reference)의 차이는 중대합니다. [최상위 수준의 변수](https://docs.racket-lang.org/reference/eval-model.html#%28tech._top._level._variable%29)는 <a href="#sub-expression">값</a>이 아니므로, 평가되어야 합니다. [변수](https://docs.racket-lang.org/reference/eval-model.html#%28tech._variable%29) 수식이 평가될 때마다, 변수의 값은 현재 정의들에서 가져옵니다. 그러나 객체 참조는 값이기 때문에 더이상의 평가를 필요로 하지 않습니다. 위의 평가 과정은 객체 참조를 [값](https://docs.racket-lang.org/reference/eval-model.html#%28tech._variable%29)의 이름과 구분하기 위해 홑화살괄호로 씌운 `<o1>`을 사용합니다.

객체 참조는 텍스트 기반 소스 프로그램에선 직접 나올 수 없습니다. 그러나, <code><a href="https://docs.racket-lang.org/reference/stxops.html#%28def._%28%28quote._~23~25kernel%29._datum-~3esyntax%29%29">datum->syntax</a></code>으로 만들어진 program representation은 기존 <a href="#object-imperative">객체</a>에 직접적인 언급을 포함할 수 있습니다.

---

<h2><a id="garbage-collection">1.1.6 가비지 컬렉션 (Garbage Collection)</a></h2>

> 가비지 컬렉션과 관련된 함수를 보려면 [메모리 관리(Memory Management)](https://docs.racket-lang.org/reference/memory.html) 항목을 참고해주십시오.

프로그램 상태(program state)에서는

&nbsp;&nbsp;&nbsp;&nbsp;objects:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto1&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 10 20))</code><br>
<code style="margin-left:5.5em">(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> &lto2&gt (<a href="https://docs.racket-lang.org/reference/vectors.html#%28def._%28%28quote._~23~25kernel%29._vector%29%29">vector</a> 0))</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;defined:<code>(<a href="https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29">define</a> x &lto1&gt)</code><br>
&nbsp;&nbsp;&nbsp;&nbsp;evaluate:<code>(<a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29">+</a> 1 x)</code>
<br>

평가할 때 `<o2>`에 의존할 수 없는데, 왜냐하면 이는 평가할 프로그램의 일부가 아닌데다 프로그램에서 접근할 수 있는 어떤 정의에도 언급되지 않기 때문입니다. 객체는 <em>도달 가능</em>하지 않다고 말합니다. 그러므로 <a href="#object-imperative">객체</a> `<o2>`는 <em>가비지 컬렉션(garbage collection)</em>을 통해 프로그램 상태에서 제거할 수 있습니다.

몇 가지 특별한 복합 데이터 형식(compound datatypes)에는 객체에 대한 <em><a id="weak-reference">약한 참조(weak references)</a></em>가 있습니다. 이러한 약한 참조는 남은 계산에 접근 가능한 객체를 결정할 때 가비지 컬렉션을 통해 특별히 처리됩니다. 만약 <em>오직</em> 약한 참조를 통해서만 <a href="#object-imperative">객체</a>에 도달할 수 있다면, <a href="#object-imperative">객체</a>는 회수될 수 있으며 <a href="#weak-reference">약한 참조</a>는 다른 <a href="#sub-expression">값</a>으로 대체됩니다 (일반적으로 `#f`로 대체됩니다).

특별한 경우로, [fixnum](https://docs.racket-lang.org/reference/numbers.html#%28tech._fixnum%29)는 언제나 가비지 컬렉션에 의해 접근 가능한 것으로 취급됩니다. 다양한 값에 언제나 접근 가능한 이유는 값이 시행되고 사용되는 방법 때문입니다: Latin-1 범위에 있는 [문자](https://docs.racket-lang.org/reference/characters.html#%28tech._character%29)는 언제나 접근 가능한데, 이는 <code><a href="https://docs.racket-lang.org/reference/Equality.html#%28def._%28%28quote._~23~25kernel%29._equal~3f%29%29">equal?</a></code> Latin-1 문자가 언제나 <code><a href="https://docs.racket-lang.org/reference/Equality.html#%28def._%28%28quote._~23~25kernel%29._eq~3f%29%29">eq?</a></code>이며, 모든 Latin-1 문자는 내부 모듈에 의해 언급됩니다. 비슷하게, <code><a href="https://docs.racket-lang.org/reference/pairs.html#%28def._%28%28quote._~23~25kernel%29._null%29%29">null</a></code>, `#t`, `#f`, <code><a href="https://docs.racket-lang.org/reference/port-ops.html#%28def._%28%28quote._~23~25kernel%29._eof%29%29">eof</a></code>, 그리고 `#<void>`는 언제나 접근 가능합니다. [quote](https://docs.racket-lang.org/reference/quote.html#%28form._%28%28quote._~23~25kernel%29._quote%29%29)로 생성된 값은 [quote](https://docs.racket-lang.org/reference/quote.html#%28form._%28%28quote._~23~25kernel%29._quote%29%29) 수식 자체가 접근 가능할 때 접근 가능합니다.

---

<h2><a id="procedure-applications-and-local-variables">1.1.7 프로시져 응용과 지역 변수 (Procedure Applications and Local Variables)</a></h2>
