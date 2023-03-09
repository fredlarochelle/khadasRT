# khadasRT
IN PROGRESS Simply a Khadas VIM2 image with the RT_PREEMPT patch that I compiled using [Fenix](https://github.com/khadas/fenix).

# Instructions
Mostly following the [fenix guide](https://github.com/khadas/fenix):
1. Installing required packages: 
```bash
sudo apt-get install git make lsb-release qemu-user-static
```
2. Clone the fenix repo:
```bash
git clone https://github.com/khadas/fenix.git
```
3. Change `LINUX_GIT_BRANCH` to Linux 6.1 (latest LTS release at the timing of writing) in your respective `config/boards/...` file.
4. Setup build environment and follow the instructions:
```bash
source env/setenv.sh
```
5. Build (`-j` is the number of CPU core you want to use): 
```bash
NO_GIT_UPDATE=1 make -j14
```
6. You will find your newly compiled image in `~/build/images/`!
7. After building a "standard" image, we can build a RT one. Go to https://cdn.kernel.org/pub/linux/kernel/projects/rt/6.1/ to find the latest RT patch and copy the link of the file.
8. Create a RT directory in the fenix folder:
```bash
mkdir RT
cd RT
```
9. Download the RT patch (use the latest link you got at step 7):
```bash
wget https://cdn.kernel.org/pub/linux/kernel/projects/rt/6.1/patch-6.1.12-rt7.patch.gz
```
10. Decompress the file:
```bash
gzip -d patch-6.1.12-rt7.patch.gz
```
11. Apply the patch:
```bash
cd ..
cd /build/linux
patch -p1 < ../RT/patch-6.1.12-rt7.patch
```
12. Kernel Configuration. You need to enable `Enable CONFIG_PREEMPT_RT`, `Enable CONFIG_HIGH_RES_TIMERS`, `Enable CONFIG_NO_HZ_FULL`, `Set CONFIG_HZ_1000` and `Set CPU_FREQ_DEFAULT_GOV_PERFORMANCE [=y]`:
```bash
make kernel-config
```
13. Build again:
```bash
NO_GIT_UPDATE=1 make -j14
```
