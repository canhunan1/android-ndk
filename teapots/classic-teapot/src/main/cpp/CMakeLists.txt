#
# Copyright (C) The Android Open Source Project
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

cmake_minimum_required(VERSION 3.4.1)

# build native_app_glue as a static lib
add_library(app-glue STATIC
            ${ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -Wall -fno-exceptions -fno-rtti")

# build the ndk-helper library
set(ndk_helper_dir ../../../../common/ndk_helper)
add_subdirectory(${ndk_helper_dir} ndk_helper)

# now build app's shared lib
add_library(TeapotNativeActivity SHARED
            TeapotNativeActivity.cpp
            TeapotRenderer.cpp)

target_include_directories(TeapotNativeActivity PRIVATE
                           ${ANDROID_NDK}/sources/android/cpufeatures
                           ${ANDROID_NDK}/sources/android/native_app_glue
                           ${ndk_helper_dir})

# add lib dependencies
target_link_libraries(TeapotNativeActivity
                      android
                      app-glue
                      atomic
                      EGL
                      GLESv2
                      log
                      ndk-helper)
