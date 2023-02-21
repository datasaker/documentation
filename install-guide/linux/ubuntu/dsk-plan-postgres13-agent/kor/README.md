# Ubuntu 환경에 DataSaker Plan Postgres13 agent 설치하기

## DataSaker 설정파일을 생성하셨나요?
현재 Ubuntu 환경에서는 `DataSaker` 에이전트를 설치하기 위해서는 기본 설정 파일이 생성되어 있어야 합니다. 만약 기본 설정 파일을 생성하지 않았다면 생성하여 주시기 바랍니다. [DataSaker 설정하기](../../../README.md)

# Plan Postgres13 Agent 설치하기
## 1. agent-config 설정
```bash
>$ sudo dsk-plan-postgres13-agent init ${VAR_CLUSTER_NAME}
>$ cat /etc/datasaker/dsk-plan-postgres13-agent/agent-config.yml
>agent:
  metadata:
    agent_name: "dsk-plan-postgres13-agent"         # replace you want
    cluster_id: ${VAR_CLUSTER_NAME}                 # replace you want
  data_source_name:
    user:                                           # <user_name>
    password:                                       # <user_password>
    address:                                        # <database_address>
    port:                                           # <database_port>
    DBName:                                         # <database_name>
  explain:
    scrape_interval: 30s                            # <activity_session_scrape_time>
    scrape_timeout: 5s                              # <activity_session_scrape_query_timeout>
    slow_query_standard: 5s                         # <slow_query_standard> 
    executor_number: 10                             # <explain executor number>
    sender_number: 10                               # <explain sender number>
    activity_query_buffer: 50                       # <activity query buffer>
    plan_sender_buffer: 50                          # <explain result buffer>
```

## 2. 패키지 설치
> sudo 권한이 필요합니다.
```bash
> DSK_GLOBAL_APIKEY=${VAR_GLOBAL_APIKEY}

> curl -fsSL -o installer.sh https://nextcloud.exem-oss.org/s/nDbcJedX6dDynKL/download/dsk-plan-postgres13-agent-install.sh
> chmod 700 installer.sh
> sudo ./installer.sh

> dsk-plan-postgres13-agent --help
```

## 3. 패키지 실행
```bash
$ sudo dsk-plan-postgres13-agent start
Agent is running
```

## 4. 패키지 실행 상태 확인
### Running
```bash
$ sudo dsk-plan-postgres13-agent status
Agent is running
```
### Not Running
```bash
$ sudo dsk-plan-postgres13-agent status
Agent is not running
```

# Plan Postgres13 Agent 제거하기
## 1. 패키지 중단
```bash
$ sudo dsk-plan-postgres13-agent stop
```

## 2. 패키지 제거
```bash
$ sudo apt remove dsk-plan-postgres13-agent
```