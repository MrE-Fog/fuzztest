# Copyright 2022 Google LLC
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

# FuzzTest: a coverage-guided fuzzing / property-based testing framework.

load("@rules_proto//proto:defs.bzl", "proto_library")

package(default_visibility = ["//visibility:public"])

licenses(["notice"])

exports_files(["LICENSE"])

cc_library(
    name = "absl_domain",
    hdrs = ["internal/absl_domain.h"],
    deps = [
        ":logging",
        "@com_google_absl//absl/time",
    ],
)

cc_library(
    name = "any",
    hdrs = ["internal/any.h"],
    deps = [
        ":logging",
        ":meta",
    ],
)

cc_test(
    name = "any_test",
    srcs = ["internal/any_test.cc"],
    deps = [
        ":any",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "compatibility_mode",
    srcs = ["internal/compatibility_mode.cc"],
    hdrs = ["internal/compatibility_mode.h"],
    deps = [
        ":fixture_driver",
        ":logging",
        ":runtime",
        "@com_google_absl//absl/random:distributions",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/time",
    ],
)

cc_library(
    name = "coverage",
    srcs = ["internal/coverage.cc"],
    hdrs = ["internal/coverage.h"],
    deps = [
        ":logging",
        ":table_of_recent_compares",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/types:span",
    ],
)

cc_library(
    name = "table_of_recent_compares",
    hdrs = ["internal/table_of_recent_compares.h"],
    deps = [
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/random:bit_gen_ref",
        "@com_google_absl//absl/random:distributions",
    ],
)

cc_test(
    name = "table_of_recent_compares_test",
    srcs = ["internal/table_of_recent_compares_test.cc"],
    deps = [
        ":domain",
        ":table_of_recent_compares",
        "@com_google_absl//absl/random",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "domain",
    srcs = ["internal/grammar.cc"],
    hdrs = [
        "domain.h",
        "internal/domain.h",
        "internal/grammar.h",
        "internal/protobuf_domain.h",
    ],
    deps = [
        ":absl_domain",
        ":any",
        ":coverage",
        ":logging",
        ":meta",
        ":regexp",
        ":serialization",
        ":table_of_recent_compares",
        ":type_support",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/numeric:bits",
        "@com_google_absl//absl/numeric:int128",
        "@com_google_absl//absl/random",
        "@com_google_absl//absl/random:bit_gen_ref",
        "@com_google_absl//absl/random:distributions",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:span",
    ],
)

cc_library(
    name = "fixture_driver",
    srcs = ["internal/fixture_driver.cc"],
    hdrs = ["internal/fixture_driver.h"],
    deps = [
        ":logging",
        ":registration",
        ":type_support",
    ],
)

cc_test(
    name = "fixture_driver_test",
    size = "small",
    srcs = ["internal/fixture_driver_test.cc"],
    deps = [
        ":domain",
        ":fixture_driver",
        ":registration",
        ":type_support",
        "@com_google_absl//absl/types:span",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "fuzztest",
    srcs = ["fuzztest.cc"],
    hdrs = ["fuzztest.h"],
    deps = [
        ":domain",
        ":registry",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
    ],
)

cc_library(
    name = "fuzztest_gtest_main",
    testonly = 1,
    srcs = ["fuzztest_gtest_main.cc"],
    deps = [
        ":fuzztest",
        ":googletest_adaptor",
        ":runtime",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/flags:parse",
        "@com_google_absl//absl/time",
        "@com_google_googletest//:gtest",
    ],
)

cc_library(
    name = "googletest_adaptor",
    testonly = True,
    hdrs = ["googletest_adaptor.h"],
    deps = [
        ":registry",
        ":runtime",
        "@com_google_googletest//:gtest",
    ],
)

cc_library(
    name = "googletest_fixture_adapter",
    testonly = True,
    hdrs = ["googletest_fixture_adapter.h"],
    deps = [
        ":fixture_driver",
        "@com_google_googletest//:gtest",
    ],
)

cc_library(
    name = "io",
    srcs = ["internal/io.cc"],
    hdrs = ["internal/io.h"],
    deps = [
        ":logging",
        "@com_google_absl//absl/hash",
        "@com_google_absl//absl/strings:str_format",
    ],
)

cc_test(
    name = "io_test",
    srcs = ["internal/io_test.cc"],
    deps = [
        ":fuzztest",
        ":io",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "logging",
    srcs = ["internal/logging.cc"],
    hdrs = ["internal/logging.h"],
    deps = [
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "meta",
    hdrs = ["internal/meta.h"],
    deps = ["@com_google_absl//absl/numeric:int128"],
)

cc_library(
    name = "regexp",
    srcs = ["internal/regexp.cc"],
    hdrs = ["internal/regexp.h"],
    deps = [
        ":logging",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/random:bit_gen_ref",
        "@com_google_absl//absl/random:distributions",
        "@com_googlesource_code_re2//:re2",
    ],
)

cc_library(
    name = "registration",
    hdrs = ["internal/registration.h"],
    deps = [
        ":domain",
        ":meta",
        ":type_support",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:span",
    ],
)

cc_library(
    name = "registry",
    srcs = ["internal/registry.cc"],
    hdrs = ["internal/registry.h"],
    deps = [
        ":compatibility_mode",
        ":fixture_driver",
        ":registration",
        ":runtime",
        "@com_google_absl//absl/container:flat_hash_map",
    ],
)

cc_library(
    name = "runtime",
    srcs = ["internal/runtime.cc"],
    hdrs = ["internal/runtime.h"],
    deps = [
        ":coverage",
        ":domain",
        ":fixture_driver",
        ":io",
        ":logging",
        ":meta",
        ":registration",
        ":seed_seq",
        ":serialization",
        ":type_support",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/functional:function_ref",
        "@com_google_absl//absl/random",
        "@com_google_absl//absl/random:bit_gen_ref",
        "@com_google_absl//absl/random:distributions",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:span",
    ],
)

cc_test(
    name = "runtime_test",
    srcs = ["internal/runtime_test.cc"],
    deps = [
        ":domain",
        ":runtime",
        ":test_protobuf_cc_proto",
        "@com_google_absl//absl/time",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "seed_seq",
    srcs = ["internal/seed_seq.cc"],
    hdrs = ["internal/seed_seq.h"],
    deps = [
        ":logging",
        "@com_google_absl//absl/random",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
    ],
)

cc_test(
    name = "seed_seq_test",
    srcs = ["internal/seed_seq_test.cc"],
    deps = [
        ":seed_seq",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "serialization",
    srcs = ["internal/serialization.cc"],
    hdrs = ["internal/serialization.h"],
    deps = [
        ":meta",
        "@com_google_absl//absl/numeric:int128",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:span",
    ],
)

cc_test(
    name = "serialization_test",
    srcs = ["internal/serialization_test.cc"],
    deps = [
        ":serialization",
        ":test_protobuf_cc_proto",
        "@com_google_googletest//:gtest_main",
        "@com_google_protobuf//:protobuf",
    ],
)

cc_library(
    name = "subprocess",
    srcs = ["internal/subprocess.cc"],
    hdrs = ["internal/subprocess.h"],
    deps = [
        ":logging",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/time",
    ],
)

cc_test(
    name = "subprocess_test",
    srcs = ["internal/subprocess_test.cc"],
    deps = [
        ":subprocess",
        "@com_google_absl//absl/time",
        "@com_google_googletest//:gtest_main",
    ],
)

proto_library(
    name = "test_protobuf",
    srcs = ["internal/test_protobuf.proto"],
)

cc_proto_library(
    name = "test_protobuf_cc_proto",
    deps = [":test_protobuf"],
)

cc_library(
    name = "type_support",
    srcs = ["internal/type_support.cc"],
    hdrs = ["internal/type_support.h"],
    deps = [
        ":absl_domain",
        ":meta",
        "@com_google_absl//absl/debugging:symbolize",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/time",
    ],
)

cc_test(
    name = "type_support_test",
    srcs = ["internal/type_support_test.cc"],
    deps = [
        ":domain",
        ":test_protobuf_cc_proto",
        ":type_support",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/time",
        "@com_google_googletest//:gtest_main",
    ],
)
