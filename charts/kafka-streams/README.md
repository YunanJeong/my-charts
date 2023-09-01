# kafka-streams

카프카 스트림즈용 헬름 차트 (kstreams) 와 이를 활용하는 예시

이 저장소에서는 차트만 편집하고, [배포용 스트림즈앱 이미지는 다른 저장소](https://github.com/YunanJeong/kafka-streams-deploy)에서 작업하도록 한다.

## 이미지 관련

많은 헬름 차트에서,
앱(image.repository)은 고정되어있고, 이미지 허브(image.registry)와 버전(image.tag)만이 자주 변경된다.

그러나 **Kafka Streams 차트에서는,
앱(image.repository)이 지속적으로 바뀌어야** 한다. 여러 요구사항과 유스케이스에 따른 비즈니스로직을 일반화하기 어렵기 때문이다.

이것이 헬름차트로 구현되려면 **이미지 자체(Docker Context)가 헬름 차트의 커스텀 Value**인 것처럼 함께 취급될 필요가 있다.

따라서 본 저장소에서는 헬름 차트 파일(kstreams/)뿐 아니라 Kafka Streams 앱 배포 예시(sample-values/mavenapp/)도 함께 첨부한다.

## Tree

```sh
├── kstreams/          # 헬름 차트
├── chartrep/          # 배포용 헬름 차트 아카이브
├── skaffold.yaml      # 차트 개발용 skaffold
└── sample-values/     # 커스텀 value 오버라이딩 예시
    └── mavenapp/        # 스트림즈 앱 example     
        ├── image/          # Docker Context
        └── value.yaml      # helm-value
```

## 차트

```sh
# 배포용 차트 아카이브 생성
helm package kstreams/
```

## 스트림즈 앱 개발

```sh
# 개발용 이미지 빌드
# skaffold build -p {skaffold's profile}
skaffold build -p mavenapp

# 개발모드
# skaffold dev -p {skaffold's profile}
skaffold dev -p mavenapp

# 배포용 이미지 빌드 및 push
# skaffold build -p {skaffold's profile} --default-repo="{registry}" --tag={version} --push
skaffold build -p mavenapp -d "private.docker.wai/yunan" -t live --push
```

## 앱 배포

```sh
# 배포설치
# helm install {releaseName} {chart} -f {value.yaml}
helm install mavenapp chartrepo/kstreams-0.0.4.tgz -f values/mavenapp.yaml

# 환경변수 반영하여 배포설치
envsubst < values/mavenapp.yaml | mavenapp chartrepo/kstreams-0.0.4.tgz -f -
```
