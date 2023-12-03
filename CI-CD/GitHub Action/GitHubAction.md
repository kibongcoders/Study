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

### Workflows

하나 이상의 작업을 실행 구성이 가능한 자동화 프로세스
워크플로우는 Yaml 파일로 정의
해당 파일은 레보지토리에 체크인 되어있습니다.
레포지토리에서 이벤트에 의해 트리거가될 때 실행되거나 수동으로 트리거하거나 정의된 일정에 따라 실행 가능

레포지토리의 .github/workflows 디렉토리에 정의되며, 여러 워크플로우를 가질 수 있습니다.
각각의 워크플로우는 다른 작업 집합을 수행할 수 있습니다.

### Events

워크플로우 실행을 트리거하는 리포지토리 내의 특정 활동
일정에 따라 워크플로우를 실행하거나 REST API에 게시하거나 수동으로 실행할 수도 있습니다.

### Job

워크플로우 내 실행되고 있는 같은 러너에서 실행되는 Step의 집합입니다.
각 Step은 실행될 셸 스크립트 이거나 실행될 액션입니다.
스텝은 순서대로 실행되며 서로 의존하고 있습니다.
동일한 러너에서 실행되기 때문에 한 단계에서 다른 단계로 데이터 공유가 가능합니다.

Job은 다른 작업과의 종속성을 구성할 수 있습니다.
기본적으로 작업은 종속성이 없고 서로 병렬로 실행됩니다.
작업이 다른 작업에 종속성을 가지면 종속된 작업이 완료될 때까지 대기한 후 실행됩니다.

### Actions

GitHub Actions 플랫폼을 위한 커스텀 프로그램이고, 복잡하지만 자주 반복되는 작업을 수행합니다.
워크플로우 파일에서 작성하는 반복적인 코드 양을 줄일 수 있습니다.

### Runners

러너는 워크플로우가 트리거 될 때 실행되는 서버입니다.
각 러너는 한번에 하나의 작업을 실행할 수 있습니다.
다양한 OS에서 러너를 제공하여 워크플로우를 실행하며, 각 워크플로우 실행은 새롭게 프로비저닝된 가상머신에서 실행됩니다.
GitHub 또한 큰 러너를 제공하고, 이는 더 큰 구성으로 사용 가능합니다.

## 기본적인 Yaml 파일 알아보기

```
name: learn-github-actions
run-name: ${{ github.actor }} is learning GitHub Actions
on: [push]
	jobs:
	check-bats-version:
		runs-on: ubuntu-latest
	    steps:
		    - uses: actions/checkout@v4
		    - uses: actions/setup-node@v3
		        with:
			        node-version: '14'
			- run: npm install -g bats
		    - run: bats -v

```

- name : 워크플로우의 이름입니다. 생략 시 워크플로우 파일 이름이 대신 생성됩니다.
- run-name : 워크플로우 실행 이름으로, 워크플로우 실행 목록에 표시되는 이름입니다.
- on : 워크플로우에 대한 트리거를 지정합니다. 위에 예제의 경우 push 이벤트를 사용하므로 변경을 푸시하거나 풀 리퀘스트를 병합할 때마다 워크플로우 실행이 트리거됩니다.
- jobs : 워크플로우에서 실행되는 모든 작업을 그룹화 가능합니다.
- check-bats-version : 'check-bats-version' 이라는 작업을 정의합니다.
- runs-on : 작업의 환경을 설정할 수 있습니다. GitHub에서 호스팅하는 가상머신에서 실행됨을 의미합니다.
- steps : job에서 실행되는 모든 단계를 그룹화 합니다.
- uses : 


# `uses` 키워드는 이 단계가 `actions/checkout` 액션의 `v4`를 실행하도록 지정합니다. 이는 저장소를 러너에 체크아웃하여 코드에 대한 스크립트 또는 다른 액션을 실행할 수 있게 합니다(빌드 및 테스트 도구와 같은). 워크플로가 저장소의 코드를 사용할 때마다 체크아웃 액션을 사용해야 합니다.
      - uses: actions/checkout@v4

# 이 단계는 `actions/setup-node@v3` 액션을 사용하여 지정된 Node.js 버전(이 예제에서는 버전 14)을 설치합니다. 이로써 `node` 및 `npm` 명령이 `PATH`에 추가됩니다.
      - uses: actions/setup-node@v3
        with:
          node-version: '14'

# `run` 키워드는 작업이 러너에서 명령을 실행하도록 합니다. 이 경우 `npm`을 사용하여 `bats` 소프트웨어 테스트 패키지를 설치합니다.
      - run: npm install -g bats

# 마지막으로 `bats` 명령을 실행하여 소프트웨어 버전을 출력합니다.
      - run: bats -v
