# 2026년 6월 금융 SNS용 MyData 합성 데이터셋

이 레포지토리는 금융 SNS 서비스를 설계하고 검증하기 위해 만든 합성 데이터셋입니다.

하나핀크 리얼리처럼 인증된 금융 활동을 기반으로 사용자의 금융 습관을 보여주고, 뱅크샐러드처럼 자산과 가계부를 분석하며, 인스타그램처럼 피드와 팔로우가 있는 서비스를 가정했습니다. 이를 위해 2026년 6월 한 달 동안의 대한민국 청년 금융 활동을 사람 단위로 구성했습니다.

전체 산출물은 아래 폴더에 있습니다.

```text
outputs/financial_sns_mydata_202606/
```

## 무엇이 들어 있나요?

총 199명의 가상 페르소나가 들어 있습니다. 각 페르소나는 만 19~34세의 대한민국 청년으로 설정되어 있으며, 직업, 소득대, 지출대, 가구 형태, 소비 성향, 금융 목표, 투자 성향이 함께 정의되어 있습니다.

각 사람별 번들은 다음 데이터를 포함합니다.

- `profile.json`: 나이, 직업, 소득/지출 수준, 라이프스타일, 금융 목표 등 페르소나 정보
- `ledger.csv`, `ledger.xlsx`: 2026년 6월 한 달간의 소득, 소비, 저축, 투자 내역
- `api/`: 은행, 카드, 전금, 금투, 공통 영역의 MyData 유사 JSON 응답
- `social/`: 금융 SNS 앱 안에서 필요한 익명 프로필, 팔로우, 피드, 반응 데이터
- `manifest.json`: 해당 페르소나 번들의 파일과 검증 정보를 묶은 인덱스

## SNS 데이터는 무엇인가요?

`social/` 폴더의 데이터는 MyData API 응답이 아닙니다.

이 데이터는 금융 SNS 앱을 만들 때 필요한 서비스 기능 검증용 companion 데이터입니다. 예를 들어 사용자의 금융 목표 공유, 저축 달성 피드, 투자 기록 공유, 팔로우 관계, 좋아요와 댓글 같은 앱 내부 활동을 표현합니다.

따라서 이 데이터셋은 다음처럼 구분해서 봐야 합니다.

- `api/`: 금융기관 조회 결과를 모사한 원천 응답 데이터
- `ledger.csv`: 원천 금융 데이터를 분석하기 쉽게 정리한 가계부 데이터
- `profile.json`: 사용자를 설명하는 합성 페르소나 데이터
- `social/`: 원천 금융 데이터에서 파생된 앱 기능 검증용 SNS 데이터

즉, SNS 데이터는 금융 데이터에 섞어 넣은 것이 아니라 금융 SNS 서비스 설계를 위해 별도 레이어로 추가한 데이터입니다.

## 폴더 구조

```text
outputs/financial_sns_mydata_202606/
  bundles/
    P001/
      profile.json
      ledger.csv
      ledger.xlsx
      api/
      social/
      manifest.json
    ...
    P199/
  aggregates/
    personas.jsonl
    ledger_all.csv
    feature_matrix.csv
    api_index.json
    social_edges.csv
    social_feed.csv
    social_reactions.csv
  validation/
    validation_report.json
    first10_review.md
    cluster_readiness_report.md
    schema_drift_report.md
    subagent_review_summary.md
  references/
    input_manifest.json
  README.md
```

## 먼저 보면 좋은 파일

- 데이터셋 설명: `outputs/financial_sns_mydata_202606/README.md`
- 전체 가계부 통합본: `outputs/financial_sns_mydata_202606/aggregates/ledger_all.csv`
- 전체 페르소나 목록: `outputs/financial_sns_mydata_202606/aggregates/personas.jsonl`
- 군집화/분석용 특성 테이블: `outputs/financial_sns_mydata_202606/aggregates/feature_matrix.csv`
- 전체 검증 결과: `outputs/financial_sns_mydata_202606/validation/validation_report.json`
- 최초 10건 리뷰: `outputs/financial_sns_mydata_202606/validation/first10_review.md`
- 생성 스크립트: `work/generate_financial_sns_dataset.py`

## 어떤 분석을 할 수 있나요?

이 데이터셋은 다음 질문을 검증할 수 있도록 구성되어 있습니다.

- 개인별 1개월 자산 흐름을 추적할 수 있는가?
- 식비, 교통, 구독, 쇼핑 등 소비 카테고리별 집계가 가능한가?
- 소득 대비 저축률을 계산할 수 있는가?
- 주식 투자 매수/보유 내역을 집계할 수 있는가?
- 거래 내역만 보고도 소득, 지출, 라이프스타일 같은 개인 특성을 유추할 수 있는가?
- 전체 페르소나를 대상으로 군집화가 가능한가?
- 금융 SNS 피드, 팔로우, 반응 같은 앱 기능을 함께 테스트할 수 있는가?

## 검증 결과

생성된 데이터셋은 구조 검증, JSON 파싱 검증, 가계부/API 참조 연결 검증, 집계 가능성 검증, 군집화 준비성 검증을 통과했습니다.

- 검증 상태: `PASS`
- 페르소나 수: `199`
- 검증 에러 수: `0`
- 군집화 silhouette 유사 점수: `0.2893`
- 최소 군집 크기: `8`

## 주의사항

이 레포지토리의 데이터는 모두 합성 데이터입니다. 실제 개인의 금융 거래나 개인정보를 복사한 데이터가 아닙니다.

원본 엑셀과 API 예시 파일은 형식 참고용으로만 사용했으며, 공개 레포지토리에는 포함하지 않았습니다.

공개된 데이터는 서비스 기획, UX 설계, 데이터 모델링, 추천/군집화 실험, 목업 API 개발을 위한 샘플로 사용하는 것을 전제로 합니다.
