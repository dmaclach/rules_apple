load(
    "//apple:resources.bzl",
    "apple_bundle_import",
    "apple_resource_bundle",
    "apple_resource_group",
)
load(
    "@build_bazel_rules_swift//swift:swift.bzl",
    "swift_library",
)
load(
    "//test/starlark_tests:common.bzl",
    "common",
)
load(
    "//test/starlark_tests:rules/dummy_apple_resource_info.bzl",
    "dummy_apple_resource_info",
)

licenses(["notice"])

package(
    default_visibility = ["//test/starlark_tests:__subpackages__"],
)

# Exports all files in this package as targets to make it easier to depend on them.
# Because these are for Starlark tests, we can scope the visibility to just this
# package.
exports_files(
    glob(["**"]),
    visibility = ["//test/starlark_tests:__subpackages__"],
)

objc_library(
    name = "objc_common_lib",
    srcs = ["common.m"],
    hdrs = ["common.h"],
    tags = common.fixture_tags,
)

objc_library(
    name = "objc_common_test_lib",
    srcs = [
        "//test/starlark_tests/resources:common.h",
        "//test/starlark_tests/resources:common_test.m",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "objc_common_lib_with_structured_resources",
    srcs = ["common.m"],
    hdrs = ["common.h"],
    data = [":structured_image_resources"],
    tags = common.fixture_tags,
)

objc_library(
    name = "objc_common_lib_with_structured_images",
    srcs = [
        ":common.h",
        ":common.m",
    ],
    data = [":structured_image_resources"],
    tags = common.fixture_tags,
)

# --------------------------------------------------------------------------------
# Objective-C shared.h libraries

objc_library(
    name = "objc_shared_lib",
    srcs = ["shared.m"],
    hdrs = ["shared.h"],
    tags = common.fixture_tags,
    deps = [
        ":objc_common_lib",
    ],
)

objc_library(
    name = "objc_shared_lib_with_resources",
    srcs = [
        "shared.h",
        "shared.m",
    ],
    data = [
        ":structured_resources",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "objc_shared_lib_with_transitive_resources",
    srcs = ["shared.m"],
    hdrs = ["shared.h"],
    tags = common.fixture_tags,
    deps = [
        ":objc_common_lib_with_structured_resources",
    ],
)

objc_library(
    name = "another_objc_shared_lib_with_transitive_resources",
    srcs = ["shared.m"],
    hdrs = ["shared.h"],
    tags = common.fixture_tags,
    deps = [
        ":objc_common_lib_with_structured_resources",
    ],
)

objc_library(
    name = "objc_shared_lib_with_transitive_structured_resources",
    srcs = [
        "shared.h",
        "shared.m",
    ],
    deps = [":objc_common_lib_with_structured_images"],
    alwayslink = 1,
)

objc_library(
    name = "objc_shared_lib_with_resource_bundles",
    srcs = [
        "shared.h",
        "shared.m",
    ],
    data = [
        "//test/testdata/resources:basic_bundle",
        "//test/testdata/resources:simple_bundle_library",
    ],
)

# --------------------------------------------------------------------------------
# Objective-C main.m libraries

objc_library(
    name = "objc_main_lib",
    srcs = ["main.m"],
    tags = common.fixture_tags,
    deps = [
        ":objc_common_lib",
    ],
)

objc_library(
    name = "objc_lib_with_sdk_dylibs",
    srcs = ["common.m"],
    hdrs = ["common.h"],
    sdk_dylibs = ["libz"],
    tags = common.fixture_tags,
)

objc_library(
    name = "objc_lib_with_sdk_fmwks",
    srcs = ["common.m"],
    hdrs = ["common.h"],
    sdk_frameworks = ["CoreData"],
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_main_lib",
    srcs = ["//test/testdata/sources:main.swift"],
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_lib_with_structured_resources",
    testonly = True,
    srcs = [
        "Shared.swift",
    ],
    data = [
        ":structured_resources",
    ],
    features = ["swift.no_generated_module_map"],
    module_name = "swift_lib_with_resources",
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_lib_with_structured_macos_resources",
    testonly = True,
    srcs = [
        "Shared.swift",
    ],
    data = [
        ":structured_resources",
    ],
    features = ["swift.no_generated_module_map"],
    module_name = "swift_lib_with_macos_resources",
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_common_lib",
    testonly = True,
    srcs = ["Common.swift"],
    features = ["swift.no_generated_module_map"],
    module_name = "swift_common_lib",
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_shared_lib",
    testonly = True,
    srcs = ["Shared.swift"],
    features = ["swift.no_generated_module_map"],
    module_name = "swift_shared_lib",
    tags = common.fixture_tags,
    deps = [
        "//test/starlark_tests/resources:swift_common_lib",
    ],
)

swift_library(
    name = "swift_transitive_lib",
    testonly = True,
    srcs = ["Transitives.swift"],
    features = ["swift.no_generated_module_map"],
    module_name = "swift_transitive_lib",
    tags = common.fixture_tags,
    deps = [
        "//test/starlark_tests/resources:swift_common_lib",
        "//test/starlark_tests/resources:swift_shared_lib",
    ],
)

swift_library(
    name = "swift_lib_with_transitives",
    testonly = True,
    srcs = ["Shared.swift"],
    data = [
        "//test/starlark_tests/targets_under_test/ios:swift_common_lib_framework",
        "//test/starlark_tests/targets_under_test/ios:swift_shared_lib_framework",
    ],
    features = ["swift.no_generated_module_map"],
    module_name = "dynamic_framework_with_resources",
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_framework_lib",
    srcs = ["//test/testdata/sources:main.swift"],
    generates_header = True,
    module_name = "swift_framework_lib",
    tags = common.fixture_tags,
)

swift_library(
    name = "watchkit_ext_main_lib",
    srcs = ["WatchKitExtMain.swift"],
    tags = common.fixture_tags,
)

objc_library(
    name = "objc_test_lib",
    srcs = ["test.m"],
    tags = common.fixture_tags,
)

objc_library(
    name = "objc_swift_depending_test_lib",
    srcs = ["test.m"],
    tags = common.fixture_tags,
    deps = [":swift_uikit_appdelegate"],
)

swift_library(
    name = "swift_uikit_appdelegate",
    srcs = ["AppDelegate.swift"],
    tags = common.fixture_tags,
)

apple_bundle_import(
    name = "settings_bundle_ios",
    bundle_imports = [":settings_bundle_ios_files"],
    tags = common.fixture_tags,
)

filegroup(
    name = "settings_bundle_ios_files",
    srcs = glob(["settings_ios.bundle/**"]),
)

genrule(
    name = "generate_strings",
    outs = ["generated.strings"],
    cmd = "echo '\"generated_string\" = \"I like turtles!\";' > $@",
    tags = common.fixture_tags,
)

genrule(
    name = "generate_structured_strings",
    outs = ["structured/generated.strings"],
    cmd = "echo '\"generated_structured_string\" = \"I like turtles too!\";' > $@",
    tags = common.fixture_tags,
)

dummy_apple_resource_info(
    name = "custom_apple_resource_info",
    tags = common.fixture_tags,
)

apple_resource_bundle(
    name = "resource_bundle",
    infoplists = ["Another.plist"],
    resources = [":custom_apple_resource_info"],
    tags = common.fixture_tags,
)

apple_resource_bundle(
    name = "resource_bundle_with_structured_resource_group",
    resources = [":structured_resources"],
    tags = common.fixture_tags,
)

apple_resource_bundle(
    name = "resource_bundle_with_bundle_id",
    bundle_id = "org.bazel.rules_apple.resource_bundle",
    infoplists = ["Info-noversion.plist"],
    resources = [
        "en.lproj/files.stringsdict",
        "en.lproj/greetings.strings",
    ],
    tags = common.fixture_tags,
)

apple_resource_bundle(
    name = "localization",
    resources = [
        "en.lproj/files.stringsdict",
        "en.lproj/greetings.strings",
    ],
    tags = common.fixture_tags,
)

filegroup(
    name = "example_filegroup",
    srcs = [
        ":Another.plist",
    ],
)

filegroup(
    name = "all_nested",
    srcs = [
        "nested/nested.txt",
        "non_nested.txt",
    ],
)

apple_resource_group(
    name = "resources_with_resource_bundle",
    resources = [":resource_bundle"],
    tags = common.fixture_tags,
)

apple_resource_group(
    name = "structured_resources",
    structured_resources = [
        "Another.plist",
    ],
    tags = common.fixture_tags,
)

apple_resource_group(
    name = "structured_resources_duplicate",
    structured_resources = [
        "Another.plist",
    ],
    tags = common.fixture_tags,
)

apple_resource_group(
    name = "structured_resources_in_resources",
    structured_resources = [
        "Resources/some.file",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "objc_lib_with_resources",
    srcs = ["@bazel_tools//tools/objc:objc_dummy.mm"],
    data = [
        ":structured_resources",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "structured_resources_in_resources_lib",
    srcs = ["main.m"],
    data = [
        ":structured_resources_in_resources",
    ],
    tags = common.fixture_tags,
)

apple_resource_group(
    name = "processed_resources_in_structured_resources",
    structured_resources = [
        ":localization",
    ],
)

objc_library(
    name = "processed_resources_in_structured_resources_lib",
    srcs = ["main.m"],
    data = [
        ":processed_resources_in_structured_resources",
    ],
)

filegroup(
    name = "localized_plists",
    srcs = glob(["*.lproj/*.plist"]),
)

filegroup(
    name = "assets",
    srcs = glob(["assets.xcassets/**"]),
)

filegroup(
    name = "app_icons_ios",
    srcs = glob(["app_icons_ios.xcassets/**"]),
)

filegroup(
    name = "alternate_icons_ios",
    srcs = glob(["alticons/**"]),
)

filegroup(
    name = "launch_images_ios",
    srcs = glob(["launch_images_ios.xcassets/**"]),
)

objc_library(
    name = "launch_images_ios_lib",
    data = [
        ":launch_images_ios",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "launch_images_ios_lib_duplicated_target",
    data = [
        ":launch_images_ios",
    ],
    tags = common.fixture_tags,
)

filegroup(
    name = "basic_bundle_files",
    srcs = glob(["basic.bundle/**"]),
)

filegroup(
    name = "localized_storyboards_ios",
    srcs = glob(["*.lproj/*.storyboard"]),
)

filegroup(
    name = "localized_strings",
    srcs = glob(["*.lproj/*.strings"]),
)

filegroup(
    name = "localized_xibs_ios",
    srcs = glob(["*.lproj/*.xib"]),
)

filegroup(
    name = "mapping_model",
    srcs = glob(["mapping_model.xcmappingmodel/**"]),
)

filegroup(
    name = "metal_files",
    srcs = glob(["*.metal"]) + ["shader_defines.h"],
)

filegroup(
    name = "localized_generic_resources",
    srcs = glob(["*.lproj/*.txt"]),
)

filegroup(
    name = "sticker_pack_ios",
    srcs = glob(["sticker_pack_ios.xcstickers/**"]),
)

objc_library(
    name = "sticker_pack_ios_lib",
    data = [
        ":sticker_pack_ios",
    ],
    tags = common.fixture_tags,
)

apple_bundle_import(
    name = "basic_bundle",
    bundle_imports = [":basic_bundle_files"],
    tags = common.fixture_tags,
)

objc_library(
    name = "basic_bundle_lib",
    srcs = ["@bazel_tools//tools/objc:objc_dummy.mm"],
    data = [
        ":basic_bundle",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "another_basic_bundle_lib",
    srcs = ["@bazel_tools//tools/objc:objc_dummy.mm"],
    data = [
        ":basic_bundle",
    ],
    tags = common.fixture_tags,
)

apple_bundle_import(
    name = "nested_bundle",
    bundle_imports = glob(["nested_bundle/nested.bundle/**"]),
    tags = common.fixture_tags,
)

objc_library(
    name = "nested_bundle_lib",
    data = [
        ":nested_bundle",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "ios_non_localized_assets_lib",
    srcs = ["main.m"],
    data = [
        ":assets",
        ":mapping_model",
        ":nonlocalized.plist",
        ":nonlocalized.strings",
        ":sample.png",
        ":storyboard_ios.storyboard",
        ":unversioned_datamodel",
        ":versioned_datamodel",
        ":view_ios.xib",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "macos_non_localized_assets_lib",
    srcs = ["main.m"],
    data = [
        ":assets",
        ":mapping_model",
        ":nonlocalized.plist",
        ":nonlocalized.strings",
        ":sample.png",
        ":storyboard_macos.storyboard",
        ":unversioned_datamodel",
        ":versioned_datamodel",
        # ":view_macos.xib",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "ios_localized_assets_lib",
    srcs = ["main.m"],
    data = [
        ":localized_generic_resources",
        ":localized_plists",
        ":localized_storyboards_ios",
        ":localized_strings",
        ":localized_xibs_ios",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "apple_non_localized_assets_lib",
    srcs = ["main.m"],
    data = [
        ":mapping_model",
        ":nonlocalized.plist",
        ":nonlocalized.strings",
        ":sample.png",
        ":unversioned_datamodel",
        ":versioned_datamodel",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "apple_localized_assets_lib",
    srcs = ["main.m"],
    data = [
        ":localized_generic_resources",
        ":localized_plists",
        ":localized_strings",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "empty_strings_file_lib",
    data = [
        "empty.strings",
    ],
    tags = common.fixture_tags,
)

apple_resource_bundle(
    name = "bundle_library_ios",
    infoplists = [
        "Info-template.plist",
    ],
    resources = [
        "generated.strings",
        "nonlocalized.plist",
        "nonlocalized.strings",
        "nonlocalized_resource.txt",
        "sample.png",
        "storyboard_ios.storyboard",
        "view_ios.xib",
        ":assets",
        ":basic_bundle",
        ":localized_generic_resources",
        ":localized_plists",
        ":localized_storyboards_ios",
        ":localized_strings",
        ":localized_xibs_ios",
        ":mapping_model",
        ":metal_files",
        ":unversioned_datamodel",
        ":versioned_datamodel",
    ],
    structured_resources = [
        ":generate_structured_strings",
        ":structured",
    ],
    tags = common.fixture_tags,
)

apple_resource_bundle(
    name = "bundle_library_macos",
    infoplists = [
        "Info-template.plist",
    ],
    resources = [
        "generated.strings",
        "nonlocalized.plist",
        "nonlocalized.strings",
        "nonlocalized_resource.txt",
        "sample.png",
        "storyboard_macos.storyboard",
        # "view_macos.xib",
        ":assets",
        ":basic_bundle",
        ":localized_generic_resources",
        ":localized_plists",
        ":localized_strings",
        ":mapping_model",
        ":metal_files",
        ":unversioned_datamodel",
        ":versioned_datamodel",
    ],
    structured_resources = [
        ":generate_structured_strings",
        ":structured",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "bundle_library_ios_lib",
    data = [
        ":bundle_library_ios",
    ],
    tags = common.fixture_tags,
)

apple_resource_bundle(
    name = "bundle_library_apple",
    infoplists = [
        "Info-template.plist",
    ],
    resources = [
        "generated.strings",
        "nonlocalized.plist",
        "nonlocalized.strings",
        "nonlocalized_resource.txt",
        "sample.png",
        ":basic_bundle",
        ":localized_generic_resources",
        ":localized_plists",
        ":localized_strings",
        ":mapping_model",
        ":metal_files",
        ":unversioned_datamodel",
        ":versioned_datamodel",
    ],
    structured_resources = [
        ":generate_structured_strings",
        ":structured",
    ],
    tags = common.fixture_tags,
)

objc_library(
    name = "bundle_library_apple_lib",
    data = [
        ":bundle_library_apple",
    ],
    tags = common.fixture_tags,
)

apple_resource_group(
    name = "swift_structured_resources",
    structured_resources = [
        "//test/testdata/resources:structured",
    ],
)

swift_library(
    name = "swift_lib_with_resources",
    srcs = ["AppDelegate.swift"],
    data = [
        ":swift_structured_resources",
        "//test/testdata/resources:assets",
        "//test/testdata/resources:localized_generic_resources",
        "//test/testdata/resources:localized_storyboards_ios",
        "//test/testdata/resources:localized_strings",
        "//test/testdata/resources:localized_xibs_ios",
        "//test/testdata/resources:nonlocalized.strings",
        "//test/testdata/resources:nonlocalized_resource.txt",
        "//test/testdata/resources:storyboard_ios.storyboard",
        "//test/testdata/resources:unversioned_datamodel",
        "//test/testdata/resources:versioned_datamodel",
        "//test/testdata/resources:view_ios.xib",
    ],
    module_name = "EasyToSearchForModuleName",
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_lib_with_macos_resources",
    srcs = ["AppDelegate-macos.swift"],
    data = [
        ":swift_structured_resources",
        "//test/testdata/resources:assets",
        "//test/testdata/resources:localized_generic_resources",
        "//test/testdata/resources:localized_storyboards_macos",
        "//test/testdata/resources:localized_strings",
        # "//test/testdata/resources:localized_xibs_macos",
        "//test/testdata/resources:nonlocalized.strings",
        "//test/testdata/resources:nonlocalized_resource.txt",
        "//test/testdata/resources:storyboard_macos.storyboard",
        "//test/testdata/resources:unversioned_datamodel",
        "//test/testdata/resources:versioned_datamodel",
        # "//test/testdata/resources:view_macos.xib",
    ],
    module_name = "EasyToSearchForModuleName",
    tags = common.fixture_tags,
)

swift_library(
    name = "macos_swift_lib_with_resources",
    srcs = ["main.swift"],
    data = [
        ":swift_structured_resources",
        "//test/testdata/resources:localized_generic_resources",
        "//test/testdata/resources:localized_strings",
        "//test/testdata/resources:nonlocalized.strings",
        "//test/testdata/resources:nonlocalized_resource.txt",
        "//test/testdata/resources:unversioned_datamodel",
        "//test/testdata/resources:versioned_datamodel",
    ],
    module_name = "EasyToSearchForModuleName",
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_lib_with_transitive_resources",
    srcs = ["Dummy.swift"],
    tags = common.fixture_tags,
    deps = [
        ":swift_lib_with_resources",
    ],
)

swift_library(
    name = "swift_lib_with_transitive_macos_resources",
    srcs = ["Dummy.swift"],
    tags = common.fixture_tags,
    deps = [
        ":swift_lib_with_macos_resources",
    ],
)

swift_library(
    name = "swift_lib_with_storyboard_a",
    srcs = ["AppDelegate.swift"],
    data = [
        "//test/testdata/resources:localized_storyboards_ios",
    ],
    tags = common.fixture_tags,
    deps = [
        ":swift_lib_with_storyboard_b",
    ],
)

swift_library(
    name = "swift_lib_with_storyboard_b",
    srcs = ["Dummy.swift"],
    data = [
        "//test/testdata/resources:storyboard_ios.storyboard",
    ],
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_lib_with_macos_storyboard_a",
    srcs = ["AppDelegate-macos.swift"],
    data = [
        "//test/testdata/resources:localized_storyboards_macos",
    ],
    tags = common.fixture_tags,
    deps = [
        ":swift_lib_with_macos_storyboard_b",
    ],
)

swift_library(
    name = "swift_lib_with_macos_storyboard_b",
    srcs = ["Dummy.swift"],
    data = [
        "//test/testdata/resources:storyboard_macos.storyboard",
    ],
    tags = common.fixture_tags,
)

swift_library(
    name = "macos_swift_lib_with_asset_catalog_a",
    srcs = ["AppDelegate-macos.swift"],
    data = [
        "//test/testdata/resources:assets",
    ],
    tags = common.fixture_tags,
    deps = [
        ":swift_lib_with_asset_catalog_b",
    ],
)

swift_library(
    name = "swift_lib_with_asset_catalog_a",
    srcs = ["AppDelegate.swift"],
    data = [
        "//test/testdata/resources:assets",
    ],
    tags = common.fixture_tags,
    deps = [
        ":swift_lib_with_asset_catalog_b",
    ],
)

swift_library(
    name = "swift_lib_with_asset_catalog_b",
    srcs = ["Dummy.swift"],
    data = [
        "//test/testdata/resources:assets2",
    ],
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_lib_with_storyboard_copy",
    srcs = ["Dummy.swift"],
    data = [
        "//test/testdata/resources:generate_storyboard_copy",
    ],
    tags = common.fixture_tags,
    deps = [":swift_lib_with_resources"],
)

swift_library(
    name = "swift_lib_with_macos_storyboard_copy",
    srcs = ["Dummy.swift"],
    data = [
        "//test/testdata/resources:generate_macos_storyboard_copy",
    ],
    tags = common.fixture_tags,
    deps = [":swift_lib_with_macos_resources"],
)

genrule(
    name = "DummySwiftFile",
    outs = ["Dummy.swift"],
    cmd = "echo 'struct Dummy {}' > $@",
)

filegroup(
    name = "structured",
    srcs = glob(["structured/**"]),
)

filegroup(
    name = "swift_datamodel",
    srcs = glob(["core_data_models/swift_datamodel.xcdatamodeld/**"]),
)

filegroup(
    name = "objc_datamodel",
    srcs = glob(["core_data_models/objc_datamodel.xcdatamodeld/**"]),
)

filegroup(
    name = "no_code_generation_datamodel",
    srcs = glob(["core_data_models/no_code_generation_datamodel.xcdatamodeld/**"]),
)

filegroup(
    name = "unversioned_datamodel",
    srcs = glob(["unversioned_datamodel.xcdatamodel/**"]),
)

filegroup(
    name = "versioned_datamodel",
    srcs = glob(["versioned_datamodel.xcdatamodeld/**"]),
)

objc_library(
    name = "framework_resources_lib",
    data = [
        "framework_resources/nonlocalized.plist",
    ],
    tags = common.fixture_tags,
)

genrule(
    name = "generate_structured_image",
    srcs = [":sample.png"],
    outs = ["Images/foo.png"],
    cmd = "cp $(location :sample.png) $@",
    tags = common.fixture_tags,
)

apple_resource_group(
    name = "structured_image_resources",
    structured_resources = [":generate_structured_image"],
)

filegroup(
    name = "MacAppIcon.xcassets",
    srcs = glob(["MacAppIcon.xcassets/**"]),
)

filegroup(
    name = "TVBrandAssets.xcassets",
    srcs = glob(["TVBrandAssets.xcassets/**"]),
)

filegroup(
    name = "WatchAppIcon.xcassets",
    srcs = glob(["WatchAppIcon.xcassets/**"]),
)

filegroup(
    name = "complications_watchos",
    srcs = glob(["complications_watchos.xcassets/**"]),
)

cc_binary(
    name = "cc_test_binary",
    srcs = ["main.cc"],
    tags = common.fixture_tags,
)

genrule(
    name = "swift_importing_imported_dynamic_xcfw",
    outs = ["swift_importing_imported_dynamic_xcfw.swift"],
    cmd = "echo 'import ios_dynamic_xcframework' > $@",
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_lib_importing_imported_dynamic_xcfw",
    testonly = True,
    srcs = [":swift_importing_imported_dynamic_xcfw"],
    tags = common.fixture_tags,
    deps = ["//test/starlark_tests/targets_under_test/apple:ios_imported_dynamic_xcframework_old"],
)

genrule(
    name = "swift_importing_imported_static_xcfw",
    outs = ["swift_importing_imported_static_xcfw.swift"],
    cmd = "echo 'import ios_static_xcframework_with_module_map' > $@",
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_lib_importing_imported_static_xcfw",
    testonly = True,
    srcs = [":swift_importing_imported_static_xcfw"],
    tags = common.fixture_tags,
    deps = ["//test/starlark_tests/targets_under_test/apple:ios_imported_static_xcframework_with_module_map"],
)

genrule(
    name = "swift_importing_imported_xcfw_bundling_static_fmwks",
    outs = ["swift_importing_imported_xcfw_bundling_static_fmwks.swift"],
    cmd = "echo 'import ios_xcframework_bundling_static_fmwks' > $@",
    tags = common.fixture_tags,
)

swift_library(
    name = "swift_lib_importing_imported_xcfw_bundling_static_fmwks",
    testonly = True,
    srcs = [":swift_importing_imported_xcfw_bundling_static_fmwks"],
    tags = common.fixture_tags,
    deps = ["//test/starlark_tests/targets_under_test/apple:ios_imported_xcframework_bundling_static_xcfmwks"],
)

genrule(
    name = "objc_importing_imported_static_xcfw",
    outs = ["objc_importing_imported_static_xcfw.m"],
    cmd = """echo '#import "shared.h"' > $@""",
    tags = common.fixture_tags,
)

objc_library(
    name = "objc_lib_importing_imported_static_xcfw",
    testonly = True,
    srcs = [":objc_importing_imported_static_xcfw"],
    tags = common.fixture_tags,
    deps = ["//test/starlark_tests/targets_under_test/apple:ios_imported_static_xcframework_old"],
)
