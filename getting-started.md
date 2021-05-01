# 시작하기 (Getting Started)

Racket을 사용하기 전, 웹페이지에서 [다운로드](https://racket-lang.org/download/) 후 설치해야 합니다. 만약 초심자이거나 graphical environment을 사용해야 할 경우, DrRacket executable를 실행합니다. 그렇지 않을 경우, racket executable은 커맨드라인 Read-Eval-Print-Loop([REPL](https://docs.racket-lang.org/guide/intro.html#%28tech._repl%29))을 실행할 것입니다.

윈도우 환경에서는, 시작 메뉴에서 바로가기 메뉴를 통해 DrRacket을 실행할 수 있습니다. Windows Vista 혹은 신버전에서는 DrRacket을 검색하기만 해도 됩니다. 또한, 폴더에서 `Program Files -> Racket -> DrRacket` 경로를 통해서도 실행 가능합니다.

맥 OS 환경에서는, 애플리케이션 폴더 안에 드래그한 Racket 폴더 안에 있는 DrRacket 아이콘을 더블클릭합니다. DrRacket 앱 대신 커맨드라인을 사용하고 싶을 경우에는 Racket 폴더 내의 `bin` 디렉토리에서 Racket executables를 실행합니다 (환경변수(PATH)를 설정하고자 하는 경우에는 이 과정을 직접 실행해야 합니다).

유닉스(리눅스 포함) 환경에서는, drracket executable가 path에 있을 경우 커맨드라인에서 직접 실행할 수 있습니다. 이는 보통 DrRacket을 설치할 때 유닉스 스타일 분류(Unix-style distribution)를 선택한 경우에 해당합니다. 그렇지 않을 경우, Racket distribution이 설치된 위치로 이동하여 `bin` 서브디렉토리에서 drracket executable을 찾을 수 있습니다.

개발이 처음이거나 배우기 위해 교재를 둘러볼 인내심이 있는 사람들에게 추천합니다:

- [How to Design Programs, Second Edition](http://htdp.org/): 시작하기에 좋은 교재입니다.
- [Continue: Web Applications in Racket](https://docs.racket-lang.org/continue/index.html): 모듈(module)과 웹 어플리케이션(web application)을 소개해줍니다.
- [Racket 가이드](https://docs.racket-lang.org/guide/index.html): 교재의 learning-oriented languages에서 설명하지 않은 Racket 언어의 나머지 부분을 설명합니다. 교재에서 함수형 프로그래(functional programming)을 이미 배웠으므로 가이드의 챕터 1과 2는 쉽게 넘어갈 수 있을 겁니다.

개발자이거나 시간이 많지 않은 사람들에게 추천합니다:

- [Quick: 그림과 함께 Racket 시작하기](/튜토리얼/introduction.md): Racket의 맛보기입니다.
- [More: Systems Programming with Racket](https://docs.racket-lang.org/more/index.html): 더 빠르고 깊게 Racket을 알아봅니다. 지나치다는 생각이 들면 그냥 넘어가도 괜찮습니다.
- [Racket 가이드](https://docs.racket-lang.org/guide/index.html): Racket의 기초에 대한 튜토리얼으로 시작하여 전체적인 Racket 언어에 대해 설명합니다.

위의 두 추천 목록은 서로 다른 내용을 포함하므로 섞어서 공부해도 상관없습니다.
