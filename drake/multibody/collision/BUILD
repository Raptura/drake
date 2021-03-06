# -*- python -*-
# This file contains rules for Bazel; see drake/doc/bazel.rst.

load("//tools:cpplint.bzl", "cpplint")
load("//tools:drake.bzl", "drake_cc_googletest", "drake_cc_library")

package(default_visibility = ["//visibility:public"])

drake_cc_library(
    name = "model",
    srcs = [
        "collision_filter.cc",
        "element.cc",
        "model.cc",
    ],
    hdrs = [
        "collision_filter.h",
        "element.h",
        "model.h",
        "point_pair.h",
    ],
    deps = [
        "//drake/common",
        "//drake/common:autodiff",
        "//drake/common:sorted_vectors_have_intersection",
        "//drake/multibody/shapes",
    ],
)

drake_cc_library(
    name = "collision_api",
    hdrs = ["drake_collision.h"],
    deps = [
        ":model",
    ],
)

drake_cc_library(
    name = "bullet_collision",
    srcs = [
        "bullet_model.cc",
        "bullet_model.h",
        "drake_collision.cc",
    ],
    defines = ["BULLET_COLLISION"],
    deps = [
        ":collision_api",
        "//drake/common:unused",
        "@bullet//:BulletCollision",
    ],
)

# TODO(jwnimmer-tri) Remove this once CMake is no longer using it (and thus we
# can delete unusable_model.*).  This cc_library only exists a sanity check to
# ensure the file still compiles.
drake_cc_library(
    name = "unusable_collision",
    srcs = [
        "drake_collision.cc",
        "unusable_model.cc",
        "unusable_model.h",
    ],
    visibility = ["//visibility:private"],
    deps = [
        ":collision_api",
    ],
)

# N.B. This is just the FCL model, not the drake_collision dispatcher.  (The
# other foo_collision libraries *do* contain the dispatcher.)
drake_cc_library(
    name = "fcl_collision",
    srcs = [
        "fcl_model.cc",
        "fcl_model.h",
    ],
    deps = [
        ":collision_api",
        "//drake/common:unused",
        "@fcl",
    ],
)

# By default, supply a bullet-only model.  As we gain more collision library
# choices, we may want to be more subtle here.
alias(
    name = "collision",
    actual = ":bullet_collision",
)

drake_cc_googletest(
    name = "model_test",
    data = [":test_models"],
    deps = [
        ":collision",
        "//drake/common:eigen_matrix_compare",
        "//drake/common:find_resource",
    ],
)

drake_cc_googletest(
    name = "fcl_model_test",
    deps = [
        ":fcl_collision",
    ],
)

drake_cc_googletest(
    name = "collision_filter_group_test",
    data = [":test_models"],
    deps = [
        ":collision",
        "//drake/common:find_resource",
        "//drake/multibody:rigid_body_tree",
        "//drake/multibody/joints",
        "//drake/multibody/parsers",
    ],
)

filegroup(
    name = "test_models",
    testonly = 1,
    srcs = glob([
        "test/**/*.obj",
        "test/**/*.sdf",
        "test/**/*.urdf",
        "test/**/*.xml",
    ]),
)

cpplint()
