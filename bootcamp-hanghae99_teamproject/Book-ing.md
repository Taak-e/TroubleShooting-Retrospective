# Introduce (프로젝트 소개)
### Book-ing (북-잉) 🔗[Github](https://github.com/Book-ing) 🔗[Page](https://www.book-ing.co.kr/)

- 독서 스터디 온 / 오프라인 모임 웹 서비스

# Issue & TroubleShooting
- 목차
  1. [다중 선택 카테고리](#다중-선택-카테고리)
  2. [스터디 노트](#스터디-노트)

## 다중 선택 카테고리

### 문제 & 해결방법
 - 문제
    - 검색 페이지에 사용 될 다중 선택이 가능한 버튼별 / 클릭상태별 CSS 디자인이 다른 카테고리 버튼을 개발해야했다.  
  
  ![다중카테고리](https://user-images.githubusercontent.com/83898103/197116027-2695d132-da1e-45b4-8243-a255444e12f7.jpg)
 
 - 해결방법
    1. 먼저 이 카테고리를 스터디 생성시에 동일하게 사용하기 위해 CSS를 컴포넌트화 활 필요성이 있다고 판단해 CSS-in-JS 라이브러리 중 많은 정보가 있는 Styled Components를 사용했다.
    2. 체크 유무를 `checkedList` 라는 변수명으로 상태관리하고 선택된 카테고리만을 검색시 데이터 요청할 수 있도록 배열로 만들어 관리하기로 했다.  
       ```
       const [checkedList, setCheckedList] = useState([]);
        const onCheckedElement = (checked, item) => {
          if (checked) {
            setCheckedList([...checkedList, item]);
          } else if (!checked) {
            setCheckedList(checkedList.filter((el) => el !== item));
          }
        };
        ```
    3. 해당 카테고리는 스터디를 생성할 때 재활용성을 생각해서 CSS-in-JS의 장점인 컴포넌트 레벨로 추상화했습니다.(모듈성) 

### 트러블슈팅

- 트러블슈팅 내용

  `bye wolrd`

## 스터디 노트

### 이슈

- 스터디를 기록 할 수 있는 노트를 개발함에 있어 오픈소스 에디터를 활용하기로 했다.  

### 트러블슈팅

# Restropective (회고)
