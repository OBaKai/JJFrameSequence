cmake_minimum_required(VERSION 3.4.1)

#项目备忘 - 下面的源码都来自于版本 android10-c2f2-release
#framesequence - 源码地址 https://android.googlesource.com/platform/frameworks/ex/+/refs/heads/android10-c2f2-release
#gif -源码地址 https://android.googlesource.com/platform/external/giflib/+/refs/heads/android10-c2f2-release
#webp -源码地址 https://android.googlesource.com/platform/external/webp/+/refs/heads/android10-c2f2-release

# 参考文章
# Android播放webp和gif 上 https://www.jianshu.com/p/67b72fd814f6
# Android播放webp和gif 下 https://www.jianshu.com/p/9afba23905ae
# Android使用giflib库高效加载gif图片 https://www.jianshu.com/p/70a1b8cd833d
# https://github.com/McoyJiang/Android-WebP

project("frame-sequence")

file(GLOB_RECURSE GIF_LIB ${CMAKE_SOURCE_DIR}/src/main/cpp/gif/*.c)
add_library(native-gif
            STATIC
            ${GIF_LIB}
)
#引入gif静态库头文件目录
include_directories(src/main/cpp/gif)

file(GLOB_RECURSE WEBP_DEC_LIB ${CMAKE_SOURCE_DIR}/src/main/cpp/webp/dec/*.c)
file(GLOB_RECURSE WEBP_DSP_LIB ${CMAKE_SOURCE_DIR}/src/main/cpp/webp/dsp/*.c)
file(GLOB_RECURSE WEBP_UTILS_LIB ${CMAKE_SOURCE_DIR}/src/main/cpp/webp/utils/*.c)
add_library(native-webp
            STATIC

            ${WEBP_DEC_LIB}
            ${WEBP_DSP_LIB}
            ${WEBP_UTILS_LIB}
            src/main/cpp/webp/demux/demux.c
)
#引入webp静态库头文件目录
include_directories(src/main/cpp/webp/webp)

file(GLOB_RECURSE FRAMESEQUENCE_LIB ${CMAKE_SOURCE_DIR}/src/main/cpp/framesequence/*.cpp)
add_library(frame-sequence
             SHARED

             ${FRAMESEQUENCE_LIB}
)

find_library(log-lib
             log
)

target_link_libraries(frame-sequence
                        native-gif
                        native-webp

                        android
                        jnigraphics
                        ${log-lib}
)