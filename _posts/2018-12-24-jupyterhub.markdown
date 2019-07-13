---
layout: post
title:  "다중 사용자를 위한 Jupyterhub 설치Install jupyterhub for multi-user"
date:   2018-12-24
tags: [jupyterhub, multi-user, ubuntu]
---

![]({{ "https://jupyter.org/assets/hublogo.svg" | absolute_url }})


## Jupyterhub 설치하기

멀티유저 환경에서 구동되는 jupyter notebook 서버를 구성하기 위해서 jupyterhub를 설치해야 하는데 사용자 계정으로 로그인해야 하는 등 좀 까다로운 문제가 몇가지 있다. 더불어 conda의 가상환경과 맞물려서 상황이 더 복잡해지는데 다음과 같은 구성으로 설치하는 법을 정리했다.

본 따라하기 글에서는 다음과 같이 구성한다.
- jupyterhub는 전역환경에 설치한다.
- 개인의 개발환경은 conda환경에 설치한 다음 ipython 커널로 등록해서 쓴다.

아래 설치 과정은 metamath라는 머신에 metamath라는 유저가 설치하는 것으로 가정하고 진행한다.

### 1. 전역환경에 jupyterhub 설치하기
#### 1.1 우선 nodejs 관련 패키지를 설치한다.

```shell-script
metamath@metamath:~$ sudo apt install nodejs
metamath@metamath:~$ sudo apt install npm
metamath@metamath:~$ sudo npm install -g configurable-http-proxy
```

#### 1.2 전역환경에 jupyterhub와 인증관련 모듈을 설치한다.

```shell-script
metamath@metamath:~$ sudo apt install python3-pip

# sudospawner를 설치하면 의존성 모듈로 jupyterhub, notebook 까지 다 설치됨
metamath@metamath:~$ sudo pip3 install sudospawner 

# jupyterlab을 쓸 계획이면 설치한다.
metamath@metamath:~$ sudo pip3 install jupyterlab
```

### 2. jupyterhub를 실행할 전용 사용자 추가

```shell-script
metamath@metamath:~$ sudo useradd rhea
```

### 3. /etc/sudoers 편집

```shell-script
metamath@metamath:~$ sudo visudo -f /etc/sudoers 
```

로 파일을 열어서 다음줄 추가

```shell-script
# 전역환경에 설치한 sudospawner 실행 파일을 JUPYTER_CMD로 설정
Cmnd_Alias JUPYTER_CMD = /usr/local/bin/sudospawner
# rhea 사용자가 jupyterhub 그룹에 있는 사용자에 대해서 암호없이 JUPYTER_CMD를 실행
rhea ALL=(%jupyterhub) NOPASSWD:JUPYTER_CMD
```

### 4. jupyterhub 그룹

#### 4.1 그룹추가

```shell-script
metamath@metamath:~$ sudo groupadd jupyterhub
```

#### 4.2 jupyterhub 그룹에 사용자 할당

```shell-script
metamath@metamath:~$ sudo usermod -a -G jupyterhub rhea
metamath@metamath:~$ sudo usermod -a -G jupyterhub metamath
# metamath 이외에 추가 사용자를 한명 설정한다. 그래서 멀티 유저 환경을 테스트 해본다.
metamath@metamath:~$ sudo usermod -a -G jupyterhub ywpython 
```

### 5. 테스트

#### 5.1 sudospawner 명령어

일반유저로 로그인한 상태에서 다음을 실행하면 처음 sudo를 위한 암호를 한번 물어보고 rhea가 $USER로 /usr/local/bin/sudospawner를 실행하는 것에 대해서는 암호를 물어보면 안된다.

```shell-script
metamath@metamath:~$ sudo -u rhea sudo -n -u $USER /usr/local/bin/sudospawner --help
```

잘 되었다면 다음처럼 help가 뿌려진다.

```shell-script
Options:

  --help                       	show this help information

/home/metamath/miniconda3/envs/nn/lib/python3.6/site-packages/tornado/log.py options:

  --log-file-max-size          	max size of log files before rollover
                               	(default 100000000)
  --log-file-num-backups       	number of log files to keep (default 10)
  --log-file-prefix=PATH       	Path prefix for log files. Note that if you
                               	are running multiple tornado processes,
                               	log_file_prefix must be different for each
                               	of them (e.g. include the port number)
  --log-rotate-interval        	The interval value of timed rotating
                               	(default 1)
```

#### 5.2 다른 명령어 

다음을 실행하면

```shell-script
metamath@metamath:~$ sudo -u rhea sudo -n -u $USER echo 'fail'
sudo: 암호가 필요합니다
```

라고 암호가 필요하다고 한다. 즉, rhea가 다른 유저($USER)로 암호없이 /usr/local/bin/sudospawner만 실행가능하다.

### 6. root가 아닌 사용자가 PAM 동적인증 가능하게 하기

#### 6.1 shadow 그룹

먼저 /etc/shadow파일을 찾아본다.

```shell-script
metamath@metamath:~$ ls -l /etc/shadow
-rw-r----- 1 root shadow 1475 12월 22 21:18 /etc/shadow
```

보통 이미 shadow 그룹이 있는데 없다면 다음 명령으로 추가하고 
```shell-script
metamath@metamath:~$ sudo groupadd shadow
```
/etc/shadow 의 그룹을 다음 명령으로 바꾸고 
```shell-script
metamath@metamath:~$ sudo chgrp shadow /etc/shadow
```
속성을 다음 명령으로 바꾼다. 
```shell-script
metamath@metamath:~$ sudo chmod g+r /etc/shadow
```

우분투 18.04에는 기본으로 되있어서 위 작업은 거의 할 필요 없다.

rhea를 shadow그룹에 추가
```shell-script
metamath@metamath:~$ sudo usermod -a -G shadow rhea
```

#### 6.2 테스트

전역환경에 pamela 설치
```shell-script
metamath@metamath:~$ pip3 install pamela
```

rhea라는 사용자가 현재 명령을 실행하고 있는 사용자 $USER로 동적으로 로그인하는 실험을 한다.
```shell-script
metamath@metamath:~$ sudo -u rhea python3 -c "import pamela, getpass; print(pamela.authenticate('$USER', getpass.getpass()))"
Password: $USER의 암호입력
None
```

위 테스트의 의미는 이렇다. 우선 metamath로 python3을 실행하고 동적으로 metamath와 ywpython에 대해서 로그인을 시도해보자.

```shell-script
metamath@metamath:~$ python3
Python 3.6.7 (default, Oct 22 2018, 11:32:17) 
[GCC 8.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import pamela
>>> pamela.authenticate('metamath', '******')
>>> pamela.authenticate('ywpython', '??????')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/local/lib/python3.6/dist-packages/pamela.py", line 285, in authenticate
    return pam_end(handle, retval)
  File "/usr/local/lib/python3.6/dist-packages/pamela.py", line 244, in pam_end
    raise PAMError(errno=retval)
pamela.PAMError: [PAM Error 7] Authentication failure
>>> 
```

ywpython에 대해서는 올바른 암호를 입력했음에도 불구하고 로그인이 되지 않는다. 이제 rhea 사용자로 python3을 실행하고 동일한 작업을 반복해본다. 

```shell-script
metamath@metamath:~$ sudo -u rhea python3
Python 3.6.7 (default, Oct 22 2018, 11:32:17) 
[GCC 8.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import pamela
>>> pamela.authenticate('metamath', ******)
>>> pamela.authenticate('ywpython', ??????')
>>> 
```

모든 사용자에 대해서 암호만 제대로 입력하면 로그인 작업이 성공하는 것을 볼 수 있다. 따라서 이제 rhea 계정으로 jupyterhub를 실행하면 각 사용자가 입력하는 아이디와 비번으로 사용자 인증을 할 수 있게 된 것이다.

### 7. jupyterhub 폴더 만들기

jupyterhub를 실행할 폴더를 만든다.

```shell-script
metamath@metamath:~$ sudo mkdir /etc/jupyterhub
metamath@metamath:~$ sudo chown rhea /etc/jupyterhub
```

### 8. jupyterhub 설정 파일 만들기
7에서 만든 /etc/jupyterhub 폴더에서

```shell-script
metamath@metamath:/etc/jupyterhub$ sudo -u rhea jupyterhub --generate-config
```

위와 같이 하면 설정파일 jupyterhub_config.py이 생긴다.

### 9. jupyterhub_config.py 설정

```python
c.JupyterHub.spawner_class = 'sudospawner.SudoSpawner'
# jupyterlab을 기본으로 띄우고 싶으면 jupyterlab을 설치하고 디폴트 경로를 /lab으로 한다.
c.Spawner.default_url = '/lab'
```

나머지도 개인 환경에 맞게 적당히 설정해준다.

### 10. 실행
/etc/jupyterhub 폴더에서

```shell-script
metamath@metamath:/etc/jupyterhub$ sudo -u rhea jupyterhub -f /etc/jupyterhub/jupyterhub_config.py
```

이제 localhost:8000으로 접속하면 로그인 창이 뜨고 일반 사용자로 로그인하면 된다.

### 11. 가상환경의 커널
지금 상태로는 전역환경의 커널을 사용하게된다. 만약 numpy가 필요하다면 전역환경에 numpy를 설치해야 사용자 노트북에서 numpy를 쓸 수 있다. 사용자 마다 필요한 패키지가 다 다르므로 사용자에 맞는 커널을 직접 등록해서 쓸 필요가 있다. 따라서 사용자의 가상환경의 커널을 따로 추가하는 방법을 알아본다.

예를 들어 metamath가 numpy, matplotlib, scipy등이 필요하다고 하면 이것들을 전역환경에 다 설치하지 않고 metamath의 가상환경 nn에서 패키지를 설치한다.

```shell-script
(nn) metamath@metamath:~$ conda install ipykernel, numpy, ...
```

다음 ipykernel에 metamath_nn이란 이름으로 커널을 등록한다. 커널 등록의 기본적인 사용법은 아래와 같다.
```shell-script
python -m ipykernel install [--user] [--name <machine-readable-name>] [--display-name <"User Friendly Name">]
```

실제로 등록한다. 

```shell-script
(nn) metamath@metamath:~$ python -m ipykernel install --user --name metamath_nn --display-name nn
```

--name은 jupyter가 사용하는 커널 이름을 지정한다. jupyter가 유일하게 알아볼 수 있도록 [사용자명]_[환경명]으로 한다.
--display-name은 노트북서버에 접속해서 웹브라우저에서 보여질 이름 지정한다.

그리고 다시 metamath로 jupyterhub에 접속하면 python3이외에 nn이란 커널이 보이고 이 커널로 노트북 파일을 만들면 가상환경에 설치된 패키지를 사용할 수 있다. 반면 ywpython으로 접속하면 기본 python3 커널만 사용 가능하다.


### 12. 참고
위에서 각 사용자가 jupyter에 커널을 등록하게 되는데 등록된 커널을 보고 그중 필요없는 것을 삭제하는 명령어들이다.

#### 12.1 등록된 jupyter 커널 보기

```shell-script
jupyter kernelspec list
```

#### 12.2 등록된 커널 삭제

```shell-script
jupyter kernelspec remove [커널이름]
```


### 13. service로 실행

이제 컴퓨터가 리부팅이 되면 자동으로 jupyterhub가 실행되도록 설정한다. 위 링크를 참고하여 아래처럼 하면 된다.
다음 내용을 /lib/systemd/system/jupyterhub.service 로 저장한다.

```shell-script
[Unit]
Description=Jupyterhub

[Service]
User=root
ExecStart=/usr/local/bin/jupyterhub -f /etc/jupyterhub/jupyterhub_config.py

[Install]
WantedBy=multi-user.target
```

저장하 데몬으로 등록하고 실행해본다.

```shell-script
(nn) metamath@metamath:/lib/systemd/system$ sudo systemctl daemon-reload
(nn) metamath@metamath:/lib/systemd/system$ systemctl start jupyterhub
```

실행 중 jupyterhub의 출력 로그를 보려면 아래와 같이 하면 된다.

```shell-script
journalctl -u jupyterub
```

이제 서비스로 등록되었고 자동 실행하려면 

```shell-script
(nn) metamath@metamath:/lib/systemd/system$ sudo systemctl enable jupyterhub
Created symlink /etc/systemd/system/multi-user.target.wants/jupyterhub.service /lib/systemd/system/jupyterhub.service.
```

이제 jupyterhub가 서비스로 등록되어 리부팅후 자동으로 실행되며 사용자는 localhost:8000으로 접속해서 바로 사용하면 된다. 

이상으로 jupyterhub 설치방법을 마친다.


### 참고문헌
1. [Using sudo to run JupyterHub without root privileges][Jupyterhub1]
2. [setting up jupyterhub with sudospawner and anaconda][Jupyterhub2]
3. [How to run jupyterhub from the start][Jupyterhub3]

[Jupyterhub1]: https://github.com/jupyterhub/jupyterhub/wiki/Using-sudo-to-run-JupyterHub-without-root-privileges
[Jupyterhub2]: https://medium.com/@ybarraud/setting-up-jupyterhub-with-sudospawner-and-anaconda-844628c0dbee
[Jupyterhub3]: https://github.com/jupyterhub/jupyterhub/issues/317
