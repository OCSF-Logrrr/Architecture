# OCSF 기반 보안 로그 통합 및 분석 프로젝트

기업 내 각 부서의 로그를 수집하여 중앙 서버에서 저장, 분석, 시각화하고, 이상행위를 탐지해 실시간으로 알림까지 제공하는 **로그 분석 아키텍처**


## 전체 흐름도

1. **로그 수집**
   - 리눅스 서버(WAF, 방화벽 등)에서 발생하는 로그를 Filebeat를 통해 수집
   - 수집된 로그는 Kafka의 `raw-logs` 토픽에 저장
     (Kafka 토픽은 로그 저장소 역할)

2. **로그 정규화**
   - `raw-logs` 토픽의 로그는 Python 기반 파서 코드가 포함된 Docker 컨테이너에서 처리
   - 로그는 OCSF 스키마에 맞춰 정규화되며,
   - 결과는 Kafka의 `ocsf-logs` 토픽에 저장

3. **로그 분석 및 시각화**
   - `ocsf-logs` 토픽의 로그를 Logstash가 수집하여 파싱
   - 파싱된 데이터는 Elasticsearch에 저장
   - Kibana를 통해 시각화하고,
   - Sigma 탐지 규칙을 적용하여 이상 행위를 탐지
   - 탐지 이벤트는 Slack으로 알림을 전송
  

## 데이터 흐름 요약
> WAF / 방화벽 로그 → Filebeat → Kafka (raw-logs 토픽) → OCSF Docker 컨테이너 + Python 파서 → Kafka (ocsf-logs 토픽) → Logstash → Elasticsearch → Kibana + Sigma → 시각화 및 Slack 알림


## 주요 기술 스택

- Kafka (raw-logs / ocsf-logs)
- Filebeat
- Docker + Python (OCSF normalization)
- Logstash
- Elasticsearch
- Kibana
- Sigma Rules
- Slack (Alerting)

![image](https://github.com/user-attachments/assets/67722c31-8d9d-4e6d-8295-fca139514032)

---
### 중간발표 이후 아키텍처


![image](https://github.com/user-attachments/assets/3b04ebbc-762c-48f1-a757-3f52a974ab18)


