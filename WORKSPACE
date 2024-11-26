# -*- python -*-

workspace(name = "drake_xvisio_driver")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

(DRAKE_COMMIT, DRAKE_CHECKSUM) = (
    "v1.35.0",
    "f19ee360656c87db8b0688c676c9f2ab2ae71ea08691432979b32d71c236e768",
)

DRAKE_STRIP_PREFIX = "drake-1.35.0"
# If using commit vs. a tag, uncomment below.
# DRAKE_STRIP_PREFIX = "drake-v{}".format(DRAKE_COMMIT)

# Before changing the COMMIT, temporarily uncomment the next line so that Bazel
# displays the suggested new value for the CHECKSUM.
# DRAKE_CHECKSUM = "0" * 64

# Or to build against a local checkout of Drake, at the bash prompt set an
# environment variable before building:
#  export XVISIO_LOCAL_DRAKE_PATH=/home/user/stuff/drake

# Load an environment variable.
load("//:environ.bzl", "environ_repository")

environ_repository(
    name = "environ",
    vars = ["XVISIO_LOCAL_DRAKE_PATH"],
)

load("@environ//:environ.bzl", "XVISIO_LOCAL_DRAKE_PATH")

# This declares the `@drake` repository as an http_archive from github,
# iff XVISIO_LOCAL_DRAKE_PATH is unset.  When it is set, this declares a
# `@drake_ignored` package which is never referenced, and thus is ignored.
http_archive(
    name = "drake" if not XVISIO_LOCAL_DRAKE_PATH else "drake_ignored",
    sha256 = DRAKE_CHECKSUM,
    strip_prefix = DRAKE_STRIP_PREFIX,
    urls = [x.format(DRAKE_COMMIT) for x in [
        "https://github.com/RobotLocomotion/drake/archive/{}.tar.gz",
    ]],
)

# This declares the `@drake` repository as a local directory,
# iff XVISIO_LOCAL_DRAKE_PATH is set.  When it is unset, this declares a
# `@drake_ignored` package which is never referenced, and thus is ignored.
local_repository(
    name = "drake" if XVISIO_LOCAL_DRAKE_PATH else "drake_ignored",
    path = XVISIO_LOCAL_DRAKE_PATH,
)

print("Using XVISIO_LOCAL_DRAKE_PATH={}".format(XVISIO_LOCAL_DRAKE_PATH)) if XVISIO_LOCAL_DRAKE_PATH else None  # noqa

load("@drake//tools/workspace:default.bzl", "add_default_workspace")
add_default_workspace()


load("@drake//tools/workspace:mirrors.bzl", "DEFAULT_MIRRORS")
