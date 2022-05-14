# About
I bought an Acer Spin 713 Chromebook (Intel 11th Gen i5, 8G RAM, 256G nvme SSD) recently. These are the steps I took to turn it into a workstation for development (rust & nodejs mostly).

## nfs mount
```bash
git clone --depth=1 https://github.com/sahlberg/fuse-nfs.git
./setup.sh
./configure
make
sudo make install
sudo setcap 'cap_net_bind_service=+ep' `which fuse-nfs`
sudo mkdir /mnt/some/path
sudo chown $USER:`id -ng $USER` /mnt/some/path
fuse-nfs -n nfs://<remote_ip>/to/nfs/share?uid=`id -u $USER` -m /mnt/some/path
```

## rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## vsc
follow [https://code.visualstudio.com/blogs/2020/12/03/chromebook-get-started](https://code.visualstudio.com/blogs/2020/12/03/chromebook-get-started)



## keepassxc
```
sudo apt install keepassxc
```

## docker
```bash
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
# run this to verify docker is functional
sudo docker run hello-world
# allow non-root user to run docker
sudo usermod -aG docker $USER
# log out and back in, then
docker run hello-world
```

## sccache
```bash
sudo apt install pkg-config libssl-dev
cargo install sccache
echo 'export RUSTC_WRAPPER=$HOME/.cargo/bin/sccache' >> ~/.profile
```

## nodejs
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
# then in a new terminal tab
# this will install the currently newest stable nodejs 18.1.0
nvm install node
# or install a specific version
nvm install 16
```