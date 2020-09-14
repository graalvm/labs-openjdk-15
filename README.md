Welcome to LabsJDK CE 15.

The latest release is available at https://github.com/graalvm/labs-openjdk-15/releases/latest

This is a fork of https://github.com/openjdk/jdk15u that
exists for the purpose of building a base JDK upon which GraalVM CE 15 is built.

A labsjdk binary can be built as follows:
```
# Find latest jvmci-* tag in current branch.
JVMCI_VERSION=$(git log --decorate | grep -E 'tag: jvmci-\d+\.\d+-b\d+' | sed 's/.*(\(tag: .*\))/\1/g' | tr ',' '\n' | grep 'tag:' | sed 's/.*tag: \(jvmci-[^,)]*\).*/\1/g' | sort -nr | head -1)

# Add "-dev" suffix if not at $JVMCI_VERSION commit
test "$(git show --pretty=%H -s $JVMCI_VERSION)" = "$(git show --pretty=%H -s)" && JVMCI_VERSION="${JVMCI_VERSION}-dev"

# Configure and build
sh configure --with-conf-name=labsjdk \
    --enable-aot=no \
    --with-version-opt=$JVMCI_VERSION \
    --with-version-pre= \
    '--with-vendor-name=GraalVM Community' \
    --with-vendor-url=https://www.graalvm.org/ \
    --with-vendor-bug-url=https://github.com/oracle/graal/issues \
    --with-vendor-vm-bug-url=https://github.com/oracle/graal/issues
make CONF_NAME=labsjdk graal-builder-image
```
This will produce a labsjdk binary under `build/labsjdk/images/graal-builder-jdk`.

You can verify the labsjdk built successfully by checking the version reported by the `java` launcher:
```
./build/labsjdk/images/graal-builder-jdk/bin/java --version
```

The original JDK README is [here](README).
Further information on building JDK 15 is [here](doc/building.md).
