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
