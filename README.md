# OCSF 기반 보안 로그 통합 및 분석 프로젝트

기업 내 각 부서의 로그를 수집하여 중앙 서버에서 저장, 분석, 시각화하고, 이상행위를 탐지해 실시간으로 알림까지 제공하는 **로그 분석 아키텍처**


## 구성 요소

- **Vector / Filebeat / Winlogbeat**  
  각 부서에서 발생하는 로그를 수집하는 에이전트
  - `Vector`: syslog 기반 텍스트 로그 수집 (직접 Elasticsearch로 전송 가능)  
  - `Filebeat`: 일반 파일 로그 수집 (예: apache, nginx 로그 등)  
  - `Winlogbeat`: 윈도우 이벤트 로그 수집

- **Logstash**  
  수집된 로그를 구조화(JSON 변환), 필터링, 태깅하는 전처리 역할
  Vector 외 다른 비트 계열 로그는 Logstash를 통해 Elasticsearch로 전달

- **Elasticsearch**  
  정형화된 로그 데이터를 저장하고 빠르게 검색할 수 있도록 인덱싱
  모든 로그의 중심 저장소로 작동

- **Kibana**  
  Elasticsearch에 저장된 로그를 대시보드 형태로 시각화
  사용자(UI)는 이를 통해 로그를 탐색하고 분석 가능

- **Sigma Rule**  
  보안 탐지 룰 엔진으로, 저장된 로그에서 이상 행위나 공격 패턴을 탐지
  조건에 맞는 이벤트 발생 시 알림을 트리거

- **Slack**  
  Sigma Rule에 의해 탐지된 이벤트를 운영자에게 실시간 알림 형태로 전송
  빠른 대응을 위한 경보 채널 역할 수행
  

## 데이터 흐름 요약
> 부서 → 수집기 → Logstash / Vector → Elasticsearch → Kibana / Sigma → UI / Slack


## 주요 기능

- 다양한 로그 포맷에 대한 통합 수집 및 정규화
- Kibana를 통한 직관적인 로그 분석과 대시보드 제공
- Sigma 기반의 이상행위 탐지 룰 적용
- Slack 연동을 통한 실시간 보안 이벤트 알림

<img width="678" alt="14" src="https://github.com/user-attachments/assets/42d36979-ebf8-4e89-b417-d92b4071d0c2" />


