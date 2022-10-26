# Cli Overview

- Chaos Toolkit 의 핵심은 chaos 커맨드 라인이다. 
- chaos --help 를 통해서 사용법을 확인한다. 

```js
$ chaos --help

Usage: chaos [OPTIONS] COMMAND [ARGS]...

Options:
  --version                       Show the version and exit.
  --verbose                       Display debug level traces.
  --no-version-check              Do not search for an updated version of the
                                  chaostoolkit.
  --change-dir TEXT               Change directory before running experiment.
  --no-log-file                   Disable logging to file entirely.
  --log-file TEXT                 File path where to write the command's log.
                                  [default: chaostoolkit.log]
  --log-file-level [debug|info|warning|error]
                                  File logging level: debug, info, warning,
                                  error
  --log-format [string|json]      Console logging format: string, json.
  --settings TEXT                 Path to the settings file.  [default:
                                  /Users/1111489/.chaostoolkit/settings.yaml]
  --help                          Show this message and exit.

Commands:
  discover  가능한 실험을 발견한다. 
  info      Chaos Toolkit 환경에 대한 정보를 출력한다. 
  init      발견된 기능에서 새로운 실험을 초기화 한다. 
  run       SOURCE로 부터 로드된 실험을 실행한다. 

  settings  설정파일로 부터 읽기, 쓰기, 제거하기를 수행한다. 
  validate  SOURCE에서 실험을 검증한다. 
```

## Configure the Chaos Toolkit

- 대부분에서 Chaos Toolkit 는 설정이 필요하지 않다. 
- 그러나 그렇게 하면 로컬 머신의 yaml 파일에 저장된다. 

- Tip : 추가 구성이 필요한 기중 중 하나를 활성화 하지 않는한 해당 파일을 만들 필요가 없다. 기능에 추가 구성이 필요한 경우 해당 설명서에 그렇게 나와 있다. 

### Create the settings file

- Chaos Toolkit 의 설정 파일은 다음 경로에 있어야한다. 

```go
$HOME/.chaostoolkit/settings.yaml
```

- 이 파일은 민감한 정보를 포함한다. 주인만 읽을 수 있도록 지정해야한다. 

```go
chmod 600 $HOME/.chaostoolkit/settings.yaml
```

## How to Investigate Issues (문제 조사 방법)

- 실험이 예상대로 동작하지 않으면 chaos 명령으로 작성된 chaostoolkit.log 파일을 살펴봐야한다. 
- 이 파일은 Chaos Toolkit 코어로 부터 트레이스된 내용을 포함한다. 뿐만 아니라 툴킷의 로거를 사용한 확장도 포함되어 있다. 
- 해당 파일에 새 로그가 추가되면 파일이 커질 수 있다. 주저하지 말고 수시로 제거해 주어야한다. 

# The chaos discover command

- chaos discover 커맨드를 이용하여 Chaos Toolkit 통합 확장을 지정할 수 있다. 
- 통합에서 지원하는 경우 검색 보고설르 작성하기 위해 대상 환경을 탐색한다. 
- 이는 chaos init 명령에서 사용하여 자신의 카오스 엔지니어링 실험을 부트스트랩하는 데 도움이 된다. 

- 다음을 실행하여 사용 가능한 옵션을 볼 수 있다. 

```go
$ chaos discover --help

Usage: chaos discover [OPTIONS] PACKAGE

  Discover capabilities and experiments.

Options:
  --no-system-info       Do not discover system information.
  --no-install           Assume package already in PYTHONPATH.
  --discovery-path TEXT  Path where to save the the discovery outcome.
                         [default: ./discovery.json]
  --help                 Show this message and exit.
```

- chaos discover 명령을 사용하는 방법에 대한 자습서는 Chaos Toolkit 의 시작하기 자습서의 일부로 제공된다. 
- https://www.katacoda.com/chaostoolkit/courses/01-chaostoolkit-getting-started

## Discovering capabilities and experiements

- discover 를 실행하기 위해서 필요한것은 Chaos Toolkit 통합 확장을 지정하기만 하면 된다.(예: Kubernetes 사용)

```js
$ chaos discover chaostoolkit-kubernetes

[2021-07-30 11:43:38 INFO] Attempting to download and install package 'chaostoolkit-kubernetes'
[2021-07-30 11:43:45 INFO] Package downloaded and installed in current environment
[2021-07-30 11:43:45 INFO] Discovering capabilities from chaostoolkit-kubernetes
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.actions
[2021-07-30 11:43:45 INFO] Searching for probes in chaosk8s.probes
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.deployment.actions
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.deployment.probes
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.node.actions
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.node.probes
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.pod.actions
[2021-07-30 11:43:45 INFO] Searching for probes in chaosk8s.pod.probes
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.replicaset.actions
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.service.actions
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.service.probes
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.statefulset.actions
[2021-07-30 11:43:45 INFO] Searching for probes in chaosk8s.statefulset.probes
[2021-07-30 11:43:45 INFO] Searching for actions in chaosk8s.crd.actions
[2021-07-30 11:43:45 INFO] Searching for probes in chaosk8s.crd.probes
[2021-07-30 11:43:45 INFO] Discovery outcome saved in ./discovery.json

```

- chaos discover 커맨드는 기본적으로 ./discovery.json에 저장된 보고서를 생성한다. 
- --discover-report-path 옵션을 제공하여 이 보고서가 생성되는 위치를 지정할 수 있다. 

## Discovery without System Information

- 검색 프로세스 중에 대상 시스템을 조사하지 않으려면 --no-system-info 옵션을 제공할 수 있다. 

## Discovery without Installation of an Integration Extension¶

- 통합 확장이 이미 설치되어 있고 사용 가능한 경우 --no-install 옵션을 지정하여 검색 프로세스의 속도를 높일 수 있다. 

# The chaos init command 

- chaos init 명령을 사용하여 일반적으로 chaos discover 명령으로 생성된 검색 보고서를 만든 다음 통합 확장 및 대상환경(해당되는 경우)에 대해 발견된 내용을 기반으로 실험을 만든다. 
- 다음을 실행하여 사용 가능한 옵션을 볼 수 있다. 

```go
$ chaos init --help

Usage: chaos init [OPTIONS]

  Initialize a new experiment from discovered capabilities.

Options:
  --discovery-path PATH   Path to the discovery outcome.  [default:
                          ./discovery.json]
  --experiment-path PATH  Path where to save the experiment (.yaml or .json)
                          [default: ./experiment.json]
  --help                  Show this message and exit.
```

- chaos init 명령을 사용하는 방법에 대한 자습서는 Chaos Tolkit 의 시작하기 자습서의 일부로 제공된다. 
- https://www.katacoda.com/chaostoolkit/courses/01-chaostoolkit-getting-started

## Initialize a new experiment

- 발견된 것을 기반으로 새 실험을 초기화 하려면 chaos init 명령을 실행하기만 하면 된다. 

```go
$ chaos init

```

- 기본적으로 chaos init 명령은 ./discovery.json 파일을 찾아 새 실험 초기화의 기초로 사용한다. 
- --discovery-report-path 옵션을 제공하여 사용할 다른 파일을 지정할 수 있다. 
- 또한 init 명령의 기본 출력은 ./experiment.json 파일의 새로운 Chaos Toolkit 실험 정의이다.
- 다른 파일 이름을 선호하는 경우 --experiment-path 옵션을 사용하여 지정할 수 있다. 

# The chaos run command

- chaos run 커맨드를 이용하여 chaos 엔지니어링 실험을 실행할 수 있다.
- chaos run 커맨드의 옵션은 다음과 같이 확인할 수 있다. 

```go
$ chaos run --help

Usage: chaos run [OPTIONS] SOURCE

  Run the experiment loaded from SOURCE, either a local file or a HTTP
  resource. SOURCE can be formatted as JSON or YAML.

Options:
  --journal-path TEXT             Path where to save the journal from the
                                  execution.
  --dry                           Run the experiment without executing
                                  activities.
  --no-validation                 Do not validate the experiment before
                                  running.
  --no-verify-tls                 Do not verify TLS certificate.
  --rollback-strategy [default|always|never|deviated]
                                  Rollback runtime strategy. Default is to
                                  never play them on interruption or failed
                                  hypothesis.
  --var TEXT                      Specify substitution values for
                                  configuration only. Can be provided multiple
                                  times. The pattern must be key=value or
                                  key:type=value. In that latter case, the
                                  value will be casted as the specified type.
                                  Supported types are: int, float, bytes. No
                                  type specified means a utf-8 decoded string.
  --var-file PATH                 Specify files that contain configuration and
                                  secret substitution values. Either as a
                                  json/yaml payload where each key has a value
                                  mapping to a configuration entry. Or a .env
                                  file defining environment variables. Can be
                                  provided multiple times.
  --hypothesis-strategy [default|before-method-only|after-method-only|during-method-only|continuously]
                                  Strategy to execute the hypothesis during
                                  the run.
  --hypothesis-frequency FLOAT    Pace at which running the hypothesis. Only
                                  applies when strategy is either: during-
                                  method-only or continuously
  --fail-fast                     When running in the during-method-only or
                                  continuous strategies, indicate the
                                  hypothesis can fail the experiment as soon
                                  as it deviates once. Otherwise, keeps
                                  running until the end of the experiment.
  --help                          Show this message and exit.

```

- chaos run 명령을 사용하는 방법에 대한 자습서는 Chaos Toolkit 의 시작하기 자습서의 일부로 제공된다. 
- https://www.katacoda.com/chaostoolkit/courses/01-chaostoolkit-getting-started

## Executing an Experiment Plan

- 실험 계획을 실행하기 위해서 chaos run 커맨드에 파일 이름을 지정하면 된다. 

```go
$ chaos run experiment.json

[2018-01-30 16:35:04 INFO] Validating experiment's syntax
[2018-01-30 16:35:04 INFO] Experiment looks valid
[2018-01-30 16:35:04 INFO] Running experiment: My new experiment
[2018-01-30 16:35:04 INFO] No steady state hypothesis defined. That's ok, just exploring.
[2018-01-30 16:35:04 INFO] Action: kill_microservice
[2018-01-30 16:35:04 INFO] No steady state hypothesis defined. That's ok, just exploring.
[2018-01-30 16:35:04 INFO] Let's rollback...
[2018-01-30 16:35:04 INFO] No declared rollbacks, let's move on.
[2018-01-30 16:35:04 INFO] Experiment ended with status: completed
```

- chaos toolkit 은 기본적으로 journal.json 이라고 하는 저널에 계획에 있는 모든 단계를 기록한다. 
- journal-path 옵션을 사용하여 이 저널 출력 파일의 이름을 지정할 수 있다. 

## 실험 실행 리허설하기

- 실험이 올바른지 확인하기 위해서 --dry 옵션을 이용하여 리허설 할 수 있다. 

## 검증 없이 실행하기 

- --no-validation 옵션을 이용하여 실험 검증을 스킵할 수 있다. 

## 다양한 정상 상태 전략으로 실험 실행 

- 기본적으로 안정적인 상태는 실험 실행 전후에 테스트 된다. 
- 그러나 --hypothesis-strategy 매개변수를 통해 다른 전략을 지정할 수 있다. 
- 옵션은 다음과 같다. 
  - default
  - before-method-only
  - after-method-only
  - during-method-only
  - continuously

- 예를 들어

```go
$ chaos run ./experiment.json --hypothesis-strategy continuously
```

## 다양한 롤백 전략을 이용하여 실험 실행

- Chaos Toolkit 에서 rollback은 다음 두 가지 중 하나에 해당하지 않은한 항상 재생된다. 
  -  정상 상태 가설이 방법 이전에 벗어남
  -  컨트롤이 실행을 중단했다.
  -  chaos 명령은 SIGINT또는 SIGTERM 신호를 수신한다. 

- Chaos Toolkit 은 운영자에게 해당 동작을 변경할 수 있는 기회를 제공하는 메커니즘(v1.5.0이후)을 제공한다. 

### 항상 롤백 실행하기

- 롤백이 항상 적용되는지 확인하기

```go
$ chaos run --rollback-strategy=always experiment.json
```

### 편차에 대해서만 롤백 실행

- 실험에 편차가 발생한경우 롤백 수행하기 

```go
$ chaos run --rollback-strategy=deviated experiment.json
```

### 롤백 수행하지 않기

- 예를 들어 실험이 변경된 것을 실행 취소하지 않고 성공적인 실험 후에 시스템을 탐색하려는 경우와 같이 롤백을 실행하지 않을 수 있다. 

```go
$ chaos run --rollback-strategy=never experiment.json
```

## 런타임 시 설정 및 시크릿 재 정의

- 설정 및 시크릿은 실험 자체에서 선언되지만 런타임에 값을 재정의 해야 하는 경우가 있다. 
- 이는 --var KEY[:TYPE] 혹은 --var-file filepath.json|yaml|.env 플래그를 통해서 수행할 수 있다. 

- --var KEY[:TYPE]=VALUE 는 명령줄에서 시크릿 유출을 방지하기 위해 구성 값만 재 정의할 수 있다. 
- KEY는 실험에 사용된 최종 키이다. 예를 들면 다음과 같다. 

```go
{
    "configuration": {
        "message": "hello world"
    }
}

```

혹은 

```go
{
    "configuration": {
        "message": {
            "type": "env",
            "key": "MY_MESSAGE"
        }
    }
}
```

- 위 두 케이스에서 재정의된 키는 message 이다. 

- TYPE를 지정하는 경우 str, int, float, str 이 기본값인 바이트 중 하나여야 하므로 필요하지 않다. 
- Chaos Toolkit 은 주어진 VALUE를 지정된 유형으로 변환하려고 시도하고 변환할 수 없으면 실패한다. 

- --var-file filepath.json|yaml|env는 설정 및 시크릿 블록을 재 정의할 수 있는 기회를 제공한다. 
- json 및 yaml 파일의 형식은 다음과 같다. 

```go
{
    "configuration": {
        "KEY": VALUE
    },
    "secrets": {
        "scope": {
            "KEY": VALUE
        }
    }
}
```

```go
---
configuration:
  KEY: VALUE
secrets:
  scope:
    KEY: VALUE
```

- secrets 블록은 실험과 동일한 형식을 따르므로 범위는 실험에 제공된 범위이다.
- 예를 들어

```go
{
    "configuration": {
        "service_name": "ec2"
    },
    "secrets": {
        "aws": {
            "api_token": "1234",
            "something": "whatever"
        }
    }
}
```

- 다음 var 로 변경된다. 

```go
{
    "secrets": {
        "aws": {
            "api_token": "56787"
        }
    }
}
```

- 설정 부분을 재 정의하지 않고 시크릿 섹션의 일부만 재 정의한다. 
- 최종적으로 변수를 .env 파일에 보관하면 설정을 재정의할 수만 있다. 

# The chaos report command

- chaos report 명령을 사용하여 chaos run 명령으로 생성된 저널을 가져오고 기정된 형식으로 보고서를 생성한다. 
- chaos report 명령이 의존하는 많은 운영 체제 종속 기능으로 인해 chaos report 명령은 Chaos Toolkit CLI 와 함께 설치되지 않는다. 
- chaos report 명령을 설치하려면 chaostoolkit-report 플러그인과 자신의 운영 체제에 적합한 종속성을 설치해야 한다. 
- https://github.com/chaostoolkit/chaostoolkit-reporting

- 설치하고 나면 다음과 같이 가능한 옵션을 볼 수 있다. 

```go
$ chaos report --help

Usage: chaos report [OPTIONS] [JOURNAL]... REPORT

  Generate a report from the run journal(s).

Options:
  --export-format TEXT  Format to export the report to: html, markdown, pdf.
  --help                Show this message and exit.

```

- chaos report 명령을 사용하는 방법에 대한 자습서는 Chaos Toolkit 의 시작하기 자습서의 일부로 제공된다. 
- https://www.katacoda.com/chaostoolkit/courses/01-chaostoolkit-getting-started

## 리포트 생성하기

- chaos run 명령어를 사용한 후 실험이 완료되면 저널이 생성되어 chaos-report.json 파일에 저장된다. 
- PDF 혹은 HTML 리포트를 chaostoolkit-reporting 라이브러리를 사용하여 생성할 수 있다.

- chaos report 커맨드는 chaos-report.json 파일에 대한 경로와 필요한 실제 보고서 파일에 대한 경로를 예상한다. 
- --export-format 옵션을 사용하여 원하는 것을 지정하여 다양한 형식의 보고설르 내보낼 수 있다. 
- 
- 예를 들어 PDF 리포트를 다음 명령어를 사용하여 생성할 수 있다.

```go
$ chaos report --export-format=pdf chaos-report.json report.pdf
```

- HTML 리포트는 다음과 같이 생성할 수 있다. 

```go
$ chaos report --export-format=html5 chaos-report.json report.html
```

# 실험 스케줄링 하기

- 스케줄링은 Chaos Toolkit 자체에 내장되어 있지 않다. 
- 그러나 키보드를 사용하지 않을때 주기적으로 실험을 실행하고 싶은 경우가 많다. 

- 이러한 경우 cron과 같은 시스템을 사용하여 실험 실행을 예약하는 것이 좋다. 
- 또한 Kubernetes 작업을 사용하여 공통 Kubernetes 기능을 사용하여 해당 작업의 수명 주기를 완전히 제어할 수 있다. 

