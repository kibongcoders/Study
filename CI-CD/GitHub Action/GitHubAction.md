# GitHub Actions

GitHub Actions 빌드, 테스트 및 배포 파이프라인을 자동화할 수 있는 CI/CD 플랫폼이다.

단순한 DevOps를 넘어서, 레포지토리에서 다른 이벤트가 발생할 때 워크 플로우를 실행할 수 있도록 하는 기능을 제공

OS에 따라 변하지 않으므로 매우 유용하게 사용가능하다.

## 구성요소

GitHub Actions 워크플로우는 레포지토리에서 이벤트가 발생할 때 트리거 되도록 구성 가능합니다.
워크플로우는 하나 이상의 작업으로 구성되고, 이는 순차적 또는 병렬로 실행 가능합니다.
각 작업은 자체 가상 머신 러너 안에서 실행되거나 컨테이너 안에서 실행
각 단계에서 사용자가 정의한 스크립트를 실행하거나 워크플로우를 간소화 또는 재사용 가능한 Action을 실행 가능합니다.

![GitHub Action](https://docs.github.com/assets/cb-25535/mw-1440/images/help/actions/overview-actions-simple.webp)

