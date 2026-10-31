README
<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,100:2F80ED&height=140&section=header&text=Hyunwoo Cho&fontSize=36&fontColor=ffffff&fontAlignY=38" width="100%" />
<h3>백엔드 개발자 조현우입니다.</h3>
<p><b>[한 줄 소개. 예: 현장의 업무 흐름을 이해하고, 그것을 API와 데이터로 옮기는 개발자입니다.]</b></p>
<a href="mailto:johunwoo123456@gmail.com"><img src="https://img.shields.io/badge/EMAIL-johunwoo123456@gmail.com-2F80ED?style=for-the-badge&logo=gmail&logoColor=white" /></a>
<a href="https://app.notion.com/p/3dd1b8b9abf48116bf01e26856a457cf"><img src="https://img.shields.io/badge/NOTION-학습 정리-111111?style=for-the-badge&logo=notion&logoColor=white" /></a>
<img src="https://komarev.com/ghpvc/?username=hanwooya361&label=PROFILE VIEWS&color=2F80ED&style=for-the-badge" />
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
<p><b>문제 해결</b> — 여러 작업자가 한 문서를 나눠 검수하는 현장 요구사항과 k6 측정 결과를 근거로, 동시성 처리를 <b>대기열 방식</b>으로 결정</p>
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
<a href="[레포 주소]"><img src="https://img.shields.io/badge/AI · IoT-REPO-111111?style=for-the-badge&logo=github&logoColor=white" /></a>
</td>
</tr>
</table>
<br />
<details>
<summary><b> WMS 상세 보기 (구조 · 주요 기능 · 협업)</b></summary>
<br />
<p><b>시스템 흐름</b></p>
<pre>
거래처 입고 예정 → 입고 등록 → 검수 → 로케이션 적재 → 재고 반영 → 출고 → 재고 이력 기록
</pre>
<p><b>DB 설계</b> — 9개 테이블: 품목 · 위치 · 거래처 · 재고 · 입고(헤더/품목) · 출고(헤더/품목) · 재고이력</p>
<ul>
<li>교수님이 3개 테이블로의 통합을 제안했지만, SAP·Dynamics 365 등 실제 WMS 구조를 조사해 입출고 헤더/품목 분리와 재고 이력 테이블을 유지한 근거를 정리</li>
<li>식품 도메인 특성상 유통기한 기준 선출고(FEFO)를 고려한 재고 구조</li>
</ul>
<p><b>담당 기능 — 입고</b></p>
<ul>
<li>입고 등록 · 검수 · 적재 API</li>
<li>입고/출고에 중복되던 엑셀 등록 로직을 출고 담당 팀원과 공통 모듈로 분리</li>
<li>[추가 기능이 있으면 작성]</li>
</ul>
<p><b>동시성 처리</b></p>
<ul>
<li>쟁점: "문서 단위로 트랜잭션을 묶자" vs 작업자 여러 명이 한 문서를 나눠 검수하는 현장 흐름</li>
<li>k6로 부하를 측정해 근거를 만들고, 팀 합의로 대기열 방식 채택</li>
<li>[측정 수치가 있으면 작성: 예) 동시 요청 N건에서 오류율 ○% → ○%]</li>
</ul>
<p><b>협업 방식</b> — 개발 전 커밋 규칙 · JIRA · Git Hook · CI · Flyway를 먼저 정하고, Swagger로 API를 공유하며 파트 간 연동 오류 최소화</p>
</details>
<details>
<summary><b> Park-On 상세 보기 (AI · IoT 파트)</b></summary>
<br />
<p><b>시스템 흐름</b></p>
<pre>
입구 카메라 → YOLOv8 차량·번호판 인식 → OCR → FastAPI → Spring Boot 서버 → MySQL
 Arduino 차단기(서보모터) 제어
</pre>
<p><b>담당 기능</b></p>
<ul>
<li>YOLOv8n 모델을 Roboflow 데이터셋과 Colab으로 직접 학습</li>
<li>Raspberry Pi 입구 카메라 인식 모듈, FastAPI로 인식 결과를 백엔드에 전달</li>
<li>Arduino + 서보모터 차단기, 초음파 센서로 차량 진입 감지</li>
</ul>
<p><b>트러블슈팅</b></p>
<ul>
<li><b>원거리 번호판 OCR 실패</b> — [해결 방법 작성]</li>
<li><b>차량이 아닌 물체 오인식</b> — [해결 방법 작성]</li>
<li><b>OpenCV 한글 깨짐</b> — Pillow로 한글 폰트를 렌더링해 해결</li>
<li><b>차단기 서보모터 떨림</b> — 천천히 단계적으로 회전시키고 동작 후 <code>detach()</code>로 신호를 끊어 해결</li>
<li><b>numpy 타입 JSON 직렬화 오류</b> — 응답 전 Python 기본 타입으로 변환해 해결</li>
</ul>
</details>
