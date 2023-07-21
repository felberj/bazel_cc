# CC extension for Gazelle

This package can be used to generate BUILD files for c++.

At its core, it is using a regex to extract the imports of a cc file. #yolo
	
## External reop hack

To allow cc files to import external repositories, it relies on the fact that bazel puts the external repositories under `external`.
This feature is still under development and it has some PRs to gazelle:

- https://github.com/bazelbuild/bazel-gazelle/pull/1575
- https://github.com/bazelbuild/bazel-gazelle/pull/1573

With these PRs merged, we can analyze everything under external and transform imports from

`//external/my_external_repo/path/to:rule`

to

`@my_external_repo//path/to:rule`.

## Setup

```
## WORKSPACE
# begin gazelle

felberj_gazelle_hash = "83d7b8cfde9bf7334759243157f8f0c0b2f7fe3c"
http_archive(
    name = "bazel_gazelle",
    # cpp_hack
    urls = [
        "https://github.com/felberj/bazel-gazelle/archive/" + felberj_gazelle_hash + ".zip"
    ],
    strip_prefix = "bazel-gazelle-" + felberj_gazelle_hash,
)

cc_gazelle_hash = "dd48899"
http_archive(
    name = "cc_gazelle",
    urls = [
        "https://github.com/felberj/cc_gazelle/archive/" + cc_gazelle_hash + ".zip"
    ],
    strip_prefix = "cc_gazelle-" + cc_gazelle_hash,
)

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "6dc2da7ab4cf5d7bfc7c949776b1b7c733f05e56edc4bcd9022bb249d2e2a996",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.39.1/rules_go-v0.39.1.zip",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.39.1/rules_go-v0.39.1.zip",
    ],
)


load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")
load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies", "go_repository")

go_rules_dependencies()

go_register_toolchains(version = "1.20.5")

gazelle_dependencies()

# end gazelle
```


```
# BUILD

load("@bazel_gazelle//:def.bzl", "gazelle")

gazelle(
    name = "gazelle",
    args = [
        "--cc_test_dependency=@googletest//:gtest_main",
    ],
    gazelle = "@cc_gazelle//:gazelle-cclib",
)
```

To have the external resolving work add a symlink "external":

```
ln -s bazel-out/../../../external external
```
