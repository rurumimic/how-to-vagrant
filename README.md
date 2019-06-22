# how-to-vagrant

Vagrant 사용하기

[HashiCorp Vagrant](https://www.vagrantup.com)

## 목차

1. [Vagrant 실행](README.md)
1. [Vagrantfile 설정 (수정 중)](vagrantfile.md)

## Vagrant란?

명령어 한 번만으로 VM을 설치, 실행, 종료까지 할 수 있다.  

## 설치

필요한 것:
1. [Download Vagrant](https://www.vagrantup.com/downloads.html)
1. [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads)

Vagrant는 VirtualBox에서 공짜로 사용할 수 있다. VMWare는 플러그인이 유료다.

## Vagrant Cloud

[Vagrant Cloud](https://app.vagrantup.com/)

개발자들이 미리 만들어둔 Vagrant 설정 파일들이 있다.  
Provider가 VirtualBox인 것들을 사용하자.

## Ubuntu Box 준비

- Official Ubuntu 18.04 LTS: [ubuntu/bionic64](https://app.vagrantup.com/ubuntu/boxes/bionic64)
- Official Ubuntu 16.04 LTS: [ubuntu/xenial64](https://app.vagrantup.com/ubuntu/boxes/xenial64)

우분투를 사용하고 싶다면,   
먼저 Vagrantfile을 직접 작성하거나  
명령어 한 번으로 기본 세팅을 해야 한다.

예를 들어, 18.04 LTS를 사용한다면  
콘솔에서 다음 명령어를 차례로 입력한다.

```bash
# 디렉터리 생성 및 접근
mkdir /path/to/ubuntu18
cd /path/to/ubuntu18

# Vagrantfile 생성
vagrant init ubuntu/bionic64
```

디렉터리 안에 Vagrantfile이 생성되었고  
Vagrant를 실행할 준비가 끝났다는 설명이 나온다.

## Vagrantfile

실행하기 전에 먼저 디렉터리 안에 들어있는 Vagrantfile을 에디터로 열어본다.

주석을 제외하면 설정파일의 내용은 다음과 같다.

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
end
```

설정 파일에 여러 가지 옵션을 추가해서 가상머신의 환경을 쉽게 수정할 수 있다.

## Vagrant 실행

```bash
# Vagrant 실행
vagrant up
```

Vagrant Cloud에서 이미 만들어져있는 Ubuntu를 가져오는 것을 볼 수 있다.  

macOS에서는 `~/.vagrant.d/boxes`  
Windows는 `C:/Users/USERNAME/.vagrant.d/boxes`에 저장된다.

## Vagrant 상태 확인

```bash
# Vagrant 상태 확인
vagrant status
```

Vagrant 상태를 확인하면

```bash
Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
```

default라는 이름으로 virtualbox에서 실행되는 것을 볼 수 있다.

VirtualBox 프로그램을 열면 Ubuntu가 목록에 추가되어 실행 중이라는 것을 볼 수 있다.  

## Ubuntu 접속

```bash
# Vagrant 접속
vagrant ssh

Welcome to Ubuntu 18.04.2 LTS (GNU/Linux 4.15.0-52-generic x86_64)
vagrant@ubuntu-bionic:~$
```

가상머신 이름이 default가 아니거나 가상머신이 여러 개라면 `vagrant ssh 이름`으로 접속하면 된다.

## Vagrant 종료

먼저 `exit` 명령으로 가상머신에서 빠져 나온다.

```bash
vagrant@ubuntu-bionic:~$ exit
logout
Connection to 127.0.0.1 closed.
```

가상머신을 일시 정지하거나 종료한다.

```bash
# 가상머신 일시 정지
vagrant suspend
# 가상머신 종료
vagrant halt
```

`vagrant status`로 현재 상태를 확인할 수 있다.

가상머신을 다시 실행하고 싶다면 `resume`이나 `up`을 실행한다.

```bash
# 일시 정지 -> 재개
vagrant resume
# 종료 -> 실행
vagrant up
```

그 밖에 Vagrant 명령어를 확인하고 싶다면  
`vagrant --help` 명령을 사용한다.
