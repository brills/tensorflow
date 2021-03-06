# Generates CUDA kernels using MLIR codegen.

load(
    "//tensorflow/core/kernels/mlir_generated:build_defs.bzl",
    "gen_kernel_library",
    "if_mlir_experimental_kernels_enabled",
    "if_mlir_generated_gpu_kernels_enabled",
)
load(
    "//tensorflow:tensorflow.bzl",
    "if_cuda_or_rocm",
)
load("//tensorflow:tensorflow.bzl", "get_compatible_with_cloud")  # buildifier: disable=same-origin-load
load("//tensorflow:tensorflow.bzl", "tf_cuda_cc_test")  # buildifier: disable=same-origin-load
load("//tensorflow:tensorflow.bzl", "tf_kernel_library")  # buildifier: disable=same-origin-load
load(
    "//tensorflow/core/platform:build_config_root.bzl",
    "tf_cuda_tests_tags",
)

package(
    default_visibility = [
        "//tensorflow/core/kernels:__subpackages__",
    ],
    licenses = ["notice"],  # Apache 2.0
)

config_setting(
    name = "mlir_generated_gpu_kernels_disabled",
    define_values = {"tensorflow_enable_mlir_generated_gpu_kernels": "0"},
)

config_setting(
    name = "mlir_experimental_kernels_enabled",
    define_values = {"enable_unranked_kernels": "1"},
)

filegroup(
    name = "enabled_unary_kernel_srcs",
    srcs = [
        "gpu_op_abs.cc",
        "gpu_op_tanh.cc",
    ],
    compatible_with = get_compatible_with_cloud(),
)

filegroup(
    name = "experimental_unary_kernel_srcs",
    srcs = [
        "gpu_op_acos.cc",
        "gpu_op_acosh.cc",
        "gpu_op_asin.cc",
        "gpu_op_asinh.cc",
        "gpu_op_atan.cc",
        "gpu_op_atanh.cc",
        "gpu_op_ceil.cc",
        "gpu_op_complex.cc",
        "gpu_op_complex_abs.cc",
        "gpu_op_conj.cc",
        "gpu_op_cos.cc",
        "gpu_op_cosh.cc",
        "gpu_op_erf.cc",
        "gpu_op_exp.cc",
        "gpu_op_expm1.cc",
        "gpu_op_floor.cc",
        "gpu_op_imag.cc",
        "gpu_op_is_finite.cc",
        "gpu_op_is_inf.cc",
        "gpu_op_is_nan.cc",
        "gpu_op_log.cc",
        "gpu_op_log1p.cc",
        "gpu_op_logical_not.cc",
        "gpu_op_neg.cc",
        "gpu_op_real.cc",
        "gpu_op_rsqrt.cc",
        "gpu_op_sign.cc",
        "gpu_op_sin.cc",
        "gpu_op_sqrt.cc",
        "gpu_op_tan.cc",
    ],
    compatible_with = get_compatible_with_cloud(),
)

filegroup(
    name = "unary_kernel_srcs",
    srcs = [
        ":enabled_unary_kernel_srcs",
    ] + if_mlir_experimental_kernels_enabled(
        if_true = [":experimental_unary_kernel_srcs"],
    ),
    compatible_with = get_compatible_with_cloud(),
)

cc_library(
    name = "gpu_ops_base",
    srcs = ["gpu_ops_base.cc"],
    hdrs = ["gpu_ops_base.h"],
    compatible_with = get_compatible_with_cloud(),
    deps = [
        "//tensorflow/compiler/mlir/tools/kernel_gen:tf_framework_c_interface",
        "//tensorflow/compiler/mlir/tools/kernel_gen:tf_gpu_runtime_wrappers",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/core/framework:allocation_description_proto_cc",
        "//tensorflow/core/framework:op_requires",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:mlir_c_runner_utils",
    ],
)

tf_kernel_library(
    name = "cwise_unary_op",
    srcs = [":unary_kernel_srcs"],
    tags = [
        "manual",
    ],
    deps = [
        # Technically we only need to depend on the kernel libraries for the
        # unranked kernels which are enabled by default. But this would
        # make our BUILD target structure uglier. We already need to make
        # sure that those targets can be built, so it should not hurt to
        # link them in even if they are currently not needed yet.
        ":abs_kernels",
        ":acos_kernels",
        ":acosh_kernels",
        ":asin_kernels",
        ":asinh_kernels",
        ":atan_kernels",
        ":atanh_kernels",
        ":ceil_kernels",
        ":complex_abs_kernels",
        ":complex_kernels",
        ":conj_kernels",
        ":cos_kernels",
        ":cosh_kernels",
        ":erf_kernels",
        ":exp_kernels",
        ":expm1_kernels",
        ":floor_kernels",
        ":imag_kernels",
        ":is_finite_kernels",
        ":is_inf_kernels",
        ":is_nan_kernels",
        ":log_kernels",
        ":log1p_kernels",
        ":logical_not_kernels",
        ":neg_kernels",
        ":real_kernels",
        ":rsqrt_kernels",
        ":sign_kernels",
        ":sin_kernels",
        ":sqrt_kernels",
        ":tan_kernels",
        ":tanh_kernels",
        ":gpu_ops_base",
        "//third_party/eigen3",
    ],
)

tf_kernel_library(
    name = "cwise_binary_op",
    srcs = [
        "gpu_op_add.cc",
        "gpu_op_atan2.cc",
        "gpu_op_bitwise_and.cc",
        "gpu_op_bitwise_or.cc",
        "gpu_op_bitwise_xor.cc",
        "gpu_op_div.cc",
        "gpu_op_equal.cc",
        "gpu_op_floor_div.cc",
        "gpu_op_greater.cc",
        "gpu_op_greater_equal.cc",
        "gpu_op_left_shift.cc",
        "gpu_op_less.cc",
        "gpu_op_less_equal.cc",
        "gpu_op_logical_and.cc",
        "gpu_op_logical_or.cc",
        "gpu_op_mul.cc",
        "gpu_op_not_equal.cc",
        "gpu_op_right_shift.cc",
        "gpu_op_sub.cc",
    ],
    tags = [
        "manual",
    ],
    deps = [
        ":add_v2_kernels",
        ":atan2_kernels",
        ":bitwise_and_kernels",
        ":bitwise_or_kernels",
        ":bitwise_xor_kernels",
        ":div_kernels",
        ":equal_kernels",
        ":floor_div_kernels",
        ":gpu_ops_base",
        ":greater_equal_kernels",
        ":greater_kernels",
        ":left_shift_kernels",
        ":less_equal_kernels",
        ":less_kernels",
        ":logical_and_kernels",
        ":logical_or_kernels",
        ":maximum_kernels",
        ":minimum_kernels",
        ":mul_kernels",
        ":not_equal_kernels",
        ":right_shift_kernels",
        ":sub_kernels",
        "//third_party/eigen3",
    ],
)

tf_kernel_library(
    name = "cwise_op",
    srcs = [],
    tags = [
        "no_rocm",
    ],
    # Technically these libraries don't need --config=cuda or --config=rocm,
    # but we want to avoid building them if they are not needed.
    deps = if_cuda_or_rocm([
        ":cwise_unary_op",
    ]) + if_mlir_experimental_kernels_enabled(
        [
            ":cwise_binary_op",
        ],
    ),
)

tf_cuda_cc_test(
    name = "gpu_unary_ops_test",
    size = "small",
    srcs = if_mlir_generated_gpu_kernels_enabled(["gpu_unary_ops_test.cc"]),
    tags = tf_cuda_tests_tags() + [
        "no_cuda_asan",  # TODO(b/171341759): re-enable.
    ],
    deps = [
        ":gpu_ops_test_util",
        "//tensorflow/core:framework",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:tensorflow",
        "//tensorflow/core:test",
        "//tensorflow/core:test_main",
        "//tensorflow/core:testlib",
        "//tensorflow/core/common_runtime:device",
        "//tensorflow/core/common_runtime:device_factory",
        "//tensorflow/core/kernels:cwise_op",
        "//tensorflow/core/kernels:ops_testutil",
        "@com_google_absl//absl/container:inlined_vector",
    ],
)

tf_cuda_cc_test(
    name = "gpu_binary_ops_test",
    size = "small",
    srcs = if_mlir_generated_gpu_kernels_enabled(["gpu_binary_ops_test.cc"]),
    tags = tf_cuda_tests_tags() + [
        "no_cuda_asan",  # b/173033461
    ],
    deps = [
        ":gpu_ops_test_util",
        "//tensorflow/core:framework",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:tensorflow",
        "//tensorflow/core:test",
        "//tensorflow/core:test_main",
        "//tensorflow/core:testlib",
        "//tensorflow/core/common_runtime:device",
        "//tensorflow/core/common_runtime:device_factory",
        "//tensorflow/core/framework:types_proto_cc",
        "//tensorflow/core/kernels:cwise_op",
        "//tensorflow/core/kernels:ops_testutil",
        "@com_google_absl//absl/container:inlined_vector",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@llvm-project//llvm:Support",
    ],
)

cc_library(
    name = "gpu_ops_test_util",
    testonly = 1,
    srcs = [
        "gpu_ops_test_util.cc",
        "gpu_ops_test_util.h",
    ],
    hdrs = [
        "gpu_ops_test_util.h",
    ],
    deps = [
        "//tensorflow/core:framework",
        "//tensorflow/core:tensorflow",
        "@com_google_absl//absl/container:inlined_vector",
        "@com_google_absl//absl/strings",
        "@llvm-project//llvm:Support",
    ],
)

# TODO(b/160731748): Re-enable when it works again.
# gen_kernel_library(
#     name = "bias_add",
#     tile_size = "16x16",
#     types = [
#         "f16",
#         "f32",
#         "f64",
#     ],
# )

# TODO(b/160190568): Re-enable when it works again.
# gen_kernel_library(
#     name = "relu",
#     tile_size = "256",
#     types = [
#         "f16",
#         "f32",
#         "f64",
#     ],
# )

gen_kernel_library(
    name = "abs",
    tile_size = "256",
    types = [
        "f16",
        "f32",
        "f64",
        "i32",
        "i64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "acos",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "acosh",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "asin",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "asinh",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "atan",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "atanh",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "conj",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "2",
)

gen_kernel_library(
    name = "cosh",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "erf",
    tile_size = "256",
    types = [
        "f16",
        "f32",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "imag",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
)

gen_kernel_library(
    name = "invert",
    tile_size = "256",
    types = [
        "i8",
        "i16",
        "i32",
        "i64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "logical_not",
    tile_size = "256",
    types = ["i1"],
)

gen_kernel_library(
    name = "real",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
)

gen_kernel_library(
    name = "sign",
    tile_size = "256",
    types = [
        # TODO(b/162577610): Add bf16, c64 and c128.
        "f16",
        "f32",
        "f64",
        "i32",
        "i64",
    ],
    unroll_factors = "4",
)

[
    gen_kernel_library(
        name = name,
        tile_size = "256,1,1",
        types = [
            "f16",
            "f32",
            "f64",
            "i64",
        ],
        unroll_factors = "4",
    )
    for name in [
        "add_v2",
        "sub",
    ]
]

gen_kernel_library(
    name = "complex",
    tile_size = "256,1,1",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "2",
)

gen_kernel_library(
    name = "complex_abs",
    tile_size = "256",
    types = [
        "f32",
        "f64",
    ],
)

gen_kernel_library(
    name = "div",
    tile_size = "256,1,1",
    types = [
        "f16",
        "f32",
        "f64",
        "i16",
        "i64",
    ],
    unroll_factors = "4",
)

gen_kernel_library(
    name = "mul",
    tile_size = "256,1,1",
    types = [
        "f16",
        "f32",
        "f64",
        "i8",
        "i16",
        "i64",
    ],
    unroll_factors = "4",
)

# Bitwise operations.
[
    gen_kernel_library(
        name = name,
        tile_size = "256,1,1",
        types = [
            "i8",
            "i16",
            "i32",
            "i64",
            # TODO(b/172804967): Enable once fixed.
            # "ui8",
            # "ui16",
            # "ui32",
            # "ui64",
        ],
        unroll_factors = "4",
    )
    for name in [
        "bitwise_and",
        "bitwise_or",
        "bitwise_xor",
        "left_shift",
        "right_shift",
    ]
]

gen_kernel_library(
    name = "atan2",
    tile_size = "256,1,1",
    types = [
        "f32",
        "f64",
    ],
    unroll_factors = "4",
)

# Logical operations.
[
    gen_kernel_library(
        name = name,
        tile_size = "256,1,1",
        types = [
            "i1",
        ],
        unroll_factors = "4",
    )
    for name in [
        "logical_and",
        "logical_or",
    ]
]

[
    gen_kernel_library(
        name = name,
        tile_size = "256,1,1",
        types = [
            "f16",
            "f32",
            "f64",
            "i1",
            "i8",
            "i16",
            "i32",
            "i64",
        ],
        unroll_factors = "4",
    )
    for name in [
        "equal",
        "not_equal",
    ]
]

[
    gen_kernel_library(
        name = name,
        tile_size = "256,1,1",
        types = [
            "f16",
            "f32",
            "f64",
            "i8",
            "i16",
            "i32",
            "i64",
        ],
        unroll_factors = "4",
    )
    for name in [
        "less",
        "less_equal",
        "greater",
        "greater_equal",
    ]
]

[
    gen_kernel_library(
        name = name,
        tile_size = "256,1,1",
        types = [
            "f16",
            "f32",
            "f64",
            "i16",
            "i32",
            "i64",
        ],
        unroll_factors = "4",
    )
    for name in [
        "maximum",
        "minimum",
    ]
]

# Kernels that support all floating-point and signed int types.
[
    gen_kernel_library(
        name = name,
        tile_size = "256",
        types = [
            "f16",
            "f32",
            "f64",
            "i8",
            "i16",
            "i32",
            "i64",
        ],
        unroll_factors = "4",
    )
    for name in [
        "neg",
    ]
]

gen_kernel_library(
    name = "floor_div",
    tile_size = "256",
    # TODO(172804967): Enable for integer types also once unsigned integers are
    # supported.
    types = [
        "f16",
        "f32",
        "f64",
    ],
    unroll_factors = "4",
)

# Kernels that support all floating-point types.
[
    gen_kernel_library(
        name = name,
        tile_size = "256",
        types = [
            "f16",
            "f32",
            "f64",
        ],
        unroll_factors = "4",
    )
    for name in [
        "ceil",
        "exp",
        "expm1",
        "floor",
        "is_finite",
        "is_inf",
        "is_nan",
        "log",
        "log1p",
        "rsqrt",
        "sqrt",
        "tanh",
    ]
]

# Kernels that support all floating-point types but cannot be vectorized.
[
    gen_kernel_library(
        name = name,
        tile_size = "256",
        types = [
            "f16",
            "f32",
            "f64",
        ],
    )
    for name in [
        "cos",
        "sin",
        "tan",
    ]
]
