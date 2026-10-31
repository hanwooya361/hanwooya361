<div align="center">
<br />
<h2> 백엔드 개발자 조현우입니다.</h2>
<p><b>[한 줄 소개. 예: 현장의 업무 흐름을 이해하고, 그것을 API와 데이터로 옮기는 개발자입니다.]</b></p>
<a href="mailto:johunwoo123456@gmail.com"><img src="https://img.shields.io/badge/EMAIL-johunwoo123456@gmail.com-2F80ED?style=for-the-badge&logo=gmail&logoColor=white" /></a>
<a href="https://app.notion.com/p/3dd1b8b9abf48116bf01e26856a457cf"><img src="https://img.shields.io/badge/NOTION-학습 정리-2F80ED?style=for-the-badge&logo=notion&logoColor=white" /></a>
<a href="https://github.com/hanwooya361"><img src="https://img.shields.io/badge/GITHUB-hanwooya361-2F80ED?style=for-the-badge&logo=github&logoColor=white" /></a>
</div>
<br />
<h2> Tech Stack</h2>
<div align="center">
<p><b>Backend & Database</b></p>
<img src="https://skillicons.dev/icons?i=java,spring,hibernate,mysql" />
<p><b>Frontend</b></p>
<img src="https://skillicons.dev/icons?i=react,js,html,css" />
<p><b>Python / AI · IoT & Infra</b></p>
<img src="https://skillicons.dev/icons?i=python,fastapi,opencv,raspberrypi,arduino,aws,git,github" />
<p><sub>JPA · Flyway · Swagger · Git Hook · CI · k6 · Jira · YOLOv8 · Roboflow · Colab</sub></p>
</div>
<br />
<h2> Projects</h2>
<table>
<tr>
<td width="50%" valign="top">
<h3> WMS</h3>
<p><b>식품 창고 관리 시스템</b> · 4인 팀 · 2026.09 ~ 2026.10</p>
<p>상온 가공식품·건강기능식품 창고의 입고부터 적재, 출고, 재고 이력까지 관리하는 WMS. 실제 도메인에 배포해 운영 지표를 모니터링했습니다.</p>
<p><b>My Role</b> — 입고 파트 전담 (입고 등록 → 검수 → 적재), 입출고 공통 엑셀 등록 모듈 분리</p>
<p><b>문제 해결</b> — 여러 작업자가 한 문서를 나눠 검수하는 현장 요구사항과 k6 측정 결과를 근거로, 동시성 처리를 <b>조건부 UPDATE + 대기열 방식</b>으로 결정</p>
<p><code>Java</code> <code>Spring Boot</code> <code>JPA</code> <code>MySQL</code> <code>Flyway</code> <code>k6</code> <code>React</code></p>
<a href="https://github.com/SeungMinShin01/WMS"><img src="https://img.shields.io/badge/TEAM-REPO-2F80ED?style=for-the-badge&logo=github&logoColor=white" /></a>
</td>
<td width="50%" valign="top">
<h3> Park-On</h3>
<p><b>스마트 아파트 주차 관리 시스템</b> · 졸업작품 · [인원] · [기간]</p>
<p>입구 카메라로 차량과 번호판을 인식하고, 인식 결과에 따라 차단기를 자동으로 제어하는 주차 관리 시스템. 미니어처 RC카로 실제 환경을 시뮬레이션했습니다.</p>
<p><b>My Role</b> — 입구 카메라·AI 인식 모듈, 차단기 제어, YOLOv8 모델 학습 (Python 파트)</p>
<p><b>문제 해결</b> — 차량이 아닌 물체의 오인식과 원거리 번호판 OCR 실패를 줄여 인식 정확도 개선</p>
<p><code>Python</code> <code>YOLOv8</code> <code>OpenCV</code> <code>FastAPI</code> <code>Raspberry Pi</code> <code>Arduino</code></p>
<a href="[레포 주소]"><img src="https://img.shields.io/badge/AI %2F IoT-REPO-2F80ED?style=for-the-badge&logo=github&logoColor=white" /></a>
</td>
</tr>
</table>
<br />
<details>
<summary><b> WMS 상세 보기 (구조 · 주요 기능 · 협업)</b></summary>
<br />
<p><b>시스템 흐름</b></p>
<pre>
입고 예정 등록(화면·엑셀) → 검수 → 적치 추천 → 로케이션 적재 → 재고 반영 → FEFO 출고 할당 → 피킹 → 출고 · 폐기 → 이력 조회
</pre>
<p><b>DB 설계</b> — 문서(입·출고 공통) · 문서 품목 · 처리 상세의 3단 구조에 화주 · 품목 · 거래처 · 로케이션 · LOT · 재고 · 폐기 기록을 연결</p>
<ul>
<li>교수님이 3개 테이블로의 통합을 제안했지만, SAP·Dynamics 365 등 실제 WMS 구조를 조사해 입출고 헤더/품목 분리와 재고 이력 테이블을 유지한 근거를 정리</li>
<li>식품 도메인 특성상 소비기한 기준 선출고(FEFO)를 고려해 재고를 <b>LOT × 로케이션</b> 단위로 관리</li>
<li>여러 화주(3PL)의 재고를 화주 단위로 구분해 한 칸에 다른 화주 재고가 섞이지 않도록 설계</li>
</ul>
<p><b>담당 기능 — 입고 · 재고</b></p>
<ul>
<li>입고 등록 · 검수 · 적재 · 취소 API (적재 전 문서만 취소, 검수 기록은 이력으로 보존)</li>
<li>검수 시 남은 소비기한이 출고 허용 일수보다 짧으면 입고 거부</li>
<li><b>적치 추천</b> — 전량 적재 가능한 칸 → 같은 LOT → 같은 품목 → 잔량 칸 → 빈 칸 순으로 정렬하고, 같은 순위는 여유가 가장 적은 칸부터(Best Fit) 추천</li>
<li>적치 추천 조회를 칸·재고 쿼리 2번으로 줄여 N+1 문제 제거</li>
<li>적재 시 칸 용량 초과 · 다른 화주 재고 혼재 차단</li>
<li>재고 현황(FEFO 정렬) · 입출고 이력 조회, 소비기한 경과 재고 폐기(관리자 전용)</li>
<li>입고/출고에 중복되던 엑셀 등록 로직을 출고 담당 팀원과 공통 모듈로 분리 (한 줄이라도 틀리면 전체 롤백, 오류 행 번호 안내)</li>
<li>목록 화면 공통 페이징 · 정렬 훅(React) 작성</li>
</ul>
<p><b>동시성 처리</b></p>
<ul>
<li>쟁점: "문서 단위로 트랜잭션을 묶자" vs 작업자 여러 명이 한 문서를 나눠 검수하는 현장 흐름</li>
<li>synchronized · 비관적 락 · 조건부 UPDATE · 단일 스레드 대기열을 같은 시나리오에서 k6로 측정해 비교</li>
<li>한 행만 보는 규칙(중복 적재 방지)은 <b>조건부 UPDATE</b>, 여러 행을 보는 규칙(칸 용량 합계 · 화주 혼재 · 재고 줄 생성)은 <b>단일 스레드 대기열</b>로 나눠 적용</li>
<li>20명이 같은 검수 기록(80박스)을 동시에 적재: 성공 <b>10건 → 1건</b>, 재고 <b>800 → 80박스</b>, 행 잠금 대기 <b>380회 → 0회</b>, 응답 p95 245ms → 262ms</li>
</ul>
<p><b>협업 방식</b> — 개발 전 커밋 규칙 · JIRA · Git Hook · CI · Flyway를 먼저 정하고, Swagger로 API를 공유하며 파트 간 연동 오류 최소화</p>
</details>
<details> <summary><b> Park-On 상세 보기 (AI · IoT 파트)</b></summary> <br /> <p><b>시스템 흐름</b></p> <pre> 입구 카메라 → YOLOv8 차량·번호판 탐지 → 번호판 영역 OCR(EasyOCR) → 번호판 형식 검증 → FastAPI 등록 차량 확인 요청 → Spring Boot 서버 → MySQL → 개방 판정 시 Arduino로 시리얼 신호('O') 전송 → 서보모터 차단기 개방 → 결과와 무관하게 입구 통과 기록 저장 </pre> <p><b>담당 기능</b></p> <ul> <li>YOLOv8n 모델을 Roboflow 데이터셋과 Colab으로 직접 학습 (<code>car</code> · <code>plate</code> 2클래스, 미니어처 환경 전용 모델)</li> <li>Raspberry Pi 입구 카메라 인식 모듈 — 실시간 프레임 탐지, 번호판 영역 OCR, 한국 번호판 정규식 검증(<code>12가3456</code> · <code>123가4567</code> · 지역명 포함 형식)</li> <li>FastAPI 연동 클라이언트를 인식 코드와 분리(<code>gate_client</code>)해, 등록 차량 확인과 입구 통과 기록 저장 API 호출을 담당</li> <li>Arduino + 서보모터 차단기 제어 — Python에서 시리얼 신호를 받아 개방하고, 일정 시간 후 자동으로 닫히도록 구현</li> <li>초음파 센서 2개로 차량 감지 — 입구 쪽은 뒤따르는 차량 감지, 출구 쪽은 출차 차량 감지 시 차단기 개방</li> <li>HTTP 세션을 주입받는 구조로 만들어, 가짜 응답(Fake Session)으로 서버 없이 API 연동 로직을 검증하는 단위 테스트 작성</li> </ul> <p><b>트러블슈팅</b></p> <ul> <li><b>원거리 번호판 OCR 실패</b> — 프레임 전체가 아닌 YOLO가 찾은 번호판 영역만 잘라내 3배 확대(LANCZOS 보간)한 뒤 OCR을 수행하고, 특수문자를 제거한 결과를 한국 번호판 정규식으로 검증해 형식에 맞는 결과만 사용</li> <li><b>차량이 아닌 물체 오인식</b> — 미니어처 환경에 맞춰 <code>car</code> · <code>plate</code> 클래스로 모델을 직접 학습하고, 신뢰도 0.80 미만 탐지는 버리며, <code>plate</code> 클래스로 탐지된 영역에만 OCR을 수행</li> <li><b>같은 차량 중복 처리</b> — 매 프레임 같은 번호판이 인식되어 서버 요청과 차단기 신호가 반복되던 문제를, 마지막 인식 번호판과 5초 쿨다운 기준으로 새 차량일 때만 처리하도록 해결</li> <li><b>서버 장애 시 차단기 동작</b> — FastAPI 요청이 실패하면 미등록 차량과 같게 처리해 차단기를 닫힌 상태로 유지(fail-safe)하고, 실패 사유도 통과 기록에 남기도록 구현</li> <li><b>차단기가 내려오며 차량과 충돌할 위험</b> — 닫히는 도중에도 1°마다 초음파 거리를 확인해, 차량이 감지되면 즉시 멈추고 다시 열리도록 구현. 뒤차가 감지되는 동안은 닫힘 타이머를 계속 초기화</li> <li><b>OpenCV 한글 깨짐</b> — Pillow로 한글 폰트를 렌더링해 해결</li> <li><b>차단기 서보모터 떨림</b> — 천천히 단계적으로 회전시키고 동작 후 <code>detach()</code>로 신호를 끊어 해결</li> <li><b>numpy 타입 JSON 직렬화 오류</b> — 응답 전 Python 기본 타입으로 변환해 해결</li> </ul> </details>
