# suitesparse-eigen-vs-installation
This is a document helping you for installing suitesparse-metis and eigen on microsoft visual studio
===============================================================================
1 Installation of eigen 
------------------
The eigen is intalled with the version earlier than eigen-3.3 at http://eigen.tuxfamily.org/index.php?title=Special%3AAllPages&from=&to=&namespace=100, if version is later than 3.3(include 3.3) Some errors may
occur when executing the project.

2 Installation of suitesparse-metis-for-windows
---------------------------------------------
Some version of suitesparse-metis-for-windows can be got at https://github.com/jlblancoc/suitesparse-metis-for-windows/releases
Here we select the version 1.3.0

* install [cmake](https://cmake.org/)
* download the suitesparse-metis-for-windows to an certain path which is defined as SP_ROOT
* open SP_ROOT/metis/CMakeLists.txt, and add the command line "cmake_policy(SET CMP0022 NEW)" below "project(METIS)" 

```
cmake_minimum_required(VERSION 2.8)
project(METIS)
cmake_policy(SET CMP0022 NEW)
set(GKLIB_PATH "GKlib" CACHE PATH "path to GKlib")
set(SHARED FALSE CACHE BOOL "build a shared library")
 
if(MSVC)
  set(METIS_INSTALL FALSE)
else()
  set(METIS_INSTALL TRUE)
endif()
 
# Configure libmetis library.
if(SHARED)
  set(METIS_LIBRARY_TYPE SHARED)
else()
  set(METIS_LIBRARY_TYPE STATIC)
endif(SHARED)
 
include(${GKLIB_PATH}/GKlibSystem.cmake)
# Add include directories.
include_directories(${GKLIB_PATH})
include_directories(include)
# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("include")
add_subdirectory("libmetis")
add_subdirectory("programs")

```
* execute Cmake
1. set Source code as SP_ROOT 
</br> 
1. set path of build as SP_ROOT/build
</br>
1. press "configure"
</br>
1. then you will see some red place as follows,please ignore them and go on.
</br>
1.  press "Generate"
</br>
1. open the visual studio project "suitespareproject.sln" and ser INSTALL as the boot item and build in debug and release mode. 22 projects will be built successly.

- test
use the code

```
#include <iostream>
#include "Eigen/Eigen"
#include "Eigen/SPQRSupport"
using namespace Eigen ;
int main ( ) {
	
	SparseMatrix < double > A ( 4 , 4 ) ;
	std :: vector < Triplet < double > > triplets ;
 
	// 初始化非零元素
	int r [ 3 ] = { 0 , 1 , 2 } ;
	int c [ 3 ] = { 1 , 2 , 2 } ;
	double val [ 3 ] = { 6.1 , 7.2 , 8.3 } ;
	for ( int i = 0 ; i < 3 ; ++ i )
		triplets . push_back( Triplet < double >(r [ i ] , c [ i ] , val [ i ]) ) ;
 
	// 初始化稀疏矩阵
	A . setFromTriplets ( triplets . begin ( ) , triplets . end ( ) ) ;
	std :: cout << "A = \n" << A << std :: endl ;
 
	// 一个QR分解的实例
	SPQR < SparseMatrix < double > > qr ;
	// 计算分解
	qr . compute ( A ) ;
	// 求一个A x = b
	Vector4d b ( 1 , 2 , 3 , 4 ) ;
	Vector4d x = qr . solve ( b ) ;
	std :: cout << "x = \n" << x ;
	std :: cout << "A x = \n" << A * x ;
 
	return 0 ;
}

```

before this, you need add configures in VS property
1. VC++->include directory add the root path of eigen
and SP_ROOT\build\install\include\suitesparse
2. VC++->library directory add SP_ROOT\build\install\lib64 and SP_ROOT\lapack_windows\x64
3. Linker->Additional dependency  add
libfftw3-3.lib
libfftw3l-3.lib
libfftw3f-3.lib
libamdd.lib
libbtfd.lib
libcamdd.lib
libccolamdd.lib
libcholmodd.lib
libcolamdd.lib
libcxsparsed.lib
libklud.lib
libldld.lib
libspqrd.lib
libumfpackd.lib
suitesparseconfigd.lib
libblas.lib
liblapack.lib
metisd.lib

4.  add the following dlls to the root path of test project which can been found in 
SP_ROOT\build\install\lib\lapack_blas_windows
and three FFT libs 
all this dlls and libs are provided here.

## Note

This method has been tested on VS2013 and VS2017 sucessfully.


