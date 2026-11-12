# Ulanzi HA Control — experimental 0.3.2

독립 Ulanzi D200X Home Assistant 기기 제어 플러그인 (Studio 3.4.8 대상).

## 이름
- 플러그인: **HA 기기 제어**
- 액션: **전등·환풍기 ON/OFF**

## 기능
- WebSocket으로 HA 엔티티의 on/off 상태 변경 수신
- light, switch, fan, input_boolean 토글
- **전등: ON 노란색 / OFF 회색 정지 아이콘** (기존 Canvas 방식 복구)
- **환풍기: ON 청록색 회전 GIF** (`resources/fan-on.gif`) — OFF 아이콘과 동일한 사각 하우징·원형 테두리·4개 날개 윤곽을 유지하고 날개만 회전
- **OFF: 회색 정지 아이콘**, 연결 안 됨: ? 표시
- ON/OFF 상태가 바뀔 때만 아이콘 갱신해 GIF의 불필요한 재시작 방지
- 버튼마다 엔티티 ID, 표시명 및 아이콘 형태 설정

## 연결 및 설정
1. **서버 연결**을 눌러 HA URL과 장기 액세스 토큰으로 연결합니다. 연결되면 **HA 연결 성공**과 조회한 엔티티 수가 표시됩니다.
2. 기기 이름이나 엔티티 ID로 검색해 전등·환풍기 (`light`, `switch`, `fan`)를 선택합니다. 목록이 자동으로 접히고 선택한 기기의 이름과 엔티티 ID가 표시됩니다.
3. 다른 기기를 고르려면 **기기 변경**을 누릅니다. 표시 이름과 아이콘 종류를 확인한 뒤 **버튼 설정 저장**을 누릅니다.

HA Hub의 설정 화면에서 발생한 직접 REST fetch 오류를 피하기 위해, 설정 화면 자체에서는 HA에 직접 접속하지 않습니다. 플러그인 메인 프로세스가 HA WebSocket으로 연결해 목록을 전달합니다. 공식 Home Assistant 2.1.0 플러그인의 Node.js 백엔드와 구현이 동일한 것은 아닙니다.

## 설치
1. GitHub Code → Download ZIP 후 압축 해제.
2. `%APPDATA%\Ulanzi\UlanziDeck\Plugins\com.ulanzi.hastatetoggle.ulanziPlugin\` 폴더 생성.
3. 저장소의 `manifest.json`, `plugin`, `property-inspector`, `libs`, `resources`를 그 폴더 **바로 아래**에 복사.
4. Ulanzi Studio 완전 종료 후 재실행.
5. **전등·환풍기 ON/OFF** 액션을 배치하고 HA URL과 장기 액세스 토큰으로 **서버 연결** 후 엔티티를 검색·선택하고 이름·아이콘을 저장.

기존 **Home Assistant → 엔티티** 플러그인과 프로필은 수정하지 않습니다.

## 주의
**초기 소스 개발판입니다. 수정판 0.3.2의 D200X 및 Studio 3.4.8 GIF 표시·전환 실기 테스트는 아직 하지 못했습니다.** 먼저 빈 버튼 하나에서 시험하세요. 실제 장치 상태 변경 테스트는 사용자 승인 후 진행하세요.

## 연결 정보 저장과 보안
- 기본: Studio의 **Global Settings**로 HA URL과 토큰을 버튼 전체에서 공유합니다. 저장 시 버튼 개별 설정에는 토큰을 새로 기록하지 않습니다.
- 선택: 사용자가 플러그인 루트에 직접 만든 `config.local.json`도 읽을 수 있습니다. 내용 예시는 `{"haUrl":"https://ha.example.com","token":"직접 입력한 토큰"}` 입니다. 이 파일은 저장소에 **절대 올리지 마세요**.
- **중요: `config.local.json`은 평문 저장이며 암호화되지 않습니다. 안전한 자격 증명 저장소가 아닙니다.** 파일 접근 권한이 있는 다른 사용자/프로그램이 토큰을 읽을 수 있습니다. Studio Global Settings의 암호화 여부도 아직 확인하지 못했습니다.
- 기존에 이미 만들어둔 버튼 설정에 토큰이 포함돼 있을 수도 있습니다. 이번 변경은 과거에 기록된 자격 증명을 자동으로 정리하거나 암호화하지 않습니다.
- Windows 자격 증명 관리자/DPAPI 사용은 향후 별도 구현·검증이 필요합니다.
- 현재 소스에는 개인 HA 주소나 실제 토큰을 넣지 않았습니다.

환풍기 ON 상태에서만 GIF를 표시하며, 전등 ON/OFF 및 환풍기 OFF·오류 상태는 Canvas 정지 이미지로 표시합니다. 사용자가 제작한 PNG/GIF를 UI에서 직접 선택하는 기능은 미구현입니다.

**v0.3.2:** OFF 아이콘의 기하학적 형태에 맞춰 ON GIF를 다시 제작했습니다. 환풍기 상태 전환 시 디자인은 유지되고 색상과 날개 회전만 바뀝니다.

**업데이트 안내:** 기존 플러그인 폴더를 백업하고 Studio를 완전히 종료한 다음, `manifest.json`, `plugin/app.html`, `resources/fan-on.gif`를 반영하세요. `config.local.json`과 기존 버튼 설정은 건드리지 않습니다.

## 출처와 라이선스
- [Ulanzi 공식 Plugin SDK](https://github.com/UlanziTechnology/UlanziDeckPlugin-SDK)
- `libs/js/`는 [HA Hub](https://github.com/weemaba999/ha-hub-ulanzi)의 SDK 런타임 소스 사본이며 AGPL-3.0 관련 의무를 준수합니다.
- 이 저장소 라이선스: AGPL-3.0.

## 향후 확장
센서 상태 표시 등은 다음 버전에서 별도 액션으로 추가할 예정이며, 이번 버전은 light/switch/fan 제어만 대상으로 합니다.
