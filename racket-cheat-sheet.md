# Racket 컨닝페이퍼 (Racket Cheat Sheet)

## 목차
1. [기본 (Essentials)](#기본)
1. [기초 요소 (Primitives)](#기초-요소)
1. [초심자 문법 (Syntax for Beginners)](#초심자-문법)
1. [데이터 (Data)](#데이터)

---

## 기본

|  |  |
| --- | --- |
| Sites | [main](https://racket-lang.org/) [download](https://download.racket-lang.org/) [docs](https://docs.racket-lang.org/) [git](https://github.com/racket) |
| 커뮤니티 (Community) | [packages](https://pkgs.racket-lang.org/) [users@](https://groups.google.com/forum/#!forum/racket-users/) [dev@](https://groups.google.com/forum/#!forum/racket-dev/) [irc](https://racket-lang.org/irc-chat.html) [slack](http://racket-slack.herokuapp.com/) [twitter](https://twitter.com/racketlang) |
| [Running](https://docs.racket-lang.org/reference/running-sa.html) | Put [#lang](https://docs.racket-lang.org/guide/Module_Syntax.html#%28part._hash-lang%29) [racket](https://docs.racket-lang.org/reference/index.html) `Hello, world!` in `hello.rkt` and run racket `hello.rkt` |

---

## 기초 요소

### 숫자

| 숫자 (Numbers) |  |
| --- | --- |
| [리터럴 (Literals)](https://docs.racket-lang.org/reference/reader.html#%28part._parse-number%29) | integer `1` rational `1/2` complex `1 + 2i` floating `3.14` double `6.02e + 23` hex `#x29` octal `#o32` binary `#b010101` |
| 산술 (Arithmetic) | [+](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2B%29%29) [-](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._-%29%29) [*](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2A%29%29) [/](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._%2F%29%29) [quotient](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._quotient%29%29) [remainder](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._remainder%29%29) [modulo](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._modulo%29%29) [add1](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._add1%29%29) [sub1](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._sub1%29%29) [max](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._max%29%29) [min](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._min%29%29) [round](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._round%29%29) [floor](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._floor%29%29) [ceiling](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._ceiling%29%29) [sqrt](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._sqrt%29%29) [expt](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._expt%29%29) [exp](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._exp%29%29) [log](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._log%29%29) [sin](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._sin%29%29) ... [atan](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._atan%29%29) |
| Compare | [=](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._~3d%29%29) [<](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._~3c%29%29) [<=](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._~3c~3d%29%29) [>](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._~3e%29%29) [>=](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._~3e~3d%29%29) |
| Bitwise | [bitwise-ior](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._bitwise-ior%29%29) [bitwise-and](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._bitwise-and%29%29) [bitwise-xor](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._bitwise-xor%29%29) [bitwise-not](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._bitwise-not%29%29) [arithmetic-shift](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._arithmetic-shift%29%29) [integer-length](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._integer-length%29%29) |
| Format | [number->string](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._number-~3estring%29%29) [string->number](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28quote._~23~25kernel%29._string-~3enumber%29%29) [real->decimal-string](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._real-~3edecimal-string%29%29) |
| Test | [number?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._number~3f%29%29) [complex?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._complex~3f%29%29) ... [exact-nonnegative-integer?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._exact-nonnegative-integer~3f%29%29) ... [zero?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._zero~3f%29%29) [positive?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._positive~3f%29%29) [negative?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._negative~3f%29%29) [even?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._even~3f%29%29) [odd?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._odd~3f%29%29) [exact?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._exact~3f%29%29) [inexact?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._inexact~3f%29%29) |
| Misc | [random](https://docs.racket-lang.org/reference/generic-numbers.html#%28def._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._random%29%29) |
| Match Pattern | (? [number?](https://docs.racket-lang.org/reference/number-types.html#%28def._%28%28quote._~23~25kernel%29._number~3f%29%29) n) `42` |

### Strings

### Bytes

### Other

---

## 초심자 문법

---

## 데이터
