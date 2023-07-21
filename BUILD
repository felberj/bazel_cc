load("@bazel_gazelle//:def.bzl", "gazelle", "gazelle_binary")
load("@io_bazel_rules_go//go:def.bzl", "go_library", "go_test")

# gazelle:exclude testdata

go_library(
    name = "cc",
    srcs = ["gazelle.go"],
    importpath = "github.com/felberj/cc_gazelle",
    visibility = ["//visibility:public"],
    deps = [
        "@bazel_gazelle//config:go_default_library",
        "@bazel_gazelle//label:go_default_library",
        "@bazel_gazelle//language:go_default_library",
        "@bazel_gazelle//pathtools:go_default_library",
        "@bazel_gazelle//repo:go_default_library",
        "@bazel_gazelle//resolve:go_default_library",
        "@bazel_gazelle//rule:go_default_library",
        "@com_github_bazelbuild_buildtools//build:go_default_library",
    ],
)

go_test(
    name = "cc_test",
    srcs = ["gazelle_test.go"],
    data = [
        ":gazelle-cclib",
    ] + glob(
        [
            "testdata/**",            
        ],
        allow_empty = True,
    ),
    embed = [":cc"],
    deps = [
        "@bazel_gazelle//testtools:go_default_library",
        "@io_bazel_rules_go//go/tools/bazel:go_default_library",
    ],
)

# This gazelle binary is used exclusively for testing the gazelle language
# extension and thus only has the cclib language installed.
gazelle_binary(
    name = "gazelle-cclib",
    languages = [":cc"],
    visibility = ["//visibility:public"],
)

gazelle(
    name = "gazelle",
    gazelle = ":gazelle-cclib",
)

# The files needed for distribution
# A fake testdata directory is created so that
# the build file has nothing missing, but we
# do not bloat the distribution tarball
filegroup(
    name = "distribution",
    srcs = glob(["*.go"]) + [
        "BUILD",
        ":fake-testdata",
    ],
    visibility = ["//visibility:public"],
)

genrule(
    name = "fake-testdata",
    outs = ["testdata"],
    cmd = "touch $@",
)
