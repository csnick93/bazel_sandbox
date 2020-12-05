workspace(name = "example_repo")

# making pip install work
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "rules_python",
    url = "https://github.com/bazelbuild/rules_python/releases/download/0.1.0/rules_python-0.1.0.tar.gz",
    sha256 = "b6d46438523a3ec0f3cead544190ee13223a52f6a6765a29eae7b7cc24cc83a0",
)


# personalized python
http_archive(
	name = "python_interpreter",
	urls = ["https://www.python.org/ftp/python/3.8.3/Python-3.8.3.tar.xz"],
	sha256 = "dfab5ec723c218082fe3d5d7ae17ecbdebffa9a1aea4d64aa3a2ecdd2e795864",
	strip_prefix = "Python-3.8.3",
	patch_cmds = [
		"mkdir $(pwd)/bazel_install",
		"./configure --prefix=$(pwd)/bazel_install",
		"make",
		"make install",
		"ln -s bazel_install/bin/python3 python_bin",
	],
	build_file_content = """
exports_files(["python_bin"])
filegroup(
name = "files",
srcs = glob(["bazel_install/**"], exclude = ["**/* *"]),
visibility = ["//visibility:public"],
)
""",
)

register_toolchains("//:my_py_toolchain")

load("@rules_python//python:pip.bzl", "pip_install")

pip_install(requirements = "//:requirements.txt")
