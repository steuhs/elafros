load("@io_bazel_rules_go//go:def.bzl", "go_binary", "go_library")
load("@k8s_object//:defaults.bzl", "k8s_object")

k8s_object(
    name = "elaservice",
    template = ":elaservice.yaml",
)

k8s_object(
    name = "revisiontemplate",
    template = ":revisiontemplate.yaml",
)

load("@io_bazel_rules_k8s//k8s:objects.bzl", "k8s_objects")

k8s_objects(
    name = "everything",
    objects = [
        "//sample/templates:node-fn",
        ":elaservice",
        ":revisiontemplate",
    ],
)
