# 컨테이너 런타임 개념과 종류

## **Container Runtime**

런타임은 흔히 어떤 프로그램을 실행시키기 위한 환경이나 소프트웨어라고 볼 수 있다. 예를 들어 자바스크립트 언어로 된 애플리케이션을 실행시키기 위한 NodeJS, Java 애플리케이션을 실행하기 위한 JRE(Java Runtime Environment)가 있다.

컨테이너도 마찬가지로 실행시키는 주체가 필요한데, 이때 사용되는 것이 컨테이너 런타임이다. 2007년 리눅스 커널에 cgroup이 추가된 이후로 컨테이너화에 대한 기술이 많이 언급이 되었지만, 추구하고자 하는 목표와 명세가 너무 달라 이에 대한 표준을 잡기 시작했다. 이러한 표준으로 나온 것이 OCI와 CRI이다.

## **Open Container Initiative (OCI)**

OCI 공식 [홈페이지](https://opencontainers.org)에 가면 아래와 같이 정의하고 있다.

> The **Open Container Initiative** is an open governance structure for the express purpose of creating open industry standards around container formats and runtimes.

OCI는 일종의 프로젝트 혹은 재단이라고 볼 수 있는데, 컨테이너 기술에 대한 표준을 만드려고 노력하며 리눅스 재단으로부터 후원을 받는다고 한다.

OCI는 두가지 스펙을 제시하고 있는데, 하나는 런타임 명세이고 다른 하나는 이미지 명세이다. OCI 스펙을 구현한 컨테이너 런타임 종류는 크게 두가지로 볼 수 있는데, 단순히 컨테이너를 실행시키기 위한 네이티브 런타임과 특별한 기능을 포함하고 있는 샌드박스형 런타임이 있다.

- Native Runtimes
  - runC
  - Railcar
  - Crun
  - rkt
- Sandboxed and Virtualized Runtimes
  - gVisor
  - nabla-containers
  - runV
  - clearcontainers
  - kata-containers

## **Container Runtime Interface (CRI)**

- containerd
- cri-o

------

참조

- https://opencontainers.org
- https://www.capitalone.com/tech/cloud/container-runtime/