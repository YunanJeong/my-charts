# kafka-streams

카프카 스트림즈용 헬름 차트 (kstreams) 와 이를 활용하는 예시

## 이미지 관련

많은 헬름 차트에서,
앱(image.repository)은 고정되어있고, 이미지 허브(image.registry)와 버전(image.tag)만이 자주 변경된다.

그러나 **Kafka Streams 차트에서는,
앱 자체(image.repository)가 지속적으로 바뀌어야** 한다. 여러 요구사항과 유스케이스에 따라 구현될 앱을 일반화하기 어렵기 때문이다.

이것이 헬름차트로 구현되려면 **이미지가 헬름 차트의 커스텀 Value**로 취급될 필요가 있다.

따라서 본 저장소에서는 헬름 차트 파일(kstreams/)뿐 아니라 Kafka Streams 앱 배포 예시(values/images/)도 함께 첨부한다.

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
# registry   e.g.): private.docker.wai/yunan

# App. 이미지 빌드
skaffold build --default-repo="private.docker.wai/yunan" --tag=alpha

# 개발
skaffold dev -p mavenapp --default-repo="private.docker.wai/yunan" --tag=alpha

# App. 이미지 빌드 및 push
skaffold build --default-repo="private.docker.wai/yunan" --tag=live

# 배포설치
helm install mavenapp kstreams/ -f values/mavenapp.yaml
```
