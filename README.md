# Jenkins를 활용한 CI/CD 테스트

## 기술 스택
- AKS
- Github
- Jenkins (VM)
- Promethus
- Grafana


## 목표

### CI/CD 구성
1. (로컬)index.html 수정 및 github push 
2. (Jenkins) Nginx 이미지 빌드 및 Docker registry에 이미지 저장
    a. 이미지 저장 히스토리를 남기기 위한 Jenkins Build Number 태그
    b. AKS 배포를 위한 latest 태그
3. (K8S)deployment rollout을 사용해 신규 이미지 재배포

### 모니터링 구성
1. promethus 활용을 통한 기본적인 메트릭 수집
    - 메트릭 보존을 위한 영구 볼륨으로 Azure Files 사용
2. Grafana를 활용해 시각화 대시보드 구성