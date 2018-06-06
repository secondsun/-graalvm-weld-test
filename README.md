# graalvm-weld-test

## Setup
 This requires the graalvm community edition which is linux only.  You can probably run it in a container or VM.
 This requires that you install some jars from GraalVM into your maven repository as follows :
```
export JAVA_HOME=YOUR_GRAAL_INSTALL
mvn install:install-file -Dfile=${JAVA_HOME}/jre/lib/svm/builder/svm.jar -DgroupId=com.oracle.svm -DartifactId=svm -Dversion=GraalVM-1.0.0-rc1 -Dpackaging=jar
```

## Building Native Image

The classpath and reflection options probably aren't required

```
mvn install

cd target

native-image -cp ./lib/weld-environment-common-3.0.4.Final.jar -cp ./lib/cdi-api-2.0.jar -cp ./lib/jboss-interceptors-api_1.2_spec-1.0.0.Final.jar -cp ./lib/jboss-el-api_3.0_spec-1.0.7.Final.jar -cp ./lib/jboss-classfilewriter-1.2.2.Final.jar -cp ./lib/javax.el-api-3.0.0.jar -cp ./lib/jboss-annotations-api_1.3_spec-1.0.0.Final.jar -cp ./lib/weld-api-3.0.SP3.jar -cp ./lib/weld-se-core-3.0.4.Final.jar -cp ./lib/weld-probe-core-3.0.4.Final.jar -cp ./lib/weld-core-impl-3.0.4.Final.jar -cp ./lib/slf4j-simple-1.7.22.jar -cp ./lib/slf4j-api-1.7.22.jar -cp ./lib/javax.inject-1.jar -cp ./lib/weld-spi-3.0.SP3.jar -cp ./lib/jboss-logging-3.2.1.Final.jar -cp ./lib/javax.interceptor-api-1.2.jar -cp lib/svm-GraalVM-1.0.0-rc1.jar -H:ReflectionConfigurationFiles=./classes/reflection.json  -jar ./original-graalvm-weld-test-1.0-SNAPSHOT.jar  
```

## Interesting Bits

Main2 starts up the application, ServiceLoader is copied from Java's ServiceLoader class and I added logging to figuring things out.  However, the magic that makes serviceLoader work is in the Main2Feature.java file
