# my-helm-charts

커스텀 헬름 차트를 개발 & 배포하는 저장소

## 차트 수정 시

```shell
# dependency 다운로드 및 Chart.lock 최신화 (Chart.yaml이 있는 곳에서 실행)
helm dependency update

# 차트를 아카이브 파일로 생성
helm package {차트 경로}
```
