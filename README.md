# GT3 작화 스테이션

도면 사진과 작화 JSON으로 GT Designer3 패널 화면을 미리 보고, GT3가 깔린 PC의 **GT3 연결 서버**(plc-helper)로 자동 작화하는 홈페이지입니다.

## 구성
| 부분 | 어디서 | 하는 일 |
|---|---|---|
| 이 홈페이지 (`index.html`) | GitHub Pages | 도면 사진 넣기, 작화 JSON 불러오기·검사, GOT 800×600 미리보기, 라벨 CSV, GT3에 만들기 요청 |
| GT3 연결 서버 (`plc-helper/gt3_web_server.py`) | GT3가 깔린 PC (`https://127.0.0.1:8765`) | 요청을 받아 GT Designer3를 화면 자동화로 조작 |
| 작화 JSON | AI(Claude, Copilot 등) 또는 사람 | 도면 → GT3 오브젝트 설계도 |

GT Designer3에는 외부 조작 API가 없어서, 실제 작화는 PC의 파이썬 프로그램(pywinauto)이 합니다.

## 쓰는 법
1. GT3 PC에서 `plc-helper\시작.bat` 실행 → 창에 나온 **연결 키**를 1번 칸에 넣고 [연결 확인]
2. 도면 사진을 넣고 (끌어다 놓기·Ctrl+V), 작화 JSON 파일을 넣는다
3. 미리보기에서 도면과 겹쳐 확인 → [GT3에 만들기]

## 서버와 주고받는 것 (헤더 `X-GT3-Key: 연결 키`)
| 요청 | 하는 일 |
|---|---|
| `GET /status` | 설치·실행 상태, `key_ok`, `busy` |
| `GET /screens` | Base 화면 목록 |
| `POST /screens/prepare` `{"screens":[{"no":1,"name":"테스트동작"}]}` | 없는 Base 화면 만들기 → `{"job"}` |
| `POST /build` (작화 JSON, `clear_first`) | 화면 하나 작화 → `{"job","advice"}` |
| `GET /job/<번호>` | `state`(queued/running/done/error), `log`, `result` |

서버는 그 PC 안(127.0.0.1)에서만, `허용_주소.txt`에 적힌 홈페이지 주소에서 온 요청만 받습니다.
