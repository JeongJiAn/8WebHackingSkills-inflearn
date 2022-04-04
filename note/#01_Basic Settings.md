# 01_Basic Settings

웹 해킹 기술을 배우기 전 웹 해킹을 실습할 수 있는 환경을 구성하는 방법
<br>

## **Index**

* VirtualBox
* Kali Linux
* XAMPP
* DVWA

<br>

## **VirtualBox**

<br>VirtualBox는 가상머신 소프트웨어로 실제 물리PC와 OS의 자원 일부를 나눠 사용하는 가상화 솔루션이다.<br>
VMWare가 아닌 VirtualBox를 사용하는 이유는 후자의 경우 무료이기 때문이다.<br><br>

https://www.virtualbox.org/<br>
위 링크를 통해 VirtualBox 홈페이지에 접속한 후 ``` Download VirtualBox``` 버튼을 클릭한다.<br>
이후, 자신의 OS에 맞는 VirtualBox를 다운로드 한다.<br><br>

다운로드가 끝난 후 같은 페이지에 있는 ```VirtualBox Extension Pack``` 를 다운로드 한다.<br>
<img src="https://user-images.githubusercontent.com/83760210/161372540-2fd8d13d-68aa-4105-87b5-812b63b3414a.png"><br>
Extension Pack은 USB장치 인식 등 부가 기능을 제공한다.<br><br>

다운로드 한 두 파일을 같은 경로에 둔 뒤 VirtualBox 파일을 실행한다.<br>
기본 설정값으로 설치를 진행한다.<br>
VirtualBox를 다운로드 하는 동안 네트워크 연결이 잠시 끊길 수 있다.<br><br>

다음으로 Extension Pack 파일을 실행한다.<br>
VirtualBox가 실행되며 Extension Pack 설치가 진행되는데, 역시 기본 설정값으로 설치한다.<br><br>

## **Kali Linux**

<br>Kali Linux는 debian을 기반으로 만든 OS로 해킹과 관련된 도구와 설명서를 포함하는 OS이다.<br>
이러한 이유로 모의 해킹을 시도하는 리눅스로 많이 사용된다.<br><br>

물리PC의 OS를 변경하는 것이 아닌 VirtualBox 가상머신에 Kali Linux를 설치할 것이다.<br>
Kali Linux의 iso파일은 공식 홈페이지에서 무료로 배포하고 있으나, 강좌에서는 원활한 진행을 위해 Kali Linux 설치 파일을 따로 제공하고 있다.<br>
http://www.secuacademy.com/files/<br>
위의 링크를 통해 Kali Linux파일을 다운로드 한다.<br><br>

다운로드 후 VirtualBox를 실행시켜 메뉴 툴바에서 ```파일->가상시스템 가져오기```를 통해 ova 파일을 실행한다. (파일 탐색기에서 더블 클릭으로 실행하여도 문제 없다.)<br>
이 역시 기본 설정값으로 설치한다.<br><br>

성공적으로 설치가 끝나면 VirtualBox 왼쪽에 Kali Linux가 표시 될 것이다.<br>
Kali Linux에 우클릭을 하고 설정에 들어간다.<br>

### Kali Linux 설정
1. 시스템에서 기본 메모리를 3~4기가로 변경 (본인 PC 사양에 따라)
2. 네트워크에서 어댑터 2를 사용하기로 변경한 후 호스트 전용 어댑터에 연결한다.
3. 디스플레이에서 그래픽 컨트롤러를 VBox로 변경한 후 3차원 가속 선택을 해제한다.(additional)

<br>Kali Linux 아이콘을 더블클릭 하거나 우측 상단의 시작 버튼을 누르면 Kali Linux를 실행 가능하다.<br>
부팅 후 Login을 요구하는데, ```Username : root, Password : toor``` 을 입력한다.<br>
(이는 2020.1 버전부터 ```kali, kali```로 변경되었다.)<br><br>

이제 Kali Linux에 실습 환경을 구축해보자.<br><br>

### 네트워크 설정
로그인에 성공하면 왼쪽 메뉴 바에서 Terminal을 실행하여 아래 명령어를 실행한다.<br>
```
ip addr
```
어댑터 2를 호스트 전용 어댑터로 연결했기에 eth0과 eth1 두 개의 네트워크가 출력되어야 한다.<br>
eth0과 eth1 중 어느 하나의 ip주소는 이미 할당 되어 있을 것이다. (필자의 경우 eth1이 할당되어 있었다.)<br>
실습을 위해서는 두 이더넷 모두 ip주소가 할당 되어야 한다.<br><br>

왼쪽 메뉴바에서 Leafpad를 실행하고 ```file->open```으로 파일을 하나 불러온다.<br>
파일의 경로는 ```File System/etc/network/interfaces```이다.<br><br>

파일이 열리면 파일 가장 아래에
```
auto eth0
iface eth0 inet dhcp

auto eht1
iface eth1 inet dhcp
```
을 추가해준다.<br>
<img src="https://user-images.githubusercontent.com/83760210/161373543-822c3848-1d10-4699-bdd7-b77b37c567b4.png"><br><br>

```
auto eth0
iface eth0 inet dhcp
```
위 명령어는 eth0 인터페이스를 dhcp를 이용해 부팅과 동시에 ip주소를 자동으로 할당받겠다는 의미이다.<br><br>

```
systemctl restart networking
```
위 명령어로 파일의 변경 내용이 적용이 되고
```
ip addr
```
다시 한번 위 명령어를 실행하면 eth0, eth1 모두 ip가 할당되어 있는 것을 확인할 수 있다.<br><br>

### 폰트 설정

이후 왼쪽 메뉴 바에서 firefox를 통해 www.naver.com에 접속해보면, 접속은 잘 되나 문자가 깨져서 나올 것이다.<br>
이는 한글 폰트가 없어서 발생하는 문제로, 한글 폰트를 설치해주어야 한다.<br>

다시 Terminal을 연 후 아래 명령어를 입력한다.<br>
```
apt-get install -y font-nanum
```
그러나 명령어를 실행하면 에러가 발생한다.<br>
그 이유는 나눔 폰트가 기본 저장소에 없기 때문이다.<br>
따라서 패키지 저장소를 변경해주어야 한다.<br><br>

```
nano etc/apt/sources.list
```
위 명령어를 Terminal에 입력한다.<br>
nano라는 편집기로 다음 파일을 편집한다는 의미이다.<br><br>

편집기에서 3번째 줄을 주석처리 하고(#) 그 아래에 다음과 같이 작성한다.<br>
```
deb http://httpredir.debian.org/debian jessie main non-free contrib
```
기본 저장소는 kali 저장소인데, 이를 kali의 upstream인 debian 저장소로 변경하였다.<br>
이후 ctrl+x를 통해 nano를 종료한 후 Terminal로 돌아간다.<br><br>

Terminal에서 다음 명령어를 실행한다.<br>
```
apt-get update
```
위 명령어를 통해 변경된 내용을 적용해준다.<br>
이후 다시 ```apt-get install -y fonts-nanum```을 실행하면 정상적으로 font가 다운로드 된다.<br><br>

다운로드가 끝난 후 아래 명령어를 Terminal에 입력한다.
```
fc-cache -f -v
```
이후 다시 www.naver.com에 접속하면 한글이 깨지지 않는 것을 확인할 수 있다.<br><br>

### 스냅샷
스냅샷은 VirtualBox에서 지원하는 기능으로, 가상 머신의 상태를 저장하는 기능이다.<br>
실습 도중 치명적인 오류가 발생하면 스냅샷으로 가상 머신을 돌릴 수 있다.<br>
일종의 백업 장치이다<br>
따라서 세팅이 끝난 후, 혹은 실습을 한 이후에는 스냅샷을 남기는 버릇을 들이자.<br><br>

스냅샷을 저장하기 위해서는 VirtualBox 인터페이스로 돌아간다.<br>
왼쪽 Kali Linux아이콘의 오른쪽 부분을 보면 리스트 모양으로 생긴 버튼이 있다.<br>
버튼을 눌러보면 정보, 스냅샷, 로그 버튼이 나오는데, 스냅샷 버튼을 클릭한다.<br>
이후 오른쪽 상단의 찍기 버튼을 누르면 스냅샷을 저장할 수 있다.<br><br>

Kali Linux를 종료한 상태에서 스냅샷을 선택하고 복원 버튼을 누르면 스냅샷을 저장한 당시 상황으로 Kali Linux를 부팅할 수 있다.<br><br>

## **XAMPP**

<br>XAMPP는 웹 서버로 아파치, MariaDB, PHP, Pearl을 포함한다. (PHP 공부할 때 사용한 Bitnami와 유사)<br>
마찬가지로 Kali Linux에 설치한다.<br>

firefox를 통해 www.apachefriends.org 에 접속하여 DOWNLOAD 버튼 아래의 Click here for other versions 버튼을 누른다.<br>
XAMPP for Linux 아래의 More Downloads를 클릭한다.<br>
XAMPP Linux 폴더에 들어가 5.6.23버전을 찾아 클릭한다.<br>
.run 파일 중 64비트용 .run파일을 다운받는다.<br><br>

다운이 끝난 후 Terminal을 열어 Downloads 폴더로 이동한다.<br>
```
cd Downloads/
```
이후 다운받은 run파일을 실행해주어야 하는데, 권한이 없어 실행이 불가능하다.<br>
```
chmod +x ./xampp-linux-x64-5.6.23-0-installer.run
```
위 명령어를 입력해 파일에 권한을 부여해주고 
```
./xampp-linux-x64-5.6.23-0-installer.run
```
위 명령어를 입력해 run파일을 실행한다.<br>
XAMPP installer의 인터페이스가 실행되며 이 역시 기본 설정값으로 설치해준다.<br><br>

다운로드가 완료 되면 아래 명령어를 입력한다.<br>
```
gedit /opt/lampp/etc/php.ini
```
gedit 에디터로 php.ini 파일이 열리면 ctrl+f로 allow_url_include를 검색하여 off를 on으로 바꿔준다.<br>
이후 XAMPP 인터페이스에서 MySQL Database를 start해주고 Apache Web Server를 restart 해준다.<br><br>

## **DVWA**

<br>firefox url창에 localhost/phpmyadmin/을 입력하거나 XAMPP 인터페이스에서 go to application을 클릭해 phpmyadmin에 접속한다.<br>
상단 메뉴바에서 Databases에 접속해 db를 하나 만든다. (이름 : dvwa)<br><br>

http://secuacademy.com/files 에서 dvwa를 다운받는다.<br>
Terminal에서 Downloads 폴더로 이동한 후
```
unzip DVWA-1.9-captcha-patched.zip
```
명령어를 통해 zip파일을 unzip해준다.<br>
이후 dvwa를 XAMPP 경로로 이동시킨다.<br>
```
mv DVWA-1.9 /opt/lampp/htdocs/dvwa
```

<br>이후 www.google.com/recaptcha/admin에 접속한다.<br>
dvwa에 captcha 키를 입력해야하므로 type 선택 항목에 reCAPTCHA v2 를 선택한 후<br>
```Label : dvwa, Domains : localhost```<br>
를 입력하고 Accept 버튼을 누른다.<br><br>

```
gedit /opt/lampp/htdocs/dvwa/config/config.ini.php
```
위 명령어를 입력해 설정 파일을 연 후, ctrl+f 로 ReCAPTCHA settings를 검색한다.<br>
public_key에 site key를, private_key에 secret key를 입력한다.<br><br>

```
chmod 777 /opt/lampp/htdocs/dvwa/hackable/uploads
```
```
chmod 777 /opt/lampp/htdocs/dvwa/external/phpids/0.6/lib/IDS/tmp/phpids_log.txt
```
위의 두 명령어를 입력하면 설정은 끝이난다.<br><br>

localhost/dvwa에 접속하여 빨간색 경고메시지가 뜨지 않으면 설정은 모두 완료된 것이다.<br>