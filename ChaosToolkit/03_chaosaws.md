# Extension chaosaws

- 프로젝트는 actions과 prove의 집합이다. 이는 Chaos Toolkit 으로 확장된다. 

## Install

- 패키지는 Python 3.6 이상이 필요하다. 
- 실험에서 사용하려면 chaostoolkit 이 의미 있는 Python 환경에 패키지를 설치해야한다. 

```go
$ pip install -U chaostoolkit-aws
```

## 사용하기

- 이 패키지의 프로브와 작업을 사용하려면 실험 파일에 다음을 추가하라. 

```go
{
    "name": "stop-an-ec2-instance",
    "provider": {
        "type": "python",
        "module": "chaosaws.ec2.actions",
        "func": "stop_instance",
        "arguments": {
            "instance_id": "i-123456"
        }
    }
},
{
    "name": "create-a-new-policy",
    "provider": {
        "type": "python",
        "module": "chaosaws.iam.actions",
        "func": "create_policy",
        "arguments": {
            "name": "mypolicy",
            "path": "user/Jane",
            "policy": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Action": [
                            "s3:ListAllMyBuckets",
                            "s3:GetBucketLocation"
                        ],
                        "Resource": "arn:aws:s3:::*"
                    }
                ]
            }
        }
    }
}

```

- AZ에서 무작위로 하나를 선택한다. 

```go
{
    "name": "stop-an-ec2-instance-in-az-at-random",
    "provider": {
        "type": "python",
        "module": "chaosaws.ec2.actions",
        "func": "stop_instance",
        "arguments": {
            "az": "us-west-1"
        }
    }
}
```

- 이것으로 되었다. 
- 기존 prove및 작업을 보려면 코드를 탐색하라. 

## Configuration

### Credentials

- 이 확장은 내부적으로 boto3 라이브러리를 사용한다.
- 이 라이브러리는 AWS 서비스에 연결하고 인증하도록 환경을 적절하게 구성했다고 예상한다. 

#### '~/.aws/credentials 또는 ~/.aws/config 의 기본 프로필 사용'

- 이것은 가장 기본적인 경우이며, 기본 프로필이 ~/.aws/credentials(또는 ~/.aws/config) 에 올바르게 구성되어 있다고 가정하면 특정 자격 증명을 실험에 전달할 필요가 없다. 

#### '~/.aws/credentials 또는 ~/.aws/config 로 부터 기본이 아는 프로필 사용'

- ~/.aws/credentials(또는 ~/.aws/config) 파일에 프로필을 구성했다고 가정하고 실험에서 다음과 같이 선언할 수 있다. 

```go
{
    "configuration": {
        "aws_profile_name": "dev"
    }
}
```

- ~/.aws/credentials 는 다음과 같다. 

```go
[dev]
aws_access_key_id = XYZ
aws_secret_access_key = UIOPIY
```

- 혹은 ~/.aws/config 는 다음과 같을 것이다. 

```go
[profile dev]
output = json
aws_access_key_id = XYZ
aws_secret_access_key = UIOPIY
```

#### 기본이 아닌 프로필에서 ARN 역할 받음

- 실험 중에 수임하려는 특정 ARN 역할을 사용하여 ~/.aws/config 파일에 프로필을 구성했다고 가정한다. 

```go
{
    "configuration": {
        "aws_profile_name": "dev"
    }
}
```

- ~/.aws/config 는 다음과 같다. 

```go
[default]
output = json

[profile dev]
role_arn = arn:aws:iam::XXXXXXX:role/role-name
source_profile = default
```

#### Assume an ARN role from within the experiment¶

- 실험에서 역할 ARN 을 직접 선언하여 역할을 맡을 수 있다. 
- 이 경우 프로필도 설정하면 영향을 주지 않는다. 

```go
    "configuration": {
        "aws_assume_role_arn": "arn:aws:iam::XXXXXXX:role/role-name",
        "aws_assume_role_session_name": "my-chaos"
    }
```

- aws_assume_role_session_name 키는 선택이다. 제공되지 않으면 "ChaosToolkit"으로 설정된다. 
- 이 접근 방식을 사용하면 확장 프로그램이 AWS STS 서비스에 대해 역할 수입 호출을 수행하여 자격 증명을 동적으로 가져온다. 

#### Pass credentials explicitely

- 다음과 같이 자격 증명을 실험 정의에 시크릿으로 전달할 수 있다. 

```go
{
    "secrets": {
        "aws": {
            "aws_access_key_id": "your key",
            "aws_secret_access_key": "access key",
            "aws_session_token": "token",
        }
    }
}
```

- token은 선택적이다. 
- 그리고 다음과 같이 이용한다.

```go
{
    "name": "stop-an-ec2-instance",
    "provider": {
        "type": "python",
        "module": "chaosaws.ec2.actions",
        "func": "stop_instance",
        "secrets": ["aws"],
        "arguments": {
            "instance_id": "i-123456"
        }
    }
}
```

## region 설정하기

- 인증 자격 증명 외에도 사용하려는 지역을 구성해야한다. 
- 실험의 최상위 수준에서 선언하고 다음을 추가할 수 있다. 

```go
{
    "configuration": {
        "aws_region": "us-east-1"
    }
}
```

- 혹은

```go
{
    "configuration": {
        "aws_region": {
            "type": "env",
            "key": "AWS_REGION"
        }
    }
}
```

- 그러나 실험에서 아무 것도 선언하지 않고 터미널 세션에서 AWS_REGION 또는 AWS_DEFAULT_REGION을 간단히 설정할 수 있다.
- 이 중 아무것도 설정하지 않으면 실험이 실패할 수 있다. 

