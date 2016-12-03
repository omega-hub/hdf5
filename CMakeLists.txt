set(HDF5_BASE_DIR ${CMAKE_BINARY_DIR}/3rdparty/hdf5)

ExternalProject_Add(
    hdf5_build
    URL "http://www.hdfgroup.org/ftp/HDF5/releases/hdf5-1.8.14/src/hdf5-1.8.14.tar.gz"
    CMAKE_ARGS
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        #-DBUILD_SHARED_LIBS=ON
        -DHDF5_BUILD_HL_LIB=OFF
        -DCMAKE_INSTALL_PREFIX:PATH=${HDF5_BASE_DIR}/install
        
    # directories
    TMP_DIR ${CMAKE_BINARY_DIR}/3rdparty/tmp
    STAMP_DIR ${CMAKE_BINARY_DIR}/3rdparty/stamp
    DOWNLOAD_DIR ${HDF5_BASE_DIR}
    SOURCE_DIR ${HDF5_BASE_DIR}/source
    BINARY_DIR ${HDF5_BASE_DIR}/build
    INSTALL_DIR ${HDF5_BASE_DIR}/install
)
add_library(hdf5 STATIC IMPORTED GLOBAL)
add_dependencies(hdf5 hdf5_build)

# create the include directory path, or set_target_properties with
# INTERFACE_INCLUDE_DIRECTORIES will fail since this path is created at 
# target install time
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/3rdparty/hdf5/install/include)

set_target_properties(hdf5 PROPERTIES
    INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_BINARY_DIR}/3rdparty/hdf5/install/include)

if(WIN32)
    # hdf is prepended with lib even on windows
    set(HDF5_LIBNAME libhdf5${CMAKE_STATIC_LIBRARY_SUFFIX})
else()
    set(HDF5_LIBNAME ${CMAKE_STATIC_LIBRARY_PREFIX}hdf5${CMAKE_STATIC_LIBRARY_SUFFIX})
endif()

set_target_properties(hdf5 PROPERTIES
    IMPORTED_LOCATION 
        ${HDF5_BASE_DIR}/install/lib/${HDF5_LIBNAME}
)
