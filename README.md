# 🍽️ COOKiT
## 요리 초보를 위한 보유 식재료 기반 맞춤 레시피 큐레이터 서비스

COOKiT은 요리 초보자를 위한 AI 기반의 요리 지원 서비스로, 사용자가 보유한 식재료를 기반으로 맞춤형 레시피를 추천하고 식재료를 쉽게 관리할 수 있도록 지원합니다. OCR을 통한 식재료 자동 등록, GPT-4o Vision 기반 신선도 분석, 개인화된 레시피 추천 기능을 통해 요리의 진입장벽을 낮추고 식재료 낭비를 줄이는 것을 목표로 합니다.
## 📁 Repository 구조

- [WEFRESH-FRONT](https://github.com/2024EwhaCapstone/WEFRESH-FRONT) (React Native-ios)
- [WEFRESH-SERVER](https://github.com/2024EwhaCapstone/WEFRESH-SERVER) (SpringBoot)

---

## 📦 Source Code 설명

### Frontend (📱 Mobile App)
- React Native 기반 앱 (ios) + Typescript
- AWS Amplify를 통해 배포하여 CI/CD 자동화와 애자일 방식의 개발 목표로 함.

### Backend (🌐 Server)
- Spring Boot 기반으로 REST API를 개발하며, Docker로 컨테이너화하여 EC2 인스턴스 내에서 구동.
- Blue-Green Deployment 방식으로 두 개의 컨테이너를 운영하고, 무중단 배포를 위해 Nginx로 트래픽을 전환.
- 데이터베이스는 Amazon RDS(MySQL)를 사용하고, 이미지 및 파일 저장은 Amazon S3로 처리.


### AI 🤖

#### GPT-4o mini API + DALL·E : 맞춤형 레시피 추천 기능
- 사용자 식재료 데이터 수집
  - 사용자가 냉장고에 등록한 식재료 목록을 백엔드 DB에서 불러옴
  - 유통기한이 임박한 재료 우선순위로 정렬
- 프롬프트 구성 및 모델 호출
- GPT-4o Mini API 호출 및 결과 파싱
  - 모델의 응답을 전처리하여 제목, 재료, 조리법 등으로 분리
- 영문 제목 생성
  - GPT-4o Mini에 레시피 제목을 영어로 변환 요청
  - 이미지 생성에 활용할 수 있는 간결하고 직관적인 영어 문구 확보
- 레시피 이미지 생성 (DALL·E)
  - 영문 제목을 기반으로 DALL·E 모델을 호출하여 레시피에 어울리는 이미지 생성
- 이미지 URL 영구화 처리
- 최종 응답 반환 

#### GPT-4o Vision API: 이미지로 신선도 상세 분석 기능
- 사용자 이미지 입력
  - 사용자가 앱에서 식재료 사진을 촬영 
- 원본 이미지를 GPT-4o Vision에 입력
- GPT-4o Vision이 신선도 분석 결과 텍스트로 출력 
  - 예: “김치 표면에 하얀 점이 관찰됨 → 효모균일 가능성 있음”
  
#### AWS Textract API: 이미지로 식재료 상세 정보 자동 입력
- 사용자 이미지 입력
  - 사용자가 영수증, 식품 포장지, 라벨 등을 촬영  
- OCR로 텍스트 인식
  - 예: 이름: 김치 / 유통기한: 2025.04.01 / 중량: 300g
- 텍스트 전처리 및 파싱
  - 정규 표현식, 키워드 기반 파싱을 통해 다음 정보를 자동 추출 : 이름, 유통기한, 중량/수량 
- 자동 등록 폼으로 전달 
  - 파싱된 정보를 식재료 등록 화면의 입력 폼에 자동 채워 넣음 
  - 사용자는 확인 후 수정하거나 바로 저장 가능

---
## 📱 How to build
레포지토리 클론해서 프로젝트 돌리기

```bash
git clone https://github.com/2024EwhaCapstone/WEFRESH-FRONT.git
cd WEFRESH-FRONT
npm install
cd ios
pod install
cd ..
npx react-native bundle \
  --entry-file index.js \
  --platform ios \
  --dev false \
  --bundle-output ios/main.jsbundle \
  --assets-dest ios
npx react-native run-ios
```

## 🛠️ How to test
####  iOS 시뮬레이터에서 `.app` 바이너리 실행 (빌드된 앱 테스트)

1. xcode 실행
2. `.app` 바이너리 파일 준비 (예: `000.app`)
   (cookit.app binary file 설치)
  [📦 Download cookit.zip](https://github.com/2024EwhaCapstone/Growth/releases/download/v1.0.0/cookit.zip)
3. 다음의 명령어 실행
```bash
open -a Simulator
xcrun simctl install booted ~/app파일 주소
xcrun simctl launch booted org.reactjs.native.yoonjinchoi.wefresh
```
