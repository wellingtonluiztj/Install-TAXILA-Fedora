# Install-TAXILA-Fedora

# TAXILA Installation on Clusters (Josephson and Bernal)

## You will need to install 
---
- **gcc-7.3.0**: Gnu Compile Collection is a compiler that support a large number of programming languages, hardware architectures and operating systems
- **Valgrid-3.13.0**: programming tool for memory debugging, memory leak detection, and profiling.
- **Openmpi-3.1.2**: Message Passing Interface implementation used to parallelize calculations on clusters 
- **OpenBlas**: OpenBLAS is an optimized Basic Linear Algebra Subprograms (BLAS) library based on GotoBLAS2 1.13 BSD version.
- **PETSc**: PETSc, the Portable, Extensible Toolkit for Scientific Computation, pronounced PET-see (/ˈpɛt-siː/), is a suite of data structures and routines for the scalable (parallel) solution of scientific applications modeled by partial differential equations.

---



## Previous Installations 
---
Some dependences required before start to install TAXILA on debian

<pre>
sudo apt-get update
sudo apt-install git
sudo apt-install build-essential
sudo apt-install -y fftw3
sudo apt-install libblas-dev
sudo apt-install liblapack-dev
sudo apt-install textinfo
</pre>
---
## Main Directory
---
*1.* Make a diretory to install Taxila: 
<pre>
mkdir LBM_NEW
</pre>
*2.* Enter in LBM_NEW diretory
<pre>
cd LBM_NEW
</pre>

---
## Install gcc-7.3.0
---

*1.* Download gcc-7.3.0
<pre>
wget https://ftp.gnu.org/gnu/gcc/gcc-7.3.0/gcc-7.3.0.tar.gz
</pre>
*2.* Unpack gcc
<pre>
tar -xvf gcc-7.3.0.tar.gz
</pre>
*3.* Make a *bin* diretory into the folder gcc-7.3.0
<pre>
mkdir bin
</pre>
*4.* Clear loaded libraries
<pre>
module purge
</pre>
*5.* Download the prerequisites
<pre>
./contrib/download_prerequisites
</pre>
*6.* Configure the build (all the times you find name wsantos, replace by your diretory name on scratch)
<pre>
./configure --enable-languages=c,c++,fortran,objc,obj-c++ --prefix=/scratch/wsantos/LBM_NEW/gcc-7.3.0/bin --disable-multilib
</pre>
*7.* Compile using the maximum number of processors (64 for AMD nodes and 28 for Intel nodes)
<pre>
make -j64
</pre>
*8.* Install
<pre>
make install
</pre>
*9.* Go to file .bashrc in your home
<pre>
cd; vim .bashrc
</pre>
*10.* Past the following lines
<pre>
export PATHLD_LIBRARY_PATH=$LD_LIBRARY_PATH:/scratch/wsantos/LBM_NEW/gcc-7.3.0/bin/lib64
export PATH=$PATH:/scratch/wsantos/LBM_NEW/gcc-7.3.0/bin/lib64
</pre>
*11.* Update bashrc file
<pre>
source .bashrc
</pre>
---
## Install valgrind-3.13.0
---
*1.* Donwload valgrind into LBM_NEW folder
<pre>
wget ftp://sourceware.org/pub/valgrind/valgrind-3.13.0.tar.bz2
</pre>
*2.* Unpack valgrind
<pre>
tar -xvf valgrind-3.13.0.tar.bz2
</pre>
*3.* Make *bin* diretory into valgrind-3.13.0 folder
<pre>
mkdir bin
</pre>
*4.* Into valgrind-3.13.0 folder, configure compilation
<pre>
./configure CC=/scratch/wsantos/LBM_NEW/gcc-7.3.0/bin/bin/gcc FC=/scratch/wsantos/LBM_NEW/gcc-7.3.0/bin/bin/gfortran --prefix=/scratch/wsantos/LBM_NEW/valgrind-3.13.0/bin
</pre>
*5.* Compile program using maximum number of processors
<pre>
make -j64
</pre>
*6.* Intall Valgrid
<pre>
make install
</pre>
---
## Install openmpi-3.1.2
---
*1.* Download do openmpi-3.1.2 into LMB_NEW folder
<pre>
wget https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-3.1.2.tar.gz
</pre>
*2.* Umpack openmpi-3.1.2
<pre>
tar -xvf openmpi-3.1.2.tar.gz
</pre>
*3.* Make bin diretory into the openmpi-3.1.2 folder 
<pre>
mkdir bin
</pre>
*4.* Configure Compilation
<pre>
./configure CC=/scratch/wsantos/LBM_NEW/gcc-7.3.0/bin/bin/gcc FC=/scratch/wsantos/LBM_NEW/gcc-7.3.0/bin/bin/gfortran --prefix=/scratch/wsantos/LBM_NEW/openmpi-3.1.2/bin
</pre>
*5.* Compile using maximum number of processors
<pre>
make -j64
</pre>
*6.* Execute executable to install openmpi
<pre>
make install
</pre>
---
## Install openBlas
---
*1.* Download do openBLAS dentro de LMB_NEW 
<pre>
git clone https://github.com/xianyi/OpenBLAS.git)
</pre>
*2.* Make *bin* diretoty into openBLAS folder
<pre>
mkdir bin
</pre>
*3.* 
<pre>
 make CC=/scratch/wsantos/LBM_NEW/openmpi-3.1.2/bin/mpicc FC=/scratch/wsantos/LBM_NEW/openmpi-3.1.2/bin/mpif90 NO_LAPACKE=1 PREFIX=/scratch/wsantos/LBM_NEW/OpenBLAS/bin HOSTCC=gcc NO_STATIC=1 DYNAMIC_ARCH=1 NO_AFFINITY=1 USE_OPENMP=1
</pre>
*4.*
<pre>
 make CC=/scratch/wsantos/LBM_NEW/openmpi-3.1.2/bin/mpicc FC=/scratch/wsantos/LBM_NEW/openmpi-3.1.2/bin/mpif90 NO_LAPACKE=1 PREFIX=/scratch/wsantos/LBM_NEW/OpenBLAS/bin HOSTCC=gcc NO_STATIC=1 DYNAMIC_ARCH=1 NO_AFFINITY=1 USE_OPENMP=1 install 
</pre>
---
## Install petsc-3.4.4
---
Make sure that you're installing this version

*1.* Make Download of petsc-3.4.4  
<pre>
wget http://romeo.if.usp.br/~kirch/CODES.tar
</pre>
*2.* Umpack petsc-lite-3.4.4.tar.gz
<pre>
tar -xvf petsc-lite-3.4.4.tar.gz
</pre>
*3.* Congigure Compillation
<pre>
./configure --with-x=0 --with-mpi-dir=/scratch/wsantos/LBM_NEW/openmpi-3.1.2 --with-mpiexec=/scratch/wsantos/LBM_NEW/openmpi-3.1.2/bin/mpirun --with-blas-lib=/scratch/wsantos/LBM_NEW/OpenBLAS/bin/lib/libopenblas.a --with-lapack-lib=/scratch/wsantos/LBM_NEW/OpenBLAS/bin/lib/libopenblas.a --known-mpi-shared-libraries=0 --with-shared-libraries=0 --with-valgrind=yes –with-valgrind-dir=/scratch/wsantos/LBM_NEW/valgrind-3.13.0/bin
</pre>
*3.* Run suggested command gave at the end as it seen bellow
<pre>
Dar o comando sugerido ao final(make PETSC_DIR=/scratch/wsantos/LBM_NEW/petsc-3.4.4 PETSC_ARCH=arch-linux2-c-debug all)
</pre>
*4.* Edit .bashrc file 
<pre>
export PETSC_DIR=/scratch/wsantos/LBM_NEW/petsc-3.4.4  
export LBM_DIR=/scratch/wsantos/LBM_NEW/taxila-font-Mar14
</pre>
*6.* Update bashrc file
<pre>
source .bashrc
</pre>
---
## Intall Taxila
---
*1.* Umpack folder taxila-font-Mar14
<pre>
tar -xvf taxila-font-Mar14
</pre>
*2.* Into taxila-font-Mar14 directory type comand bellow
<pre>
gzip -d -r *
</pre>
Enter on diretory /scratch/wsantos/LBM_NEW/taxila-font-Mar14/src/lbm and type command bellow
<pre>
make
</pre>
---
## Run Taxila
---
*1.* Download folder 2D_Displacement (availible into this repository) and compile Taxila using commands bellow
<pre>
make clean
make
</pre>
*2.* Run Taxila
<pre>
sbatch script_model.sh
</pre>
---
## End
---
