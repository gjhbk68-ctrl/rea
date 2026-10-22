# JBni 온라인대시보드 (jbnionline) 작업 규칙

브레이크패드 온라인 판매(네이버·쿠팡·지마켓) 주문·출고 자동화 대시보드. Flask 백엔드 + 분리형 프론트엔드.
(이 파일에는 API키·고객정보를 절대 쓰지 않는다.)

## 기본 원칙
- 서버PC뿐 아니라 외부PC·모바일 접속을 모두 고려한다. 필요하면 "서버PC 방식 / 외부PC 방식" 두 가지 옵션을 제시한다.
- 파일 크기를 줄이는 최적의 코드·아이디어를 우선 제공한다.
- PC·모바일 모두 가독성 우선: 폰트 크기·색상을 신중히 고르고, 어두운 바탕색은 피한다(밝은 바탕 우선).
- 새 메커니즘 도입 전에 기존 패턴(`<details>`, CSS 변수, 기존 라우트, 기존 함수)을 먼저 재사용한다.

## 작업이력
- 모든 수정은 파일 docstring에 `[YYYY-MM-DD] 변경 내용 + 이유`를 기록한다.
- 날짜별 기록은 `작업이력_대시보드.md`에 추가한다(찾기 쉽게 최신 날짜가 위).
- 마이너 로직 수정으로는 파일명 버전 번호를 올리지 않는다.

## 검증 (수정 후 필수)
- Python: `py_compile` + `ast.parse` / JS: `node -c` / CSS: 중괄호 개수 확인
- 배치파일(.bat)은 순수 ASCII. 한글·박스문자 금지, `chcp 65001` 사용 안 함.
- 문자열 치환이 실패하면 Python 스크립트로 패치한다.

## Git / 배포
- 저장소: https://github.com/gjhbk68-ctrl/jbnionline (private, main). 최신본 기준은 GitHub.
- 작업 전 확인은 두 가지만: "서버PC에서 업데이트.bat(git pull)을 했는가 / 서버에서 직접 고친 파일이 있는가".
- 커밋 메시지: `YYYY-MM-DD 내용`
- 수정 후 서버PC 안내: `업데이트.bat` 실행(받기 + 서버 재시작 + 화면 자동 새로고침).
  - `업데이트.bat` 자체가 바뀐 날만 먼저 `git pull`.
  - `register_task.bat` 등 재실행이 필요한 변경은 따로 안내.
- 서버PC에서 소스를 직접 고치지 않는다(pull 충돌).
- 실행은 `dashboard_run_v5.bat`. 주문이력조회 폴더는 `온라인판매\주문이력조회`, 스케줄러는 매일 09:00.

## 올리지 않는 것 (.gitignore)
- API키·설정파일: `.dashboard_config.json`, `네이버API_설정.json` 등
- DB(sqlite), 엑셀(xlsx)
- 주문·고객정보: `orders.json`, `주문이력조회.html`, `orders_template.html`
- 새 파일/폴더를 추가한 뒤에는 `git add -n .` 결과를 확인하고 푸시한다. 코드에 키를 직접 쓰지 않는다.

## 코드베이스 주의사항
- 채널 시트(취합/발주발송관리)는 수정 금지(이카운트 복붙용 수식 포함). 수정은 작업지시서 시트에서만.
- `dashboard_core.py`, `네이버주문_자동화_v638.py`, `작업지시서_생성_v5.py`는 항상 함께 배포(위치/가격 조회 불일치 방지).
- 최신 `v*` 파일은 importlib glob으로 자동 감지되므로 새 버전 파일 추가만으로 업그레이드된다.
- openpyxl은 `read_only=False` 사용(`True`는 메타데이터 오류 시 0행 반환).
- 품번 추출 규칙은 `jh_common.py` 한 곳에서만 관리한다. VIN 속 JP/JS 코드 오탐에 주의.
- `.dashboard_config.json`은 OneDrive 동기화로 서버PC/외부PC 경로가 충돌할 수 있으니 주의.
- 네이버 API: POST 사용, 30건 배치 제한. 품번 대체발송 등록은 송장매칭 실행 전에 끝낸다.

## 주요 파일
- `dashboard_v5.py`(Flask 메인) / `dashboard_core.py`(오케스트레이터) / `dashboard.html·js·css`
- `작업지시서_생성_v5.py`, `네이버주문_자동화_v638.py`, `송장번호_매칭_v5.py`
- `네이버API_연동_v1.py`, `쿠팡API_연동_v1.py`, `이카운트_전송_v1.py`, `db_updater.py`
