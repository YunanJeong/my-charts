# kafka-streams

카프카 스트림즈용 헬름 차트 (kstreams) 와 이를 활용하는 예시

## Tree

```sh
├── kstreams/          # 헬름 차트
├── skaffold.yaml      # 차트 개발용 skaffold
└── values/            # 커스텀 value 오버라이딩 예시
    ├── images/           
    │   └── mavenapp/     
    └── mavenapp.yaml
```

## 실행

```sh
# 개발
skaffold dev -p mavenapp --default-repo="127.0.0.1:5000/yunan" --tag=alpha

# App. 이미지 빌드 및 push
skaffold build --default-repo="127.0.0.1:5000/yunan" --tag=live --push

# 배포설치
helm install mavenapp kstreams/ -f values/mavenapp.yaml
```
