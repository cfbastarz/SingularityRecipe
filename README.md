# SingularityRecipe

Recipe on how to create a singularity image file. 

## Requirements

* Linux OS and singularity installed.

# Download a base Ubuntu image to customize

Use Ubuntu 18.04 as a base to customize:

```bash
sudo singularity build --sandbox ubuntu_remix_vx.y.z/ library://ubuntu:18.04
```

* Reference: https://www.youtube.com/watch?v=nrgO3Q8-6hQ&t=342s&ab_channel=DanielPersson


# Mount the image for customization

```bash
sudo singularity exec --writable ubuntu_remix_vx.y.z /bin/bash
```

# System customization

Update the base system, install `vim` (or `nano`) to configure the system repository list:

```bash
apt update 
apt upgrade
apt install vim
apt install vim-gtk
```

## Update system repository

Edit the `/etc/apt/sources.list`:

```bash
vim /etc/apt/sources.list
```

It should contain:

```bash
deb http://archive.ubuntu.com/ubuntu/ bionic main restricted
deb http://archive.ubuntu.com/ubuntu/ bionic-updates main restricted
deb http://archive.ubuntu.com/ubuntu/ bionic universe
deb http://archive.ubuntu.com/ubuntu/ bionic-updates universe
deb http://archive.ubuntu.com/ubuntu/ bionic multiverse
deb http://archive.ubuntu.com/ubuntu/ bionic-updates multiverse
deb http://archive.ubuntu.com/ubuntu/ bionic-backports main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu bionic-security main restricted
deb http://security.ubuntu.com/ubuntu bionic-security universe
deb http://security.ubuntu.com/ubuntu bionic-security multiverse
```

Update the system:

```bash
apt update
apt upgrade
```

## Locale configuration

```bash
apt install locales locales-all
dpkg-reconfigure locales
```

*Note*: choose the locales 378 e 158 (i.e., 378. pt_BR.UTF-8 UTF-8, 158. en_US.UTF-8 UTF-8)
*Note*: default will be `158`

And run:

```bash
locale-gen
```

* Reference: https://www.linuxquestions.org/questions/linux-newbie-8/bash-warning-setlocale-lc_all-cannot-change-locale-en-us-utf-8-a-4175628657/

## Packages instalation

At this point, your requirements may be different. Install everything you need at this point.

```bash
apt install build-essential  
apt install gdb gdb-doc
apt install cgdb
apt install cmake ksh csh tcsh automake autoconf autoconf-archive libtool pkg-config
apt install libssl-dev uuid-dev libgpgme11-dev squashfs-tools
apt install xauth
apt install rsync wget
apt install zip p7zip-full
apt install subversion git
apt install gcc-4.8 g++-4.8 gfortran-4.8
apt install gcc-5 g++-5 gfortran-5
apt install gcc-6 g++-6 gfortran-6
apt install gcc-7 g++-7 gfortran-7
apt install gcc-8 g++-8 gfortran-8
apt install mpich libstdc++-6-dev manpages-dev
apt install libhugetlbfs0 libhugetlbfs-dev libblas3 liblapack3
apt install grads cdo nco ncl-ncarg gnuplot
apt install python3 curl htop ncftp sshfs sshpass
apt install netcdf-bin libnetcdf-c++4 libnetcdf-dev libnetcdf13 libnetcdff6 ncview 
apt install gdal-bin gdal-data 
apt install cdftools hdf5-tools 
apt install glueviz libjpeg8 imagemagick 
apt install pandoc
apt install doxygen
apt install emacs
apt install mpi-default-dev
```

## `gmake` alias

```bash
cd /usr/bin
ln -s make gmake
```

* Reference: https://www.programmersought.com/article/97402906000/

## `libmpi.so` configuration

*Note*: only needed if you use MPI.

Create a symlink to the `libmpi_mpifh.so`:

```bash
cd /usr/lib/x86_64-linux-gnu
ln -s libmpi_mpifh.so.20 libmpi_mpifh.so.40
```

And run:

```bash
ldconfig
```

* Reference: https://stackoverflow.com/questions/14769599/mpi-error-loading-shared-libraries/40817816

## `locate` command

```bash
apt install mlocate
updatedb
```

* Reference: https://askubuntu.com/questions/215503/how-to-install-the-locate-command

## `gcc`, `g++` and `gfortran` versions

```bash
update-alternatives --remove-all gcc
update-alternatives --remove-all g++
update-alternatives --remove-all gfortran

update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 10
update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 20
update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 30
update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 40
update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 50

update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 10
update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 20
update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-6 30
update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 40
update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 50

update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-4.8 10
update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-5 20
update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-6 30
update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-7 40
update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-8 50

update-alternatives --config gcc
update-alternatives --config g++
update-alternatives --config gfortran
```

* Reference: https://askubuntu.com/questions/26498/how-to-choose-the-default-gcc-and-g-version

## JAVA install and configuration

```bash
apt install default-jre default-jdk openjdk-8-jdk
```

```bash
update-alternatives --config java
```

*Choose*: `/usr/lib/jvm/java-11-openjdk-amd64/bin/java`

```bash
update-alternatives --config javac 
```

*Choose*: `/usr/lib/jvm/java-11-openjdk-amd64/bin/javac`

* Reference: https://www.digitalocean.com/community/tutorials/como-instalar-o-java-com-apt-no-ubuntu-18-04-pt

## System cleanup

Perform a system cleanup before the image creation:

```bash
apt update
apt upgrade
apt autoremove
apt autoclean
apt clean
```

## Exit the singularity shell

```bash
exit
```

# Create the SIF - Singularity Image File

This is the part where the singularity container image is created. At this point, you've exited the singularity shell and are at your local machine.

```bash
sudo singularity build ubuntu_remix_vx.y.z.sif ubuntu_remix_vx.y.z
```

* Reference: https://www.youtube.com/watch?v=nrgO3Q8-6hQ&t=342s&ab_channel=DanielPersson

If you just want to use the container, you may stop here.

## Sign the image

```bash
singularity keys newpair
singularity remote list
singularity remote login SylabsCloud
```

*Note*: Go to https://cloud.sylabs.io/auth/tokens and create a new token, copy and paste at the prompt.

```bash
singularity key push 085718024004F8B0A4A7FC15C8F8538735EC9DD1
singularity sign ubuntu_remix_vx.y.z.sif
```

## Push the created container to the cloud

```bash
singularity push ubuntu_remix_vx.y.z.sif library://your_user_name/default/ubuntu_remix
```

## To pull the container

```bash
singularity pull --arch amd64 library://your_user_name/default/ubuntu_remix:vx.y.z
singularity pull library://your_user_name/default/library:demo
```

## To verify the image

```bash
singularity verify ubuntu_remix_vx.y.z.sif
```

* Reference: https://sylabs.io/guides/3.0/user-guide/signNverify.html

## To use the created SIF

```bash
singularity shell -e image_file.sif
```
