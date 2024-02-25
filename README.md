#ubuntu-nginx
- issue 참고

- # ubuntu-nginx


<br/>1.각자 REPO 에 ubuntu-nginx 를 생성
<br/>2. hub 에서 ubuntu pull & run
<br/>3. (2) 에 들어가서 nginx.org 참조하여 nginx 설치
<br/>4. (1), (2), (3) 과정 기록
<br/>5. (4) 의 기록을 바탕으로 Dockerfile 생성

## ref
- https://github.com/beyond-sw-camp/be01-101/issues/37
<br/>
<br/>A. https://nginx.org/en/docs/
<br/>B. https://nginx.org/en/docs/beginners_guide.html
<br/>C. https://docs.docker.com/engine/reference/builder/#dockerfile-reference
<br/>D. https://github.com/nginxinc/docker-nginx/blob/1f227619c1f1baa0bed8bed844ea614437ff14fb/mainline/debian/Dockerfile#L119




### reset
```
$ sudo docker stop $(sudo docker ps -a -q)
$ sudo docker rmi $(sudo docker images -q)
```

### install 
```
$  sudo docker pull ubuntu:22.04
$  sudo docker run -it --name -d ubuntu-nginx ubuntu:22.04
```

### run 및 bash 실행
```
run 을 안했을 경우 
$ sudo docker start ubuntu-nginx4

$ sudo docker exec -it ubuntu-nginx bash
```
### bash 환경 install nginx
```
$ apt update
$ apt install nginx
```

### Dockerfile 생성
```
FROM ubuntu:22.04
RUN apt update -y
RUN apt install -y nginx
```
- error 발생함 
- 

### Dockerfile 추가 수정
```
# 베이스 이미지로부터 시작
FROM ubuntu:22.04

# 패키지 목록 업데이트 및 Nginx 설치
RUN apt update -y && \
    apt install -y nginx

# 컨테이너가 시작될 때 Nginx가 자동으로 실행되도록 설정
CMD ["nginx", "-g", "daemon off;"] //추가
```



### image build
``` 
$ sudo docker build -t ubuntu-nginx:0.1.0 .


 sudo docker build -t ubuntu-nginx:0.1.0 .
[+] Building 7.9s (8/8) FINISHED                                                                                                   docker:default
 => [internal] load build definition from Dockerfile                                                                                         0.0s
 => => transferring dockerfile: 92B                                                                                                          0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                                                              3.1s
 => [auth] library/nginx:pull token for registry-1.docker.io                                                                                 0.0s
 => [internal] load .dockerignore                                                                                                            0.0s
 => => transferring context: 2B                                                                                                              0.0s
 => [internal] load build context                                                                                                            0.0s
 => => transferring context: 238B                                                                                                            0.0s
 => [1/2] FROM docker.io/library/nginx:latest@sha256:74c1bb767ec295c0fcab2f4057f5f402953979c9385b86951673d335aa537536                        4.6s
 => => resolve docker.io/library/nginx:latest@sha256:74c1bb767ec295c0fcab2f4057f5f402953979c9385b86951673d335aa537536                        0.0s
 => => sha256:74c1bb767ec295c0fcab2f4057f5f402953979c9385b86951673d335aa537536 9.85kB / 9.85kB                                               0.0s
 => => sha256:05aa73005987caaed48ea8213696b0df761ccd600d2c53fc0a1a97a180301d71 2.29kB / 2.29kB                                               0.0s
 => => sha256:c3ea3344e711fd7111dee02f17deebceb725ed1d0ee998f7fb472114dc1399ce 629B / 629B                                                   0.5s
 => => sha256:e4720093a3c1381245b53a5a51b417963b3c4472d3f47fc301930a4f3b17666a 7.04kB / 7.04kB                                               0.0s
 => => sha256:88f6f236f401ac07aa5309d8ade2b0c9d24b9f526bd4e73311bf5c1787cfd49c 41.39MB / 41.39MB                                             3.4s
 => => sha256:cc1bb4345a3a849289cfb3e2471c096f374423ec1ef74766137b9de546498612 957B / 957B                                                   0.3s
 => => sha256:da8fa4352481b358fc60d40ee20d92da64124d2cf405115640d45980339f47e5 394B / 394B                                                   1.0s
 => => sha256:c7f80e9cdab20387cd09e3c47121ef0eb531043cf0aca1a52aab659de3ccb704 1.21kB / 1.21kB                                               1.0s
 => => sha256:18a869624cb60aaa916942dc71c22b194a078dcbbb9b8f54d40512eba55f70b8 1.40kB / 1.40kB                                               1.4s
 => => extracting sha256:88f6f236f401ac07aa5309d8ade2b0c9d24b9f526bd4e73311bf5c1787cfd49c                                                    0.7s
 => => extracting sha256:c3ea3344e711fd7111dee02f17deebceb725ed1d0ee998f7fb472114dc1399ce                                                    0.0s
 => => extracting sha256:cc1bb4345a3a849289cfb3e2471c096f374423ec1ef74766137b9de546498612                                                    0.0s
 => => extracting sha256:da8fa4352481b358fc60d40ee20d92da64124d2cf405115640d45980339f47e5                                                    0.0s
 => => extracting sha256:c7f80e9cdab20387cd09e3c47121ef0eb531043cf0aca1a52aab659de3ccb704                                                    0.0s
 => => extracting sha256:18a869624cb60aaa916942dc71c22b194a078dcbbb9b8f54d40512eba55f70b8                                                    0.0s
 => [2/2] COPY config/default.conf /etc/nginx/conf.d/                                                                                        0.2s
 => exporting to image                                                                                                                       0.0s
 => => exporting layers                                                                                                                      0.0s
 => => writing image sha256:d5f7584cbcc8955fd7583b89c18b61099ca5637af7e89061bb8d92c9e2bd231b                                                 0.0s
 => => naming to docker.io/library/ubuntu-nginx:0.1.0
```

### run 
```
$ sudo docker run -d --name ubuntu-nginx -p 9001:80 ubuntu-nginx:0.1.0
```

### 확인
```
$ sudo docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS         PORTS                                   NAMES
4a4cd60097fc   ubuntu-nginx:0.1.0   "nginx -g 'daemon of…"   10 seconds ago   Up 9 seconds   0.0.0.0:9001->80/tcp, :::9001->80/tcp   ubuntu-nginx

$ sudo docker images
REPOSITORY     TAG       IMAGE ID       CREATED          SIZE
ubuntu-nginx   0.1.0     b87fa83e4db6   6 minutes ago    182MB
```

### 화면
-  ubuntu-nginx bash 내에서 실행
```
root@4a4cd60097fc:/# service nginx status
 * nginx is running
 ```
 
- 
![image](https://github.com/raheego/ubuntu-nginx/assets/54056684/f7613edd-373d-4032-80d4-9acec40c4082)
