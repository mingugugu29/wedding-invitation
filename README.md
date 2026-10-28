# 박준우 ♥ 이민지 모바일 청첩장

- 예식장: 아젠토피오레 컨벤션 ([네이버 지도](https://naver.me/5UEFHTK6))
- 시간: 오후 1시

## 폴더 구조

```
index.html        페이지 구조
css/style.css     디자인 (색상·폰트는 맨 위 :root 변수에서 변경)
js/config.js      ★ 청첩장 내용 (이름·날짜·주소·계좌 등) — 대부분 여기만 고치면 됨
js/main.js        동작 (달력, D-day, 갤러리, 복사, 참석 여부 전송)
images/           사진 폴더 (main.jpg = 메인 사진)
```

## 아직 채워야 할 정보 (`js/config.js`의 TODO)

- [ ] 예식 날짜 (시간 13:00은 확정)
- [ ] 양가 부모님 성함
- [ ] 예식장 홀 이름, 도로명 주소
- [ ] 교통·주차 안내
- [ ] 메인 사진, 갤러리 사진
- [ ] 계좌번호 (공개 저장소라면 배포 직전에 입력)
- [ ] 참석 여부 저장 주소 (`rsvpEndpoint`)

## 내 컴퓨터에서 확인하기

`index.html`을 브라우저로 열면 됩니다. 휴대폰 화면으로 보려면 개발자 도구(F12) → 기기 모드를 켜세요.

## GitHub Pages로 배포하기

1. 저장소 **Settings → Pages**
2. Source: **Deploy from a branch**, Branch: **main / (root)** → Save
3. 1~2분 후 `https://<사용자명>.github.io/<저장소명>/` 에서 확인

## 참석 여부 연결하기 (Google 스프레드시트)

GitHub Pages는 서버가 없어서, 응답을 Google 스프레드시트에 저장합니다.

1. Google 스프레드시트를 새로 만들고, 1행에 다음 제목을 입력합니다.
   `제출시각 | 구분 | 참석 | 성함 | 인원 | 식사 | 메시지`
2. 메뉴 **확장 프로그램 → Apps Script**를 열고 아래 코드를 붙여넣습니다.

   ```js
   function doPost(e) {
     const d = JSON.parse(e.postData.contents);
     SpreadsheetApp.getActiveSpreadsheet().getActiveSheet().appendRow([
       d.submittedAt, d.side, d.attend, d.name, d.count, d.meal, d.message,
     ]);
     return ContentService.createTextOutput("ok");
   }
   ```

3. **배포 → 새 배포 → 유형: 웹 앱**, 실행 사용자: **나**, 액세스 권한: **모든 사용자** → 배포
4. 나온 웹 앱 URL을 `js/config.js`의 `rsvpEndpoint`에 넣습니다.

비워두면 테스트 모드로 동작하며, 제출 내용은 브라우저 콘솔에만 표시됩니다.

## 협업 규칙

- 기능별 브랜치: `feature/기능이름` (예: `feature/gallery`)
- 작업 후 Pull Request → 확인 후 `main`에 병합
- PR 설명에 `Closes #이슈번호` 를 쓰면 이슈가 자동으로 닫힙니다
