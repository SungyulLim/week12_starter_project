# OSS Week 12 — React Hooks Playground (22200619-임성열)


## Project Information
### 프로젝트 소개 
### Starter Project 정보

## Installation
### 설치·실행 방법
### 실행 화면

## Source Code Exploration
### 주요 파일 구조
### useEffect 및 useRef 분석
### 주요 코드 2개 설명

## Lab Implementation
### LAB 1: useEffect Timer
- useState로 카운트 값을 관리하고, useEffect 내부에서 setInterval()을 사용하여 1초마다 카운트가 증가하도록 구현하였다. 
- 함수형 업데이트인 setCount(prev => prev + 1)을 사용하여 이전 상태를 기준으로 값을 증가시키고, Cleanup에서 clearInterval()을 호출하여 타이머를 정리하도록 하였다. 
- 정상 실행 시 화면에 표시된 카운트가 0부터 1초마다 증가하며, 다른 LAB으로 이동하면 타이머가 정리된다.

![lab1 사진](../week12_starter_project/capture/lab1.png)


### LAB 2: Timer Start / Stop / Reset
- running 상태와 이를 의존성으로 갖는 useEffect를 이용하여 타이머의 실행과 정지를 제어하도록 구현하였다. 
- Start 버튼은 running을 true로 변경하여 타이머를 시작하고, Stop 버튼은 false로 변경하여 Cleanup을 통해 타이머를 정지한다. 
- Reset 버튼은 타이머를 정지하고 카운트를 0으로 초기화한다. 정상 실행 시 Start를 누르면 카운트가 증가하고, Stop을 누르면 현재 값에서 멈추며, 다시 Start를 누르면 이어서 증가한다. 
- 실행 중에는 Start 버튼이 비활성화되어 중복 실행을 방지한다.
#### 초기 사진
![lab2 사진](../week12_starter_project/capture/lab2_초기상태.png)
#### Running 사진
![lab2 사진](../week12_starter_project/capture/lab2_running.png)
#### Stopped 사진
![lab2 사진](../week12_starter_project/capture/lab2_stopped.png)
#### Reset 사진
![lab2 사진](../week12_starter_project/capture/lab2_reset.png)


### LAB 3: Input Focus with useRef
- useRef로 생성한 inputRef를 입력창에 연결하여 실제 DOM 요소를 제어하도록 구현하였다. 
- Focus 버튼은 inputRef.current.focus()를 호출하여 입력창으로 포커스를 이동하고, Clear 버튼은 입력값을 빈 문자열로 변경한 뒤 다시 포커스를 설정한다. 
- 정상 실행 시 Focus 버튼을 누르면 바로 입력할 수 있으며, Clear 버튼을 누르면 기존 내용이 지워지고 입력창을 다시 클릭하지 않아도 새로운 내용을 입력할 수 있다.

#### focus/clear 버튼 사진
![lab3 사진](../week12_starter_project/capture/lab3_focus_clear.png)


### LAB 4: Timer with useRef
- useRef에 타이머 ID를 저장하고, start()와 stop() 이벤트 핸들러에서 타이머를 직접 제어하도록 구현하였다. 
- Start 실행 시 이미 저장된 타이머 ID가 있으면 새로운 타이머 생성을 막고, Stop 실행 시 타이머를 해제한 뒤 ref를 null로 초기화한다. 
- Reset은 타이머 정지 후 카운트를 0으로 변경하며, 언마운트 시에는 Effect의 Cleanup으로 타이머를 정리한다. 
- 정상 실행 시 시작·정지·재시작·초기화가 가능하며, 타이머 ID는 ref로 유지하고 화면에 표시되는 카운트와 실행 상태는 state로 관리한다.

#### 초기 사진
![lab4 사진](../week12_starter_project/capture/lab4_초기상태.png)
#### Running 사진
![lab4 사진](../week12_starter_project/capture/lab4_running.png)
#### Stopped 사진
![lab4 사진](../week12_starter_project/capture/lab4_stopped.png)
#### Reset 사진
![lab4 사진](../week12_starter_project/capture/lab4_reset.png)


## Lab Comparison

1. **useEffect의 Dependency Array가 필요한 이유는?**  
- Dependency Array는 Effect가 값의 변화에 따라 다시 실행될지 정하기 위해서 필요하다. 
- 배열을 생략하면 컴포넌트가 커밋될 때 실행되고, 빈 배열 []을 전달하면 최초 마운트 시 실행되며, [count]처럼 값을 넣으면 최초 마운트 및 count 변경이 일어날 때 실행된다. 
- 이를 통해 불필요한 실행을 줄이고 상태 변화에 맞춰 외부 작업을 관리할 수 있다.

2. **Cleanup을 사용하지 않으면 어떤 문제가 발생할 수 있는가?**  
- Cleanup을 사용하지 않으면 기존 타이머나 이벤트 리스너가 제거되지 않아 일어나지 않아도 되는 작업이 계속 실행될 수 있다. 
- 타이머의 경우 Effect가 다시 실행될 때 중복 생성되어 카운트가 예상보다 빠르게 증가하거나, 컴포넌트가 사라진 이후에도 실행될 수 있다. 
- 따라서 Effect가 다시 실행되기 전과 컴포넌트가 언마운트될 때 Cleanup에서 clearInterval() 등을 호출하여 기존 작업을 정리해야 한다.

3. **useState와 useRef의 가장 중요한 차이는?**  
- 가장 중요한 차이는 값의 변경이 다시 렌더링을 유발하는지 여부이다. 
- useState는 setter를 통해 다른 값으로 변경하면 다시 렌더링을 요청하므로 count, running처럼 화면에 반영할 값을 관리하는 데 사용한다. 
- 반면 useRef는 .current를 변경해도 재렌더링을 유발하지 않는다.

4. **LAB 2와 LAB 4의 Timer 관리 방식은 어떻게 다른가?**  
- LAB 2는 running 상태 변화에 따라 Effect가 타이머를 생성하고 Cleanup이 타이머를 정리하며, 타이머 ID를 Effect 내부의 지역 변수에 저장한다. 
- 반면 LAB 4는 start()와 stop() 이벤트 핸들러에서 타이머를 직접 생성하거나 해제하고, 타이머 ID를 useRef에 저장한다. 
- LAB 4의 running은 화면 상태와 버튼 활성화를 관리하며, Effect는 컴포넌트가 언마운트될 때 타이머를 정리하는 역할을 한다. 

## Customizing: 수정 내용 및 파일명


## Deployment: 실제 접근 가능한 Vercel URL

## Weekly Review:
### 새롭게 배운 내용 3가지
### 어려웠던 점과 해결 방법
## AI 활용 내용 및 느낀 점