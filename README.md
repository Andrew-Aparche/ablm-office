# 에이블램 에이전트OS 오피스

**AI 직원들이 일하는 작은 사무실입니다.** 대표님은 평소 말하듯 일을 부탁하고, 팀장 그레이스가 직원들에게 나눠 줍니다. 직원들의 말풍선과 진행 상황을 보다가 완성된 글·파일을 열어 볼 수 있습니다. 직원들은 실제 사람이 아닌 AI입니다.

![AI 직원 6명이 함께 일하는 아이소메트릭 오피스 화면 예시](images/office.png)

Claude Code와 Codex에서 쓰는 플러그인입니다. 아이와 어르신이 사용할 때는 처음 설치와 계정 로그인만 도와주세요. 그 뒤에는 브라우저에서 사용합니다. AI 사용 요금과 한도는 연결한 Claude·OpenAI 계정의 조건을 따릅니다.

## 1. 처음 한 번 준비해 주세요

- **Mac 또는 Windows PC**와 인터넷, 최신 브라우저가 필요합니다. 오피스는 PC에서 실행합니다. 켜 둔 뒤에는 같은 와이파이의 휴대폰·태블릿에서도 볼 수 있습니다(4단계).
- **Claude Code** 또는 **Codex** 중 한쪽을 준비합니다. 없다면 [Claude 공식 설치 안내](https://code.claude.com/docs/en/setup) / [Codex 공식 설치 안내](https://learn.chatgpt.com/docs/cli)를 따라 한쪽만 설치하세요. 오피스 안에서 다른 엔진도 나중에 준비할 수 있습니다.
- **Node.js 20 이상**이 필요합니다. [Node.js 다운로드](https://nodejs.org/en/download)에서 **LTS**를 고르고, Mac은 `.pkg`, Windows는 `.msi` 설치 파일을 받아 안내대로 설치합니다.
- **Git**이 필요합니다. [Mac 설치 안내](https://git-scm.com/download/mac) / [Windows 설치 안내](https://git-scm.com/download/win)를 참고합니다. GitHub 계정은 없어도 됩니다.

**명령을 쓰는 창 열기:** Mac은 `⌘ + Space` → `터미널` 검색 → Enter, Windows는 시작 메뉴 → `PowerShell` 검색 → 열기입니다. 관리자 권한은 필요 없습니다. 아래 회색 상자에서 한 줄씩 복사해 붙여 넣고 Enter를 누르세요. 명령 앞에 `$` 같은 기호를 덧붙이지 않습니다.

```sh
node --version
git --version
```

`v24...`, `git version ...`처럼 버전이 나오면 준비됐습니다. 찾을 수 없다고 나오면 설치 후 창을 완전히 닫았다 다시 여세요.

## 2. 쓰는 AI 한쪽에 오피스 설치하기

아래 **A 또는 B 중 한쪽만** 진행합니다. Mac의 터미널과 Windows PowerShell에서 같은 명령을 사용합니다. 저장소를 따로 내려받거나 `npm install`을 실행할 필요는 없습니다.

### A. Claude Code를 쓴다면

```sh
claude plugin marketplace add Andrew-Aparche/ablm-office
claude plugin install ablm-office@ablm-agent-os
claude
```

첫 명령에 `Successfully added marketplace: ablm-agent-os`가 나오면 등록됐습니다. Claude 대화가 열리면 **새 대화**에 아래를 입력하세요.

```text
/ablm-office
```

명령이 보이지 않으면 `에이블램 오피스 열어줘`라고 말합니다. 오피스 전용 도구 연결·실행 승인이 나오면 내용을 읽고 허용하세요. 웹 브라우저에 오피스가 열립니다.

### B. Codex를 쓴다면

```sh
codex plugin marketplace add Andrew-Aparche/ablm-office
codex plugin add ablm-office@ablm-agent-os
```

설치가 끝나면 **오피스를 여닫는 도구를 한 번 연결**합니다. 이 단계는 사용자가 직접 실행하고 동의합니다.

Mac 터미널:

```sh
node "$(find "${CODEX_HOME:-$HOME/.codex}/plugins/cache/ablm-agent-os/ablm-office" -path '*/bin/setup-codex.mjs' | sort -V | tail -1)"
```

Windows PowerShell:

```powershell
$officeCodexHome = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { "$env:USERPROFILE\.codex" }
$officeSetup = Get-ChildItem "$officeCodexHome\plugins\cache\ablm-agent-os\ablm-office\*\bin\setup-codex.mjs" | Where-Object { $_.Directory.Parent.Name -match '^\d+\.\d+\.\d+$' } | Sort-Object { [version]$_.Directory.Parent.Name } -Descending | Select-Object -First 1
if ($officeSetup) { node $officeSetup.FullName } else { Write-Host "플러그인이 설치됐는지 먼저 확인해 주세요." }
```

`에이블램 오피스를 열고 닫는 전용 도구를 연결할까요?`가 나오면 **예**를 입력하고 Enter를 누릅니다. `연결했어요`가 나오면 끝입니다. 셸·네트워크의 전체 권한을 여는 설정은 하지 않습니다.

터미널에서 `codex`를 실행하거나 Codex 앱에서 **새 대화**를 열어 `/ablm-office`를 입력합니다. 명령이 보이지 않으면 `ablm-office 스킬로 오피스 열어줘`라고 말하세요. 웹 브라우저에 오피스가 열립니다.

## 3. 회사 세우기 → 사원증 발급

1. 첫 화면의 **회사 세우기**에서 팀장 그레이스가 사용할 **Claude 또는 Codex**를 고르고 **이렇게 할게요 👍**를 누릅니다.
2. **사원증 발급 데스크**에서 팀장의 엔진에 **시작**을 누릅니다. 화면은 **점검 → 설치 → 서명(로그인) → 확인** 순서로 바뀝니다. 이미 준비된 프로그램과 로그인은 다시 만들지 않습니다.
3. 로그인 브라우저가 열리면 본인 계정으로 승인하고 오피스로 돌아옵니다. 이미 로그인됐는데 문제가 생겼다면 로그아웃하지 말고 **자세히 보기**를 도와주는 분께 보여 주세요.
4. 팀장의 사원증이 나오면 **🏢 사무실로 들어가기**를 누릅니다. 다른 엔진은 나중에 위쪽 **🪪 사원증**에서 준비해도 됩니다. 그 엔진 직원은 그동안 **입사 대기**로 보입니다.
5. 그레이스를 눌러 말해 보세요: **“레오에게 우리 동네 빵집 홍보 문구 3개를 맡기고, 홍보문구.md 파일로 만들어 주세요.”** 말풍선·작업 표시·결과물 카드·그레이스 보고를 차례로 확인합니다.

사원증의 마지막 **확인** 단계도 작은 AI 요청을 사용합니다. 설치·로그인·업무에는 인터넷 연결이 필요합니다.

## 4. 매일 열고 닫기

- 새 Claude/Codex 대화에서 **`/ablm-office`** 또는 **“에이블램 오피스 열어줘”**라고 말합니다.
- 오피스 위쪽 **⋯ 메뉴**에서 **🖥️ 바탕화면에 아이콘 만들기**를 한 번 누릅니다. 자동으로 만들지 않으므로 원할 때만 누르세요. 그 뒤 바탕화면의 **에이블램 오피스** 아이콘을 두 번 클릭하면 새 접속 주소로 열립니다. 브라우저 주소를 즐겨찾기로 저장해서 대신 사용하지 마세요.
- **휴대폰·태블릿으로 보기:** PC 오피스의 **⋯ 메뉴 → 📱 휴대폰으로 보기 → 휴대폰 연결 켜기**를 누르고, 나온 QR 코드를 휴대폰 카메라로 찍습니다. 휴대폰은 PC와 **같은 와이파이**에 있어야 합니다. 처음 켤 때 방화벽 창이 나오면 **허용**을 누릅니다. 다 쓰면 **휴대폰 연결 끄기**(2시간 동안 안 쓰면 저절로 꺼짐). 사원증 발급은 PC에서만 합니다.
- 업무 하나를 멈추려면 직원 화면의 **그만!**, 프로젝트 전체를 멈추려면 프로젝트 중단 버튼을 누릅니다.
- 서버까지 닫으려면 Claude/Codex에 **“오피스 닫아줘”**라고 말합니다. 브라우저 탭만 닫으면 서버와 작업은 계속될 수 있습니다.

## 5. 잘 안 될 때

| 보이는 상황 | 이렇게 해 주세요 |
|---|---|
| `401` 또는 “다시 열어주세요” | `/ablm-office`나 바탕화면 아이콘으로 다시 엽니다. 서버를 다시 켜면 예전 접속 주소가 만료됩니다. |
| 브라우저가 안 열림 | 대화에 표시된 **`http://127.0.0.1:숫자/?t=...` 전체 주소**를 복사해 같은 PC의 브라우저에 붙여 넣습니다. `?t=` 뒤를 자르지 않습니다. |
| 주소가 열린 뒤 `?t=`가 사라짐 | 정상입니다. 브라우저가 접속 쿠키를 받은 뒤 주소에서 토큰을 지웁니다. |
| 직원이 **입사 대기** | 위쪽 **🪪 사원증**에서 해당 엔진의 **시작**을 누릅니다. 로그인 승인 후 확인까지 끝내세요. |
| 휴대폰에서 QR 주소가 안 열림 | 휴대폰과 PC가 같은 와이파이인지, 방화벽 창에서 **허용**했는지 확인합니다. 학교·카페·회사 공용 와이파이는 기기끼리 연결을 막는 경우가 많아 안 될 수 있습니다(집 공유기나 휴대폰 핫스팟으로 시도). |
| 휴대폰에 “QR 코드를 다시 찍어 주세요” | 휴대폰 연결이 꺼졌거나 서버가 다시 시작됐습니다. PC에서 다시 켜고 새 QR을 찍습니다. |
| `/ablm-office`가 안 보임 | 설치한 쪽의 **새 대화**를 엽니다. Codex는 전용 도구 연결도 확인합니다. |
| `node`를 찾을 수 없음 | Node.js LTS 설치 후 터미널과 Claude/Codex 앱을 완전히 닫았다 다시 엽니다. |
| 그 밖의 오류 | **자세히 보기**의 문구와 문제 시각을 전달합니다. 접속 토큰·로그인 코드·인증 파일은 가리고 보내세요. |

## 6. 자료와 권한

결과물은 Mac의 `~/AblmOffice/workspace`, Windows의 `%USERPROFILE%\AblmOffice\workspace`에 저장됩니다. 중요한 원본 대신 복사본으로 시작하세요.

오피스는 이 PC의 `127.0.0.1`에서 열립니다. 접속 주소의 토큰은 오피스를 여는 열쇠이므로 공유하지 않습니다. 로컬 오피스여도 요청한 글과 업무 자료는 연결한 AI 제공자에게 전송될 수 있습니다.

기본 Claude 직원은 파일·검색 도구만 쓰고 **셸(Bash) 명령은 차단**합니다. Codex 직원은 전용 작업 폴더를 작업 위치로 삼아 샌드박스에서 실행하며, 팀장은 읽기 전용입니다. 이는 PC 전체에 대한 완전한 격리를 보장한다는 뜻은 아닙니다. 보안 설정을 낮추거나 전체 디스크·임의 명령 허용을 켜지 마세요.

## 7. 지우기

먼저 **“오피스 닫아줘”**로 종료하고, 설치한 쪽의 명령만 터미널/PowerShell에서 실행합니다.

Claude Code:

```sh
claude plugin uninstall ablm-office@ablm-agent-os
claude plugin marketplace remove ablm-agent-os
```

Codex:

```sh
codex plugin remove ablm-office@ablm-agent-os
codex plugin marketplace remove ablm-agent-os
```

Codex에서 전용 도구를 연결했다면 `~/.codex/config.toml`(Windows: `%USERPROFILE%\.codex\config.toml`)에 아래 두 줄이 남아 있는지 확인하고, **남아 있을 때만** 백업한 뒤 지웁니다. 다른 설정은 지우지 않습니다.

```toml
[plugins."ablm-office@ablm-agent-os".mcp_servers."ablm-office-launcher".tools.office]
approval_mode = "approve"
```

바탕화면의 오피스 바로가기는 직접 휴지통으로 옮깁니다. 결과물은 `AblmOffice`에 남습니다. 필요하다면 먼저 백업하고, 자료까지 지울 때만 그 폴더를 직접 삭제하세요.

## 라이선스

이 플러그인은 오픈소스가 아닙니다. 개인이 자신의 컴퓨터에 설치해 사용할 수 있으며, 복제·재배포·판매·수정·역설계는 허락 없이 할 수 없습니다. 자세한 내용은 [LICENSE](LICENSE)를 참고하세요.
