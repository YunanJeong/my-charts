# simple-kafka-deploy

로컬테스트 등 용도로 가벼운 Kafka를 빠르게 배포하기 위한 Helm Chart

Kubernetes용 대시보드, Kafka, Kafka-ui만 포함한다.

다른 컴포넌트(Grafana, Streams, ...) 추가 시 별도 릴리즈로 배포하자.

## 구성

```sh
├── Chart.lock         # dependency 버전 확정 내용    
├── Chart.yaml         # 차트 파일(차트버전,앱버전,dependency버전 관리)
├── charts/            # Depedency chart 모음
├── templates/         # Helm template
└── values.yaml        # default value
```

## 메모

- 헬름차트 bitnami/kafka:23.0.7에서 보안설정이 없으면 파워쉘에서 Kafka에 네트워크 연결이 안될 수 있다. [참고](https://stackoverflow.com/questions/48603203/powershell-invoke-webrequest-throws-webcmdletresponseexception)
  - 윈도에선 일반 cmd를 사용한다.
- 외부연결 테스트는 curl, kafkacat, kafka-topics.sh 등으로 확인

- Dependency Charts 바로가기
  - [bitnami/kafka](https://artifacthub.io/packages/helm/bitnami/kafka)
  - [provectus/kafka-ui](https://artifacthub.io/packages/helm/kafka-ui/kafka-ui)
