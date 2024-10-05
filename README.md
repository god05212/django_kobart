# News Summary Web Application
## 뉴스 요약 웹 애플리케이션
프로젝트 기간: 2024/09/09(월) ~ 2024/09/27(금)  
## 개요
이 프로젝트는 ‘네이버 뉴스’ 사이트의 정보를 활용하여 사용자가 입력한 검색어에 따라 관련 뉴스 기사를 추천하고 요약하는 웹 기반 애플리케이션입니다. 현재 정보 과부하 시대에서 효과적인 뉴스 소비의 필요성이 커지고 있으며, 사용자는 방대한 정보 속에서 원하는 내용을 쉽게 찾아야 합니다. 이에 따라, 사용자 맞춤형 뉴스 추천과 요약 기능을 통해 효율적인 정보 소비를 돕기 위해 이 프로젝트를 기획하였습니다.

프로젝트 목표  
- 데이터 수집: BeautifulSoup을 사용하여 네이버 뉴스에서 관련 기사를 자동으로 수집합니다.  
- 기사 요약: BART 모델을 활용하여 수집된 뉴스 기사를 간결하고 이해하기 쉬운 형식으로 요약합니다.  
- 명사 추출: 요약된 기사에서 주요 명사를 분석하고, 빈도수에 따라 추천 검색어를 생성합니다.  
- 사용자 맞춤 추천: 사용자가 입력한 키워드에 따라 관련 기사를 정렬하고 추천하여 정보 탐색을 돕습니다.  

이 프로젝트를 통해 사용자는 원하는 뉴스 기사를 보다 효율적으로 찾을 수 있으며, 기업은 뉴스 소비 트렌드를 이해하는 통찰을 얻고 향후 고객 맞춤형 서비스 개발에 기여할 수 있습니다.
<br/>
## 사용한 데이터셋
웹 크롤링한 데이터 (네이버 뉴스)
<br/>

## 프로젝트 목록
1. 모델 로드 및 데이터 처리
   - 1.1 KoBART 모델 및 토크나이저 로드  
   - 1.2 불용어 리스트 정의  

2. 기능 구현
   - 2.1 POST 요청 처리 및 응답 생성 (first_view)  
   - 2.2 뉴스 기사 크롤링  
     - 2.2.1 Naver 뉴스 검색 API 활용  
     - 2.2.2 기사 정보 추출 및 구조화  
       - 2.2.2.1 기사 제목  
       - 2.2.2.2 기사 링크  
       - 2.2.2.3 기사 본문 내용  
   - 2.3 본문 요약 생성 (summarize_texts)  
   - 2.4 명사 추출 및 빈도수 계산 (extract_nouns)  
   - 2.5 빈도수 높은 명사 10개 선정  

3. 사용자 인터페이스 설계
   - 3.1 검색창 및 음성 입력 버튼의 HTML 구조  
   - 3.2 결과 표시 및 로딩 인디케이터 구현  
   - 3.3 검색 결과 표시 페이지 구성  
     - 3.3.1 기사 목록 출력 및 포맷 설정  
     - 3.3.2 추천 검색어 목록 제공  
     - 3.3.3 재검색 링크 제공  

4. 음성 인식 기능 통합
   - 4.1 SpeechRecognition API 사용 및 설정  
   - 4.2 음성 인식 결과를 입력란에 자동 입력  

5. 결론 및 향후 연구 방향
   - 5.1 프로젝트 요약 및 성과  
   - 5.2 향후 발전 방향 및 개선 제안
  


## 프로젝트 수행 과정
### 1. 모델 로드 및 데이터 처리
#### 1.1 KoBART 모델 및 토크나이저 로드
- **작업 설명**: transformers 라이브러리를 사용하여 KoBART 모델과 토크나이저를 로드.
- **구현 과정**
  - PreTrainedTokenizerFast와 BartForConditionalGeneration 클래스를 사용하여 사전 훈련된 모델을 불러옴.
  - GPU를 사용할 수 있도록 설정하여 처리 속도를 높임.

#### 1.2 불용어 리스트 정의
- **작업 설명**: 자연어 처리에서 의미를 전달하지 않는 불용어 목록을 정의.
- **구현 과정**
  - 한국어 불용어를 수집하여 리스트 형태로 저장.
  - 이 리스트는 나중에 명사 추출 시 필터링.

### 2. 기능 구현
#### 2.1 POST 요청 처리 및 응답 생성 (first_view)
- **작업 설명**: 클라이언트로부터 POST 요청을 처리하여 사용자의 검색어를 받아옴.
- **구현 과정**
  - Django의 뷰 함수를 정의하여 POST 요청이 들어올 경우 `news_summary` 함수를 호출.
  - GET 요청 시에는 초기 페이지를 렌더링.

#### 2.2 뉴스 기사 크롤링
##### 2.2.1 Naver 뉴스 검색 API 활용
- **작업 설명**: 사용자가 입력한 검색어를 기반으로 Naver 뉴스 API를 호출.
- **구현 과정**
  - requests 라이브러리를 사용하여 API URL에 GET 요청을 보냄.
  - 응답 상태를 체크하고 오류 처리 로직을 추가.

##### 2.2.2 기사 정보 추출 및 구조화
- **작업 설명**: API 응답에서 뉴스 기사의 제목, 링크, 본문을 추출.
- **구현 과정**
  - BeautifulSoup를 사용하여 HTML 내용을 파싱.
  - 기사 제목, 링크, 본문 내용을 리스트에 저장.

###### 2.2.2.1 기사 제목
- **작업 설명**: 뉴스 기사의 제목을 추출.
- **구현 과정**: find 메서드를 사용하여 제목을 가져오고 공백을 제거.

###### 2.2.2.2 기사 링크
- **작업 설명**: 각 기사의 URL 링크를 추출.
- **구현 과정**: 링크 정보를 속성으로 가져옴.

###### 2.2.2.3 기사 본문 내용
- **작업 설명**: 기사 본문 내용을 추출.
- **구현 과정**: 본문이 없을 경우 '본문 없음'으로 표시하고 내용을 리스트에 추가.

#### 2.3 본문 요약 생성 (summarize_texts)
- **작업 설명**: 추출한 기사 본문을 요약.
- **구현 과정**: KoBART 모델을 사용하여 각 기사의 본문을 요약하고 요약된 내용을 리스트에 저장.

#### 2.4 명사 추출 및 빈도수 계산 (extract_nouns)
- **작업 설명**: 요약된 본문에서 명사를 추출하고 빈도수를 계산.
- **구현 과정**:
  - Okt 형태소 분석기를 사용하여 명사를 추출하고 불용어를 필터링.
  - Counter 객체를 사용하여 빈도수를 계산.

#### 2.5 빈도수 높은 명사 10개 선정
- **작업 설명**: 가장 빈도수가 높은 명사 10개를 선정.
- **구현 과정**: Counter 객체의 `most_common` 메서드를 사용하여 빈도수가 높은 명사를 리스트 형태로 저장.

### 3. 사용자 인터페이스 설계
#### 3.1 검색창 및 음성 입력 버튼의 HTML 구조
- **작업 설명**: 사용자가 검색어를 입력할 수 있는 입력란과 음성 입력 버튼을 구현.
- **구현 과정**:
  - HTML 양식을 사용하여 입력란과 버튼을 설계.
  - CSS를 사용하여 스타일을 설정.

#### 3.2 결과 표시 및 로딩 인디케이터 구현
- **작업 설명**: 검색 결과를 표시하고 로딩 인디케이터를 구현.
- **구현 과정**: JavaScript를 사용하여 비동기 요청 중 로딩 상태를 표시.

#### 3.3 검색 결과 표시 페이지 구성
##### 3.3.1 기사 목록 출력 및 포맷 설정
- **작업 설명**: 검색 결과로부터 추출한 기사 정보를 동적으로 출력.
- **구현 과정**: Django의 템플릿 엔진을 사용하여 기사 목록을 출력하고 스타일을 적용.

##### 3.3.2 추천 검색어 목록 제공
- **작업 설명**: 사용자 검색을 기반으로 추천 검색어를 동적으로 생성.
- **구현 과정**: 빈도수 높은 명사를 추천 검색어로 표시.

##### 3.3.3 재검색 링크 제공
- **작업 설명**: 사용자가 쉽게 재검색할 수 있도록 링크를 제공.
- **구현 과정**: Django의 URL 템플릿 태그를 사용하여 링크를 생성.

### 4. 음성 인식 기능 통합
#### 4.1 SpeechRecognition API 사용 및 설정
- **작업 설명**: 웹 브라우저의 SpeechRecognition API를 설정.
- **구현 과정**: JavaScript에서 음성 인식 객체를 생성하고 언어를 한국어로 설정.

#### 4.2 음성 인식 결과를 입력란에 자동 입력
- **작업 설명**: 음성 인식이 완료되면 인식된 텍스트를 검색 입력란에 자동으로 입력.
- **구현 과정**: 음성 인식 결과를 텍스트 입력란에 추가하여 사용자 편의성을 높임.



## 모델의 test dataset에 대한 R2 Score (소수점 다섯째 자리에서 반올림) 
| Model | R2 Score |
|:--------------------------|:-------|
| GradientBoostingRegressor | 0.8802 |
| LightGBMRegressor         | 0.8760 |
| RandomForestRegressor     | 0.8748 |
| Stacking                  | 0.8655 |
| Multi-Layer Perceptron    | 0.8582 |
| TabNetRegressor           | 0.7426 |

## 최종 모델
GradientBoostingRegressor
- test dataset에 대한 결과
  - RMSE: 약 4311.7676
  - R2 Score: 약 0.880248

## 결론
**GradientBoostingRegressor**: 하이퍼파라미터 최적화 후 RMSE가 4329.21에서 4311.77로, R² Score가 0.8793에서 0.8802로 향상되며, 최종적으로 가장 높은 성능을 기록함.

**LightGBMRegressor, RandomForestRegressor, Stacking**: GradientBoostingRegressor와 비교했을 때 상대적으로 낮은 성능을 보임.

**Keras-Tuner를 이용한 딥러닝 하이퍼파라미터 최적화**: 여전히 GradientBoostingRegressor와 같은 머신러닝 모델에 비해 성능이 미치지 못함. 이는 정형 데이터에서 딥러닝 모델의 성능이 제한적일 수 있음.

**TabNetRegressor**: RMSE가 6321.47로 가장 높고, R² Score는 0.7426으로 가장 낮음. 이는 정형 데이터에서 딥러닝 기반 모델의 성능이 기대 이하였으며, 데이터 수가 부족해 딥러닝 모델이 최적의 성능을 발휘하지 못했을 가능성이 있음.

## 향후 보안할 점
**데이터 수와 특성 부족**: 데이터 수가 부족하고 의료 관련 특성이 단순하다는 점이 아쉬웠습니다. 새로운 특성을 추가하고 피처 간의 상호작용을 고려하면 모델의 예측 성능을 향상될 것입니다.

**Feature Importance 분석**: 모델이 어떤 특성에 의존하는지 파악하기 위한 Feature Importance 분석이 부족했습니다. 이를 통해 중요 특성을 강조하고 불필요한 특성을 제거하면 모델 성능이 더욱 개선될 것입니다.

**한국 실정 반영**: 미국 의료 기반 시스템으로 측정된 의료비 데이터를 한국 실정에 맞게 소득분위 및 보험료 분석을 추가하면, 가계 재정에 대한 실질적인 인사이트를 제공하여 분석의 유용성이 높아질 것입니다.
