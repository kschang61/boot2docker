A modified "update.sh" to build boot2docker.iso with different TinyCoreLinux (4.14, 4.19, 5.4)

To use this update, perform the following steps

0. Provision a minimal Ubuntu 20.04 (focal) with Linux kernel version 5.4.
    You need at least a 5.4 kernel otherwise you will encounter an error like "chroot: FATAL: kernel too old"

1. Install git, jq & docker.io and update kernel

    sudo apt update && sudo apt install -y git jq docker.io && sudo apt upgrade -y

    sudo systemctl enable docker

    sudo usermod -aG docker ubuntu

    sudo reboot

2. Login as "ubuntu" (or any user in the docker group)

3. Clone boot2docker

    git clone https://github.com/boot2docker/boot2docker.git

4. Backup and upload "modifed" update.sh

    cd boot2docker
    
    cp -p Dockerfile Dockerfile.bak
    
    mv update.sh update.sh.bak
    
    <copy/upload> update.sh # from this GIT

5. Edit update.sh to select TinyCoreLinux and Linux kernel version

    major='11.x'

    version='11.1'
    
    ...
    
    kernelBase='5.4'

6. Build boot2docker.iso

    docker build -t boot2docker .

7. Copy iso from docker

    docker run -d --rm --name boot2docker boot2docker sleep 200
    
    docker cp boot2docker:/tmp/boot2docker.iso boot2docker.iso

