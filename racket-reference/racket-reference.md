# Racket 참고 문헌 ([원문](https://docs.racket-lang.org/reference/index.html))

이 매뉴얼은 핵심 Racket 언어를 정의하고 가장 중요한 라이브러리에 대해 설명합니다. 매뉴얼 [Racket 가이드](https://docs.racket-lang.org/guide/index.html)는 언어에 대해 보다 친숙한 (비록 정밀도와 완성도가 떨어지지만) 개요를 제공합니다.

> 이 매뉴얼의 소스(source)는 [GitHub](https://github.com/racket/racket/tree/master/pkgs/racket-doc/scribblings/reference)에서 확인할 수 있습니다.

<pre>
<a href="https://docs.racket-lang.org/guide/Module_Syntax.html#%28part._hash-lang%29">#lang</a> racket/base                          package: <a href="https://pkgs.racket-lang.org/package/base">base</a>
<a href="https://docs.racket-lang.org/guide/Module_Syntax.html#%28part._hash-lang%29">#lang</a> racket
</pre>

따로 적혀있지 않은 한, 이 매뉴얼에 정의된 바인딩은 <code><a href="https://docs.racket-lang.org/reference/index.html">racket/base</a></code>와 <code><a href="https://docs.racket-lang.org/reference/index.html">racket</a></code> 언어에 의해 내보내집니다.

<blockquote>
<code><a href="https://docs.racket-lang.org/reference/index.html">racket/base</a></code> 라이브러리는 <code><a href="https://docs.racket-lang.org/reference/index.html">racket</a></code> 라이브러리에 비해 훨씬 작기 때문에 불러오는 속도가 빠른 경우가 대부분입니다.

<code><a href="https://docs.racket-lang.org/reference/index.html">racket</a></code> 라이브러리는 <code><a href="https://docs.racket-lang.org/reference/index.html">racket/base</a></code>, <code><a href="https://docs.racket-lang.org/reference/booleans.html#%28mod-path._racket%2Fbool%29">racket/bool</a></code>, <code><a href="https://docs.racket-lang.org/reference/bytestrings.html#%28mod-path._racket%2Fbytes%29">racket/bytes</a></code>, <code><a href="https://docs.racket-lang.org/reference/mzlib_class.html">racket/class</a></code>, <code><a href="https://docs.racket-lang.org/reference/Command-Line_Parsing.html">racket/cmdline</a></code>, <code><a href="https://docs.racket-lang.org/reference/contracts.html">racket/contract</a></code>, <code><a href="https://docs.racket-lang.org/reference/dicts.html">racket/dict</a></code>, <code><a href="https://docs.racket-lang.org/reference/Filesystem.html#%28mod-path._racket%2Ffile%29">racket/file</a></code>, <code><a href="https://docs.racket-lang.org/reference/strings.html#%28mod-path._racket%2Fformat%29">racket/format</a></code>, <code><a href="https://docs.racket-lang.org/reference/procedures.html#%28mod-path._racket%2Ffunction%29">racket/function</a></code>, <code><a href="https://docs.racket-lang.org/reference/futures.html">racket/future</a></code>, <code><a href="https://docs.racket-lang.org/reference/include.html">racket/include</a></code>, <code><a href="https://docs.racket-lang.org/reference/keywords.html#%28mod-path._racket%2Fkeyword%29">racket/keyword</a></code>, <code><a href="https://docs.racket-lang.org/reference/pairs.html#%28mod-path._racket%2Flist%29">racket/list</a></code>, <code><a href="https://docs.racket-lang.org/reference/local.html">racket/local</a></code>, <code><a href="https://docs.racket-lang.org/reference/match.html">racket/match</a></code>, <code><a href="https://docs.racket-lang.org/reference/generic-numbers.html#%28mod-path._racket%2Fmath%29">racket/math</a></code>, <code><a href="https://docs.racket-lang.org/reference/More_Path_Utilities.html">racket/path</a></code>, <code><a href="https://docs.racket-lang.org/reference/places.html">racket/place</a></code>, <code><a href="https://docs.racket-lang.org/reference/port-lib.html">racket/port</a></code>, <code><a href="https://docs.racket-lang.org/reference/pretty-print.html">racket/pretty</a></code>, <code><a href="https://docs.racket-lang.org/reference/Delayed_Evaluation.html">racket/promise</a></code>, <code><a href="https://docs.racket-lang.org/reference/sequences.html#%28mod-path._racket%2Fsequence%29">racket/sequence</a></code>, <code><a href="https://docs.racket-lang.org/reference/sets.html">racket/set</a></code>, <code><a href="https://docs.racket-lang.org/reference/shared.html">racket/shared</a></code>, <code><a href="https://docs.racket-lang.org/reference/streams.html">racket/stream</a></code>, <code><a href="https://docs.racket-lang.org/reference/strings.html#%28mod-path._racket%2Fstring%29">racket/string</a></code>, <code><a href="https://docs.racket-lang.org/reference/symbols.html#%28mod-path._racket%2Fsymbol%29">racket/symbol</a></code>, <code><a href="https://docs.racket-lang.org/reference/subprocess.html#%28mod-path._racket%2Fsystem%29">racket/system</a></code>, <code><a href="https://docs.racket-lang.org/reference/tcp.html">racket/tcp</a></code>, <code><a href="https://docs.racket-lang.org/reference/udp.html">racket/udp</a></code>, <code><a href="https://docs.racket-lang.org/reference/mzlib_unit.html">racket/unit</a></code>, 그리고 <code><a href="https://docs.racket-lang.org/reference/vectors.html#%28mod-path._racket%2Fvector%29">racket/vector</a></code>를 결합합니다.
</blockquote>

---

## [1 언어 모델 (Language Model)](language-model/language-model.md)

### [1.1 평가 모델 (Evaluation Model)](language-model/evaluation-model.md)

<h4><a href="evaluation-model.html#sub-expression">1.1.1 하위 식의 평가와 후속문 (Sub-expression Evaluation and Continuations)</a></h4>

<h4><a href="evaluation-model.html#tail-position">1.1.2 꼬리 위치 (Tail Position)</a></h4>

<h4><a href="language-model/evaluation-model.html#multiple-return">1.1.3 많은 리턴 값 (Multiple Return Values)</a></h4>

<h4><a href="language-model/evaluation-model.html#top-level">1.1.4 최상위 수준의 변수 (Top-Level Variables)</a></h4>

<h4><a href="language-model/evaluation-model.html#object-imperative">1.1.5 객체와 명령형 업데이트 (Objects and Imperative Update)</a></h4>

<h4><a href="language-model/evaluation-model.html#garbage-collection">1.1.6 가비지 컬렉션 (Garbage Collection)</a></h4>

<h4><a href="language-model/evaluation-model.html#procedure-applications-and-local-variables">1.1.7 프로시져 응용과 지역 변수 (Procedure Applications and Local Variables)</a></h4>

---

## 2 Notation for Documentation

## 3 Syntactic Forms

## 4 Datatypes

## 5 Structures

## 6 Classes and Objects

## 7 Units

## 8 Contracts

## 9 Pattern Matching

## 10 Control Flow

## 11 Concurrency and Parallelism

## 12 Macros

## 13 Input and Output

## 14 Reflection and Security

## 15 Operating System

## 16 Memory Management

## 17 Unsafe Operations

## 18 Running Racket

---

## [Bibliography](https://docs.racket-lang.org/reference/doc-bibliography.html)

## Index
