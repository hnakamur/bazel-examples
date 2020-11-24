# Stage 4

In this stage we step it up and showcase how to download and use [google/googletest: Googletest - Google Testing and Mocking Framework](https://github.com/google/googletest).

Below, we see a similar configuration from Stage 3, except that this WORKSPACE contains `http_archive` for `googletest` and `test` directory is added.
Also an overloaded `get_greet` function is added to `hello-greet` and `visilibity` is added to `main/BUILD` for test code to use `get_greet` function.

Here is `WORKSPACE` file.

```
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "gtest",
    url = "https://github.com/google/googletest/archive/release-1.10.0.zip",
    sha256 = "94c634d499558a76fa649edb13721dce6e98fb1e7018dfaeba3cd7a083945e91",
    strip_prefix = "googletest-release-1.10.0",
)
```

And here is `test/BUILD` file.

```
cc_test(
    name = "hello-test",
    srcs = ["hello-test.cc"],
    copts = ["-Iexternal/gtest/include"],
    deps = [
        "@gtest//:gtest_main",
        "//main:hello-greet",
    ],
)
```

To build this example you use (notice that 3 slashes are required in windows)
```
bazel build //main:hello-world

# In Windows, note the three slashes

bazel build ///main:hello-world
```

To run test you use
```
bazel test test:hello-test
```
See [External dependencies - Bazel 3.7.0](https://docs.bazel.build/versions/3.7.0/external.html) for details.
