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
2. [모바일, PC에서 보았을 때 읽기 편하도록 이슈 목록 페이지 UI개발](#2-이슈-목록-페이지-ui-개발)

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
      <Box sx={{ border: '4px dashed grey' }}>
        <List sx={{ width: '100%', minWidth: 300, bgcolor: 'background.paper' }}>
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

## 2. 이슈 목록 페이지 UI 개발

### 문제 & 해결방법

# Retrospective (회고)

### 팀으로 일하는 법 배우기
