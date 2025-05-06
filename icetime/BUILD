load("//:vars.bzl", "CHIPS", "ICE40_DEVICES")

genrule(
    name = "iceutil_renamed_find_chipdb",
    srcs = ["iceutil.cc"],
    outs = ["generated/iceutil_renamed_find_chipdb.cc"],
    cmd = "cat $(SRCS) | sed 's/find_chipdb/find_chipdb_renamed_to_be_replaced/g' > $@",
)

cc_binary(
    name = "icetime",
    srcs = [
        "icetime.cc",
        "generated/iceutil_renamed_find_chipdb.cc",
        "bazel_find_chipdb.cc",
    ] + ["//:generated/timings_%s.cc" % chip for chip in CHIPS],
    copts = [
        "-Wno-string-plus-int",
    ],
    data = ["//:chipdb-%s.txt" % device for device in ICE40_DEVICES],
    defines = [
        "PREFIX=\\\"unspecified\\\"",
        "CHIPDB_SUBDIR=\\\"unspecified\\\"",
    ],
    visibility = ["//visibility:public"],
    deps = [
        "@bazel_tools//tools/cpp/runfiles",
        "@abseil-cpp//absl/strings",
    ],
)

py_binary(
    name = "timings",
    srcs = ["timings.py"],
    python_version = "PY3",
    visibility = ["//visibility:public"],
)
