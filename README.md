# UART 제어 시계·스톱워치

PC에서 송신한 ASCII 명령을 시계·스톱워치의 시작, 정지, 초기화 및 시각 조정 신호로 변환하였습니다.

- 수행 시기 2026.08
- 참여 형태 개인
- 담당 범위 UART 제어 경로 확장 및 시뮬레이션

[포트폴리오 상세 페이지](https://eugeneson3.github.io/projects/fpga-uart-stopwatch/)

## 프로젝트 목표

기존 버튼 기반 시계·스톱워치를 PC의 UART 명령으로 제어하도록 확장하는 것을 목표로 하였습니다.

## 설계 및 구현

- 100MHz 기준 클럭에서 9600 baud의 16배 tick을 생성하고 UART 송수신 FSM을 구성하였습니다.
- r·s·c·m 명령을 스톱워치 제어에, L·R·U·D 명령을 시계 조정에 연결하였습니다.
- 기존 버튼 경로와 UART 경로가 사용하는 제어 신호를 Top에서 통합하였습니다.

## Troubleshooting

### 버튼 토글과 UART 시작·정지 명령의 차이

버튼의 run/stop 토글과 UART의 r·s 명령은 입력 의미가 다릅니다.

공통 OR 결합만으로는 명령별 시작·정지 상태를 명시하기 어렵습니다.

UART run 및 stop 신호를 별도 경로로 연결하고 Control Unit의 상태 제어에 반영하였습니다.

시작·정지·모드·초기화 및 시계 위치 이동·증감의 12개 시나리오를 확인하였습니다.

## 검증 및 결과

- 발표 자료에 12개 시뮬레이션 시나리오와 Basys3 동작 영상이 수록되어 있습니다.

## 소스코드 구조

소스 스냅샷과 발표 자료를 기준으로 작성하였습니다. 파일별 원본 경로와 SHA-256은 SOURCE_MANIFEST.json에 기록하였습니다.

| 경로 | 내용 |
| --- | --- |
| [src/uart.v](src/uart.v) | UART TX·RX 및 baud tick |
| [src/ascii_decoder.v](src/ascii_decoder.v) | ASCII 제어 명령 해석 |
| [src/top_watch_stopwatch.v](src/top_watch_stopwatch.v) | 시계·스톱워치 제어 경로 통합 |
| [sim/](sim/) | 기존 시뮬레이션 소스 |

## 실행 방법

1. files.f를 기준으로 RTL을 가져오고 사용할 보드 핀을 XDC에 지정합니다.
2. PC UART를 9600 baud로 설정한 후 r, s, c, m, L, R, U, D를 송신합니다.

## 동작 동영상

- [UART 시계·스톱워치 동작](https://www.youtube.com/watch?v=lRf44Yrbjak)

## 참고 자료

- [발표 자료](https://docs.google.com/presentation/d/1KFO2DHhuy-Bv6-T-DO49XrcA7kDt3f-wE3dZz5YqphA/edit)

## 소스 이력

2026년 11월 18일 로컬 프로젝트에서 구성한 공개 스냅샷입니다. 원본 파일의 상대 경로와 SHA-256은 [SOURCE_MANIFEST.json](SOURCE_MANIFEST.json)에 기록하였습니다. 빌드 산출물과 로컬 개발 환경은 제외하였습니다. 파일별 저작권 및 라이선스 고지를 유지하였습니다.

## 공개본의 공통 모듈 구성

소스 스냅샷과 발표 자료를 기준으로 작성하였습니다. 파일별 원본 경로와 SHA-256은 SOURCE_MANIFEST.json에 기록하였습니다. 공개본 구성 과정에서 uart_run·uart_stop 포트가 있는 후속 센서 프로젝트의 공통 Control Unit을 연결하였습니다. 기존 파일은 docs/original-control_unit.v에 보존하였습니다.

합성 최상위 모듈은 `top_watch_stopwatch`로 지정합니다. 각 파일에 함께 정의된 `top_watch` 및 `top_stopwatch`는 별도 과제용 모듈입니다. 정적 검사 결과는 docs/에 수록하였습니다.
