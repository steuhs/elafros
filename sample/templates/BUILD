package(default_visibility = ["//visibility:public"])

load("@bazel_tools//tools/build_defs/pkg:pkg.bzl", "pkg_tar")

pkg_tar(
    name = "default_creds",
    srcs = [":config.json"],
    mode = "0600",
    package_dir = "/root/.docker",
)

pkg_tar(
    name = "cred_helper",
    srcs = ["@docker_credential_gcr//:docker-credential-gcr"],
    mode = "0555",
    package_dir = "/usr/local/bin",
)

load("@io_bazel_rules_docker//docker:docker.bzl", "docker_build")

# TODO(mattmoor): We should upstream adding the credential helper and default
# configuration to github.com/googlecloudplatform/cloud-builders
docker_build(
    name = "docker_with_creds",
    base = "@cb_docker//image",
    env = {
        # We override HOME, so direct the client to the
        # appropriate credential configuration.
        "DOCKER_CONFIG": "/root/.docker",
    },
    tars = [
        ":cred_helper.tar",
        ":default_creds.tar",
    ],
)

load("@k8s_object//:defaults.bzl", "k8s_object")

k8s_object(
    name = "docker-build",
    images = {
        "docker:latest": ":docker_with_creds",
    },
    template = "docker-build.yaml",
)

k8s_object(
    name = "node-app",
    template = "node-app.yaml",
)

k8s_object(
    name = "node-fn",
    template = "node-fn.yaml",
)

k8s_object(
    name = "php-app",
    template = "php-app.yaml",
)

load("@io_bazel_rules_k8s//k8s:objects.bzl", "k8s_objects")

k8s_objects(
    name = "all",
    objects = [
        ":docker-build",
        ":node-app",
        ":php-app",
    ],
)
