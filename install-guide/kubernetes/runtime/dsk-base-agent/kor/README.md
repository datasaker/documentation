# 쿠버네티스 환경에서 DataSaker Base agent 설치하기
`Base agent`는 서버에서 발생하는 다양한 정보를 실시간으로 수집합니다. 
예를 들어, 메모리, CPU 사용량 등 서버의 성능 지표, 네트워크 트래픽, Container 정보 등 다양한 정보를 수집할 수 있습니다. 
이를 통해 고객은 서버의 상태를 실시간으로 모니터링할 수 있으며, 이를 기반으로 서버의 성능을 최적화하고 안정성을 높일 수 있습니다. 
고객의 요구사항에 맞게 에이전트 설정을 조정하여 최적의 결과를 제공해 드립니다.

# DataSaker 선행 작업을 진행하였나요?
현재 Kubernetes 환경에 `DataSaker`의 선행 작업이 진행되지 않으셨다면 `DataSaker` 선행 작업을 먼저 진행하여 주시기 바랍니다. [DataSaker 선행 작업](${MANUAL_KUBERNETES_KR})

# Base agent 설치하기
## 1. Base agent 설정 값 등록

```shell
cat << EOF >> ~/datasaker/config.yaml

baseAgent:
  enabled: true
  enableMaster: true
  nodeAgent:
    logLevel: 'INFO'
  containerAgent:
    logLevel: 'INFO'
EOF
```

## 2. Base agent 설치

```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```

# Base agent 설정하기
Base agent의 설정 값의 의미와 default값은 다음과 같습니다. 사용자마다 에이전트 설정에 대해 다른 요구사항이 있습니다. 따라서 에이전트 설정을 사용자 설정에 맞게 조정해야 합니다. 최적의 결과를 위해 에이전트 설정을 조정하세요.
"~/datasaker/config.yaml"에서 해당 값을 추가하거나 수정하세요.

```yaml
baseAgent:
  enabled: false          # Base agent를 활성화를 설정합니다.
  enableMaster: true      # 기본적으로 마스터 노드의 정보를 수집합니다.
  tolerations: []         # 워커 노드에 taint가 설정되어 있을 경우 taint를 추가합니다.
  nodeAgent:              # 노드의 정보를 수집하는 agent입니다.
    imgPolicy: 'Always'   # agent의 Image Policy를 설정합니다. [Always, IfNotPresent, Never]
    imgVersion: 'latest'  # agent의 Image Version를 설정합니다.
    logLevel: 'INFO'      # agent의 log level을 설정합니다. [debug > info > warn > error > panic > fatal]
    listenPort: 19110     # agent가 사용하는 port를 설정합니다.
    resources:            # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
      requests:
        cpu: 100m
        memory: 512Mi
      limits:
        cpu: 1000m
        memory: 1000Mi
  containerAgent:         # 컨테이너의 정보를 수집하는 agent입니다.
    imgPolicy: 'Always'   # agent의 Image Policy를 설정합니다. [Always, IfNotPresent, Never]
    imgVersion: 'latest'  # agent의 Image Version를 설정합니다.
    logLevel: 'INFO'      # agent의 log level을 설정합니다. [debug > info > warn > error > panic > fatal]
    listenPort: 14194     # agent가 사용하는 port를 설정합니다.
    resources:            # agent의 resource를 설정합니다. 너무 작게할 경우 정상동작을 못할 수 있습니다.
      requests:
        cpu: 100m
        memory: 512Mi
      limits:
        cpu: 1000m
        memory: 1000Mi
```

# 트러블슈팅

## 포트 사용 이슈

Base Agent는 모니터링을 위해, 설치 환경에서 두 개의 포트를 사용합니다. (default=`14194`,`19110`) 해당 포트를 서버의 다른 프로그램이 이미 점유중일때, Base agent는 정상적으로 기동하지 못하며 다음과 같은 로그를 출력합니다.

```shell
{"level":"error","ts":"2023-03-09T08:19:03Z","msg":"error occurred from subprocess","error":"error occured during running child process. err: exit status 255"}
```

이 문제를 해결하기 위해서, config.yaml 파일에 `listenPort` 설정을 추가할 수 있습니다.

예를들어, `base agent` 내부의 `container agent`가 사용하는 14194 포트를 14195로 바꾸기 위해 다음과 같이 설정을 수정할 수 있습니다.

```yaml
baseAgent:
  enabled: true
  enableMaster: true
  nodeAgent:
    logLevel: 'INFO'
  containerAgent:
    logLevel: 'INFO'
    listenPort: 14195
```
