
https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker
Kubernetes 의 default container runtime 이었던 docker 가 deprecated 될 예정이라는 post
docker 가 CRI 지원을 미뤄옴에 따라 Kubernetes 1.23 ver 부터는 docker 를 container runtime 으로 사용할 수 없을 예정이다.
이는 docker 자체의 지원을 중단했다기 보다 kubelet 에서 docker-shim 의 지원이 deprecated 되었다고 보면 된다.
---
### CRI
Container Runtime Interface
CRI 가 생기기 이전에는 docker 또는 rkt 가 kubelet 에 직접 통합되어 왔음.
docker, rkt 이외에도 다양한 container runtime tool 이 생겨남에 따라 kubelet 이 이들을 모두 지원하는데 어려움이 생김.
따라서 container runtime tool 지원을 용이하게 하기 위해 여러 기업이 모여 CRI 를 만듬
이후 CRI spec 에 맞춰 컴포넌트를 구현하면 kubelet 과 쉽게 호환될 수 있게 됨.
---
![kubelet_structure.png](../assets/kubelet_structure_1669793636073_0.png){:height 486, :width 442}
kubelet 과 docker 의 구조
---
### docker-shim
kubelet 과 docker 간 통신을 위해 구현된 프로젝트
CRI 를 준수하지 않은 docker 는 kubelet 과 직접 통신을 못하는데 , 이 때문에 CRI 를 지원하는 docker-shim 을 만들어 사용
---
### dockerd
docker daemon
docker image, docker ps 등의 명령어는 docker cli 임
우리는 docker cli 를 통해 명령어를 입력하고 이게 dockerd 로 전달됨.
dockerd 는 daemon process 로 실행되고 있고 docker cli 에서 RESTful API 형식의 요청을 수신하여 처리.
unix, tcp, fd 소켓 유형을 통해 api 요청을 수신할 수 있음.
dockerd 는 container build, security, volume, networking, secrets, orchestration, distributed state 와 같은 docker 기능을 담당.
default 경로로 /usr/bin/dockerd 에 위치하고 docker engine 이라고 부르기도 함.
containerd 에 의존적이기 때문에 단독으로 실행이 불가하다.
---
### containerd
container runtime tool
docker 는 1.11 ver 이후 부터 containerd 를 container runtime tool 로 사용하고 있음.
networking, volume 등의 기능은 dockerd 가 담당하고 container 에 대한 기능을 담당하는게 containerd
dockerd 와 마찬가지로 daemon process 로 실행되고 ctrcli 라는 cli client 가 존재.
docker 를 통해 containerd 가 실행되면 "moby" namespace 에 container 를 실행함
conatiner 생성
	containerd-shim, runC 를 이용하여 container 생성
image pull
	container 구동에 필요한 image 가 존재하지 않으면 OCI spec 기반으로 register server 에서 image pull
container lifecyle 관리
	container stop, start 등 status 관리
containerd 는 high-level container runtime
---
### containerd-shim
container 를 실제로 생성하는건 runC
runC 는 container 를 생성한 뒤 바로 exit
runC 가 종료된 후에도 container 를 관리할 수 있도록 해주는게 containerd-shim
containerd 는 containerd-shim 을 통해 runC 를 호출하고 runC 는 container 를 생성하고 종료
containerd-shim 을 supreaper 로 만들어 생성된 container 의 stdin/out/err 및 init process 의 exit code 를 담당하는 process 가 되도록 함
즉 container 와 containerd 의 통신은 containerd-shim 이 담당
---
### runC
OCI runtime spec 을 준수하는 low level container runtime
원래 libcontainer 였는데 docker project 에서 OCI 에 기부되고 이후 runC 가 됨.
docker 는 1.8 ver 이전에는 LXC 드라이버를 기준으로 동작했는데 이후 자체 개발한 libcontainer Go libarary 를 사용하면서 host kernel 의 namespace, cgroups 에 의존되지 않고 os 에서 독립되어 동작할 수 있게 됨.
libcontainer 에서 cgroups 관리 모듈로는 cgroupfs 또는 systemd 둘 중 하나를 사용할 수 있지만 점점 systemd 를 사용하는 쪽으로 upgrade 되는 중
---
### docker-shim 의 문제점
docker 는 container 생성에 직접 관여 하지 않고 containerd 에 의존적임
	즉 containerd 상위의 dockerd 가 kubelet 과 통신하지 않아도 containerd 에서 바로 kubelet 통신이 가능
또한 docker 는 19년 3월 이후 update 가 없음
	k8s 에서 CRI 스펙을 준수하라고 1년의 시간을 주었으나 docker 에서 update 하지 않았음
docker 의 모든 명령어는 root 권한이 필요함
	이는 보안문제를 야기할 수 있음. (cir-o 같은 tool 들은 root 권한 없이 실행 가능)
docker deamon 이 너무 무거움
	image build, 관리, 공유, 실행 등 많은 기능을 가지고 있고 deamon 하위에 모든 container 를 자식 process 로 소유함.
	이는 docker daemon 에 문제가 생기면 하위 container 모두에 영향을 끼치는 단일 실패점 위험이 있음 (single-point of failure)
	docker deamon 에 문제가 생기면 kubuernetes 전반에도 영향을 미친다는 이야기
이러한 점을 말미암아 kubernetes 는 docker-shim 을 deprecated 하기로 결정
1.23 ver 부터 docker-shim 을 container runtime tool 로 사용할 수 없게 되었음.
따라서 kubelet 에서 docker 를 사용하려고 하는 경우 docker-shim 을 제거하고 containerd 와 바로 통신해야함.
---
### docker 내부에 있는 containerd 를 그대로 사용할 수 있을까?
kubelet 의 container runtime detect 로직은 시스템에 어떤 .sock 이 있는지 판별해서 container runtime tool 을 선정함
1.23 ver 부터 docker-shim 을 지원하지 않으므로 자동으로 containerd 를 detecting 할 것
다만 관리자가 수동으로 containerd 의 namespace 에 특정 작업을 진행해주는 등의 작업이 필요
docker 는 moby 를 namespace 로 물고 돌아감
그러나 kubelet 에서 containerd 로 바로 통신하는 경우 k8s 를 namespace 로 이용
이렇게 되면 서로 바라보는 namespace 가 달라 문제가 발생
k8s 에서 containerd 를 API 로 호출할 때 namespace 명시하기 때문에 kubelet -> containerd 설정 후 작업하면 k8s namespace 에 container 를 생성할 수 있음
	1. node 차단 및 drain
		kubectl cordon, kubectl drain
	2. kubelet stop
		3. docker 삭제 후 containerd 재설치 or k8s 에서 container runtime tool 을 containerd 로 변경
		4. kubelet start
		5. containerd 에서 k8s namespace 로 container 재 생성 했는지 확인
