# Copyright 2020 Google LLC
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

# BUILD file for Project Icestorm.
#
# iceprog is not included in this BUILD file because it depends on libftdi which
# is not available at this time.

load("@rules_python//python:defs.bzl", "py_binary")
load("@rules_cc//cc:defs.bzl", "cc_binary")
load("//:vars.bzl", "CHIPS", "ICE40_DEVICES")

licenses(["notice"])

exports_files([
    "LICENSE",
    "icefuzz/timings_hx1k.txt",
    "icefuzz/timings_hx8k.txt",
    "icefuzz/timings_lp1k.txt",
    "icefuzz/timings_lp8k.txt",
    "icefuzz/timings_lp384.txt",
    "icefuzz/timings_u4k.txt",
    "icefuzz/timings_up5k.txt",
])

py_binary(
    name = "icebox_chipdb",
    srcs = [
        "icebox/icebox.py",
        "icebox/icebox_chipdb.py",
        "icebox/iceboxdb.py",
    ],
    imports = [
        "icebox",
    ],
    python_version = "PY3",
)

[genrule(
    name = "chipdb_%s_txt" % device,
    srcs = [],
    outs = ["chipdb-%s.txt" % device],
    cmd = "$(location :icebox_chipdb) %s > $@" % ICE40_DEVICES[device],
    tools = [":icebox_chipdb"],
    visibility = ["//visibility:public"],
) for device in ICE40_DEVICES.keys()]

cc_binary(
    name = "icebram",
    srcs = ["icebram/icebram.cc"],
    visibility = ["//visibility:public"],
)

cc_binary(
    name = "icemulti",
    srcs = ["icemulti/icemulti.cc"],
    copts = [
        "-Wno-implicit-fallthrough",
    ],
    visibility = ["//visibility:public"],
)

cc_binary(
    name = "icepack",
    srcs = ["icepack/icepack.cc"],
    visibility = ["//visibility:public"],
)

cc_binary(
    name = "icepll",
    srcs = ["icepll/icepll.cc"],
    visibility = ["//visibility:public"],
)

[genrule(
    name = "timings_%s_cc" % chip,
    srcs = ["icefuzz/timings_%s.txt" % chip],
    outs = ["generated/timings_%s.cc" % chip],
    cmd = "(dir=$$(pwd) && cd $$(dirname $(location :icefuzz/timings_%s.txt)) && $$dir/$(location //icetime:timings) %s) > $@" % (chip, chip),
    tools = ["//icetime:timings"],
    visibility = ["//visibility:public"],
) for chip in CHIPS]
