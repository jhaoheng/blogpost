---
layout: post
title: "【xcode】combine 'framework' with auto build"
date: 2016-06-25 16:14
comments: true
categories: 
---
Thanks **Steven Shen** share his [knowledge](https://medium.com/@syshen/create-an-ios-universal-framework-148eb130a46c#.cdtevktvm)

[Demo code](https://github.com/jhaoheng/combine_framework)
1. Create a new project(select framework item) and workspace. Put the project to the workspace
	- **Workspace and project name should be the same.** 
2. Create a New Target from your framework project
	- Select `Other` -> Aggregate -> set Name is `xxx-Universal`
3. Copy below code, and paste to `New Target:Aggregate` script
4. Go to scheme manager and `Click the [shared]` with `project name` and `xxx-Universal name`
5. Run the `New Target:Aggregate`, and folder include universal-framework will be open.

ps : If xcode alert `No find xxx.framework from Debug-iphoneos and Debug-iphonesimulator`
Go to check the name of workspace and project, the name should be the same.

```
######################
# Options
######################

REVEAL_ARCHIVE_IN_FINDER=true

FRAMEWORK_NAME="${PROJECT_NAME}"

SIMULATOR_LIBRARY_PATH="${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${FRAMEWORK_NAME}.framework"

DEVICE_LIBRARY_PATH="${BUILD_DIR}/${CONFIGURATION}-iphoneos/${FRAMEWORK_NAME}.framework"

UNIVERSAL_LIBRARY_DIR="${BUILD_DIR}/${CONFIGURATION}-iphoneuniversal"

FRAMEWORK="${UNIVERSAL_LIBRARY_DIR}/${FRAMEWORK_NAME}.framework"


######################
# Build Frameworks
######################

xcodebuild -workspace ${PROJECT_NAME}.xcworkspace -scheme ${PROJECT_NAME} -sdk iphonesimulator -configuration ${CONFIGURATION} clean build CONFIGURATION_BUILD_DIR=${BUILD_DIR}/${CONFIGURATION}-iphonesimulator 2>&1

xcodebuild -workspace ${PROJECT_NAME}.xcworkspace -scheme ${PROJECT_NAME} -sdk iphoneos -configuration ${CONFIGURATION} clean build CONFIGURATION_BUILD_DIR=${BUILD_DIR}/${CONFIGURATION}-iphoneos 2>&1

######################
# Create directory for universal
######################

rm -rf "${UNIVERSAL_LIBRARY_DIR}"

mkdir "${UNIVERSAL_LIBRARY_DIR}"

mkdir "${FRAMEWORK}"


######################
# Copy files Framework
######################

cp -r "${DEVICE_LIBRARY_PATH}/." "${FRAMEWORK}"


######################
# Make an universal binary
######################

lipo "${SIMULATOR_LIBRARY_PATH}/${FRAMEWORK_NAME}" "${DEVICE_LIBRARY_PATH}/${FRAMEWORK_NAME}" -create -output "${FRAMEWORK}/${FRAMEWORK_NAME}" | echo

# For Swift framework, Swiftmodule needs to be copied in the universal framework
if [ -d "${SIMULATOR_LIBRARY_PATH}/Modules/${FRAMEWORK_NAME}.swiftmodule/" ]; then
    cp -f ${SIMULATOR_LIBRARY_PATH}/Modules/${FRAMEWORK_NAME}.swiftmodule//* "${FRAMEWORK}/Modules/${FRAMEWORK_NAME}.swiftmodule/" | echo
fi

if [ -d "${DEVICE_LIBRARY_PATH}/Modules/${FRAMEWORK_NAME}.swiftmodule/" ];then
    cp -f ${DEVICE_LIBRARY_PATH}/Modules/${FRAMEWORK_NAME}.swiftmodule//* "${FRAMEWORK}/Modules/${FRAMEWORK_NAME}.swiftmodule/" | echo
fi

######################
# On Release, copy the result to release directory
######################
OUTPUT_DIR="${PROJECT_DIR}/Output/${FRAMEWORK_NAME}-${CONFIGURATION}-iphoneuniversal/"
                                                                      
rm -rf "$OUTPUT_DIR"
mkdir -p "$OUTPUT_DIR"
                                                                 
cp -r "${FRAMEWORK}" "$OUTPUT_DIR"
                                                                      
if [ ${REVEAL_ARCHIVE_IN_FINDER} = true ]; then
    open "${OUTPUT_DIR}/"
fi
```