# Vagrantfile 설정

문서: [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile/)

잘 쓰고 싶다면 필요한 것.

[예제](#예제)

- [x] 가상머신 이름 바꾸기
- [x] OS 호스트 이름 바꾸기
- [X] 개수 설정
- [X] IP 설정
- [X] 포트 설정
- [X] 사양 설정
- [X] 공유 디렉터리 설정
- [X] Provision 설정

---

## Vagrantfile

Vagrantfile은 ruby 언어로 작성한다.

```ruby
# -*- mode: ruby -*-
# # vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # 설정 1
  # 설정 2
  # ...
end
```

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

## 개수 설정

문서: [Tips & Tricks](https://www.vagrantup.com/docs/vagrantfile/tips.html)

```ruby
(1..3).each do |i|
  config.vm.define "node-#{i}" do |node|
    node.vm.provision "shell",
      inline: "echo hello from node #{i}"
  end
end
```

반복문 안에서 VM마다 설정을 다르게 할 수 있다.

참고: [coreos/coreos-vagrant: Vagrantfile#90](https://github.com/coreos/coreos-vagrant/blob/08572747857bab12c4b9e81632063c4ccc94651d/Vagrantfile#L90)

```ruby
$num_instances = 3
$instance_name_prefix = "core"

(1..$num_instances).each do |i|
  config.vm.define vm_name = "%s-%02d" % [$instance_name_prefix, i] do |config|
    config.vm.hostname = vm_name
  end
end
```

core-01, core-02, core-03이라는 VM이 생성된다.

## IP 설정

### Private Networks

문서: [Private Networks](https://www.vagrantup.com/docs/networking/private_network.html)

```ruby
config.vm.network "private_network", type: "dhcp"
config.vm.network "private_network", ip: "192.168.50.4"
```

참고: [coreos/coreos-vagrant: Vagrantfile#140](https://github.com/coreos/coreos-vagrant/blob/08572747857bab12c4b9e81632063c4ccc94651d/Vagrantfile#L140)

```ruby
$num_instances = 3
$instance_name_prefix = "core"

(1..$num_instances).each do |i|
  config.vm.define vm_name = "%s-%02d" % [$instance_name_prefix, i] do |config|
    ip = "172.17.8.#{i+100}"
    config.vm.network :private_network, ip: ip
  end
end
```

## 포트 설정

문서: [Forwarded Ports](https://www.vagrantup.com/docs/networking/forwarded_ports.html)

```ruby
config.vm.network "forwarded_port", guest: 80, host: 8080
config.vm.network "forwarded_port", guest: 8888, host: 8888, auto_correct: true
```

- `auto_correct: true`: 여러 VM을 사용할 때 겹치는 포트를 자동으로 수정한다.

참고: [coreos/coreos-vagrant: Vagrantfile#120](https://github.com/coreos/coreos-vagrant/blob/08572747857bab12c4b9e81632063c4ccc94651d/Vagrantfile#L120)

```ruby
$forwarded_ports = { 
  "80" => "8080", 
  "8888" => "8888" 
}

$forwarded_ports.each do |guest, host|
  config.vm.network "forwarded_port", guest: guest, host: host, auto_correct: true
end
```

## 사양 설정

문서: [Machine Settings](https://www.vagrantup.com/docs/vagrantfile/machine_settings.html)

참고: [coreos/coreos-vagrant: Vagrantfile#132](https://github.com/coreos/coreos-vagrant/blob/08572747857bab12c4b9e81632063c4ccc94651d/Vagrantfile#L132)

```ruby
config.vm.provider :virtualbox do |vb|
  vb.gui = false
  vb.memory = 1024
  vb.cpus = 1
  vb.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
  config.ignition.config_obj = vb
end
```

## 공유 디렉터리 설정

문서: [Synced Folders](https://www.vagrantup.com/docs/synced-folders/basic_usage.html)

```ruby
config.vm.synced_folder "src/", "/srv/website"
config.vm.synced_folder "src/", "/srv/website", type: "nfs"
```

참고: [coreos/coreos-vagrant: Vagrantfile#147](https://github.com/coreos/coreos-vagrant/blob/08572747857bab12c4b9e81632063c4ccc94651d/Vagrantfile#L147)

```ruby
$shared_folders = { "src/" => "/srv/website" }

#config.vm.synced_folder ".", "/home/core/share", id: "core", :nfs => true, :mount_options => ['nolock,vers=3,udp']
$shared_folders.each_with_index do |(host_folder, guest_folder), index|
  config.vm.synced_folder host_folder.to_s, guest_folder.to_s, id: "core-share%02d" % index, nfs: true, mount_options: ['nolock,vers=3,udp']
end
```

VM끼리 `home` 디렉터리 공유

```ruby
config.vm.synced_folder ENV['HOME'], ENV['HOME'], id: "home", :nfs => true, :mount_options => ['nolock,vers=3,udp']
```

## Provision 설정

### Shell Provisioner

문서: [Shell Provisioner](https://www.vagrantup.com/docs/provisioning/shell.html)

```ruby
config.vm.provision "shell", inline: "echo Hello, World"
```

### Docker Provisioner

문서: [Docker Provisioner](https://www.vagrantup.com/docs/provisioning/docker.html)

도커를 설치할 수 있다.

```ruby
config.vm.provision "docker"
```

도커 이미지를 빌드할 수 있다.

```ruby
config.vm.provision "docker" do |d|
  d.build_image "/vagrant/app"
end
```

도커 이미지를 다운받는 방법은 두 가지가 있다.

```ruby
config.vm.provision "docker", images: ["ubuntu"]
```

```ruby
config.vm.provision "docker" do |d|
  d.pull_images "ubuntu"
  d.pull_images "vagrant"
end
```

도커 컨테이너를 실행할 수 있다.

```ruby
config.vm.provision "docker" do |d|
  d.run "rabbitmq"
end
```

---

## 예제

```bash
cd /path/to/dir
mkdir share
vi Vagrantfile
vagrant up
```

```ruby
# -*- mode: ruby -*-
# # vi: set ft=ruby :

$num_instances = 2
$instance_name_prefix = "my"
$shared_folders = { "share" => "/share" }
$forwarded_ports = { 80 => 8080 }

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"

  (1..$num_instances).each do |i|
    config.vm.define vm_name = "%s-%02d" % [$instance_name_prefix, i] do |config|
      config.vm.hostname = vm_name

      $forwarded_ports.each do |guest, host|
        config.vm.network "forwarded_port", guest: guest, host: host, auto_correct: true
      end

      config.vm.provider :virtualbox do |vb|
        vb.gui = false
        vb.memory = 1024
        vb.cpus = 1
      end

      ip = "172.17.8.#{i+100}"
      config.vm.network :private_network, ip: ip

      $shared_folders.each_with_index do |(host_folder, guest_folder), index|
        config.vm.synced_folder host_folder.to_s, guest_folder.to_s, id: "core-share%02d" % index, nfs: true, mount_options: ['nolock,vers=3,udp']
      end
    end
  end
end
```