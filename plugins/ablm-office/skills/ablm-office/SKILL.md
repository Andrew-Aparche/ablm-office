---
name: ablm-office
description: Use when the user asks to open, start, or close the 에이블램 에이전트OS 오피스 (ABLM Agent OS Office) — e.g. "/ablm-office", "에이블램 오피스 열어줘", "회사 열어줘", "오피스 닫아줘". Launches the local office web app in the browser.
---

# 에이블램 에이전트OS 오피스 열기

이 스킬은 플러그인에 들어 있는 오피스(`app/server.mjs`)를 로컬에서 띄우고 브라우저를 여는 얇은 런처다. 다른 작업은 하지 않는다.
오피스는 에이전트 대화가 끝나도 계속 켜져 있다. 대표님이 닫아 달라고 할 때만 닫는다.

## 호스트의 오피스 도구 사용 (우선)

플러그인의 `ablm-office-launcher` MCP 서버가 제공하는 `office` 도구를 사용한다.
도구가 검색형이면 먼저 `ablm-office-launcher office`로 찾는다.

- 열기: `office({"action":"open"})`
- 닫기: `office({"action":"close"})`
- 상태: `office({"action":"status"})`
- 테스트에서만 브라우저 생략: `office({"action":"open","openBrowser":false})`

출력 첫 줄 `에이블램 에이전트OS 오피스: http://127.0.0.1:<port>/?t=<token>` 의 주소를 대표님께 **그대로** 알려준다.
`?t=` 뒤는 이번 실행의 출입증이다. 주소를 줄이거나 바꾸지 말고, 외부에 공유하지 말라고 한 번 덧붙인다.
이미 켜져 있으면 새로 띄우지 않고 같은 주소를 다시 연다 (출력에 `already running`).

호스트가 설치된 MCP 프로세스를 시작하므로 Codex의 셸 샌드박스에서도 로컬 서버를 열 수 있다.
이 도구는 정해진 세 동작만 받는다. 임의 명령·경로·외부 URL은 받지 않는다.

Codex에서 도구가 없으면 플러그인을 업데이트/활성화한 뒤 새 세션을 열도록 안내한다.
처음 설치 후 아직 도구가 연결되지 않았으면, 대표님이 일반 터미널에서 플러그인의
`node "<plugin-root>/bin/setup-codex.mjs"`를 직접 실행해 전용 도구 연결에 동의하도록 안내한다.
스킬/에이전트가 이 설정 프로그램을 대신 실행하거나 동의 문자를 입력하지 않는다.
연결 프로그램은 이 플러그인의 `office` 도구 1개에만 권한을 저장하며 셸·네트워크 설정은 바꾸지 않는다.
샌드박스 설정·승인 정책을 수정하거나 권한 상승, exec 규칙 설치, `nohup`/`open` 등의 우회 실행을 시도하지 않는다.
호스트/관리자가 도구를 차단했으면 그 정책을 따른다.
아래 셸 명령은 Claude Code에서 MCP 도구를 쓸 수 없을 때 또는 사용자가 터미널에서 직접 실행할 때만 쓴다.

## Node.js가 없거나 오래됐을 때

오피스는 Node.js 20 이상이 필요하다. 다음 중 하나면 Node.js 문제다.

- `office` 도구가 아예 보이지 않고 셸에서 `node --version`이 실패한다 (`command not found`, `'node'은(는) ... 아닙니다`).
- `node --version`이 `v19` 이하다.
- 도구/런처 출력에 `Node.js 20 이상이 필요해요`가 있다.

이때 대표님께 쉬운 말로 이렇게 안내한다. Node.js를 대신 설치하지 않는다.

> 오피스를 열려면 **Node.js**(무료 프로그램)가 필요해요.
> 1. https://nodejs.org 에 들어가서 **LTS** 버튼을 눌러 설치 파일을 받아요.
> 2. 받은 파일을 열어 '다음'만 눌러 설치해요. (Mac은 .pkg, Windows는 .msi)
> 3. Claude(또는 Codex)를 완전히 껐다가 다시 열고 `/ablm-office`를 입력해 주세요.

## 런처 경로 찾기

런처는 플러그인 루트의 `bin/launch.mjs`다. 플러그인 루트는 이 SKILL.md 파일에서 두 단계 위 폴더(`skills/ablm-office/` 의 상위의 상위)다.

- Claude Code: `${CLAUDE_PLUGIN_ROOT}/bin/launch.mjs`
- 위 변수가 치환되지 않았거나 Codex 등 다른 호스트라면: 이 SKILL.md의 절대 경로에서 `skills/ablm-office/SKILL.md`를 떼어낸 폴더 + `bin/launch.mjs`

경로에 공백이 있을 수 있으니 항상 따옴표로 감싼다.

## 열기

```sh
node "<plugin-root>/bin/launch.mjs"
```

- 출력 첫 줄의 `http://127.0.0.1:<port>/?t=<token>` 주소를 대표님께 그대로 알려준다.
- 샌드박스 안의 셸에서 `listen EPERM` 또는 `바로 종료되었습니다`로 실패하면 위 MCP 도구를 사용한다. `detached`는 서버 수명 관리용이며 샌드박스 권한을 바꾸지 않는다.
- `오피스 프로그램 파일이 없습니다`가 나오면 플러그인 업데이트/재설치를 안내한다.

## 닫기

대표님이 오피스를 닫아 달라고 하면:

```sh
node "<plugin-root>/bin/launch.mjs" --stop
```

## 상태 확인

```sh
node "<plugin-root>/bin/launch.mjs" --status
```

## 바탕화면 아이콘

대표님이 바탕화면 아이콘을 원하면, 열린 오피스 화면 오른쪽 위 **⋯** 메뉴의 **🖥️ 바탕화면에 아이콘 만들기**를 누르도록 안내한다.
스킬/에이전트가 아이콘을 대신 만들지 않는다. 아이콘은 플러그인이 업데이트돼도 그대로 쓸 수 있다.

데이터와 작업 결과는 `~/AblmOffice` (Windows `%USERPROFILE%\AblmOffice`)에 저장된다. `ABLM_HOME`을 지정하면 그 폴더를 쓴다.
