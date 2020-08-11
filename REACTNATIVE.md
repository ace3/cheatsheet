# Compiled React Native Cheatsheet

Index

- [Single Source of App Version](#single-source-of-app-version)

<a name ="single-source-of-app-version"></a>

### Single Source of App Version

#### **_Android_**

Go to `android/build.gradle`, add the following method and subproperty

```
// add this import
import groovy.json.JsonSlurper
...
allprojects {
  ...
}
// and this method
def getNodePackageVersion() {
    def packageFile = new File("$rootDir/../package.json")
    def packageJson = new JsonSlurper().parseText(packageFile.text)
    return packageJson["version"]
}
subprojects {
    // then this block
    ext {
        nodePackageVersion = getNodePackageVersion()
    }
    ...
}
```

It's used for passing property named nodePackageVersion in our `package.json`.

Edit the `android/app/build.gradle`

```
...
android {
    ...
    defaultConfig {
        ...
        versionName nodePackageVersion
    }
}
...
```

#### **_iOS_**

1.Open the iOS workspace in Xcode
2.Click on the project on left sidebar, go
to `Build Phases` , click on `+` button.
3.Select `New Run Script Phase` , name it `Set Version`.

4. Add this script

```
NODE_PACKAGE_VERSION="$(grep -w 'version":' "${SRCROOT}"/../package.json | cut -d '"' -f4)"
/usr/libexec/PlistBuddy -c "Set :CFBundleShortVersionString ${NODE_PACKAGE_VERSION}" "${TARGET_BUILD_DIR}/${INFOPLIST_PATH}"
```

\*It's used to edit the version inside `Info.plist`.
