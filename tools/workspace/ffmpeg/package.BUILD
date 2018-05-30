# -*- python -*-

# Copyright 2018 Josh Pieper, jjp@pobox.com.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load("@com_github_mjbots_bazel_deps//tools/workspace:template_file.bzl",
     "template_file")
load("@com_github_mjbots_bazel_deps//tools/workspace:generate_file.bzl",
     "generate_file")
load("@com_github_mjbots_bazel_deps//tools/workspace/ffmpeg:config.bzl",
     "flatten_config")
load("@com_github_mjbots_bazel_deps//tools/workspace/nasm:nasm.bzl",
     "nasm_objects")

package(default_visibility = ["//visibility:public"])

cc_library(
    name = "ffmpeg",
    deps = [
        ":swscale",
        ":avresample",
        ":avformat",
        ":avfilter",
        ":avdevice",
        ":avcodec",
    ],
    srcs = select({
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : [
            ":x86_nasm_objects",
        ],
        "//conditions:default" : [],
    }),
)

CONFIG_HEADERS = [
    "private/avversion.h",
    "private/config.h",
]

COMMON_COPTS = [
    "-I$(GENDIR)/external/ffmpeg/private",
    "-Iexternal/ffmpeg/private",
    "-DHAVE_AV_CONFIG_H",

    "-Wno-deprecated-declarations",
    "-Wno-unused-function",
] + select({
    "@com_github_mjbots_bazel_deps//conditions:gcc" : [
        "-Wno-discarded-qualifiers",
    ],
    "@com_github_mjbots_bazel_deps//conditions:clang" : [
        "-Wno-shift-negative-value",
        "-Wno-incompatible-pointer-types-discards-qualifiers",
        "-Wno-constant-conversion",
        "-Wno-unused-const-variable",
        "-Wno-#warnings",
    ],
    "//conditions:default" : [],
})

cc_library(
    name = "avutil",
    hdrs = @AVUTIL_HEADERS@ + ["libavutil/" + x for x in [
        "ffversion.h",
        "colorspace.h",
        "internal.h",

        "aarch64/cpu.h",
        "ppc/util_altivec.h",
        "ppc/cpu.h",
        "x86/asm.h",
        "x86/cpu.h",
        "x86/pixelutils.h",

        "lzo.h",

        "avconfig.h",
    ]] + [
        # Yay, fix up some circular header dependencies.
        "libavfilter/bufferqueue.h",
        "libavfilter/avfilter.h",
        "libavfilter/version.h",
        "libavfilter/window_func.h",
        "libavcodec/mathops.h",
        "libavcodec/x86/mathops.h",
        "libavcodec/arm/mathops.h",
        "compat/va_copy.h",
        "compat/nvenc/nvEncodeAPI.h",
        "compat/cuda/dynlink_loader.h",
        "compat/cuda/dynlink_cuda.h",
        "compat/cuda/dynlink_nvcuvid.h",
        "compat/cuda/dynlink_cuviddec.h",
        "compat/w32dlfcn.h",
    ],
    srcs = @AVUTIL_SOURCES@ + glob(["libavutil/**/*.h"]) + CONFIG_HEADERS +select({
        "@com_github_mjbots_bazel_deps//conditions:arm" : @AVUTIL_ARM_SOURCES@,
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : @AVUTIL_X86_SOURCES@,
    }),
    textual_hdrs = ["libavutil/" + x for x in [
        "log2_tab.c",

        "arm/asm.S",
    ]],
    copts = COMMON_COPTS + [
        "-Wno-pointer-sign",
        "-Wno-incompatible-pointer-types",
        "-Wno-parentheses",
        "-Wno-switch",
    ],
    includes = ["."],
)

generate_file(
    name = "libavutil/avconfig.h",
    content = """
/* Generated by ffconf */
#ifndef AVUTIL_AVCONFIG_H
#define AVUTIL_AVCONFIG_H
#define AV_HAVE_BIGENDIAN 0
#define AV_HAVE_FAST_UNALIGNED 0
#endif /* AVUTIL_AVCONFIG_H */
    """,
)

cc_library(
    name = "swscale",
    linkstatic = 1,
    hdrs = @SWSCALE_HEADERS@,
    srcs = @SWSCALE_SOURCES@ + glob(["libswscale/**/*.h"]) + CONFIG_HEADERS + select({
        "@com_github_mjbots_bazel_deps//conditions:arm" : @SWSCALE_ARM_SOURCES@,
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : @SWSCALE_X86_SOURCES@,
    }),
    textual_hdrs = ["libswscale/" + x for x in [
        "rgb2rgb_template.c",
        "bayer_template.c",
        "x86/rgb2rgb_template.c",
        "arm/rgb2yuv_neon_common.S",
    ]],
    copts = COMMON_COPTS + [
        "-Wno-switch",
        "-Wno-unused-function",
        "-Wno-incompatible-pointer-types",
        "-Wno-parentheses",
    ],
    deps = [
        ":avutil",
    ],
)

cc_library(
    name = "avresample",
    hdrs = @AVRESAMPLE_HEADERS@,
    srcs = @AVRESAMPLE_SOURCES@ + glob(["libavresample/**/*.h"]) + CONFIG_HEADERS + select({
        "@com_github_mjbots_bazel_deps//conditions:arm" : @AVRESAMPLE_ARM_SOURCES@,
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : @AVRESAMPLE_X86_SOURCES@,
    }),
    textual_hdrs = ["libavresample/" + x for x in [
        "resample_template.c",
    ]],
    copts = COMMON_COPTS + [
        "-Wno-pointer-sign",
        "-Wno-switch",
    ],
    deps = [
        ":avutil",
    ],
)

cc_library(
    name = "swresample",
    hdrs = @SWRESAMPLE_HEADERS@,
    srcs = @SWRESAMPLE_SOURCES@ + glob(["libswresample/**/*.h"]) + CONFIG_HEADERS + select({
        "@com_github_mjbots_bazel_deps//conditions:arm" : @SWRESAMPLE_ARM_SOURCES@,
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : @SWRESAMPLE_X86_SOURCES@,
    }),
    textual_hdrs = ["libswresample/" + x for x in [
        "noise_shaping_data.c",
        "resample_template.c",
        "dither_template.c",
        "rematrix_template.c",
    ]],
    copts = COMMON_COPTS + [
        "-Wno-pointer-sign",
        "-Wno-switch",
    ],
    deps = [
        ":avutil",
    ],
)

cc_library(
    name = "avcodec",
    linkstatic = 1,
    hdrs = @AVCODEC_HEADERS@ + @AVFORMAT_HEADERS@ + [
        "libavformat/apetag.h",
        "libavformat/id3v1.h",
        "libavcodec/mips/lsp_mips.h",
        "libavcodec/mips/amrwbdec_mips.h",
   ],
    srcs = @AVCODEC_SOURCES@ + ["libavcodec/" + x for x in [
        # HAVE_THREADS
        "pthread.c",
        "pthread_slice.c",
        "pthread_frame.c",
    ]] + glob(["libavcodec/**/*.h"]) + select({
        "@com_github_mjbots_bazel_deps//conditions:arm" : @AVCODEC_ARM_SOURCES@,
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : @AVCODEC_X86_SOURCES@,
    }),
    textual_hdrs = ["libavcodec/" + x for x in [
        "aacps.c",
        "ac3dec.c",
        "aacpsdata.c",
        "eac3dec.c",
        "codec_list.c",
        "parser_list.c",
        "x86/h264_cabac.c",
        "x86/rnd_template.c",
        "x86/vp9dsp_init_16bpp_template.c",
        "x86/hpeldsp_rnd_template.c",
        "x86/mpegvideoenc_qns_template.c",
        "arm/vp9dsp_init_16bpp_arm_template.c",
        "arm/neon.S",
    ]] + [
        ":libavcodec/bsf_list.c",
    ] + glob(["libavcodec/*_template.c"]),
    copts = COMMON_COPTS + [
        "-Wno-pointer-sign",
        "-Wno-parentheses",
        "-Wno-switch",
        "-Wno-incompatible-pointer-types",
        "-Iexternal/ffmpeg/libavcodec/",
    ],
    deps = [":avutil", ":avresample", ":swresample"],
)

cc_library(
    name = "avformat",
    hdrs = @AVFORMAT_HEADERS@,
    srcs = @AVFORMAT_SOURCES@ + glob(["libavformat/**/*.h"]) + select({
        "@com_github_mjbots_bazel_deps//conditions:arm" : @AVFORMAT_ARM_SOURCES@,
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : @AVFORMAT_X86_SOURCES@,
    }),
    textual_hdrs = [
        "libavformat/protocol_list.c",
        "libavformat/muxer_list.c",
        "libavformat/demuxer_list.c",
    ],
    copts = COMMON_COPTS + [
        "-Wno-parentheses",
        "-Wno-pointer-sign",
        "-Wno-switch",
        "-Iexternal/ffmpeg/libavcodec",
    ],
    deps = ["@bzip2", ":avutil", ":avcodec"],
)

cc_library(
    name = "avfilter",
    linkstatic = 1,
    hdrs = @AVFILTER_HEADERS@,
    srcs = @AVFILTER_SOURCES@ + ["libavfilter/" + x for x in [
        "pthread.c",
    ]] + glob(["libavfilter/**/*.h"]) + select({
        "@com_github_mjbots_bazel_deps//conditions:arm" : @AVFILTER_ARM_SOURCES@,
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : @AVFILTER_X86_SOURCES@,
    }),
    textual_hdrs = ["libavfilter/" + x for x in [
        "colorspacedsp_template.c",
        "colorspacedsp_yuv2yuv_template.c",
        "filter_list.c",
        "all_channel_layouts.inc",
    ]],
    copts = COMMON_COPTS + [
        "-Wno-switch",
        "-Wno-deprecated-declarations",
        "-Wno-incompatible-pointer-types",
        "-Wno-parentheses",
        "-Wno-pointer-sign",
        "-Iexternal/ffmpeg/libavfilter",
    ],
    deps = [":avutil", ":avresample", ":avcodec", ":avformat", ":swscale"],
)

cc_library(
    name = "avdevice",
    hdrs = @AVDEVICE_HEADERS@,
    srcs = @AVDEVICE_SOURCES@ + glob(["libavdevice/**/*.h"]),
    textual_hdrs = ["libavdevice/" + x for x in [
        "outdev_list.c",
        "indev_list.c",
    ]],
    copts = COMMON_COPTS + [
        "-Wno-pointer-sign",
    ],
    deps = [":avutil", ":avcodec", ":avformat", ":avfilter"],
)

nasm_objects(
    name = "x86_nasm_objects",
    srcs = select({
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : @ASM_FILES@,
        "//conditions:default" : [],
    }),
    textual_hdrs = [
        "libavresample/x86/util.asm",
        "libavutil/x86/x86util.asm",
        "libavutil/x86/x86inc.asm",
        "libavcodec/x86/vp9itxfm_template.asm",
        "libavcodec/x86/simple_idct10_template.asm",
        "libavcodec/x86/vp9itxfm_16bpp.asm",
    ],
    force_includes = [
        "private/config.asm",
    ],
    extra_args = [
        "-f", "elf64",
        "-g",
        "-F", "dwarf",
        "-Iexternal/ffmpeg/",
        "-Iexternal/ffmpeg/libavresample/x86/",
        "-Iexternal/ffmpeg/libavutil/x86/",
        "-Iexternal/ffmpeg/libavcodec/x86/",
    ],
)

generate_file(
    name = "private/avversion.h",
    content = """
#define LIBAV_VERSION "12.3"
    """,
)

template_file(
    name = "private/config.asm",
    src = "private/config.h",
    substitutions = {
        "#define" : "%define",
        "#endif" : "",
        "#ifndef FFMPEG_CONFIG_H" : "",
        "#define FFMPEG_CONFIG_H" : "",
    },
)

X86_OPTS = [
    "MMX",
    "SSE",
    "SSE2",
    "SSE3",
    "SSE4",
    "SSE42",
    "AVX",
    "AVX2",
]

ARM_OPTS = [
    "NEON",
    "ARMV6",
    "ARMV6T2",
]

template_file(
    name = "private/config.h",
    src = "private/config.h.in",
    substitution_list = select({
        "@com_github_mjbots_bazel_deps//conditions:arm" : ([
            "#define ARCH_ARM 0=#define ARCH_ARM 1",
        ] + [
            "#define HAVE_{x} 0=#define HAVE_{x} 1".format(x=x)
            for x in ARM_OPTS
        ] + [
            "#define HAVE_{x}_EXTERNAL 0=#define HAVE_{x}_EXTERNAL 1".format(x=x)
            for x in ARM_OPTS
        ]),
        "@com_github_mjbots_bazel_deps//conditions:x86_64" : ([
            "#define ARCH_X86 0=#define ARCH_X86 1",
            "#define ARCH_X86_64 0=#define ARCH_X86_64 1",
            "#define HAVE_X86ASM 0=#define HAVE_X86ASM 1",
        ] + [
            "#define HAVE_{x} 0=#define HAVE_{x} 1".format(x=x)
            for x in X86_OPTS
        ] + [
            "#define HAVE_{x}_EXTERNAL 0=#define HAVE_{x}_EXTERNAL 1".format(x=x)
            for x in X86_OPTS
        ]),
    }),
)


generate_file(
    name = "libavutil/ffversion.h",
    content = """
/* Automatically generated by version.sh, do not manually edit! */
#ifndef AVUTIL_FFVERSION_H
#define AVUTIL_FFVERSION_H
#define FFMPEG_VERSION "3.4.2"
#endif /* AVUTIL_FFVERSION_H */
""")
