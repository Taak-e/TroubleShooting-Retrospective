# Introduce (프로젝트 소개)

### 이슈 목록, 상세 웹 사이트 🔗[Github](https://github.com/wanted-pre-onboarding-fe-6th-team2/pre-onboarding-assignment-week-3-1-team-2) 🔗[Page](https://github-issue-viewer-team2.netlify.app/)

- 원티드 프리온보딩 기업 협업과제
- 2022.09.13 - 2022.09.15
- 팀 프로젝트 (팀장)
  - Front-End 5명
- Skill Stack
  - React, contextAPI, Axios, Emotion, Material-UI, Vite

---

### 프로젝트 요구사항

- 특정 깃헙 레파지토리(‣)의 이슈 목록과 상세 내용을 확인하는 웹 사이트 구축

- 필수 요구 사항

  - 이슈 목록 및 상세 화면 기능 구현
  - Context API를 활용한 API 연동
  - 지정된 조건(open 상태, 코멘트 많은 순)에 맞게 데이터 요청 및 표시
  - 데이터 요청 중 로딩 표시
  - UI는 데스크톱, 모바일에서 보았을 때 모두 읽기 편하게 구현

- 선택 사항

  - 에러 화면 구현
  - CSS-in-JS 구현

- 과제 범위

  - 이슈 목록 화면

    - 이슈 목록 가져오기 API 활용
    - open 상태의 이슈 중 코멘트가 많은 순으로 정렬
    - 각 행에는 ‘이슈번호, 이슈제목, 작성자, 작성일, 코멘트수’를 표시
    - 다섯번째 셀에는 광고 이미지 출력
    - 광고 이미지 클릭 시 특정 페이지로 이동
    - 화면을 아래로 스크롤 할 시 이슈 목록 추가 로딩(인피니티 스크롤)

  - 이슈 상세 화면

    - 이슈의 상세 내용 표시
    - ‘이슈번호, 이슈제목, 작성자, 작성일, 코멘트 수, 작성자 프로필 이미지, 본문' 표시

  - 공통 헤더
    - 두 페이지는 공통 헤더를 공유합니다.
    - 헤더에는 Organization Name / Repository Name이 표시됩니다.

# Issue / TroubleShooting (문제 & 해결방법 / 트러블슈팅)

## 목차

1. [리스트 내 고정된 셀에 광고 이미지 출력하기](#1-고정된-셀에-광고-이미지-출력하기)
2. [Context API에 대한 이해와 활용](#2-context-api에-대한-이해와-활용)

---

## 1. 고정된 셀에 광고 이미지 출력하기

### 문제 & 해결방법

- 문제

  - 이슈 목록페이지에서 다섯번째 셀에는 광고 이미지 출력하기

    ![issue-list](https://user-images.githubusercontent.com/83898103/199915455-86c1ddda-dbbb-4773-9bb6-340c8b46c5e4.jpg)

- 해결방법
  1. 제공해주는 api에서는 이슈에 대한 정보를 배열 형태로 전달해주기 때문에 고민했던 방법은 배열 정열 함수 중 map 함수를 이용하기로 했다.
  2. 가독성을 높이기 위해 광고와 리스트를 각각 컴포넌트화 해서 상위 컴포넌트에서 두 컴포넌트를 조건문으로 재배열 하도록 코드를 작성했다. (사실상 광고는 4번째 배열 한 군데에 무조건 고정되는 방식)
     ```javascript
     <Box sx={{ border: "4px dashed grey" }}>
       <List sx={{ width: "100%", minWidth: 300, bgcolor: "background.paper" }}>
         {issues.map((issueInfo, number) => {
           if (number === 4) {
             return <ViewAd key={number} />;
           }
           return <ViewListItem issueInfo={issueInfo} key={number} />;
         })}
       </List>
     </Box>
     ```
  3. components/viewList 아래 ViewAd 와 ViewListItem 을 각각 컴포넌트화 해주고 ViewTemplate 라는 컴포넌트로 가져오고 관심사의 분리를 통해 코드의 재사용성과 가독성을 올릴 수 있었다.

---

## 2. Context API에 대한 이해와 활용

### 문제 & 해결방법

- 문제
  - 필수 요구사항으로 Context API를 활용한 API 연동
- 해결방법
  1. Context API를 사용하게 하는 의도를 파악해보고 싶었다.
  2. Why? Context API
     - 과거에는 리액트의 Context가 굉장히 불편해서 전역 상태 관리 라이브러리를 사용하는 것이 당연시 여겨졌던 시절이 있었지만 이제는 사용하기 편해져서 단순히 전역적인 상태를 관리하기 위함이라면 더 이상 사용해야 할 이유가 없다.
     - 단, "상태 관리 라이브러리"와 Context는 완전히 별개의 개념임을 잘 이해하자. Context는 전역 상태 관리를 할 수 있는 수단일 뿐이고, 상태 관리 라이브러리는 상태 관리를 더욱 편하고, 효율적으로 할 수 있게 해주는 기능들을 제공해주는 도구이다.
  3. 전역 상태 라이브러리는 결국 상태 관리를 조금 더 쉽게 하기 위해서 사용하는 것이며 취향에 따라 선택해서 사용하면 되는 것이다.
     - 🔗 [Context API 참조 글](https://velog.io/@velopert/react-context-tutorial#%EA%B0%92%EA%B3%BC-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8-%ED%95%A8%EC%88%98%EB%A5%BC-%EB%91%90%EA%B0%9C%EC%9D%98-context%EB%A1%9C-%EB%B6%84%EB%A6%AC%ED%95%98%EA%B8%B0)

# Retrospective (회고)

### 좋은 코드란 무엇인가?

- 협업을 위한

  - 협업은 소통이 중요하고 개발자의 소통은 곧 코드라고 생각한다. 내가 내 의도를 잘 전달하기위해서는 상대방이 알아 듣기 쉽게 말하는 것이 중요한데 코드도 마찬가지이다.
  - 나 아닌 상대가 알아먹기 쉬운 즉, 읽기 쉬운 코드를 짜는 것이 중요하다고 생각했다.

- 유지보수를 생각하자!

  - 개발이란 한번에 끝나는 것이 아니라 런칭 이후에도 수많은 변경이 필요하기 때문에 기능 개발 만큼 중요한 것이 바로 유지보수이다.

- 그래서 좋은 코드는?

  - 좋은 코드를 짜기 위해서는 유지보수를 생각하며 고민하여 코드를 작성해야한다.
  - 즉, 가독성이 좋아 이해하기 쉽고, 변경하기 쉬우며, 재사용하기 쉬운 효율적인 코드라고 생각한다.

- 좋은 코드를 작성하기 위해서

  - 코딩 컨벤션

    - 한 프로젝트의 코딩 컨벤션이 통일되어야 코드의 가독성이 좋아진다.
    - 🔗 [우리가 프로젝트간 사용한 코딩 컨벤션](https://github.com/wanted-pre-onboarding-fe-6th-team2/pre-onboarding-assignment-week-2-1-team-2/wiki/%EC%BD%94%EB%94%A9-%EC%BB%A8%EB%B2%A4%EC%85%98)

  - 알기 쉬운 이름 짓기

    - 변수, 함수, 클래스명 무엇이든 가장 먼저 누구나 이해할 수 있는 이름으로 이름을 짓는 것이 중요하다.
    - 이름의 길이를 줄이는 것 보다는 이름이 명확한 뜻을 가지고 있는것이 더 중요하다.
    - 🔗 [좋은 이름 짓기에 대한 참조 글_1](https://velog.io/@humonnom/%EB%84%A4%EC%9D%B4%EB%B0%8D-%EC%BB%A8%EB%B2%A4%EC%85%98%EA%B3%BC-%EB%B3%80%EC%88%98%EC%9D%B4%EB%A6%84-%EC%A7%93%EA%B8%B0)
    - 🔗 [좋은 이름 짓기에 대한 참조 글_2](https://soojin.ro/blog/naming-boolean-variables)

  - 반복 줄이기(Don't Repeat Yourself)

    - DRY(Don't Repeat Yourself) 법칙은 SOLID와 함께 가장 유명한 프로그래밍 원칙 중 하나이다. 즉, 똑같은 코드를 절대 Copy&Paste(반복) 하지 말라는 원칙
    - 똑같은 기능을 하는 코드가 중복된다면 해당 코드의 수정이 필요하면 똑같은 모든 코드를 수정해야한다. 이는 사람의 실수를 유발하고 버그 발생을 만드는 가장 큰 요인중 하나이다.
    - DRY 법칙을 지키지 않는 코드는 변경하기 어려운 코드가 되기에 중복을 최대한 배제하는 습관을 들이는게 좋은 것 같다.

  - 파악하기 쉬운 프로젝트 구조를 만들기
    - 기술적인 용도에 따라 폴더를 구분하는 방법
      - 만약 Redux를 쓴다면 Components, Pages, Store, Reducer, Actions, Middleware 를 같은 폴더 구조로 가지고 갈 수있을 것
    - 도메인에 따라 폴더를 분리하는 방법
      - 쇼핑몰을 예로 들자면, 쇼핑몰을 만들기 위해서는 고객정보, 쇼핑 아이템, 주문 정보, 포인트, 결제 방식 등을 만들어야 한다. 이런 각각을 도메인이라 하고 도메인 별로 폴더를 묶어주는 방식을 사용할 수 있다.
      - User, Item, Order, Point, Payment 이런식으로 폴더가 구분될 수 있다

- 좋은 코드는 상황에 따라서 조금씩은 달라 질 수 있겠지만 공통적으로 읽기 쉽게 작성하기라는 바탕이 되는 것을 알 수 있었고, 그 안에서 여러 방법을 통해 읽기 쉬운 코드의 완성도를 높이기 위해 어떤 고민을 해야하는지 이번 협업을 통해 동작만에 치중해 코드를 짜던 나를 조금 성장시키는 계기가 되었다.
