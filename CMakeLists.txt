set(HDF5_BASE_DIR ${CMAKE_BINARY_DIR}/3rdparty/hdf5)

ExternalProject_Add(
    hdf5
    URL "http://www.hdfgroup.org/ftp/HDF5/releases/hdf5-1.8.14/src/hdf5-1.8.14.tar.gz"
    CMAKE_ARGS
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        -DBUILD_SHARED_LIBS=ON
        -DHDF5_BUILD_HL_LIB=ON
        -DCMAKE_INSTALL_PREFIX:PATH=${HDF5_BASE_DIR}/install
        
    # directories
    TMP_DIR ${CMAKE_BINARY_DIR}/3rdparty/tmp
    STAMP_DIR ${CMAKE_BINARY_DIR}/3rdparty/stamp
    DOWNLOAD_DIR ${HDF5_BASE_DIR}
    SOURCE_DIR ${HDF5_BASE_DIR}/source
    BINARY_DIR ${HDF5_BASE_DIR}/build
    INSTALL_DIR ${HDF5_BASE_DIR}/install
)

ExternalProject_Get_Property(hdf5 SOURCE_DIR)

set(ENV{HDF5_ROOT} ${HDF5_BASE_DIR}/install)
set(ENV{CPPFLAGS} -I${HDF5_BASE_DIR}/install/include)
set(ENV{LDFLAGS} -L${HDF5_BASE_DIR}/install/lib)
set(ENV{LD_LIBRARY_PATH} ${HDF5_BASE_DIR}/install/lib)

copy_shared_libs(hdf5 "hdf5;hdf5_hl" ${HDF5_BASE_DIR}/install/bin)