# 💕 우리 둘이서 — 배포 가이드

이 가이드를 따라 하면 **30분 안에** 둘이 함께 쓰는 진짜 홈페이지가 생겨요. **전부 무료**입니다.

---

## 📋 준비물

- Google 계정 (Firebase 용)
- GitHub 계정 또는 Netlify/Vercel 계정 (호스팅 용)
- `index.html` 파일

---

## 1단계. Firebase 설정 (10분)

데이터를 둘이서 공유하려면 Firebase (구글의 무료 데이터베이스) 가 필요해요.

### 1-1. 프로젝트 만들기
1. [Firebase 콘솔](https://console.firebase.google.com) 접속 → Google 로그인
2. **"프로젝트 추가"** 클릭
3. 프로젝트 이름 입력 (예: `우리둘이서`) → 계속
4. Google Analytics: **사용 안 함** 선택 → 만들기
5. 1~2분 기다리면 완성!

### 1-2. Firestore 데이터베이스 만들기
1. 좌측 메뉴에서 **"Firestore Database"** 클릭
2. **"데이터베이스 만들기"** 클릭
3. **테스트 모드로 시작** 선택 → 다음
4. 위치: `asia-northeast3 (서울)` 선택 → 사용 설정

### 1-3. 보안 규칙 설정 ⚠️ 중요!
테스트 모드는 30일 후 만료되니까 영구 규칙로 바꿔야 해요.

1. Firestore Database 페이지에서 **"규칙"** 탭 클릭
2. 아래 내용으로 **전체 교체** → **게시**

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /couples/{coupleCode} {
      allow read, write: if true;
      match /{document=**} {
        allow read, write: if true;
      }
    }
  }
}
```

> 💡 이 규칙은 누구든 커플 코드만 알면 데이터를 읽고 쓸 수 있게 해요. 코드를 추측하기 어렵게 만들면 (예: `sweet-honey-1234`처럼 길게) 외부인이 접근할 수 없어요.

### 1-4. 웹 앱 등록하고 설정값 복사
1. 프로젝트 개요 (좌측 상단 ⚙️ → 프로젝트 설정)
2. "내 앱" 영역에서 **`</>` (웹)** 아이콘 클릭
3. 앱 닉네임 입력 (아무거나, 예: `web`) → **앱 등록**
4. 표시되는 `firebaseConfig` 코드를 통째로 복사!

```javascript
// 이런 거 나와요:
const firebaseConfig = {
  apiKey: "AIzaSyB...",
  authDomain: "우리둘이서-12345.firebaseapp.com",
  projectId: "우리둘이서-12345",
  storageBucket: "우리둘이서-12345.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc..."
};
```

### 1-5. index.html에 붙여넣기
1. `index.html` 파일을 텍스트 편집기로 열기 (메모장, VSCode, 뭐든 OK)
2. `firebaseConfig` 라는 부분 찾기 (Ctrl+F)
3. 위 4번에서 복사한 본인 설정으로 **통째로 교체**
4. 저장!

---

## 2단계. 인터넷에 올리기 (5분)

### 방법 A: Netlify Drop (제일 쉬움 ⭐ 추천)

1. [app.netlify.com/drop](https://app.netlify.com/drop) 접속
2. `index.html` 파일을 **창에 드래그&드롭**
3. 끝! 30초 안에 `https://랜덤이름.netlify.app` URL 생성됨
4. (선택) 가입하면 URL 이름을 바꿀 수 있어요 (예: `우리둘이서.netlify.app`)

### 방법 B: Vercel
1. [vercel.com](https://vercel.com) 가입 (GitHub 로그인이 편함)
2. "Add New Project" → "Browse" → `index.html` 업로드
3. Deploy 클릭

### 방법 C: GitHub Pages
1. GitHub 에 새 저장소 만들기 (예: `our-app`, public)
2. `index.html` 업로드
3. Settings → Pages → Branch: `main`, Folder: `/` → Save
4. `https://본인아이디.github.io/our-app/` 으로 접속

---

## 3단계. 사용하기! 💕

1. 배포된 URL 에 첫 접속하면 "커플 코드 만들기" 화면이 나와요
2. **"새 코드 만들기"** 클릭 → 자동으로 생성된 코드 확인 (예: `sweet-honey-1234`)
3. **상대방에게 그 코드를 보내주세요!** (카톡으로 URL + 코드)
4. 상대방은 같은 URL 접속 → **"코드 받았어요"** → 받은 코드 입력
5. 이제 둘이 같은 데이터를 공유! 🎉
6. 모바일에서 URL 접속하면 브라우저 메뉴 → **"홈 화면에 추가"** 하면 진짜 앱처럼 써요

---

## 🔧 자주 묻는 질문

**Q. 한 사람이 추가하면 상대방 화면에 바로 보이나요?**
A. 새로고침하면 동기화돼요. (실시간 자동 업데이트는 추후 추가 가능)

**Q. 무료로 얼마나 쓸 수 있어요?**
A. Firebase 무료 한도가 매일 50,000 읽기 / 20,000 쓰기에요. 부부 둘이 쓰기엔 평생 무료입니다.

**Q. 다른 사람이 우리 코드를 알면 어떡해요?**
A. 코드를 길고 추측 어렵게 만들면 안전해요. 더 안전하게 하려면 Firebase Authentication 을 추가해야 하는데, 그건 더 복잡합니다.

**Q. 코드 바꾸고 싶어요**
A. 현재 코드로 모든 데이터를 백업 → 새 코드 만들기 → 수동 복원해야 해요. 데이터가 많이 쌓이기 전에 정하는 게 좋아요.

**Q. 폰에서 자꾸 로그인이 풀려요**
A. 시크릿 모드나 쿠키 삭제 시 풀려요. 일반 브라우저 또는 홈 화면에 추가하면 유지됩니다.

---

## 🐛 문제 생기면

### "연결 중..." 에서 안 넘어가요 / "연결 실패" 에러

거의 항상 이 3가지 중 하나예요:

1. **Firestore Database 를 안 만들었어요** (가장 흔함!)
   - Firebase 콘솔 → 좌측 메뉴 **Firestore Database** 클릭
   - "데이터베이스 만들기" 버튼이 보이면 → 아직 안 만든 거예요. 1-2단계로 돌아가세요
   - 프로젝트만 만들고 Database 생성을 빠뜨리는 경우가 많아요

2. **보안 규칙을 게시 안 했어요**
   - Firestore Database → **"규칙"** 탭
   - 1-3단계의 규칙이 들어있는지 + 우측 상단 **"게시"** 버튼을 눌렀는지 확인
   - 규칙 편집만 하고 게시 안 하면 적용 안 돼요

3. **firebaseConfig 가 잘못됐어요**
   - index.html 의 `firebaseConfig` 가 아직 `YOUR_API_KEY_HERE` 같은 상태이거나
   - 복사할 때 일부만 복사됐을 수 있어요 → 1-4단계 다시

> 💡 새 버전 index.html 은 8초 후 자동으로 구체적인 에러 메시지를 보여줘요. 그 메시지를 보고 위 항목을 체크하세요.

### 화면이 아예 안 떠요
1. 브라우저 개발자 도구 (F12) → Console 에서 빨간 에러 확인
2. Firebase 설정이 올바른지 다시 확인
3. 브라우저 강력 새로고침 (Ctrl+Shift+R)

---

만든 사람: Claude × 당신 ❤️
