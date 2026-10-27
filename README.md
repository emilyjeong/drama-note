DRAMA NOTE — 챔니 마스터리스트
Google Apps Script 안내 배너 없이 GitHub Pages에서 실행하는 중드 마스터리스트입니다. 현재 구글 스프레드시트(DRAMA_MASTER, TITLE_STUDY)를 그대로 사용합니다.
GitHub에 올릴 파일
- index.html — 사이트 화면
- sheets-bridge.js — 구글 로그인 및 스프레드시트 읽기·저장
- .nojekyll — GitHub Pages 정적 사이트 설정
세 파일을 저장소 drama-note의 **최상위(root)**에 업로드하세요. ZIP 자체를 올리는 것이 아니라 압축을 풀어 파일을 올리셔야 합니다.
GitHub Pages 켜기
저장소 Settings → Pages → Build and deployment → Deploy from a branch → main → /(root) → Save.

사이트 주소 예: https://깃허브아이디.github.io/drama-note/
처음 한 번: Google 로그인 설정
GitHub Pages만으로는 비공개 시트를 자동으로 읽고 수정할 수 없으므로 Google Sheets API + Google Identity Services OAuth가 필요합니다.
1. Google Cloud Console에서 프로젝트를 만들거나 선택합니다.
2. API 및 서비스 → 라이브러리에서 Google Sheets API를 사용 설정합니다.
3. Google Auth Platform → Branding / Audience에서 앱을 설정합니다. 개인 이용이면 External / Testing으로 진행하고, Test users에 본인의 구글 계정을 추가합니다.
4. Google Auth Platform → Clients → Create client → Web application을 만듭니다.
5. Authorized JavaScript origins에 https://깃허브아이디.github.io를 입력합니다. /drama-note 경로는 넣지 않습니다.
6. 생성된 Client ID (...apps.googleusercontent.com)를 복사합니다. Client secret은 사용하거나 GitHub에 업로드하지 마세요.
7. 새로 배포한 사이트에서 연결 설정을 열고 Client ID와 기존 구글 시트 링크를 입력한 후 저장합니다.
8. Google 로그인 · 데이터 불러오기를 눌러 구글 계정으로 인증합니다.
중요한 안내
- 사이트에 저장하는 **설정(Client ID 및 시트 ID)**은 해당 기기의 브라우저 로컬 저장소에만 저장됩니다. 엑세스 토큰은 메모리에만 보관합니다. 새 휴대폰에서는 설정을 다시 입력해야 합니다.
- OAuth 승인 범위 https://www.googleapis.com/auth/spreadsheets는 해당 스프레드시트 한 개가 아니라, 승인한 계정이 접근할 수 있는 스프레드시트에 대한 범위입니다. 본인이 직접 만든 코드이고 허용 권한을 이해한 경우에만 승인하세요.
- GitHub Pages 사이트는 기본적으로 공개 웹사이트입니다. HTML 파일에 시트의 개인 리뷰, 액세스 토큰, 비밀번호, Client secret을 직접 넣지 마세요. 이 파일에는 그런 값이 들어 있지 않습니다.
- 실제 데이터 저장 전에는 구글 시트 사본을 백업해 두세요.
- OpenAI API는 사용하지 않습니다.
기능
- 전체 / 시청 중 / 다 본 거 / 안 본 거 / ★ 별표
- 한국어·중국어 드라마 검색과 포스터
- 0.5점 단위 별점 (반쪽 별 표시), 리뷰, 상태, 별표 수정
- 마크다운 1회 붙여넣기로 여러 작품 등록 및 기존 작품 부분 수정
- TITLE_STUDY 탭의 중국어 제목 공부 및 한국어·중국어 검색
동작 테스트
- sheets-bridge.js 및 index.html 내부 JavaScript 문법 검사 통과
- 모의 Sheets API로 작품/공부 탭 불러오기, 리뷰 열만 수정, 3.5점 선택, 별표 수정, 마크다운 미리보기·등록을 확인했습니다.
- 실제 Google 계정의 OAuth 인증 및 실사용 시트 저장은 아직 테스트되지 않았습니다. 본인 사이트에 게시 후 확인해 주세요.
주의할 오류
증상	해결할 곳
Google origin_mismatch	OAuth의 Authorized JavaScript origins는 https://계정.github.io (경로 제외)
로그인 화면이 안 나옴	파일 미리보기 대신 GitHub Pages의 실제 HTTPS 주소에서 실행, 팝업 허용 확인
권한 403	Google Sheets API 사용 설정, OAuth 테스트 사용자, 계정의 시트 권한 확인
404	시트 URL과 DRAMA_MASTER/TITLE_STUDY 탭 이름 확인
인증 기간 만료	Google 로그인 · 데이터 불러오기 버튼으로 다시 로그인


Google 공식 자료
- https://developers.google.com/identity/oauth2/web/guides/use-token-model
- https://developers.google.com/workspace/sheets/api/guides/values
- https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
