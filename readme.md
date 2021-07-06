dmoj_v2.1.1 스냅샷 사용법
=========================
>이 스냅샷은 2021-07-06 일자 기준 dmoj의 releases 버전으로 만들어졌습니다.

# 유의 사항
가상 환경 저장 공간에 dmoj 구축에 필요한 모든 python 패키지들이 설치되어야 정상이지만, 이 스냅샷은 일부 패키지가 global 환경에 설치되어있습니다. 따라서 아래 캡처 이미지와 같이 root 권한과 가상 환경이 켜진 상태에서 명령을 수행해야합니다.   

* ### root 권한 얻기
```sudo -s```
* ### 가상환경 활성화
``` 
. dmojsite/bin/activate
(dmojsite) root@jota:/home/jota#
```

# django 설정 파일 수정
```(dmojsite) root@jota:/home/jota/site/dmoj# vi local_settings.py```

ALLOWD_HOST에 본인이 할당받은 외부 IP 입력

``` ALLOWED_HOSTS = ['IP address 입력'] ```

# django 서버(site) start

* 서버를 동작하기 전에 필요한 명령 수행
```
(dmojsite) root@jota:/home/jota/site/dmoj# ./make_style.sh
(dmojsite) root@jota:/home/jota/site/dmoj# python3 manage.py collectstatic
(dmojsite) root@jota:/home/jota/site/dmoj# python3 manage.py compilemessages
(dmojsite) root@jota:/home/jota/site/dmoj# python3 manage.py compilejsi18n
```
* 서버 start
```
(dmojsite) root@jota:/home/jota/site/dmoj# python3 manage.py runserver 0.0.0.0:8000 
```

**[외부 ip 주소]:8000 url로 접근**   
![site](https://user-images.githubusercontent.com/42068110/124557855-f00eb380-de74-11eb-8b38-780df93f938b.PNG)

```
* 초기 계정   
id : admin   
passwd : Jotajota!
```
# site와 judge-server를 bridge로 연결

* ### bridged 작동   
```
(dmojsite) root@jota:/home/jota/site/dmoj# python3 manage.py runbridged
```
* ### judge-server start   
judge-server를 시작할 때는 root 권한을 해제하여 가상 환경만 활성화된 상태에서 진행해야합니다.
```
(dmojsite) ubuntu@jota:/home/jota/site/dmoj# dmoj -c judge.yml localhost
```

dmoj 사이트의 **관리자 페이지 - 제출 - 채점기** 에서 jota-judge가 아래와 같이 활성화 될 경우 정상
![judge](https://user-images.githubusercontent.com/42068110/124560838-36b1dd00-de78-11eb-8216-4b60c01f1708.PNG)


> **참고**    
> judge-server 경로 : /usr/lib/python3.8/site-packages/dmoj/

# code-server
```
vi ~/.config/code-server/config.yaml
```
code-server를 외부에서 접근하기 위해 bind-addr을 아래와 같이 수정
```
bind-addr: 0.0.0.0:8000
    .
    .
    .
```