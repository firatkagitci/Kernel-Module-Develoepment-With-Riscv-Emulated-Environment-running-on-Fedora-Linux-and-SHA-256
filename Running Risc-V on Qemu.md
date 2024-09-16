The following instructuins are given to direct you to create your emulation. 

Prepare the Pi 

sudo apt update
sudo apt install build-essential nano git htop ninja-build wget

Build QEMU

git clone https://github.com/qemu/qemu
'
cd qemu
mkdir build
cd build
../configure --target-list=riscv64-softmmu
make  -j3
sudo make install
cd ..
'
Download the RISC-V version of Fedora
You can download Fedora RISC-V from https://dl.fedoraproject.org/pub/alt/risc-v/repo/virt-builder-images/images/

wget https://dl.fedoraproject.org/pub/alt/risc-v/repo/virt-builder-images/images/Fedora-Minimal-Rawhide-20200108.n.0-fw_payload-uboot-qemu-virt-smode.elf
wget https://dl.fedoraproject.org/pub/alt/risc-v/repo/virt-builder-images/images/Fedora-Minimal-Rawhide-20200108.n.0-sda.raw.xz
unxz Fedora-Minimal-Rawhide-20200108.n.0-sda.raw.xz
Run QEMU
qemu-system-riscv64 \
   -nographic \
   -machine virt \
   -smp 2 \
   -m 2047M \
   -kernel Fedora-Minimal-Rawhide-*-fw_payload-uboot-qemu-virt-smode.elf \
   -bios none \
   -object rng-random,filename=/dev/urandom,id=rng0 \
   -device virtio-rng-device,rng=rng0 \
   -device virtio-blk-device,drive=hd0 \
   -drive file=Fedora-Minimal-Rawhide-20200108.n.0-sda.raw,format=raw,id=hd0 \
   -device virtio-net-device,netdev=usernet \
   -netdev user,id=usernet,hostfwd=tcp::10000-:22
Fedora
Login with riscv and fedora_rocks!

sudo dnf install gcc htop nano git file
mkdir src
cd src
curl https://raw.githubusercontent.com/garyexplains/examples/master/doublelinkedlist.c --output doublelinkedlist.c
gcc -O3 -o doublelinkedlist doublelinkedlist.c
file doublelinkedlist 
./doublelinkedlist 
