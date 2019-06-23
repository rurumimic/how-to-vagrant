# Vagrantfile 설정

문서: [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile/)

잘 쓰고 싶다면 필요한 것.

- [x] 가상머신 이름 바꾸기
- [x] OS 호스트 이름 바꾸기
- [ ] 개수 설정
- [ ] IP 설정
- [ ] 포트 설정
- [ ] 사양 설정
- [ ] 공유 디렉터리 설정

## 가상머신 이름

문서: [Multi-Machine](https://www.vagrantup.com/docs/multi-machine/)

`config.vm.define "VM 이름"`으로 가상머신 이름을 설정할 수 있다.

```ruby
config.vm.define "mymachine"
```

```bash
$ vagrant status
Current machine states:

mymachine                  not created (virtualbox)
```

vagrant 상태를 확인했을 때 이름이 바뀌었다.

## 호스트 이름

문서: [config.vm.hostname](https://www.vagrantup.com/docs/vagrantfile/machine_settings.html#config-vm-hostname)

```ruby
config.vm.hostname = "myhost"
```

vagrant를 접속해보면 호스트 이름이 바뀌었다.

```bash
vagrant up
vagrant ssh

Welcome to Ubuntu 18.04.2 LTS (GNU/Linux 4.15.0-52-generic x86_64)
vagrant@myhost:~$
```

## 포트 설정

문서: [Forwarded Ports](https://www.vagrantup.com/docs/networking/forwarded_ports.html)
