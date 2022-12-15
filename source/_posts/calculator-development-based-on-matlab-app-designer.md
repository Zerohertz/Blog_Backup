---
title: MATLAB App Designer 기반 계산기 GUI 개발
date: 2022-12-13 23:26:46
categories:
- MATLAB
tags:
- MATLAB
mathjax: true
---
# Introduction

> 앱 디자이너는 앱 레이아웃을 설계하고 앱 동작을 프로그래밍할 수 있는 대화형 방식의 개발 환경입니다. 앱 디자이너는 완전히 통합된 MATLAB® 편집기와 다양한 대화형 UI 구성요소를 제공합니다. 또한, 사용자 인터페이스를 구성하는 그리드 레이아웃 관리자와 앱이 화면 크기의 변화를 감지하여 그에 맞게 반응하도록 하는 자동 재배치 옵션도 제공합니다. 앱 디자이너를 사용하면 앱 디자이너 툴스트립에서 바로 앱을 인스톨러 파일로 패키징하거나 독립 실행형 데스크탑 또는 웹 앱을 생성하여(MATLAB Compiler™가 필요함) 앱을 배포할 수 있습니다. [Ref](https://kr.mathworks.com/help/matlab/app-designer.html)

<img width="353" alt="Start App Designer" src="https://user-images.githubusercontent.com/42334717/207360037-0e4cd914-728f-4884-a911-5f4a7583deb8.png">

<!--More -->

***

# 계산기 GUI 개발을 위한 조건

+ Input
  + 숫자 입력 창
  + $+$, $-$, $\times$, $\div$, $=$
+ Output
  + 결과 출력 창

***

# 개발 단계 수립

1. 숫자 입력 창 개발
2. 사칙연산 버튼을 입력하면 Output 창은 0으로 초기화되고 선입선출로 계산되는 함수 개발

***

# 개발 시작!

## 숫자 입력 창 개발

<img width="1367" alt="숫자 입력 창" src="https://user-images.githubusercontent.com/42334717/207373754-1a069ac2-9fa6-4985-8ba1-a8e4454f7790.png">

> 등호를 입력했을 때 입력 값을 저장하고 결과 출력 창에 출력 후 입력 창 초기화

<img width="527" alt="현재 숫자 계산 속성 추가" src="https://user-images.githubusercontent.com/42334717/207365522-b004d224-a58c-43cd-83a8-8dc471f91b17.png">

~~~MATLAB
    properties (Access = private)
        tmp % Description
    end
~~~

+ `app.tmp`를 통해 변수 사용 가능

<img width="531" alt="Callback 추가_1" src="https://user-images.githubusercontent.com/42334717/207372215-6811c759-74f3-4e47-945d-0ed6a8f26713.png">

<img width="401" alt="Callback 추가_2" src="https://user-images.githubusercontent.com/42334717/207372125-5d54618c-8fa9-4d93-8dda-3adf12e706b8.png">

~~~MATLAB
        % Value changed function: InputEditField
        function InputEditFieldValueChanged(app, event)
            value = app.InputEditField.Value;
            app.tmp = value;
        end

        % Button pushed function: ButtonEq
        function ButtonEqPushed(app, event)
            app.OutputEditField.Value = app.tmp;
        end
~~~

<img width="1380" alt="Eq Button Click" src="https://user-images.githubusercontent.com/42334717/207373312-ef7d5784-d57f-49ab-94f0-be2a7849d415.png">

## 사칙연산 버튼을 입력하면 Output 창은 0으로 초기화되고 선입선출로 계산되는 함수 개발

~~~MATLAB
        % Code that executes after component creation
        function startupFcn(app)
            app.tmp = 0;
        end

...

        % Button pushed function: ButtonPlus
        function ButtonPlusPushed(app, event)
            app.tmp = app.tmp + app.InputEditField.Value;
            app.InputEditField.Value = 0;
            app.OutputEditField.Value = app.tmp;
        end

        % Button pushed function: ButtonMinus
        function ButtonMinusPushed(app, event)
            app.tmp = app.tmp - app.InputEditField.Value;
            app.InputEditField.Value = 0;
            app.OutputEditField.Value = app.tmp;    
        end

        % Button pushed function: ButtonTimes
        function ButtonTimesPushed(app, event)
            app.tmp = app.tmp * app.InputEditField.Value;
            app.InputEditField.Value = 0;
            app.OutputEditField.Value = app.tmp;
        end

        % Button pushed function: ButtonDiv
        function ButtonDivPushed(app, event)
            app.tmp = app.tmp / app.InputEditField.Value;
            app.InputEditField.Value = 0;
            app.OutputEditField.Value = app.tmp;
        end
~~~

+ `InputEditFieldValueChanged(app, event)` 삭제