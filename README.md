# StoryForge 집필실

작가가 쓰는 원고와 인물·세계관·연표·복선을 한곳에서 관리하고, AI 편집자가 설정 충돌을 찾아 주는 소설 집필 웹앱입니다.
작품은 **GitHub 저장소**에 커밋으로 저장되므로, 모든 수정이 버전으로 남고 언제든 이전 원고로 돌아갈 수 있습니다.

서버가 필요 없는 정적 웹앱이라 GitHub Pages에 그대로 올려 쓸 수 있습니다.

## 주요 기능

- **집필** — 장과 장면 단위 원고, 시점 인물·장소·작중 시간 지정, 원고지 매수 표시, 브라우저 자동 저장
- **작품 자료** — 인물(비밀·지식 상태), 관계도, 세계관(독자 공개/작가 전용 분리), 장소, 연표(현재 = 0년), 복선, 아이템
- **작가 모드 / 독자 모드** — 독자 모드에서는 비밀·스포일러 관계·미공개 사건을 숨김
- **AI 편집자** — Claude, ChatGPT, Gemini, OpenRouter, OpenAI 호환 주소(DeepSeek · Groq · Mistral · 로컬 Ollama 등) 중에서 골라 씀
  - 선택한 문장 다듬기 · 문체에 맞게 · 감정/묘사/긴장감 강화 · 짧게/길게
  - 설정 검사: 설정 충돌, 개연성, 인물 일관성, 인물 지식 위반을 찾아 목록으로 보여 줌 (원고는 고치지 않음)
  - 전개 제안: 선택지 5개 → 작가가 순서를 고른 뒤에만 초안 작성
  - 문체 프로필 만들기, 뜻으로 찾는 의미 검색
- **AI 없이 동작하는 검사** — 인물 나이 불일치, 금지어
- **GitHub 버전 관리** — 커밋, 불러오기, 버전 기록, 이전 버전 복원, 다른 기기와의 충돌 감지, 10분 자동 커밋(선택)
- **내보내기** — 작품 전체 JSON 백업, 원고 TXT

## 저장소에 저장되는 모습

작품 하나가 폴더 하나입니다. 원고는 Markdown이라 GitHub에서 바로 읽을 수 있고, 커밋마다 어느 문장이 바뀌었는지 diff로 보입니다.

```
<작품 폴더>/
├── project.json                  작품 정보
├── manuscript/
│   ├── ch1.md                    장 하나 = 파일 하나 (장면 여러 개)
│   └── ch2.md
├── characters/
│   ├── iroha.json                인물 한 명 = 파일 하나
│   ├── kaguya.json
│   └── _relationships.json       관계
├── world/lore.json               세계관
├── places/places.json            장소
├── timeline/events.json          연표
├── foreshadowing/clues.json      복선
├── items/items.json              아이템
└── ai/
    ├── style_profile.json        문체 프로필
    └── issues.json               AI 편집 메모
```

장 파일의 생김새:

```markdown
---
id: "ch1"
order: 1
title: "연구소의 소녀"
summary: ""
---

# 연구소의 소녀

<!-- scene: {"id":"sc1","order":1,"title":"문 너머","pov":"iroha",...} -->
## 문 너머

이로하는 연구소의 문을 열었다.
...
```

`<!-- scene: … -->` 줄이 장면의 경계와 정보입니다. GitHub 웹에서 본문을 직접 고쳐도 되지만, 이 줄은 지우지 마세요.

## 시작하기

### 1. 앱 올리기 (GitHub Pages)

1. 이 폴더의 파일을 새 GitHub 저장소(예: `storyforge`)에 올립니다.
2. 저장소 **Settings → Pages → Build and deployment → Source**를 **GitHub Actions**로 바꿉니다.
3. `main` 브랜치에 푸시하면 `.github/workflows/pages.yml`이 배포합니다. 주소는 `https://<아이디>.github.io/storyforge/`입니다.

설치 없이 써 보려면 `index.html`을 브라우저로 바로 열어도 됩니다.
로컬 서버로 열려면 이 폴더에서 `npx serve` 또는 `python3 -m http.server`를 실행하세요.

### 2. 원고를 저장할 저장소 만들기

앱 저장소와 **별도의 비공개 저장소**(예: `my-novel`)를 만드는 것을 권합니다. 비어 있어도 됩니다.
공개 저장소에 저장하면 원고와 비밀 설정을 누구나 볼 수 있습니다.

### 3. 토큰 만들기

GitHub → Settings → Developer settings → **Personal access tokens → Fine-grained tokens → Generate new token**

- Repository access: **Only select repositories** → 원고 저장소 하나만 선택
- Repository permissions → **Contents: Read and write**

### 4. 앱에서 연결

앱의 **작품 설정 → GitHub 저장소**에 소유자, 저장소 이름, 브랜치, (선택) 작품 폴더, 토큰을 넣고 **연결 확인**을 누릅니다.

- 폴더가 비어 있으면 **지금 작품으로 첫 커밋**
- 이미 작품이 있으면 **불러오기**

그 뒤로는 상단의 GitHub 버튼이나 **Ctrl+S (⌘+S)** 로 커밋합니다.

### 5. AI 편집자 켜기 (선택)

**작품 설정 → AI 편집자**에서 제공자를 고르고 키와 모델을 넣은 뒤 **이 AI 쓰기**를 누릅니다.
여러 제공자의 키를 넣어 두면 AI 편집자 패널 맨 위에서 바로 바꿔 쓸 수 있습니다.

| 제공자 | 키 만드는 곳 | 비고 |
|---|---|---|
| Claude (Anthropic) | console.anthropic.com | Messages API |
| ChatGPT (OpenAI) | platform.openai.com/api-keys | Responses API |
| Gemini (Google) | aistudio.google.com/apikey | 무료 사용량이 있음 |
| OpenRouter | openrouter.ai/keys | 키 하나로 여러 회사 모델, 모델 이름은 `회사/모델` |
| OpenAI 호환 주소 | 각 서비스 | 주소 예: `https://api.deepseek.com/v1`, `https://api.groq.com/openai/v1`, `http://localhost:11434/v1`(Ollama) |

- 모델 이름은 자주 바뀌므로, **모델 목록 불러오기**를 눌러 내 키로 쓸 수 있는 모델을 고르세요.
- **연결 확인**은 아주 짧은 요청을 한 번 보내 키와 모델이 맞는지 확인합니다.
- 사용한 만큼 각 키 주인의 계정에 요금이 청구됩니다.
- 로컬 Ollama는 브라우저 요청을 허용해야 합니다. 예: `OLLAMA_ORIGINS="https://<아이디>.github.io" ollama serve`
- OpenAI 호환 서비스 중 일부는 브라우저에서 직접 부르는 것(CORS)을 막습니다. 그런 곳은 OpenRouter를 거쳐 쓰세요.

## 보안 메모

- GitHub 토큰과 AI 키는 **이 브라우저의 localStorage**에만 저장되며, 토큰은 `api.github.com`, AI 키는 그 키를 만든 제공자의 API 주소로만 보냅니다.
- 앱은 서버가 없고, 다른 곳으로 데이터를 보내지 않습니다.
- 공용 컴퓨터에서는 쓰지 말고, 다 쓴 뒤 설정에서 **토큰 지우기 / 키 지우기**를 누르세요.
- 토큰은 원고 저장소 하나에만 권한을 주세요.

## 여러 기기에서 쓰기

기기마다 같은 저장소·폴더로 연결하고, 쓰기 전에 **불러오기**, 다 쓴 뒤 **커밋**하세요.
다른 기기에서 먼저 커밋한 상태로 커밋하려 하면 앱이 멈추고 두 가지 중 하나를 고르게 합니다.

- GitHub 버전 불러오기 (이 브라우저의 변경은 버림)
- 내 내용으로 덮어쓰기 커밋 (GitHub의 이전 커밋은 기록에 남음)

## 파일 구성

```
index.html          화면 틀
css/app.css         디자인
js/format.js        저장 형식 (작품 ↔ Markdown/JSON 파일)
js/github.js        GitHub API (커밋, 불러오기, 기록)
js/ai.js            AI 제공자 연결 (Claude · ChatGPT · Gemini · OpenRouter · OpenAI 호환)
js/app.js           앱 본체
js/sample.js        예시 작품 「달의 기억」
sample/             예시 작품 백업 파일 (.json, 가져오기로 불러올 수 있음)
tests/              저장 형식·커밋 흐름·AI 제공자 테스트 (npm test)
```

빌드 도구나 의존성이 없습니다. 테스트는 Node.js 18 이상에서 `npm test`로 돌립니다(가짜 GitHub·AI 서버를 쓰므로 키가 필요 없음). 푸시하면 GitHub Actions가 테스트를 자동으로 돌립니다. 파일을 고치고 새로 고치면 바로 반영됩니다.
