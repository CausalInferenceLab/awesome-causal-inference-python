# Awesome Causal Inference with Python

인과추론 방법을 주제별 파이썬 노트북으로 정리하는 가짜연구소 인과추론팀의 코드북입니다.

- 웹페이지: https://causalinferencelab.github.io/awesome-causal-inference-python/main/intro.html
- 노트북 원본: [`book/`](book)

| 파트 | 다루는 내용 |
| --- | --- |
| Part 1 | OLS, IPW·PSM, 이중차분법, 통제집단합성법, SDID, 도구변수, RDD |
| Part 2 | DML, 메타러너, 정책 학습, 신경망 CATE, 해석 가능성, 백도어 기준, 인과 발견, MSM |
| Examples | Fractional Uplift, 예산 제약 최적화, 프론트도어 기준 |

## 🛠️ 우리의 개발 문화 (Our Development Culture)
**우리의 개발 문화**  
```python
class CollaborationFramework:
    def __init__(self):
        self.tools = {
            'communication': 'kakaoTalk',
            'version_control': 'GitHub Projects',
            'ci/cd': 'Github Actions',
            'docs': 'TeachBooks'
        }
    
    def workflow(self):
        return """주간 사이클:
        [일요일 미팅 종료]
                ↓ (issue + reviewer + backup_reviewer 할당)
        [월~토요일 자정]
          각자 issue 작업
          로컬 → 브랜치 push
          PR 생성 + 리뷰어 태그
                ↓
        [일요일 낮]
          리뷰어 PR 리뷰 & 코멘트
                ↓
        [일요일 저녁]
          미팅:
           - 리뷰어 발표 & assignee 발표 내용 보충
           - 논의 → merge & issue close
           - 새로운 issue/reviewer 할당
                ↓
         [새로운 주차 사이클 시작]"""
```


### 주차별 활동 (2025 가을 일정)

| idx | Date | 미팅 | 불참 예정 | 토픽 발표 1 | 토픽 발표 2 |
| --- | ---- | ---- | -------- | ----------- | ----------- |
| 1 | 9/7 | ✅ |  |  |  |
| 2 | 9/14 | ✅ |  |  |  |
| 3 | 9/21 | ✅ |  |  |  |
| 4 | 9/28 | ✅ |  | 권명석 |  |
| 5 | 10/5 | ✅ | 강민석(가족여행_비행기 탑승) | 김정현 |  |
| 6 | 10/12 | ✅ |  | 조해창 |  |
| 7 | 10/19 | ✅ |  | 장성은 | 강민석 |
| 8 | 10/26 |  | — | — | — |
| 9 | 11/2 | ✅ | 장성은 (학회), 조해창 (회사 워크샵) | 김시은 |  |
| 10 | 11/9 | ✅ | 권명석, 장성은 (학회) |  |  |
| 11 | 11/16 |  | — | 전소영 | — |
| 12 | 11/23 | ✅ |  | 김시은 |  |
| 13 | 11/30 | ✅ |  | 김정현 |  |
| 14 | 12/7 | ✅ |  | 강민석 | 전소영 |
| 15 | 12/14 | ✅ | 전소영 (학회) | 장성은 | 조해창 |
| 16 | 12/21 | ✅ |  | — | — |

