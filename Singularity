Bootstrap: docker
From: centos:centos7.4.1708
IncludeCmd: yes

%setup

mkdir -p ${SINGULARITY_ROOTFS}/tmpdir
cp qe-6.4.tar.gz ${SINGULARITY_ROOTFS}/tmpdir
cp openmpi-2.1.1.tar.gz ${SINGULARITY_ROOTFS}/tmpdir
######### yambo 4.3.2 source #########
cp 4.3.2.tar.gz ${SINGULARITY_ROOTFS}/tmpdir

%post

echo "Installation software"
yum -y install wget vim which
yum -y groupinstall "Development tools"
yum -y install binutils
yum -y install dapl dapl-utils ibacm infiniband-diags libibverbs libibverbs-devel libibverbs-utils libmlx4 librdmacm librdmacm-utils mstflint opensm-libs perftest qperf rdma

##############################
# OpenMPI 2.1.1 installation
cd /tmpdir
tar -xvf  openmpi-2.1.1.tar.gz
rm openmpi-2.1.1.tar.gz
cd openmpi-2.1.1
./configure --prefix=/usr/local/openmpi --disable-getpwuid --enable-orterun-prefix-by-default

make
make install

export PATH=/usr/local/openmpi/bin:${PATH}
export LD_LIBRARY_PATH=/usr/local/openmpi/lib:${LD_LIBRARY_PATH}
##############################
# QE 6.4 installation
cd /tmpdir
tar -xvf qe-6.4.tar.gz
rm qe-6.4.tar.gz
cd q-e-qe-6.4
export QE_INSTALL_DIR=/usr/local/q-e-qe-6.4/
./configure --enable-openmp --enable-parallel
make all

export PATH=/tmpdir/q-e-qe-6.4/bin:$PATH
export MPICC=mpicc
export MPICXX=mpicxx
export MPIF90=mpif90
export MPIF77=mpif77
export MPIFC=mpifort

##############################
# Yambo 4.3.2 installation
cd /tmpdir
tar -xvf 4.3.2.tar.gz
rm 4.3.2.tar.gz
cd yambo-4.3.2
./configure
make yambo interfaces

##############################

# Test case compilation
#mkdir -p /test
#chmod 777 -R /test
#cd /test
#wget https://gitlab.hpc.cineca.it/container_data_example/data_example/raw/master/mpi/hello.c
#/usr/local/openmpi/bin/mpicc -o hello.x hello.c

##############################

echo "Creation mountpoints"
mkdir -p /marconi /galileo /scratch
chmod 777 -R /scratch

#############################

%environment

export PATH=/tmpdir/yambo-4.3.2/bin:/tmpdir/q-e-qe-6.4/bin:/usr/local/openmpi/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/openmpi/lib:$LD_LIBRARY_PATH
export MPICC=mpicc
export MPICXX=mpicxx
export MPIF90=mpif90
export MPIF77=mpif77
export MPIFC=mpifort
