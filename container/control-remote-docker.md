# 원격으로 도커 엔진 제어하기

가장 친숙한 구조인 클라이언트-서버 구조를 도커에서도 확인할 수 있다. 별다른 설정을 하지 않으면 로컬 호스트의 도커 데몬에 요청을 보내기 때문에, 다른 호스트에 있는 도커에 요청(ex. `docker images`)을 보내기 위해서는 추가적인 작업을 해야한다.

## 도커 데몬이 있는 서버에서

> macOS나 Windows가 아닌 Ubuntu를 기준으로 설명

### 도커 데몬 설정 변경

`/lib/systemd/system/docker.service` 파일을 변경한다.

변경 전

```bash
[Service]
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

변경 후

```bash
[Service]
ExecStart=/usr/bin/dockerd --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
```
`-H` 옵션에 대한 설명은 아래와 같다.

```bash
$ man dockerd
...
-H, --host=[unix:///var/run/docker.sock]: tcp://[host:port] to bind or unix://[/path/to/socket] to use.
         The socket(s) to bind to in daemon mode specified using one or more
         tcp://host:port, unix:///path/to/socket, fd://* or fd://socketfd.
```

### 서비스 재시작

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker.service
```

## 도커 클라이언트에서

도커 데몬에 요청을 보내는 클라이언트에서는 `DOCKER_HOST` 값이 없을 경우에 tcp 기반으로 요청을 보내지 않고, 유닉스 소켓을 통해 로컬 호스트의 도커 데몬과 통신한다. 그렇기 때문에 `DOCKER_HOST` 값을 지정해주어야 하는데, `tcp://{ip or host}:2375` 형태가 된다(도커 데몬의 포트 번호 사용).

### 도커 CLI 사용

```bash
$ DOCKER_HOST="tcp://{ip or host}:2375" docker ps # 로컬 호스트의 도커가 아니라 원격에 있는 도커에 요청
```

혹은 `docker context`를 사용할 수 있다.

```bash
$ docker context
Manage contexts

Usage:
  docker context [command]

Available Commands:
  create      Create new context
  export      Export a context to a tar or kubeconfig file
  import      Import a context from a tar or zip file
  inspect     Display detailed information on one or more contexts
  list        List available contexts
  rm          Remove one or more contexts
  show        Print the current context
  update      Update a context
  use         Set the default context

Flags:
  -h, --help   Help for context

Use "docker context [command] --help" for more information about a command.
```



### Docker SDK for Python 사용

```python
# !pip install docker

import docker

client = docker.DockerClient(base_url="tcp://{ip or host}:2375")
images = client.images.list()
print(images)
```

혹은

```python
import docker

client = docker.from_env()
images = client.images.list()
print(images)
```

```bash
$ DOCKER_HOST="tcp://{ip or host}:2375" python3 main.py
# or
$ export DOCKER_HOST="tcp://{ip or host}:2375"
$ python3 main.py
```

이외에도 Golang, JavaScript 등 다양한 언어의 SDK로도 사용할 수 있다.

---

Reference

- https://stackoverflow.com/questions/56130644/connecting-to-a-remote-docker-daemon

