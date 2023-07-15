cmake_minimum_required(VERSION 3.3)
MESSAGE("[NvCloth]cmake/mac/CMakeList.txt")
include(../common/CMakeLists.txt)


IF(NOT DEFINED TARGET_BUILD_PLATFORM) # Not defined, default to mac
	SET(TARGET_BUILD_PLATFORM "mac")
ENDIF()

SET(PLATFORM_LIST mac)

IF (NOT ${TARGET_BUILD_PLATFORM} IN_LIST PLATFORM_LIST)
	MESSAGE(FATAL_ERROR "Invalid platform:" ${TARGET_BUILD_PLATFORM})
ENDIF()

IF(CMAKE_OSX_ARCHITECTURES STREQUAL "arm64")
    SET(CMAKE_CXX_FLAGS "-arch arm64")
ELSEIF(DEFINED PX_32BIT)
	SET(CMAKE_CXX_FLAGS "-arch i386 -msse2")
ELSEIF()
	SET(CMAKE_CXX_FLAGS "-arch x86_64 -msse2")
ENDIF()

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -ffast-math -fno-rtti -fno-exceptions -ffunction-sections -fdata-sections -Werror -ferror-limit=0 -Wall -Wextra -fstrict-aliasing -Wstrict-aliasing=2 -Weverything \
-Wno-alloca \
-Wno-atomic-implicit-seq-cst \
-Wno-c++98-compat-pedantic \
-Wno-cast-align \
-Wno-cast-qual \
-Wno-conversion \
-Wno-covered-switch-default \
-Wno-deprecated \
-Wno-disabled-macro-expansion \
-Wno-documentation-deprecated-sync \
-Wno-documentation-unknown-command \
-Wno-exit-time-destructors \
-Wno-error=undefined-func-template \
-Wno-extra-semi-stmt \
-Wno-float-equal \
-Wno-format-nonliteral \
-Wno-global-constructors \
-Wno-gnu-anonymous-struct \
-Wno-implicit-fallthrough \
-Wno-inconsistent-missing-destructor-override \
-Wno-inconsistent-missing-override \
-Wno-invalid-noreturn \
-Wno-missing-noreturn \
-Wno-missing-prototypes \
-Wno-missing-variable-declarations \
-Wno-nested-anon-types \
-Wno-newline-eof \
-Wno-non-virtual-dtor \
-Wno-old-style-cast \
-Wno-padded \
-Wno-reserved-id-macro \
-Wno-return-type-c-linkage \
-Wno-shadow \
-Wno-shift-sign-overflow \
-Wno-suggest-destructor-override \
-Wno-suggest-override \
-Wno-switch-enum \
-Wno-undef \
-Wno-undefined-func-template \
-Wno-undefined-reinterpret-cast \
-Wno-unknown-pragmas \
-Wno-unknown-warning-option \
-Wno-unreachable-code \
-Wno-unused-function \
-Wno-unused-local-typedef \
-Wno-unused-macros \
-Wno-unused-member-function \
-Wno-unused-private-field \
-Wno-used-but-marked-unused \
-Wno-weak-template-vtables \
-Wno-weak-vtables \
-Wno-zero-as-null-pointer-constant \
-Wno-comma \
")

SET(CMAKE_SHARED_LINKER_FLAGS "")

# Build debug info for all configurations
SET(CMAKE_CXX_FLAGS_DEBUG "-O0 -g")
SET(CMAKE_CXX_FLAGS_CHECKED "-O3 -g")
SET(CMAKE_CXX_FLAGS_PROFILE "-O3")
SET(CMAKE_CXX_FLAGS_RELEASE "-O3")

# Build libs compatible with OS X 10.15
SET(CMAKE_OSX_DEPLOYMENT_TARGET "10.15")

#set(CMAKE_XCODE_ATTRIBUTE_DEBUG_INFORMATION_FORMAT "dwarf-with-dsym")

# Controls PX_NVTX for all projects on mac
SET(PHYSX_MAC_ENABLE_NVTX 0)

# Disable cuda and dx for all projects on mac
SET(PHYSX_MAC_COMPILE_DEFS DISABLE_COMPUTE_PHYSX)
SET(PHYSX_MAC_DEBUG_COMPILE_DEFS _DEBUG;PX_DEBUG=1;PX_CHECKED=1;PX_NVTX=${PHYSX_MAC_ENABLE_NVTX};PX_SUPPORT_PVD=0)
SET(PHYSX_MAC_CHECKED_COMPILE_DEFS NDEBUG;PX_CHECKED=1;PX_NVTX=${PHYSX_MAC_ENABLE_NVTX};PX_SUPPORT_PVD=0)
SET(PHYSX_MAC_PROFILE_COMPILE_DEFS NDEBUG;PX_PROFILE=1;PX_NVTX=${PHYSX_MAC_ENABLE_NVTX};PX_SUPPORT_PVD=0)
SET(PHYSX_MAC_RELEASE_COMPILE_DEFS NDEBUG;PX_SUPPORT_PVD=0)

SET(PX_SELECT_COMPONENTS PxFoundation)
FIND_PACKAGE(PxShared REQUIRED)

# Include all of the projects
INCLUDE(NvCloth.cmake)


