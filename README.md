# how-to-vagrant

Vagrant 사용하기

HashiCorp's [Vagrant](https://www.vagrantup.com)

## Vagrant란?

명령어 한 번만으로 VM을 설치, 실행, 종료까지 할 수 있다.  

## 목차

1. Vagrant 실행
   1. [설치](#설치)
   1. [Vagrant Cloud](#vagrant-cloud)
   1. [Vagrantfile 생성](#vagrantfile-생성)
   1. [Vagrantfile](#vagrantfile)
   1. [Vagrant 실행](#vagrant-실행)
   1. [Vagrant 상태 확인](#vagrant-상태-확인)
   1. [SSH 접속](#ssh-접속)
   1. [Vagrant 일시정지/종료](#vagrant-일시정지종료)
   1. [Vagrant 재개/실행](#vagrant-재개실행)
   1. [Vagrant Help](#vagrant-help)
1. [Vagrantfile 설정 (수정 중)](vagrantfile.md)

---

## 설치

필요한 것:
1. [Download Vagrant](https://www.vagrantup.com/downloads.html)
1. [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads)

Vagrant는 VirtualBox에서 공짜로 사용할 수 있다. VMWare 플러그인은 유료다.

## Vagrant Cloud

[Vagrant Cloud](https://app.vagrantup.com/)

개발자들이 미리 만들어둔 Vagrant 설정 파일들이 있다.  
Provider가 VirtualBox인 것들을 사용하자.

## Vagrantfile 생성

리눅스를 사용하고 싶다면,   
먼저 Vagrantfile을 직접 작성하거나  
명령어 한 번으로 기본 세팅을 해야 한다.

사용할 리눅스 버전에 따라  
콘솔에서 다음 명령어를 차례로 입력한다.

```bash
# 디렉터리 생성 및 접근
mkdir /path/to/dir
cd /path/to/dir
```

### CentOS

- CentOS Linux 7/x86_64 [centos/7](https://app.vagrantup.com/centos/boxes/7)

```bash
vagrant init centos/7
```

### Ubuntu

- Official Ubuntu 18.04 LTS: [ubuntu/bionic64](https://app.vagrantup.com/ubuntu/boxes/bionic64)
- Official Ubuntu 16.04 LTS: [ubuntu/xenial64](https://app.vagrantup.com/ubuntu/boxes/xenial64)

```bash
vagrant init ubuntu/bionic64
```

디렉터리 안에 Vagrantfile이 생성되었고  
Vagrant를 실행할 준비가 끝났다는 설명이 나온다.

## Vagrantfile

실행하기 전에 먼저 디렉터리 안에 들어있는 Vagrantfile을 에디터로 열어본다.

주석을 제외하면 설정파일의 내용은 다음과 같다.

### CentOS

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "cent/7"
end
```

### Ubuntu

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
end
```

설정 파일에 여러 가지 옵션을 추가해서 가상머신의 환경을 쉽게 수정할 수 있다.

## Vagrant 실행

```bash
vagrant up
```

Vagrant Cloud에서 이미 만들어져있는 리눅스 Box를 가져오는 것을 볼 수 있다.  

macOS에서는 `~/.vagrant.d/boxes`  
Windows는 `C:/Users/USERNAME/.vagrant.d/boxes`에 저장된다.

## Vagrant 상태 확인

```bash
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

VirtualBox 프로그램을 열면 리눅스가 목록에 추가되어 실행 중이라는 것을 볼 수 있다.  

## SSH 접속

```bash
vagrant ssh
```

가상머신 이름이 default가 아니거나 가상머신이 여러 개라면 `vagrant ssh 이름`으로 접속하면 된다.

## Vagrant 일시정지/종료

먼저 `exit` 명령으로 가상머신에서 빠져 나온다.

```bash
$ exit
logout
Connection to 127.0.0.1 closed.
```

가상머신을 일시 정지하거나 종료한다.

```bash
vagrant suspend # 가상머신 일시 정지
vagrant halt # 가상머신 종료
```

## Vagrant 재개/실행

`vagrant status`로 현재 상태를 확인할 수 있다.

가상머신을 다시 실행하고 싶다면 `resume`이나 `up`을 실행한다.

```bash
vagrant resume # 일시 정지 → 재개
vagrant up # 종료 → 실행
```

## Vagrant Help

그 밖에 Vagrant 명령어를 확인하고 싶다면  
`vagrant --help` 명령을 사용한다.