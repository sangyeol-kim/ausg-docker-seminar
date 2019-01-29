## 1. [Docker](https://www.docker.com/)

> Docker는 리눅스 애플리케이션을 컨테이너로 묶어서 실행할 수 있는 오픈소스 컨테이너 프로젝트로써,  
이를 통해 개발과 테스트, 그리고 서비스 환경을 하나로 통합하여 효율적으로 관리할 수 있도록 도와주는 툴 입니다.
>
> Docker를 사용하면 이미지를 통해 개발환경의 제약 없이 자유로운 배포가 가능합니다.
>
> 그 외 에도 AWS와 같은 클라우드 환경을 사용할때도 이미지 단위의 배포가 가능하며,  
트래픽 문제가 발생했을 경우 Docker 컨테이너를 원하는만큼 생성해 처리량을 쉽게 늘릴 수 있습니다.

## 2. Docker 실습


1. hello, world 출력하기


    `$ docker run -it ubuntu:latest echo 'hello, world!'`


    해당 명령어를 실행하면 호스트 환경이 아닌 ubuntu 환경의 컨테이너에서 `hello, world`가 출력됩니다.  
    또한 지금 명령어를 실행한 터미널은 본인의 호스트 환경이지만 직접 ubuntu(CentOS...) Shell을 이용할 수도 있습니다.  


2. ubuntu Shell에서 hello, world 출력하기


    1. `$ docker run -it ubuntu:latest bash`  
        bash는 기본 커맨드이므로 생략 가능합니다.  
    2. `$ echo 'hello, world'`  
    이전의 `hello world`는 호스트 환경에서 실행한 ubuntu 명령어 였지만  
    지금은 직접 ubuntu shell로 들어와 `hello world`를 출력했습니다.
    3. `$ ls`  
    ubuntu 컨테이너 이기 때문에 맥이나 윈도우가 아닌 ubuntu 파일 시스템을 확인할 수 있습니다.
    4. `$ exit`  
    ubuntu 컨테이너를 종료하고, 호스트 환경으로 돌아갑니다.
## 3. Docker에 대한 이해 


1. Docker는 VM(Vitual Machine) 일까?
    > **No**  
    실제로 굉장히 비슷하지만 Docker와 VirtualMachine 과는 여러가지 다른 점이 존재합니다.  
    그 중 하나는 하드웨어 가상화 여부 입니다.
    VirtualBox나 VirtualMachine은 하드웨어 가상화가 이루어집니다.  
    하드웨어 가상화란 운영체제 위에 소프트웨어로 구성된 하드웨어가 하나 더 있는 것이라고 생각하셔도 좋습니다.


2. 컨테이너


    > 도커에서 사용하는 컨테이너는 하나의 프로세스 이며, 하드웨어 가상화가 아닌 격리된 환경에서 실행되는 프로세스 입니다.  
    > Docker는 Linux Container 기술이기 때문에 MacOS 또는 Windows에서 사용 할 경우
    각각의 가상화 환경(xhyve / hyper-V)에서 돌아갑니다.  
    > 그렇기 때문에 가상화 환경을 지원하지 않는 CPU로 작업한다면 제대로 동작하지 않습니다.

3. 이미지


    > 이미지는 특정 프로세스를 실행하기 위한 환경입니다. (파일들의 집합)  
    도커는 레이어 저장 방식을 통해 이미지 위의 이미지를 엎는 방식을 사용하고 있습니다.


## 4. 도커 명령어


1. 컨테이너 확인  
`$ docker ps`
ps 명령어를 통해 실행중인 컨테이너를 확인 할 수 있습니다.
    > docker -a <container_id>를 통해 정지된 컨테이너도 확인이 가능합니다.

2. 컨테이너 정지  
`$ docker stop <container_id>`

3. 컨테이너 삭제  
`$ docker rm <container_id>`
rm 명령어를 통해 종료된 컨테이너를 삭제할 수 있습니다.

4. 컨테이너 로그  
`$ docker logs <-f> <container_id>`
logs 명령어를 통해 컨테이너의 동작 상태를 확인할 수 있습니다.
    > -f 옵션을 통해 실시간으로 생성된 로그를 확인 가능

5. 이미지 목록  
`$ docker images`
호스트에 설치되어 있는 이미지를 확인할 수 있습니다.

6. 이미지 삭제  
`$ docker rmi <image_name>`

7. 이미지 다운로드  
`$ docker pull <image_name>:<tag>`
    > run 명령어를 실행하면 이미지가 없는 경우 자동으로 pull 합니다.


## 5. 이미지 생성


> 지금까지 사용해 본 이미지는 DockerHub에 올라와 있는 이미지 입니다.  
>
> 지금 실습부터는 직접 이미지를 만들어 보겠습니다.  (우분투 기본 이미지에 Node.js와 npm 설치)

1. ubuntu Container 실행  
`$ docker run -it ubuntu:latest bash`
    > -it 옵션은 bash를 실행한 후 명령어를 입력 할 수 있게 해줍니다.
    > docker는 기본적으로 'root'권한으로 실행됩니다.

2. Package Manager 업데이트  
`$ apt-get update`
    > 최신 버전이 아닐 시 제대로 설치가 되지 않을 수 있습니다.

3. Node.js 및 npm 설치  
`$ apt-get install nodejs`  
`$ apt-get install npm`  
    > **기본 ubuntu 이미지로 실행한 컨테이너에 Node.js와 npm이 설치된 상태**

4. 호스트 환경으로 돌아가기
`$ exit`

5. Commit Command를 이용한 이미지 생성  
`$ docker commit <container_id> <image_name>:<tag>`
    > 종료된 컨테이너의 container_id는 'docker ps -a'를 통해 확인할 수 있습니다.
    > 'image_name'과 'tag'는 임의로 설정할 수 있습니다.

6. 이미지 확인  
`$ docker images | grep <image_name>`

7. 생성한 이미지를 이용해 컨테이너 실행  
`$ docker run -it <image_name>:<tag> bash`

8. 정상적으로 빌드 되었는지 확인
`$ node -v` / `$ npm -v`


## 6. Dockerfile


> Dockerfile이란 이미지 생성 과정을 기술한 일종의 Docker 전용 DSL(Domain Specific Language)  
> DSL이란 특정 도메인(여기선 Docker)에 특성화 된 Little Language  
> ex) Markdown Language

Dockerfile을 통해 이전에 생성한 이미지를 똑같이 만들어보겠습니다.

1. Dockerfile 생성


    `$ touch Dockerfile`

2. Dockerfile 작성
> 에디터 또는 vim 등을 이용하여 생성한 Dockerfile에 아래 코드를 작성해주세요.
``` 
    FROM ubuntu:latest
    RUN apt-get update
    RUN apt-get install -y nodejs
    RUN apt-get install -y npm 
```
> **Dockerfile에서 apt-get을 사용할 때 반드시 -y 옵션을 사용해주세요**

3. `$ docker build -t <image_name>:<tag> .`
> 여기서 . 의 의미는 현재 디렉토리 아래에 있는 Dockerfile을 이용해 이미지를 만들겠다는 의미입니다.  
> -t 는 이미지의 이름을 지정해주는 명령어 입니다.
>
> 보통 commit이 아닌 Dockerfile을 이용해 이미지를 만들게 됩니다.
>
4. 명령어를 통해 이미지가 생성되었는지 확인해보겠습니다.  
`$ docker images | grep <image_name>:<tag>`  


## 7. Dockerfile 추가 명령어
> 지금까지 작성한 Dockerfile은 FROM과 RUN 명령어로만 구성되어 있습니다.
>
> 추가적으로 중요한 명령어 몇 가지를 알아보겠습니다.

1. FROM
> FROM 명령어는 `FROM <image_name>:<tag>` 형식으로 지정할 수 있습니다.  
<image_name> 에는 base image가 지정됩니다. 
ex) ubuntu:16.04

2. ADD
> ADD 명령어는 `ADD <file_name> <file_path>`로 구성되며, 보통 Dockerfile은 애플리케이션과
같은 디렉토리에 넣게 되는데 그 이유는 디렉토리 안의 파일을 원하는 대로 추가할 수 있기 때문입니다.

3. RUN
> RUN 명령어는 `RUN <Command>`로 작성할 수 있으며, 컨테이너를 실행한 후 실행하는 명령어를 작성합니다.  
> RUN 명령어는 보통 bash에서 사용하지 않고, Dockerfile을 이용하여 실행합니다.  
**`$ apt-get install nodejs` 와 같이 중간에 응답해야 하는 부분이 있다면 꼭 -y 옵션을 추가해야 합니다.**

4. WORKDIR
> WORKDIR 명령어는 작업 디렉토리를 변경하는 명령어입니다.

5. EXPOSE
> EXPOSE 명령어는 컨테이너로 실행할 때 노출시킬 포트를 지정하는 명령어입니다.

6. CMD
> CMD 명령어는 도커를 실행했을 때 기본적으로 실행될 명령어를 지정합니다.
> 예를 들어 이전에 실행했던 `$docker run -it ubuntu:latest bash` 에서 bash는 기본 명령어로 지정되어 있기 때문에 생략해도 실행이 됩니다.

## 8. Dockerfile로 간단한 애플리케이션 실행하기
> 해당 실습에서는 Github가 이용됩니다.  
> 해당 실습은 Node.js 공식문서를 이용합니다.
> https://nodejs.org/ko/docs/guides/nodejs-docker-webapp/

**이제 본격적으로 Dockerfile을 이용해서 node-app이라는 이름을 가진 Custom Image를 만들어 보겠습니다.**

1. https://github.com/sangyeol-kim/node-app 에 접속해 해당 프로젝트를 clone 해옵니다.
> 해당 프로젝트는 Node.js로 작성된 hello, world!를 출력하는 간단한 웹 애플리케이션입니다.

2. 해당 폴더로 접근해 `$ docker build -t <dockerhub_id>/node-app:latest .` 을 입력합니다.
> 다음 실습에서 해당 이미지를 DockerHub에 업로드 할 예정입니다.  
> DockerHub에 이미지를 올리기 위해서는 이미지 이름을 <dockerhub_id>/<image_name>로 생성해야 합니다.

3. `$ docker run -p 3000:3000 <-d> <dockerhub_id>/node-app:latest`
> -d 옵션을 주면 백그라운드에서 컨테이너가 실행됩니다.  
> -p 옵션을 통해 컨테이너 내부와 호스트의 포트를 연결합니다.

4. `$ docker logs <container_id>` 를 통해 컨테이너가 정상적으로 실행됐는지 확인하고,
직접 localhost:3000에 접속해 확인해봅니다.
> <container_id>는 `$ docker ps <-a>`로 확인 가능합니다.
> Hello, world!가 정상적으로 출력된다면 실습을 성공적으로 마치셨습니다 :)

5. 컨테이너 내부에 접근하는 방법
`$ docker exec -it <container_id> bash`

## 9. DockerHub에 이미지 올리기
> 해당 실습에서는 DockerHub 계정이 필요합니다.

1. DockerHub에 로그인 합니다.  
`$ docker login`

2. DockerHub에 방금 생성한 이미지를 Push 합니다.  
`$ docker push <dockerhub_id>/node-app:latest`

3. [DockerHub](https://hub.docker.com/)로 이동합니다.  
> 아래와 같이 Repository가 생성되었으면 성공입니다!  
> DockerHub는 Repository를 별도로 생성하지 않더라도 이미지명에 따라 자동으로 생성합니다.  
> 같은 이름의 이미지라면 tag에 따라서 버전별로 저장됩니다.  

![dockerhub](./images/dockerhub.png)  


**[2. Jenkins](/2.Jenkins/README.md)로 이동해주세요!**


---
