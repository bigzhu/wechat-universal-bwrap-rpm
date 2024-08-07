# fedora 下安装微信

## 准备工作

```bash
sudo dnf install @development-tools fedora-packager rpmdevtools
sudo usermod -a -G mock $USER
```

## 下载 deb

clone 本项目, 执行下载脚本

```bash
cd wechat-universal-bwrap-rpm
./download-deb.sh
```

## 编译

```bash
mock --init
mock --buildsrpm --spec wechat-universal-bwrap.spec --sources src
cp /var/lib/mock/*-$(uname -m)/result/wechat-universal-bwrap-*.src.rpm .
mock --rebuild ./wechat-universal-bwrap-*.src.rpm
cp /var/lib/mock/*-$(uname -m)/result/wechat-universal-bwrap-*.rpm .
```

最后得到的这个不带 src 的 rpm, 就是我们可用的安装包

## 安装

用 yum 或者 dnf 是安装不了的, 得用 rpm, 并且 在 fedora40 下安装提示缺少 `libbz2.so.1.0()(64bit)`, 解决办法如下

```bash
sudo cp /usr/lib64/libbz2.so.1 /usr/lib64/libbz2.so.1.0
sudo rpm -i ./wechat-universal-bwrap-1.0.0.241-1.fc40.x86_64.rpm --nodeps
```
