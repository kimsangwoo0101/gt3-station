# Copilot 지침 — GT3 작화 스테이션

이 저장소는 GT Designer3(미쓰비시 GOT 작화 프로그램) 패널 화면을 자동으로 만드는 홈페이지다.
- `index.html` 한 파일에 HTML·CSS·JS가 다 있다. 빌드 도구 없이 GitHub Pages로 바로 올린다. 외부 라이브러리를 추가하지 않는다.
- 실제 GT3 조작은 GT3 PC의 파이썬 서버(`https://127.0.0.1:8765`, plc-helper 폴더)가 한다. 이 홈페이지는 그 서버에 요청만 보낸다. 요청 형식은 README.md의 표를 따르고 바꾸지 않는다.
- 모든 요청에 헤더 `X-GT3-Key`(연결 키)를 붙인다. 연결 키를 코드나 저장소에 적지 않는다.
- 작화 JSON 칸: screen_no, screen_name, mode("template"), d_start, clear_first, objects, labels, new_screens, check_list.
  objects의 type은 rectangle, text, bit_switch, screen_switch, bit_lamp, number_display, number_input, text_display, text_input 아홉 가지뿐이다.
  x, y, w, h, font_h는 GOT 화면(800×600) 기준 0~1 비율이다.
- 디바이스 규칙: 스위치·램프는 M, 숫자·문자 칸은 D(d_start부터 5씩). X, Y, U는 쓰지 않는다. 99번 화면은 템플릿이라 작화하지 않는다.
- `check1()`은 서버의 `gt3_rules.py check()`와 같은 규칙이다. 한쪽을 바꾸면 다른 쪽도 같이 바꾼다.
- [GT3에 만들기]는 사람이 누를 때만 보낸다. 자동으로 보내지 않는다. clear_first는 체크 상자로만 정한다.
- 화면 글은 쉬운 한국어로 쓴다. 휴대폰 폭(390px)에서 가로 스크롤이 생기지 않게 한다.
- PLC 래더 프로그램은 다루지 않는다.
