# Slack & 환율 API 연동 실습

부트캠프 4주차 수업 내용으로, Slack 메시지 전송, 환율 API 연동, AWS S3 업로드 등을 학습한 내용입니다.

## 주요 학습 내용

### 1. Slack Webhook을 이용한 메시지 전송
- Slack Incoming Webhooks를 통한 메시지 전송
- 이미지가 포함된 메시지 전송
- 오류 처리 및 재시도 로직

### 2. 환율 API 연동
- Frankfurter API를 이용한 실시간 환율 조회
- 특정 기간의 환율 데이터 수집
- CSV 및 JSON 형식으로 데이터 저장

### 3. AWS S3 연동
- boto3를 이용한 S3 파일 업로드
- Lambda 함수를 통한 자동화
- Slack 알림과 결합한 데이터 파이프라인

## 설정 방법

### 1. 환경 변수 설정

프로젝트 루트 디렉토리에 `.env` 파일을 생성하고 다음 내용을 입력하세요:

```bash
# .env.example 파일을 복사하여 사용
cp .env.example .env
```

`.env` 파일 내용:
```
SLACK_WEBHOOK=your_actual_webhook_url
BUCKET_NAME=your_s3_bucket_name
```

### 2. Slack Webhook URL 발급 받기

1. Slack 워크스페이스 설정 > Apps 메뉴로 이동
2. "Incoming Webhooks" 검색 및 설치
3. 채널 선택 후 Webhook URL 복사
4. `.env` 파일의 `SLACK_WEBHOOK`에 붙여넣기

### 3. 환경 변수 로드 (Python)

```python
import os
from dotenv import load_dotenv

# .env 파일 로드
load_dotenv()

# 환경 변수 사용
webhook = os.environ.get('SLACK_WEBHOOK')
```

## 필요한 패키지

```bash
pip install requests boto3 python-dotenv
```

## 파일 구조

```
4주차/
├── slack_message.ipynb    # 주요 실습 노트북
└── README.md             # 이 파일
```

## 사용 예시

### Slack 메시지 전송
```python
import requests
import os

webhook = os.environ.get('SLACK_WEBHOOK')
requests.post(webhook, json={'text': '테스트 메시지'})
```

### 환율 데이터 조회
```python
resp = requests.get('https://api.frankfurter.dev/latest?from=USD&to=KRW')
data = resp.json()
rate = data['rates']['KRW']
print(f'현재 환율: {rate}원')
```

## 주의사항

- Slack Webhook URL은 절대 공개 저장소에 올리지 마세요
- `.env` 파일은 `.gitignore`에 포함되어 있습니다
- API 호출 시 과도한 요청을 피하기 위해 적절한 딜레이를 설정하세요
- 환율 API는 주말/공휴일 데이터가 없을 수 있습니다

## API 문서

- Frankfurter API: https://www.frankfurter.app/
- Slack Incoming Webhooks: https://api.slack.com/messaging/webhooks

## 라이선스

학습용 프로젝트입니다.
