# Introduce
### Book-ing (북-잉) 🔗[Github](https://github.com/Book-ing) 🔗[Page](https://www.book-ing.co.kr/)

- 독서 스터디 온 / 오프라인 모임 웹 서비스
- 2022.04.22 - 2022.06.03
- 팀 프로젝트
  - Front-End 2명
  - Back-End 3명
  - Desinger 2명
- Skill Stack
  - React, Redux, Axios, ToastUI, AWS S3, Amplify, Socket.io, WebRTC, Styled-components

# Issue & TroubleShooting
## 목차
  1. [다중 선택 카테고리](#1-다중-선택-카테고리)
  2. [스터디 노트 WYSIWYG 에디터 적용](#2-스터디-노트-wysiwyg-에디터-적용)
  3. [카카오 검색API를 이용한 도서 검색 기능](#3-카카오-검색api를-이용한-도서-검색-기능)
  4. [사용자 권한에 따른 화면 구성하기](#4-사용자-권한에-따른-화면-구성하기)
  5. [좌표변환 관련 속도 개선]

## 1. 다중 선택 카테고리

### 문제 & 해결방법
 - 문제
    - 검색 페이지에 사용 될 다중 선택이 가능한 버튼별 / 클릭상태별 CSS 디자인이 다른 카테고리 버튼을 개발해야했다.  
  
    ![다중카테고리](https://user-images.githubusercontent.com/83898103/197116027-2695d132-da1e-45b4-8243-a255444e12f7.jpg)
 
 - 해결방법
    1. 먼저 이 카테고리를 스터디 생성이라는 다른 기능에서도 활용하여 재활용성을 높이기 위해 CSS를 컴포넌트화 활 필요성이 있다고 판단해 CSS-in-JS 라이브러리 중 많은 정보가 있는 Styled Components를 사용했다.
    2. 체크 유무를 `checkedList` 라는 변수명으로 상태관리하고 체크상태의 카테고리의 value 를 검색시 데이터 요청할 수 있도록 배열로 만들어 관리하기로 했다.  
       ```javascript
       const [checkedList, setCheckedList] = useState([]);
        const onCheckedElement = (checked, item) => {
          if (checked) {
            setCheckedList([...checkedList, item]);
          } else if (!checked) {
            setCheckedList(checkedList.filter((el) => el !== item));
          }
        };
        ```
    3. 해당 카테고리는 스터디를 생성할 때 재활용성을 생각해서 CSS-in-JS의 장점인 CSS를 문서 레벨이 아닌 컴포넌트 레벨로 추상화했다.(모듈성) 

## 2. 스터디 노트 WYSIWYG 에디터 적용

### 문제 & 해결방법
  - 문제
    - 스터디 모임이 진행 중이거나 후에 웹서비스 내에서 편리하게 스터디 내용을 정리할 수 있는 노트기능을 만들어야 했다. 
    
    ![스터디 노트 웹에디터](https://user-images.githubusercontent.com/83898103/197129595-66d1b8ac-3d50-461c-8b13-18f950446014.jpg)

   - 해결방법
      1. 스터디한 내용을 정리할 목적으로 사용한다면 텍스트 하나보다는 이미지 삽입이나 표 등 다양한 편집기능이 있으면 좋겠다는 생각을 통해 웹에디터를 적용하기로 했다.
      2. react-quill, react-draft-wysiwyg, ToastUI 등 다양한 리액트 라이브러리 중 ToastUI 에디터를 사용하기로 했다.
      3. 직관적인 UI, 서비스의 주 사용언어가 한글인 점에 따라 한글에 맞춤화 된 에디터, 공식문서의 한글화, 독서 스터디임에 사용자가 어려워하지 않는 wysiwyg 에디터를 위해 ToastUI 선택의 이유였다.
      4. TaostUI 에디터를 적용하는데 큰 문제가 없었지만 이미지 파일을 첨부시 base64 형태의 인코딩된 파일을 DB에 바로 저장하는 것에 대한 의심이 생겼고 트러블슈팅으로 가져가게 되었다. 
      5. 결론적으로 이미지 파일을 저장 관리할 S3 서버를 이용해 URL 주소값만을 DB에 저장 할 수 있게 만들었고, 이미지 파일에 따라 다르지만 2MB ~ 500KB의 용량의 파일의 저장테스트를 통해 평균 10배 정도의 용량을 줄여 DB의 부담을 완화 할 수 있었다.


### 트러블슈팅
- 33%나 데이터의 크기가 증가하고, 인코딩과 디코딩의 추가 연산까지 필요한 Base64 형태의 인코딩을 하는 이유
  - HTML 또는 Email과 같이 문자를 위한 Media에 Binary Data를 포함해야 될 필요가 있을 때, 포함된 Binary Data가 시스템 독립적으로 동일하게 전송 또는 저장되는 걸 보장하기 위해 사용
- 보다 효율적으로 DB에 저장하기 위해서는 크게 두 가지 방법이 존재했다.
  - File API를 통한 base64 형태의 파일 용량을 리사이징해서 DB에 저장한다.
  - 프론트에서 파일 저장을 위한 서버를 운영하면서 저장시 URL 주소값만 DB에 저장한다. 
- 개발 당시에는 용량에 중점을 두어 리사이징 했을때보다 URL 주소값을 저장하는 것이 더 큰 효율을 가져다준다고 판단했다.
- AWS S3를 이용해 이미지 파일을 저장할 수 있도록 서버를 구축하고 AWS SDK를 이용해 S3를 제어하기로했다.
- 이후 문제는 만약 사용자가 이미지를 에디터에 추가하고 삭제 후 다시 이미지를 업로드하면 사용하지 않지만 S3 서버에는 파일 데이터가 계속 쌓인다.
- 검색을 통해 이를 해결할 방법을 모색해보았고 추후 업데이트 시 수정하려고 한다.

![트러블슈팅](https://user-images.githubusercontent.com/83898103/197147293-beab542c-31a0-43a0-8534-3cebf720e6cf.jpg)

## 3. 카카오 검색API를 이용한 도서 검색 기능

### 문제 & 해결방법
  - 문제
    - 독서 스터디 모임 서비스를 구축하는 만큼 도서의 정보도 중요하다고 판단해서 도서의 이미지, 가격 등 세부 정보를 우리 서비스 내에서 편리하게 찾을 수 있도록 함이 필요하다고 생각했다. 
  
    <p align="center"><img src="https://user-images.githubusercontent.com/83898103/197445127-5e0086aa-1a91-4af0-b6da-73017c7cc035.gif"</p>


  - 해결방법
    1. 독자적으로 책 정보에 대한 DB를 구축하는 것은 불가능하기 때문에 오픈 API를 찾아보았다. 
    2. 우리 프로젝트에서는 책 검색에 대한 API 말고도 지도, 로그인에 대해서도 사용성이 있기때문에 이를 모두 부합하는 카카오의 오픈API를 사용하기로 했다.
    3. 카카오 책 검색API에서는 ISBN, 출판사, 저자 정보에서 키워드로 검색한 도서의 검색 결과를 제공한다.
    4. 키워드를 검색했을때 이미지를 기반으로 제목, 내용, 기타 정보를 보여주고 해당 도서를 선택하면 그 정보가 스터디 모임에 같이 저장될 수 있도록 스터디를 개설할 때 DB에 저장시켜 줄 수 있도록 기능을 개발했다.

## 4. 사용자 권한에 따른 화면 구성하기

### 문제 & 해결방법
  - 문제
    - 메인페이지
      - 비로그인시
      ![메인_1](https://user-images.githubusercontent.com/83898103/197461327-1bff204e-5dd1-49d5-8ff9-2f584872fc83.jpg)

      - 로그인시
      ![메인_2](https://user-images.githubusercontent.com/83898103/197461345-960b027c-3c55-4be1-9850-a2a2678881ae.jpg)

      - 보여줄 내용이 여러가지인 서비스를 구성하다보니 실사용자의 편의를 위해 로그인 / 비로그인 상태에 대한 화면 구성을 달리하는게 좋겠다고 생각했다.
  
    - 스터디 상세페이지
      - 스터디 생성 userId와 로그인 한 userId가 같을 때
      ![스터디노트_1](https://user-images.githubusercontent.com/83898103/197460924-fe620c21-b437-4be2-95d2-472336539cea.jpg)
      
      - 스터디가 열리는 시간 기준 24시간이 지났을 때 
      ![스터디노트_2](https://user-images.githubusercontent.com/83898103/197460931-e9ec3c57-08db-46ce-9b69-fe1a6e81f18e.jpg)
      
      - 스터디 생성 userId와 로그인 한 userId가 다를 때 
      ![스터디노트_3](https://user-images.githubusercontent.com/83898103/197460941-6d508626-9b3d-416b-85da-d9a8d883ed88.jpg)

      - 스터디가 진행됨에 따라 / 모임장이고 아님에 따라 필요한 기능을 제한해서 모임장의 역할을 조금 더 명확히 하려고 했다.

  - 해결방법
    - 메인페이지
      ```javascript
      // 로그인, 비로그인 상태에서 required flag를 통하여 판별하여 데이터 렌더링
      export const instance = axios.create({
        baseURL: targetServer,
        headers: {
          "content-type": "application/json;charset=UTF-8",
          accept: "application/json,",
        },
      });

      // 로그인 상태가 필수로 요구되는 API 요청시 사용하는 instance
      export const requiredInstance = axios.create({
        baseURL: targetServer,
        headers: {
          "content-type": "application/json;charset=UTF-8",
          accept: "application/json,",
        },
      });

      // formdata 로 넘길 때 쓰일 인스턴스 (모임방 생성)
      export const formDataInstance = axios.create({
        baseURL: targetServer,
        body: {
          "content-type": "multipart/form-data",
        },
      });

      // 토큰이 필요없는 요청에 쓰일 인스턴스 (로그인)
      export const loginInstance = axios.create({
        baseURL: targetServer,
        headers: {
          "content-type": "application/json;charset=UTF-8",
          accept: "application/json,",
        },
      });

      instance.interceptors.request.use((config) => {
        const accessToken = getCookie("accessToken");
        config.headers.common["Authorization"] = `Bearer ${accessToken}`;
        config.headers.common["required"] = "0";
        return config;
      });

      requiredInstance.interceptors.request.use((config) => {
        const accessToken = getCookie("accessToken");
        config.headers.common["Authorization"] = `Bearer ${accessToken}`;
        config.headers.common["required"] = 1;
        return config;
      });

      formDataInstance.interceptors.request.use((config) => {
        const accessToken = getCookie("accessToken");
        config.headers.common["Authorization"] = `Bearer ${accessToken}`;
        config.headers.common["required"] = 1;
        return config;
      });

      loginInstance.interceptors.request.use((config) => {
        const accessToken = getCookie("accessToken");
        config.headers.common["Authorization"] = `Bearer ${accessToken}`;
        return config;
      });
      ```
      1. api 요청은 이와 같이 axios 인스턴스를 생성해서 로그인, 비로그인, 토큰 유무에 따라 구분하여 쓸 때 없는 요청을 줄였다. 
      2. 메인페이지에서 userId가 있고 없음에 필요한 화면구성을 다르게 보이도록 로직을 구성했다.
      3. userId에 따라 화면구성이 달라진다는 것은 조건부 렌더링으로 컴포넌트의 분리를 충분히 고려했어야하는데 당시 동작 구현에 급급해 고려하지 못한 부분이 있다.
      4. 컴포넌트 분리가 안되어 삼항연산자로 조건부 렌더링시 코드가 길어지고 가독성이 떨어지지만 리팩토링 과제로 두고 기능구현을 마무리했다.

    - 스터디 상세페이지
      1. 스터디 상세페이지에서 노트를 관리할 수 있는 권한을 모임장에게 부여하기 위해 해당 스터디를 만든 userId와 로그인한 userId를 비교하는 방식을 선택했다.
      2. 보여줄 화면의 기준은 스터디가 시작되는 시간이 우선이고 24시간이 지나면 스터디장의 권한과 상관없이 작성할 수 없는 페이지로 바뀐다.
      3. 24시간 이내라면 userId를 비교해서 스터디장의 Id와 일치한다면 작성할 수 있는 버튼을 보여준다.
      4. 아니라면 일반 회원임으로 안내문구를 보여주어 스터디장의 권한임을 알려준다.
      5. 시간에 대한 상태변화는 백엔드로부터 A와 B의 상태로 구분값을 요청했고 이에따라 먼저 24시간 이내에 대한 구분을 하고 이후 userId에 대한 비교를 했다.
      6. 삼항연산자의 중첩이 당장의 기능 구현에 대해서는 좋은 선택이었으나 가독성과 후의 리팩토링을 생각해보면 좋은 코드가 아님을 인지한 상태였지만 시간의 문제로 중첩에 대한 코드는 사용하면서 기능 구현을 마쳤다. 

# Restropective (회고)
