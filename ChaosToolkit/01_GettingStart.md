# ChaosToolkit

from: [https://chaostoolkit.org/](https://chaostoolkit.org/)

## Feature

### Chaos As Code

- JSON/YAML 파일로 Chaos Engineering 실험을 선언하고, 저장한다.
- 이를 통해서 다른 코드들과 같이 협업하고 오케스트레이션 할 수 있다.

### Extensible

- Chaos Toolkit 은 Open API를 통해서 다른 시스템에 대해서 확장할 수 있다.

### Automation

- Chaos Toolkit 은 자동화를 좋아한다.
- 선호하는 CI/CD 체인에 추가할 수 있다.

### Open Source

- Chaos Toolkit은 오픈소스로 Apache 2 라이선스이다.
- 커뮤니티에 의해 락인 되지 않는다.

## 설치

- Python 3.7 이상 버젼이 필요

### Python 설치하기

```jsx
$ brew install python3
```

```jsx
$ sudo apt-get install python3 python3-venv
```

### 가상환경 만들기

- 패키지 관리를 통해서 의존성을 설치할 수 있다. 그러나 local virtual 환경에 제한해서 설치하기 위해서 가상환경을 만들자.

```jsx
$ python3 -m venv ~/.venvs/chaostk
$ source  ~/.venvs/chaostk/bin/activate
```

## CLI 설치하기

- chaostoolkit 을 설치하기 위해서 다음과 같이 실행하자.

```jsx
$ pip install -U chaostoolkit
```

- 검증을 위해서 다음과 같이 수행한다.

```jsx
$ chaos --version

chaos, version 1.13.0
```

## Extension 설치하기

- chaos 커맨드를 일단 설치하고 나서, 완젼한 ChaosToolkit을 이용하기 위해서는 Extension을 설치하자.
- https://github.com/search?utf8=%E2%9C%93&q=topic%3Achaostoolkit-extension&type=Repositories

## 업그레이드하기

```js
pip install -U chaostoolkit
```

