# Introduce (프로젝트 소개)

### 검색창 구현, 추천 기능 구현 🔗[Github](https://github.com/wanted-pre-onboarding-fe-6th-team2/pre-onboarding-assignment-week-5-1-team-2) 🔗[DemoVideo](https://user-images.githubusercontent.com/93869214/192932507-c920b659-b5c6-491e-af53-16b1b70a1df4.gif)

- 원티드 프리온보딩 기업 협업과제
- 2022.09.27 - 2022.09.29
- 팀 프로젝트
  - Front-End 6명
- Skill Stack
  - React, Emotion, Axios, Vite

---

### 프로젝트 요구사항

- 아래 사이트의 검색영역을 클론하기
  - 🔗 [한국 임상 정보](https://clinicaltrialskorea.com/)
- 질환명 검색시 API 호출 통해서 검색어 추천 기능 구현
  ![img](https://younuk.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F81d5016d-ca92-494c-a90c-5458ffde01c5%2FUntitled.png?table=block&id=a2f141a1-5367-48d7-9f5e-daf6b58277a3&spaceId=72b256b1-ae08-4e70-bb6c-f9c3cad5a793&width=2000&userId=&cache=v2)
- 사용자가 입력한 텍스트와 일치하는 부분 볼드처리
  - 예)
    - 사용자 입력: 담낭
    - 추천 검색어:  **담낭**의 악성 신생물, **담낭**염, **담낭**의 기타 질환, 달리 분류된 질환에서의 **담낭**, 담도 및 췌장의 장애
    - 검색어가 없을 시 “검색어 없음” 표출
- API 호출 최적화
  - API 호출별로 로컬 캐싱 구현
    - 캐싱 기능을 제공하는 라이브러리 사용 금지(React-Query 등)
    - 캐싱을 어떻게 기술했는지에 대한 내용 README에 기술
  - 입력마다 API 호출하지 않도록 API 호출 횟수를 줄이는 전략 수립 및 실행
    - README에 전략에 대한 설명 기술
  - API를 호출할 때 마다 `console.info("calling api")` 출력을 통해 콘솔창에서 API 호출 횟수 확인이 가능하도록 설정
- 키보드만으로 추천 검색어들로 이동 가능하도록 구현
  - 사용법 README에 기술

# Issue / TroubleShooting (문제 & 해결방법 / 트러블슈팅)

## 목차

1. [페어프로그래밍](#1-페어프로그래밍)
2. [키보드 인터렉션 기능 개발](#2-키보드-인터렉션-기능-개발)
3. [관심사 분리와 컴포넌트의 재사용성](#3-관심사-분리와-컴포넌트의-재사용성)

---

## 1. 페어프로그래밍

### 문제 & 해결방법

- 문제
  - 화면 UI / 키보드 인터렉션 기능 구현에 대한 역할을 1명의 다른 팀원과 같이 개발해야했다.
- 해결방법
  1. UI를 빠르게 완성해서 다른 파트 개발하는 분들에게 넘겨줘야하고 클론을 하는 과정이었는데 2명으로 파트를 나누기에는 또 애매하다고 판단해서 고민을 하게되었다.
  2. 개발영역을 나누기 애매한 상황에서 우리는 🔗 [페어프로그래밍 개발 방법론](https://blog.mathpresso.com/mathpresso-%EA%B0%9C%EB%B0%9C%EB%B0%A9%EB%B2%95%EB%A1%A0-1-%ED%8E%98%EC%96%B4-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-pair-programing-f7d07ac323d0)을 사용해보기로 했다.
  3. 물론 생산성 저하라는 단점이 있어 빠르게 개발을 해야하는 부분에서 걸림돌일 수 도 있지만, 같이 고민하고 업무에 대한 집중도가 상승할 수 있다는 장점을 생각해서 페어프로그래밍을 고집해보았다.
  4. 디스코드를 통해 온라인으로 진행했으며, 전략은 팀원이 코드작성은 내가 하는 방식으로 진행했고, 클론코딩 덕분에 그래도 많은 의견이 오고가는 것 없이 효율적으로 작업을 마칠 수 있었다.

---

## 2. 키보드 인터렉션 기능 개발

### 문제 & 해결방법

- 문제
  - 키보드만으로 추천 검색어들로 이동 가능하도록 구현하기
- 해결방법

  1. 리액트에서 키보드 이벤트에 대해서 먼저 알아보기로 했다.
     - 🔗 [키보드 이벤트 종류와 차이점](https://itprogramming119.tistory.com/entry/React-onKeyDown-onKeyUp-onKeyPress-%EC%B0%A8%EC%9D%B4)
  2. onKeyDown을 이용하고 onKeyDown에 동작하는 handleKeyDown 함수를 만들어주기로 했다.

     ```javascript
     const handleKeyDown = (event) => {
       if (hasText) {
         if (
           event.key === "ArrowDown" &&
           searchSuggestList[searchKeyword].length - 1 > selected
         ) {
           setSelected(selected + 1);
           return;
         }

         if (event.key === "ArrowUp" && selected >= 0) {
           setSelected(selected - 1);
           return;
         }
         if (event.key === "Enter" && selected >= 0) {
           handleSuggestionItemClick(
             searchSuggestList[searchKeyword][selected].sickNm
           );
           setSelected(-1);
         }
       }
     };
     ```

     - 키보드 아래, 위 화살표를 눌렀을 때 이동할 수 있도록 구현해주었다.

  3. 검색어가 아래에 나왔을 때 해당 검색어들은 div > ul > li 형태이며 li 태그에 접근하기 위해 DOM Selector 함수를 사용해서 DOM 을 선택하는데 react에서는 useRef hook으로 Dom을 접근할 수 있다.
  4. useRef hooks를 이용해 ref로 children에 접근하여 div tag를 클릭하지 않고 li tag에 있는 값에 접근할 수 있다.
  5. 최종적으로 검색어 입력 후 화살표 아래 방향키를 누르면 추천 검색어 리스트로 이동할 수 있도록 키보드 인터렉션을 구현했다.

---

## 3. 관심사 분리와 컴포넌트의 재사용성

### 문제 & 해결방법

- 문제
  - 검색어 창이 열릴때 한 화면에서 최근 검색어와 추천 검색어가 보여지는데 이를 한 컴포넌트로 관리하는게 좋을지 생각해보았다.
  - ![검색창](https://user-images.githubusercontent.com/83898103/200758186-e23790b7-f844-41dc-8792-ecb80a4686ae.jpg)

- 해결방법
  1. 한 화면에 보여지기에 한 컴포넌트 내에서의 작동함을 생각하여 개발했다. 그 이유에는 추천 검색어는 단시 화면에만 보여지는 스타일에 대한 코드기 때문에 추후 수정이 없다고 판단했고 이전 회고에서 했던 유지보수를 위한 코드작성을 간과했다.
  2. 실제 서비스였다면 최근 검색어와 추천 검색어는 엄연히 다른 기능을 하고 지속적으로 유지보수를 한다면 각자 컴포넌트를 만들어 관리하는 편이 낫다고 판단했다.
  3. 최종적으로 최근 검색어와 추천 검색어가 다루는 관심사가 다르다고 판단해 컴포넌트를 분리하여 구현했다.
     - `searchResult/RecentResult`, `searchResult/SuggestionResult`

# Retrospective (회고)

### 페어 프로그래밍을 해보면서

### 관심사의 분리에 대한 고민
