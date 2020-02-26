[![Build Status](https://travis-ci.com/vmware/vcd-api-tools.svg?branch=master)](https://travis-ci.com/vmware/vcd-api-tools)

# vCloud Director API Tools #
The projects in this folder are tools for working with the vCloud Director APIs.

## Bindings Generator ##
A command line tool for generating Typescript and Python bindings for vCD's XML-based REST API is available in `bindings-generator`.  The bindings generator can be executed like any typical application.  For instance, execution through Maven:

For `Typescript`:
`mvn exec:java -Dexec.mainClass="com.vmware.vcloud.bindings.generator.BindingsGenerator" -Dexec.args="--help"`

For `Python`:
`mvn exec:java -Dexec.mainClass="com.vmware.vcloud.bindings.generator.PythonBindingsGenerator" -Dexec.args="--help"`

will output the usage info:

```
  Options:
    -h, --help
      Prints this help message
    -o, --outputDir
      A directory to output custom files to.  If none is specified, the output
      is streamed to standard out.
    -x, --overwrite
      If the specified outputDir is not empty the generator will halt.  Adding
      this flag will DELETE the content of outputDir and force the generator
      to continue
      Default: false
  * -p, --packages
      One or more packages to scan for schema classes.
```

There is also a Maven plugin available at `vcd-bindings-maven-plugin` to facilitate Typescript and Python generation as part of a Maven project lifecycle.  Typescript can be generated for specific packages by adding the following plugin to an existing POM file:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>com.vmware.vcloud</groupId>
            <artifactId>vcd-bindings-maven-plugin</artifactId>
            <version>1.0.0</version>
            <executions>
                <execution>
                    <phase>generate-sources</phase>
                    <goals>
                        <goal>generate-typescript</goal>
                    </goals>
                </execution>
            </executions>
            <configuration>
                <packages>
                    <package>com.vmware.vcloud.api.rest.schema.ovf</package>
                    <package>com.vmware.vcloud.api.rest.schema.ovf.environment</package>
                    <package>com.vmware.vcloud.api.rest.schema.ovf.vmware</package>
                    <package>com.vmware.vcloud.api.rest.schema.versioning</package>
                    <package>com.vmware.vcloud.api.rest.schema_v1_5</package>
                    <package>com.vmware.vcloud.api.rest.schema_v1_5.extension</package>
                </packages>
            </configuration>
            <dependencies>
                <dependency>
                    <groupId>com.vmware.vcloud</groupId>
                    <artifactId>rest-api-bindings</artifactId>
                    <version>9.1.0</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>
```

For python, you need to change the goal to `generate-python`, in the above plugin to an existing POM file, e.g., update `<goal>generate-typescript</goal>` to `<goal>generate-python</goal>`.

This will output a collection of classes, enums, and barrels that are ready for transpilation.
