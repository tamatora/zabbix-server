# 構築場所
TS240vmh001P上のVM ETS240dev003Pに実装

# 初期設定
## 1.yumリポジトリ
> CentOS-Base.repo
```
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
baseurl=http://pkgsv.local/rpm/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates 
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
baseurl=http://pkgsv.local/rpm/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
baseurl=http://pkgsv.local/rpm/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```
> epel.repo
```
[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
baseurl=http://pkgsv.local/rpm/epel/7/$basearch/
#metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch&infra=$infra&content=$contentdir
failovermethod=priority
enabled=1
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7

[epel-debuginfo]
name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
#baseurl=http://download.fedoraproject.org/pub/epel/7/$basearch/debug
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-debug-7&arch=$basearch&infra=$infra&content=$contentdir
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1

[epel-source]
name=Extra Packages for Enterprise Linux 7 - $basearch - Source
#baseurl=http://download.fedoraproject.org/pub/epel/7/SRPMS
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-source-7&arch=$basearch&infra=$infra&content=$contentdir
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1
```
> jumble.repo
```
[jumble]
name=Jumble of my rpm packages
baseurl=http://pkgsv.local/rpm/jumble/
#metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch&infra=$infra&content=$contentdir
failovermethod=priority
enabled=1
gpgcheck=0
```
## 2.user追加
> 設定用ユーザの追加
```
# useradd setupuser
# passwd setupser
password:
```
> sudo付与：`visudo`
```
...
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
setupuser       ALL=(ALL)       ALL
...
```
## 3.SSH接続設定
> rootでのログイン禁止：`/etc/ssh/sshd_config`
```
...
PermitRootLogin no
...
```
> sshdの再起動：`systemctl restart sshd`

# Ansible実行コマンド
> 構文チェック(開発環境)
```
ansible-playbook site.yml -i hosts/development --syntax-check -K -k
ansible-playbook site.yml -i hosts/development -C -K -k
```
> 実行(開発環境)
```
ansible-playbook site.yml -i hosts/development -K -k
```
