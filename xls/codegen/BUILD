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

# cc_proto_library is used in this file
load("//xls/build:py_proto_library.bzl", "xls_py_proto_library")

package(
    default_visibility = ["//xls:xls_internal"],
    licenses = ["notice"],  # Apache 2.0
)

cc_library(
    name = "name_to_bit_count",
    hdrs = ["name_to_bit_count.h"],
    deps = [
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/strings",
        "//xls/common:integral_types",
        "//xls/ir:bits",
        "//xls/ir:type",
    ],
)

cc_library(
    name = "combinational_generator",
    srcs = ["combinational_generator.cc"],
    hdrs = ["combinational_generator.h"],
    deps = [
        ":flattening",
        ":module_builder",
        ":module_signature",
        ":name_to_bit_count",
        ":node_expressions",
        ":vast",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "//xls/common/logging",
        "//xls/common/logging:log_lines",
        "//xls/common/status:ret_check",
        "//xls/common/status:status_macros",
        "//xls/common/status:statusor",
        "//xls/ir",
    ],
)

cc_test(
    name = "combinational_generator_test",
    srcs = ["combinational_generator_test.cc"],
    data = glob([
        "testdata/combinational_generator_test_*",
    ]),
    shard_count = 10,
    deps = [
        ":combinational_generator",
        "//xls/common/status:matchers",
        "//xls/examples:sample_packages",
        "//xls/ir",
        "//xls/ir:function_builder",
        "//xls/ir:ir_interpreter",
        "//xls/ir:ir_parser",
        "//xls/ir:value_helpers",
        "//xls/simulation:module_simulator",
        "//xls/simulation:verilog_simulators",
        "//xls/simulation:verilog_test_base",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "sequential_generator",
    srcs = ["sequential_generator.cc"],
    hdrs = ["sequential_generator.h"],
    deps = [
        ":finite_state_machine",
        ":module_builder",
        ":module_signature",
        ":module_signature_cc_proto",
        ":pipeline_generator",
        ":vast",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
        "//xls/common:integral_types",
        "//xls/common/logging",
        "//xls/common/status:ret_check",
        "//xls/common/status:status_macros",
        "//xls/common/status:statusor",
        "//xls/delay_model:delay_estimator",
        "//xls/delay_model:delay_estimators",
        "//xls/ir",
        "//xls/ir:type",
        "//xls/passes",
        "//xls/scheduling:pipeline_schedule",
    ],
)

cc_test(
    name = "sequential_generator_test",
    srcs = ["sequential_generator_test.cc"],
    data = glob([
        "testdata/sequential_generator_test_*",
    ]),
    shard_count = 10,
    deps = [
        ":module_builder",
        ":module_signature",
        ":module_signature_cc_proto",
        ":pipeline_generator",
        ":sequential_generator",
        ":vast",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/container:flat_hash_set",
        "//xls/common/status:matchers",
        "//xls/common/status:status_macros",
        "//xls/common/status:statusor",
        "//xls/delay_model:delay_estimator",
        "//xls/ir",
        "//xls/ir:bits",
        "//xls/ir:function_builder",
        "//xls/ir:ir_parser",
        "//xls/ir:type",
        "//xls/ir:value",
        "//xls/scheduling:pipeline_schedule",
        "//xls/simulation:module_simulator",
        "//xls/simulation:module_testbench",
        "//xls/simulation:verilog_test_base",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "vast",
    srcs = ["vast.cc"],
    hdrs = ["vast.h"],
    deps = [
        ":module_signature_cc_proto",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:variant",
        "//xls/common:indent",
        "//xls/common:visitor",
        "//xls/common/logging",
        "//xls/common/status:status_macros",
        "//xls/common/status:statusor",
        "//xls/ir:bits",
        "@com_google_re2//:re2",
    ],
)

cc_library(
    name = "finite_state_machine",
    srcs = ["finite_state_machine.cc"],
    hdrs = ["finite_state_machine.h"],
    deps = [
        ":vast",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "//xls/common:casts",
        "//xls/common/logging",
        "//xls/common/status:status_macros",
    ],
)

cc_test(
    name = "vast_test",
    srcs = ["vast_test.cc"],
    deps = [
        ":vast",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "finite_state_machine_test",
    srcs = ["finite_state_machine_test.cc"],
    data = glob(["testdata/finite_state_machine_test_*"]),
    deps = [
        ":finite_state_machine",
        ":vast",
        "//xls/common/logging",
        "//xls/common/status:matchers",
        "//xls/simulation:verilog_test_base",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "pipeline_generator",
    srcs = ["pipeline_generator.cc"],
    hdrs = ["pipeline_generator.h"],
    deps = [
        ":finite_state_machine",
        ":flattening",
        ":module_builder",
        ":module_signature",
        ":module_signature_cc_proto",
        ":name_to_bit_count",
        ":node_expressions",
        ":vast",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:optional",
        "//xls/common/logging",
        "//xls/common/logging:log_lines",
        "//xls/common/status:ret_check",
        "//xls/common/status:status_macros",
        "//xls/common/status:statusor",
        "//xls/delay_model:delay_estimator",
        "//xls/delay_model:delay_estimators",
        "//xls/ir",
        "//xls/scheduling:pipeline_schedule",
    ],
)

cc_library(
    name = "module_signature",
    srcs = ["module_signature.cc"],
    hdrs = ["module_signature.h"],
    deps = [
        ":module_signature_cc_proto",
        ":vast",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:optional",
        "//xls/common/logging",
        "//xls/common/status:ret_check",
        "//xls/common/status:status_macros",
        "//xls/common/status:statusor",
        "//xls/ir",
        "//xls/ir:type",
        "//xls/ir:value",
    ],
)

cc_library(
    name = "node_expressions",
    srcs = ["node_expressions.cc"],
    hdrs = ["node_expressions.h"],
    deps = [
        ":flattening",
        ":vast",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:span",
        "//xls/common/logging",
        "//xls/common/status:ret_check",
        "//xls/common/status:status_macros",
        "//xls/common/status:statusor",
        "//xls/ir",
        "//xls/ir:type",
    ],
)

cc_library(
    name = "flattening",
    srcs = ["flattening.cc"],
    hdrs = ["flattening.h"],
    deps = [
        ":vast",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/types:span",
        "//xls/common/logging",
        "//xls/common/status:status_macros",
        "//xls/common/status:statusor",
        "//xls/ir",
        "//xls/ir:bits",
        "//xls/ir:bits_ops",
        "//xls/ir:type",
        "//xls/ir:value",
        "//xls/ir:xls_type_cc_proto",
    ],
)

cc_library(
    name = "lint_annotate",
    srcs = ["lint_annotate.cc"],
    hdrs = ["lint_annotate.h"],
    deps = [
        ":vast",
        "@com_google_absl//absl/strings:str_format",
    ],
)

cc_library(
    name = "module_builder",
    srcs = ["module_builder.cc"],
    hdrs = ["module_builder.h"],
    deps = [
        ":flattening",
        ":lint_annotate",
        ":node_expressions",
        ":vast",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
        "//xls/common/logging",
        "//xls/common/status:ret_check",
        "//xls/common/status:status_macros",
        "//xls/common/status:statusor",
        "//xls/ir",
        "//xls/ir:type",
        "//xls/ir:value",
    ],
)

proto_library(
    name = "module_signature_proto",
    srcs = ["module_signature.proto"],
    deps = ["//xls/ir:xls_type_proto"],
)

cc_proto_library(
    name = "module_signature_cc_proto",
    deps = [":module_signature_proto"],
)

xls_py_proto_library(
    name = "module_signature_py_pb2",
    srcs = ["module_signature.proto"],
    internal_deps = [
        ":module_signature_proto",
        "//xls/ir:xls_type_proto",
    ],
    deps = [
        "//xls/ir:xls_type_py_pb2",
    ],
)

cc_test(
    name = "pipeline_generator_test",
    srcs = ["pipeline_generator_test.cc"],
    data = glob([
        "testdata/pipeline_generator_test_*",
    ]),
    shard_count = 10,
    deps = [
        ":flattening",
        ":pipeline_generator",
        "//xls/common/status:matchers",
        "//xls/delay_model:delay_estimator",
        "//xls/ir",
        "//xls/ir:function_builder",
        "//xls/scheduling:pipeline_schedule",
        "//xls/simulation:module_simulator",
        "//xls/simulation:module_testbench",
        "//xls/simulation:verilog_test_base",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "module_signature_test",
    srcs = ["module_signature_test.cc"],
    deps = [
        ":module_signature",
        "//xls/common/status:matchers",
        "//xls/ir:bits",
        "//xls/ir:value",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "flattening_test",
    srcs = ["flattening_test.cc"],
    deps = [
        ":flattening",
        "//xls/common/status:matchers",
        "//xls/ir:ir_test_base",
        "//xls/ir:type",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "module_builder_test",
    srcs = ["module_builder_test.cc"],
    data = glob(["testdata/module_builder_test_*"]),
    shard_count = 10,
    deps = [
        ":module_builder",
        ":vast",
        "//xls/common/status:matchers",
        "//xls/ir",
        "//xls/ir:bits",
        "//xls/ir:function_builder",
        "//xls/ir:type",
        "//xls/ir:value",
        "//xls/simulation:verilog_test_base",
        "@com_google_googletest//:gtest_main",
    ],
)
