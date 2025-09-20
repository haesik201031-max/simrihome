# 마음건강 자가검진 — 최소비용 스타터 키트

이 폴더에는 바로 배포 가능한 정적 사이트 파일이 들어 있습니다.
구성: `index.html`(검사선택) · `test-auditk.html`(AUDIT-K) · `result.html`(결과) · `counsel.html`(심층상담 폼).

## 1) 배포 (완전 무료, GitHub Pages)
1. GitHub 계정 생성 → 새 저장소 만들기(예: `mind-check`).
2. 위 4개 파일을 저장소 루트에 업로드.
3. 저장소 **Settings → Pages** → Branch를 `main`으로 지정 → 저장.
4. 수 분 후 `https://사용자명.github.io/mind-check/` 에서 접속.

## 2) 사용법
- 홈(`index.html`)에서 검사를 고르면 AUDIT-K 화면으로 이동합니다.
- 문항 응답 후 버튼을 누르면 점수·범주가 `result.html`에 표시됩니다.
- **온라인 상담** 버튼은 `counsel.html`로 연결됩니다.

## 3) 상담 접수 저장(선택)
Google 스프레드시트에 저장하려면:
1. 구글 드라이브에서 새 스프레드시트 생성 → **확장 프로그램 > Apps Script**.
2. 아래 코드를 붙여넣고 **배포 > 새 배포 > 웹 앱**으로 게시합니다.
   ```js
   const SHEET_NAME='responses';
   function doPost(e){
     const ss=SpreadsheetApp.getActiveSpreadsheet();
     const sh=ss.getSheetByName(SHEET_NAME)||ss.insertSheet(SHEET_NAME);
     const data=JSON.parse(e.postData.contents);
     const headers=['ts','name','phone','age','sex','addr','pw','q1','q2','q3','body','consent'];
     if(sh.getLastRow()===0) sh.appendRow(headers);
     sh.appendRow([data.ts,data.name,data.phone,data.age,data.sex,data.addr,data.pw,data.q1,data.q2,data.q3,data.body,data.consent]);
     return ContentService.createTextOutput(JSON.stringify({ok:true})).setMimeType(ContentService.MimeType.JSON);
   }
   ```
3. 배포 후 나온 **웹 앱 URL**을 `counsel.html`의 `ENDPOINT` 값에 붙여넣고 저장.

## 4) 문항 추가
- 다른 검사(PSS/PHQ-9/IES-R/ISI-K/CAPE-15)는 `test-auditk.html`을 복사해 `test-pss.html` 등으로 만든 뒤 문항·배점만 바꾸면 됩니다.
- 결과 페이지(`result.html`)는 동일하게 재사용 가능합니다.

## 5) 주의
- 이 스타터는 **스스로 채점 후 화면에만 결과 표시**합니다. 의료적 진단이 아닙니다.
- 개인정보 처리 방침/이용약관 링크를 추후 푸터에 추가하세요.
