# -*- python -*-
# This file contains rules for Bazel; see drake/doc/bazel.rst.

load("@protobuf//:protobuf.bzl", "py_proto_library")
load("//tools:cpplint.bzl", "cpplint")
load("//tools:drake.bzl", "drake_cc_googletest", "drake_cc_library")
load("//tools:lcm.bzl", "lcm_cc_library")

py_proto_library(
    name = "named_vector",
    srcs = ["named_vector.proto"],
)

py_binary(
    name = "lcm_vector_gen",
    srcs = ["lcm_vector_gen.py"],
    data = [
        "//tools:clang-format",
    ],
    deps = [
        ":named_vector",
    ],
)

py_library(
    name = "formatter",
    srcs = ["formatter.py"],
    data = ["//tools:clang-format"],
)

py_binary(
    name = "clang-format-includes",
    srcs = ["clang_format_includes.py"],
    data = ["//tools:clang-format"],
    main = "clang_format_includes.py",
    deps = [":formatter"],
)

# === test/ ===

lcm_cc_library(
    name = "lcmt_sample",
    testonly = 1,
    includes = ["test/gen"],
    lcm_package = "drake",
    lcm_srcs = ["test/gen/lcmt_sample_t.lcm"],
)

drake_cc_library(
    name = "sample",
    testonly = 1,
    srcs = ["test/gen/sample.cc"],
    hdrs = ["test/gen/sample.h"],
    deps = [
        ":lcmt_sample",
        "//drake/systems/framework:vector",
    ],
)

drake_cc_library(
    name = "sample_translator",
    testonly = 1,
    srcs = ["test/gen/sample_translator.cc"],
    hdrs = ["test/gen/sample_translator.h"],
    deps = [
        ":lcmt_sample",
        ":sample",
        "//drake/systems/lcm:translator",
    ],
)

drake_cc_googletest(
    name = "sample_test",
    deps = [
        ":sample",
        "//drake/common:autodiff",
        "//drake/common:symbolic",
    ],
)

drake_cc_googletest(
    name = "sample_translator_test",
    deps = [
        ":sample_translator",
    ],
)

sh_test(
    name = "lcm_vector_gen_test",
    srcs = ["test/lcm_vector_gen_test.sh"],
    data = [
        "test/gen/lcmt_sample_t.lcm",
        "test/gen/sample.cc",
        "test/gen/sample.h",
        "test/gen/sample_translator.cc",
        "test/gen/sample_translator.h",
        "test/sample.named_vector",
        ":lcm_vector_gen",
    ],
)

py_test(
    name = "formatter_test",
    srcs = ["test/formatter_test.py"],
    deps = [":formatter"],
)

sh_test(
    name = "clang_format_includes_test",
    srcs = ["test/clang_format_includes_test.sh"],
    data = [
        ":clang-format-includes",
    ],
)

cpplint(data = ["test/gen/drake/CPPLINT.cfg"])
