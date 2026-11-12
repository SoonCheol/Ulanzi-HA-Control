# Ulanzi HA Control — experimental 0.2.0

독립 Ulanzi D200X Home Assistant 기기 제어 플러그인 (Studio 3.4.8 대상).

## 이름
- 플러그인: **HA 기기 제어**
- 액션: **전등·환풍기 ON/OFF**

## 기능
- WebSocket으로 HA 엔티티의 on/off 상태 변경 수신
- light, switch, fan, input_boolean 토글
- ON: 전등 노랑 / 환풍기 청록, OFF: 회색, 연결 안됨: ? 표시
- 버튼마다 엔티티 ID, 표시명 및 아이콘 형태 설정

## 연결 및 설정
1. **서버 연결**: HA URL 및 장기 액세스 토큰 입력 후 연결
2. **기기 검색 및 선택**: HA 상태 목록에서 전등·환풍기 (light, switch, fan) 검색
3. **저장**: 표시명과 전등/환풍기 아이콘을 지정하고 저장

HA Hub의 설정 화면에서 발생한 직접 REST fetch 오류를 피하기 위해, 설정 화면 자체에서는 HA에 직접 접속하지 않습니다. 플러그인 메인 프로세스가 HA WebSocket으로 연결해 목록을 전달합니다. 공식 Home Assistant 2.1.0 플러그인의 Node.js 백엔드와 구현이 동일한 것은 아닙니다.

## 설치
1. GitHub Code → Download ZIP 후 압축 해제.
2. `%APPDATA%\Ulanzi\UlanziDeck\Plugins\com.ulanzi.hastatetoggle.ulanziPlugin\` 폴더 생성.
3. 저장소의 `manifest.json`, `plugin`, `property-inspector`, `libs`, `resources`를 그 폴더 **바로 아래**에 복사.
4. Ulanzi Studio 완전 종료 후 재실행.
5. **전등·환풍기 ON/OFF** 액션을 배치하고 HA URL과 장기 액세스 토큰으로 **서버 연결** 후 엔티티를 검색·선택하고 이름·아이콘을 저장.

기존 **Home Assistant → 엔티티** 플러그인과 프로필은 수정하지 않습니다.

## 주의
**초기 소스 개발판입니다. 수정판 0.2.0의 D200X 및 Studio 3.4.8 로딩·연동 테스트는 아직 하지 못했습니다.** 먼저 빈 버튼 하나에서 시험하세요. 실제 장치 상태 변경 테스트는 사용자 승인 후 진행하세요.

개인 HA 주소나 인증 토큰은 저장소에 없습니다. 토큰은 로컬 Studio 설정에서만 입력하세요. Ulanzi의 설정 저장 보안 수준은 별도로 확인하지 않았습니다.

현재는 플러그인이 ON/OFF 아이콘을 Canvas로 직접 그립니다. 사용자가 제작한 PNG 아이콘을 선택하는 기능은 미구현입니다.

## 출처와 라이선스
- [Ulanzi 공식 Plugin SDK](https://github.com/UlanziTechnology/UlanziDeckPlugin-SDK)
- `libs/js/`는 [HA Hub](https://github.com/weemaba999/ha-hub-ulanzi)의 SDK 런타임 소스 사본이며 AGPL-3.0 관련 의무를 준수합니다.
- 이 저장소 라이선스: AGPL-3.0.

## 향후 확장
센서 상태 표시 등은 다음 버전에서 별도 액션으로 추가할 예정이며, 이번 버전은 light/switch/fan 제어만 대상으로 합니다.
