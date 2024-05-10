
include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR})
include_directories(${NV_SOURCE_DIR}/extern/poshlib)
include_directories(${NV_SOURCE_DIR}/extern/stb)

SUBDIRS(nvcore)
SUBDIRS(nvmath)
SUBDIRS(nvimage)
SUBDIRS(nvthread)
SUBDIRS(nvtt)
SUBDIRS(bc6h)
SUBDIRS(bc7)

# CUDA
find_package(CUDA)
if(CUDA_FOUND)
    if(MINGW)
        message(STATUS "Looking for CUDA - not supported on MinGW")
        unset(CUDA_FOUND)
    endif(MINGW)
    if(CUDA_FOUND)
        set(HAVE_CUDA ${CUDA_FOUND} CACHE BOOL "Set to TRUE if CUDA is found, FALSE otherwise")
        message(STATUS "Looking for CUDA - found")
    endif(CUDA_FOUND)
else(CUDA_FOUND)
    message(STATUS "Looking for CUDA - not found")
endif(CUDA_FOUND)

# OpenMP
# include(FindOpenMP)
# if(OPENMP_FOUND)
# 	set(HAVE_OPENMP ${OPENMP_FOUND} CACHE BOOL "Set to TRUE if OpenMP is found, FALSE otherwise")
# 	message(STATUS "Looking for OpenMP - found")
# 	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
# 	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
# else(OPENMP_FOUND)
# 	message(STATUS "Looking for OpenMP - not found")
# endif(OPENMP_FOUND)
	
# Threads
find_package(Threads REQUIRED)
message(STATUS "Use thread library: ${CMAKE_THREAD_LIBS_INIT}")
set(LIBS ${LIBS} ${CMAKE_THREAD_LIBS_INIT})

# configuration file
include(CheckIncludeFiles)

check_include_files("unistd.h" HAVE_UNISTD_H)
check_include_files("stdarg.h" HAVE_STDARG_H)
check_include_files("signal.h" HAVE_SIGNAL_H)
check_include_files("execinfo.h" HAVE_EXECINFO_H)
check_include_files("malloc.h" HAVE_MALLOC_H)
check_include_files("dispatch/dispatch.h" HAVE_DISPATCH_H)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/nvconfig.h.in ${CMAKE_CURRENT_BINARY_DIR}/nvconfig.h)

