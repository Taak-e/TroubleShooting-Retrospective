# Introduce (프로젝트 소개)
### Book-ing (북-잉) 🔗[Github](https://github.com/Book-ing) 🔗[Page](https://www.book-ing.co.kr/)

- 독서 스터디 온 / 오프라인 모임 웹 서비스

# Issue & TroubleShooting
- 목차
  1. [다중 선택 카테고리](#다중-선택-카테고리)
  2. [스터디 노트 WYSIWYG 에디터 적용](#스터디-노트-wysiwyg-에디터-적용)

## 다중 선택 카테고리

### 문제 & 해결방법
 - 문제
    - 검색 페이지에 사용 될 다중 선택이 가능한 버튼별 / 클릭상태별 CSS 디자인이 다른 카테고리 버튼을 개발해야했다.  
  
    ![다중카테고리](https://user-images.githubusercontent.com/83898103/197116027-2695d132-da1e-45b4-8243-a255444e12f7.jpg)
 
 - 해결방법
    1. 먼저 이 카테고리를 스터디 생성시에 동일하게 사용하기 위해 CSS를 컴포넌트화 활 필요성이 있다고 판단해 CSS-in-JS 라이브러리 중 많은 정보가 있는 Styled Components를 사용했다.
    2. 체크 유무를 `checkedList` 라는 변수명으로 상태관리하고 체크상태의 카테고리의 value 를 검색시 데이터 요청할 수 있도록 배열로 만들어 관리하기로 했다.  
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
    3. 해당 카테고리는 스터디를 생성할 때 재활용성을 생각해서 CSS-in-JS의 장점인 CSS를 문서 레벨이 아닌 컴포넌트 레벨로 추상화했다.(모듈성) 

## 스터디 노트 WYSIWYG 에디터 적용

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

# Restropective (회고)
