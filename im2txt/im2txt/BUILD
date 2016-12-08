package(default_visibility = [":internal"])

load("@bazel_tools//tools/build_defs/pkg:pkg.bzl", "pkg_tar", "pkg_deb")

licenses(["notice"])  # Apache 2.0

exports_files(["LICENSE"])

package_group(
    name = "internal",
    packages = [
        "//im2txt/...",
    ],
)

py_binary(
    name = "build_mscoco_data",
    srcs = [
        "data/build_mscoco_data.py",
    ],
)

sh_binary(
    name = "download_and_preprocess_mscoco",
    srcs = ["data/download_and_preprocess_mscoco.sh"],
    data = [
        ":build_mscoco_data",
    ],
)

py_library(
    name = "configuration",
    srcs = ["configuration.py"],
    srcs_version = "PY2AND3",
)

py_library(
    name = "show_and_tell_model",
    srcs = ["show_and_tell_model.py"],
    srcs_version = "PY2AND3",
    deps = [
        "//im2txt/ops:image_embedding",
        "//im2txt/ops:image_processing",
        "//im2txt/ops:inputs",
    ],
)

py_test(
    name = "show_and_tell_model_test",
    size = "large",
    srcs = ["show_and_tell_model_test.py"],
    deps = [
        ":configuration",
        ":show_and_tell_model",
    ],
)

py_library(
    name = "inference_wrapper",
    srcs = ["inference_wrapper.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":show_and_tell_model",
        "//im2txt/inference_utils:inference_wrapper_base",
    ],
)

py_binary(
    name = "train",
    srcs = ["train.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":configuration",
        ":show_and_tell_model",
    ],
)

py_binary(
    name = "evaluate",
    srcs = ["evaluate.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":configuration",
        ":show_and_tell_model",
    ],
)

py_binary(
    name = "run_inference",
    srcs = ["run_inference.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":configuration",
        ":inference_wrapper",
        "//im2txt/inference_utils:caption_generator",
        "//im2txt/inference_utils:vocabulary",
    ],
)

pkg_tar(
    name = "im2txt_inference_utils",
    extension = "tar.gz",
    package_dir = 'im2txt/inference_utils',
    files = [
	"//im2txt/inference_utils:caption_generator",
  	"//im2txt/inference_utils:inference_wrapper_base",
	"//im2txt/inference_utils:vocabulary",
    ],
)
pkg_tar(
    name = "im2txt_dist",
    extension = "tar.gz",
    files = [
        ":build_mscoco_data",
        ":download_and_preprocess_mscoco",
	":configuration",
	":show_and_tell_model",
	":inference_wrapper",
	":train",
	":evaluate",
	":run_inference",
    ],
    deps = [
	"im2txt_inference_utils",
    ],
)
