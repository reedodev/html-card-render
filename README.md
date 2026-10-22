<p align="center"><img src="docs/banner.png" alt="카드뉴스 렌더: 글자는 HTML로, 크기는 픽셀까지 정확하게" width="100%"></p>

<p align="center">
<a href="https://github.com/reedodev/html-card-render/actions/workflows/test.yml"><img src="https://github.com/reedodev/html-card-render/actions/workflows/test.yml/badge.svg" alt="test"></a>
<img src="https://img.shields.io/badge/Claude_Code-skill-2547F4" alt="Claude Code skill">
<img src="https://img.shields.io/badge/license-MIT-black" alt="MIT">
</p>

# 카드뉴스 렌더 (html-card-render)

카드뉴스·배너·썸네일을 HTML로 짜서 크롬 개발자 프로토콜(CDP)로 정확한 픽셀 크기의 PNG로 찍고, 찍기 전에 글자 넘침과 깨진 그림을 자동으로 검사합니다. 설치할 패키지가 없습니다.

## 처음 쓰시는 분을 위한 안내

### 이 스킬은 언제 쓰나요?

- 인스타그램 카드뉴스 5장을 만드는데, AI가 그린 그림 속 글자가 깨지거나 이상해서 글자만큼은 정확하게 넣고 싶을 때
- 유튜브 썸네일이나 배너를 정해진 크기(1280x720 등)로 정확히 뽑아야 하는데, 화면 캡처를 하면 크기가 어긋날 때

글자와 배치는 HTML(웹 페이지를 만드는 글)로 짜고, 이 스킬이 그것을 정확한 크기의 PNG 그림으로 찍어 줍니다. 찍기 전에 글자가 상자 밖으로 넘치거나 그림이 깨진 곳이 있는지도 검사하고, 찍은 PNG의 실제 가로세로 크기를 다시 읽어 확인합니다.

### 용어 풀이

| 용어 | 뜻 |
|---|---|
| Claude Code | 터미널에서 AI와 대화하며 파일 작업을 시키는 프로그램입니다. |
| 스킬(skill) | Claude Code에게 "이 일은 이렇게 하라"고 알려 주는 설명서와 도구 묶음입니다. |
| 터미널 | 글자로 명령을 입력하는 창입니다. Windows는 PowerShell, Mac은 터미널 앱입니다. |
| HTML | 웹 페이지를 만드는 글입니다. 글자 크기, 위치, 색을 적어 두면 그대로 그려집니다. |
| PNG | 화질이 깨지지 않는 그림 파일 형식입니다. |
| 배율(scale) | 같은 크기를 몇 배 촘촘하게 찍을지입니다. 1080x1350에 배율 2면 2160x2700 그림이 나옵니다. |
| CDP | 크롬을 프로그램으로 조종하는 공식 방법(Chrome DevTools Protocol)입니다. 이 스킬이 크롬을 몰래 열어 사진을 찍는 데 씁니다. |

### 준비 확인

이 스킬은 따로 설치할 패키지가 없습니다. 아래 두 가지만 있으면 됩니다.

```bash
node -v        # v22 이상이어야 합니다. 없거나 낮으면 https://nodejs.org 에서 최신 LTS를 설치합니다.
```

- **크롬, 엣지, 크로미움 중 하나**가 설치되어 있어야 합니다. Windows와 Mac 모두 보통 Chrome 또는 Edge가 이미 있습니다. 없으면 https://www.google.com/chrome/ 에서 설치합니다. 스킬이 자동으로 찾고, 못 찾으면 실행 파일 경로를 `CHROME_PATH`에 적어 줍니다.
- 템플릿은 글꼴(Pretendard)을 인터넷에서 불러오므로 연결된 상태에서 처음 해 보시길 권합니다. 연결이 없으면 CSS에 적어 둔 다른 한글 글꼴로 대신 그려집니다.

### 따라 하기 3단계

**1단계. 설치합니다.** Claude Code에서 입력합니다.

```
/plugin marketplace add reedodev/html-card-render
/plugin install html-card-render@html-card-render
```

**2단계. 에이전트에게 이렇게 말합니다.** 주제와 글만 내 것으로 바꿔서 복사해 씁니다.

1. `html-card-render로 인스타그램 4:5 카드뉴스 3장을 만들어 줘. 주제는 "아침 10분 정리 습관"이고 흰 바탕에 검은 테두리, 강조색은 파랑 하나만 써 줘.`
2. `렌더하고 글자가 넘치거나 그림이 깨졌다고 나오면 고쳐서 다시 찍어 줘.`
3. `다 되면 PNG 3장의 실제 크기를 알려 주고, 파일 위치도 알려 줘.`

**3단계. 결과를 확인합니다.** 터미널에 `PASS`가 나와도 PNG를 직접 열어 눈으로 한 번 봅니다. 검사는 글자가 넘치는지와 크기만 확인하고, 디자인이 예쁜지는 판단하지 못합니다.

### 직접 실행해 보기

이 저장소에 든 4:5 템플릿(3장짜리)으로 해 볼 수 있습니다. 아래는 이 저장소를 새로 받아 실제로 실행한 결과입니다. 현재 폴더에 `card-01.png`, `card-02.png`, `card-03.png`가 생깁니다.

```bash
node skills/html-card-render/scripts/render.mjs skills/html-card-render/templates/card-4x5.html card.png --size 1080x1350 --scale 2 --pages
```

```
OK   card-01.png  2160x2700 (expected 2160x2700)
OK   card-02.png  2160x2700 (expected 2160x2700)
OK   card-03.png  2160x2700 (expected 2160x2700)
PASS: sizes exact, no overflow found. 이제 PNG를 직접 열어 눈으로 확인하세요.
```

읽는 법은 이렇습니다. 줄마다 `OK`이고 앞의 크기와 `expected`(기대한 크기)가 같으면 크기가 정확합니다. 마지막 줄이 `PASS`이고 종료 코드가 0이면 넘친 글자와 깨진 그림이 없다는 뜻입니다. `--pages`는 HTML 안의 `class="page"`가 붙은 덩어리마다 PNG 한 장씩 만들라는 뜻입니다.

배너는 이렇게 만듭니다.

```bash
node skills/html-card-render/scripts/render.mjs skills/html-card-render/templates/banner.html banner.png --size 1280x720 --scale 1
```

### 막힐 때

| 증상 | 원인 | 해결 |
|---|---|---|
| `FAIL: fix the items above...` 위에 `overflow: h1 - text wider than its box "..."` (종료 코드 1) | 글자가 자기 상자보다 넓어서 잘립니다. PNG는 만들어졌지만 검사에 실패했습니다. | 글자 크기를 줄이거나 문장을 줄인 뒤 다시 렌더합니다. 장식이라 상관없는 요소에는 `data-qa-ignore`를 붙입니다. |
| `input file not found: nofile.html` (종료 코드 2) | HTML 파일 경로가 다릅니다. | 현재 터미널 위치에서 파일이 보이는지 확인합니다. |
| `bad --size or --scale` (종료 코드 2) | `--size`는 `1080x1350`처럼 가로x세로 숫자여야 합니다. | `--size 1080x1350`, `--scale 2` 형식으로 고쳐 씁니다. |
| `usage: node render.mjs <file.html\|url> <out.png> ...` (종료 코드 2) | 입력 파일과 출력 파일 이름을 모두 적지 않았습니다. | `node render.mjs 입력.html 출력.png`처럼 두 개를 적습니다. |
| `ERROR: Chrome/Edge not found. Set CHROME_PATH.` (종료 코드 2) | 크롬, 엣지, 크로미움을 찾지 못했습니다. | 크롬을 설치하거나 `CHROME_PATH`에 실행 파일 전체 경로를 넣습니다(Windows PowerShell: `$env:CHROME_PATH="C:\...\chrome.exe"`, Mac: `export CHROME_PATH="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"`). |
| `Node 22+ is required (global WebSocket).` (종료 코드 2) | Node가 22보다 낮습니다. | https://nodejs.org 에서 최신 LTS를 설치하고 `node -v`를 다시 확인합니다. |

<!-- prereq-adapt -->
## 무엇이 필요한가요?

꼭 필요한 것은 Node 22 이상, 크롬, 엣지, 크로미움 중 하나, 카드 HTML입니다. 권장·선택 준비물까지 정리한 표는 [SKILL.md의 준비물](skills/html-card-render/SKILL.md#준비물-쓰기-전에-있어야-하는-것)에 있습니다.

## 바꿔 쓰기

에이전트에게 다음처럼 말하면 옵션을 바꿔서 실행합니다.

- "유튜브 썸네일 크기로" → `--size 1280x720`
- "인스타 정사각형 / 스토리 크기로" → `--size 1080x1080` / `--size 1080x1920`
- "더 선명하게" → `--scale 3` (결과 크기 = 크기 x 배율)

말과 옵션의 대응표는 [SKILL.md의 바꿔 쓰기](skills/html-card-render/SKILL.md#바꿔-쓰기-이렇게-말하세요)에 있습니다.

## 설치

**Claude Code 플러그인으로 설치**

```
/plugin marketplace add reedodev/html-card-render
/plugin install html-card-render@html-card-render
```

**폴더만 복사**

```bash
git clone https://github.com/reedodev/html-card-render.git
cp -r html-card-render/skills/html-card-render ~/.claude/skills/
```

필요한 것: Node 22+, 크롬·엣지·크로미움

## 바로 해 보기

```bash
node skills/html-card-render/scripts/render.mjs skills/html-card-render/templates/card-4x5.html card.png --size 1080x1350 --scale 2 --pages
```

자세한 절차, 옵션, 한계는 [SKILL.md](skills/html-card-render/SKILL.md)에 있습니다. 에이전트가 이 파일을 읽고 그대로 따릅니다.

## 시험

`node tests/run.mjs`가 SKILL.md 형식을 검사하고 예시 파일로 스크립트를 실제로 실행합니다. 푸시할 때마다 GitHub Actions가 같은 시험을 돌립니다(위 test 배지).

<!-- credits -->
## 출처와 영감

- **캡처 방식**: 크롬 개발자 프로토콜([Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/))의 공개 명령(화면 크기 지정, 화면 캡처)을 씁니다. 렌더 스크립트와 넘침 검사는 이 저장소에서 만든 것입니다.
- **디자인 과정**: 템플릿의 카드 모양(순백 바탕, 굵은 검은 테두리, 코발트 강조색 하나)은 작성자가 승인한 카드에서 왔고, 그 카드를 만들 때 [anthropics/skills의 frontend-design](https://github.com/anthropics/skills/tree/main/skills/frontend-design)(Apache 2.0)의 절차를 참고했습니다. 그 스킬의 글이나 코드는 옮기지 않았습니다.
- **글꼴**: 템플릿은 [Pretendard](https://github.com/orioncactus/pretendard)(SIL OFL 1.1)를 웹 글꼴로 불러옵니다.

## 만든 사람

**리도**, AI 좌충우돌 실습 중인 20년차 엔지니어입니다. 매일 AI에게 일을 맡기다가 막힌 부분을 고쳐 가며 이 스킬을 만들었습니다.
[YouTube @Reedodev](https://www.youtube.com/@Reedodev) · [Threads @reedo.dev](https://www.threads.com/@reedo.dev) · [GitHub @reedodev](https://github.com/reedodev)

같이 만든 다른 스킬: [설명 영상 만들기](https://github.com/reedodev/explainer-studio) · [대본 하나로 만드는 단편](https://github.com/reedodev/scenario-to-film) · [옵시디언 3D 지식그래프](https://github.com/reedodev/obsidian-brain-3d) · [노트 먼저 찾아보기](https://github.com/reedodev/vault-recall) · [작업 기록 강의자료](https://github.com/reedodev/work-to-lecture-deck) · [한국어 말투 점검](https://github.com/reedodev/korean-tone-check) · [네이버 황금키워드](https://github.com/reedodev/naver-golden-keywords) · [한글 양식 채우기](https://github.com/reedodev/hwpx-form-fill) · [TTS 끝소리 고치기](https://github.com/reedodev/tts-tail-fix) · [영상 비율 검사](https://github.com/reedodev/social-video-gate) · [유튜브 게시 점검](https://github.com/reedodev/youtube-publish-checklist)

---

## English

Zero-dependency HTML to PNG renderer over the Chrome DevTools Protocol: exact pixel sizes, multi-page card batches, and an in-page overflow / broken-image check before capture.

### New here? A beginner's guide

**When to use it.** You are making an Instagram carousel and AI image tools garble the text, so you want the text to be exact. Or you need a thumbnail or banner at an exact size (such as 1280x720) and screenshots keep coming out a few pixels off. You lay out the text in HTML (the language of web pages) and this skill photographs it into a PNG of the exact size. Before capturing it checks for text spilling out of its box and for broken images, and afterwards it re-reads the real width and height of each PNG.

**Glossary**

| Term | Meaning |
|---|---|
| Claude Code | A program you talk to in the terminal; it works on your files for you. |
| Skill | A bundle of instructions and tools that tells Claude Code how to do one kind of job. |
| Terminal | The window where you type commands (PowerShell on Windows, Terminal on Mac). |
| HTML | The language of web pages. You write sizes, positions and colors and it draws them. |
| PNG | A lossless image file format. |
| Scale | How many times denser to capture. 1080x1350 at scale 2 gives a 2160x2700 image. |
| CDP | Chrome DevTools Protocol, the official way to drive Chrome from a program. The skill uses it to open Chrome invisibly and take the picture. |

**Check your setup.** There is nothing to install except these two:

```bash
node -v        # must be v22 or newer. If missing or older, install the latest LTS from https://nodejs.org
```

- **Chrome, Edge or Chromium** must be installed. Most Windows and Mac computers already have Chrome or Edge; otherwise get it from https://www.google.com/chrome/. The skill finds it by itself; if it cannot, put the full path of the executable in `CHROME_PATH`.
- The template loads its font (Pretendard) from the internet, so try it first while online. Without a connection, the fallback Korean fonts listed in the CSS are used instead.

**Three steps**

1. Install in Claude Code:
   ```
   /plugin marketplace add reedodev/html-card-render
   /plugin install html-card-render@html-card-render
   ```
2. Tell the agent (copy these, change the topic and text):
   - `Use html-card-render to make 3 Instagram 4:5 cards about "a 10-minute morning tidy-up habit": white background, black border, one blue accent color.`
   - `Render them, and if it reports overflowing text or broken images, fix and render again.`
   - `When done, tell me the real pixel size of the 3 PNGs and where the files are.`
3. Even when the terminal says `PASS`, open the PNGs and look at them. The check covers text overflow and size, not whether the design looks good.

**Try it with the bundled template.** This is real output from a fresh clone; it creates `card-01.png`, `card-02.png` and `card-03.png` in the current folder:

```bash
node skills/html-card-render/scripts/render.mjs skills/html-card-render/templates/card-4x5.html card.png --size 1080x1350 --scale 2 --pages
```

```
OK   card-01.png  2160x2700 (expected 2160x2700)
OK   card-02.png  2160x2700 (expected 2160x2700)
OK   card-03.png  2160x2700 (expected 2160x2700)
PASS: sizes exact, no overflow found. 이제 PNG를 직접 열어 눈으로 확인하세요.
```

How to read it: every line says `OK` and the real size matches `expected`, so the sizes are exact. A final `PASS` with exit code 0 means no overflowing text and no broken images. `--pages` means one PNG for every element with `class="page"` in the HTML. The last sentence of the `PASS` and `FAIL` lines is Korean: "now open the PNG and check it by eye" / "fix the items above, then render again".

For a banner:

```bash
node skills/html-card-render/scripts/render.mjs skills/html-card-render/templates/banner.html banner.png --size 1280x720 --scale 1
```

**Troubleshooting**

| Symptom | Cause | Fix |
|---|---|---|
| `FAIL: fix the items above...` below a line like `overflow: h1 - text wider than its box "..."` (exit code 1) | Text is wider than its box and gets cut. The PNG was written but failed the check. | Shrink the text or shorten it and render again. Add `data-qa-ignore` to decorative elements that may overflow on purpose. |
| `input file not found: nofile.html` (exit code 2) | Wrong path to the HTML file. | Make sure the file is visible from your current terminal folder. |
| `bad --size or --scale` (exit code 2) | `--size` must be width x height in numbers, like `1080x1350`. | Use `--size 1080x1350` and `--scale 2`. |
| `usage: node render.mjs <file.html\|url> <out.png> ...` (exit code 2) | The input and output file names are not both given. | Give two names: `node render.mjs input.html output.png`. |
| `ERROR: Chrome/Edge not found. Set CHROME_PATH.` (exit code 2) | No Chrome, Edge or Chromium was found. | Install Chrome, or set `CHROME_PATH` to the full path of the executable (Windows PowerShell: `$env:CHROME_PATH="C:\...\chrome.exe"`, Mac: `export CHROME_PATH="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"`). |
| `Node 22+ is required (global WebSocket).` (exit code 2) | Node is older than 22. | Install the latest LTS from https://nodejs.org and check `node -v` again. |

Install: `/plugin marketplace add reedodev/html-card-render` then `/plugin install html-card-render@html-card-render`, or copy `skills/html-card-render` into `~/.claude/skills/`. See [SKILL.md](skills/html-card-render/SKILL.md) for the full procedure.

**Credits.**
- Uses public [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) commands; the renderer and overflow QA are original. Card style is the author's own, designed while following the process in [anthropics/skills frontend-design](https://github.com/anthropics/skills/tree/main/skills/frontend-design) (no text or code copied). Font: [Pretendard](https://github.com/orioncactus/pretendard) (OFL).

MIT License; third-party models, fonts and libraries keep their own licenses.
