# Java Hello World with GraalVM Enterprise in Oracle Cloud Infrastructre Cloud Shell

This sample shows how you can get started quickly with GraalVM Enterprise Edition in Oracle Cloud Infrastructre (OCI) Cloud Shell. 

## What is GraalVM?

GraalVM is a high-performance JDK distribution that can accelerate any Java workload running on the HotSpot JVM.

GraalVM Native Image ahead-of-time compilation enables you to build lightweight Java applications that are smaller, faster, and use less memory and CPU. At build time, GraalVM Native Image analyzes a Java application and its dependencies to identify just what classes, methods, and fields are absolutely necessary and generates optimized machine code for just those elements.

GraalVM Enterprise Edition is available for use on Oracle Cloud Infrastructure (OCI) at no additional cost.

## What is Cloud Shell?

Cloud Shell is a free-to-use browser-based terminal accessible from the Oracle Cloud Console. It provides access to a Linux shell with pre-authenticated OCI CLI and other pre-installed developer tools. You can use the shell to interact with OCI resources, follow labs and tutorials, and quickly run commands. 


## Prerequisites

Cloud Shell comes with Maven and the following GraalVM Enterprise components preinstalled: 
- Java Development Kit (JDK), and
- Native Image 

1. Check the versions installed using:

    ```shell
    $ csruntimectl java list

      graalvmeejdk-17.0.4               /usr/lib/jvm/graalvm22-ee-java17
    * openjdk-11.0.15                   /usr/lib/jvm/java-11-openjdk-11.0.15.0.9-2.0.1.el7_9.x86_64
      openjdk-1.8.0.332                 /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.el7_9.x86_64
    ```

    ```shell
    $ csruntimectl java set graalvmeejdk-17.0.4

    The current managed java version is set to graalvmeejdk-17.0.4
    ```

    ```shell
    $ echo $JAVA_HOME

    /usr/lib/jvm/graalvm22-ee-java17
    ```

    ```shell
    $ echo $PATH

    /usr/lib/jvm/graalvm22-ee-java17:/usr/lib/jvm/java-11-openjdk-11.0.15.0.9-2.0.1.el7_9.x86_64/bin/:/ggs_client/usr/bin:/home/user_xyz/.yarn/bin:/home/user_xyz/.config/yarn/global/node_modules/.bin:/opt/oracle/sqlcl/bin:/usr/lib/oracle/21/client64/bin/:/home/oci/.pyenv/plugins/pyenv-virtualenv/shims:/home/oci/.pyenv/shims:/home/oci/.pyenv/bin:/opt/rh/rh-ruby27/root/usr/local/bin:/opt/rh/rh-ruby27/root/usr/bin:/opt/rh/rh-maven36/root/usr/bin:/opt/rh/rh-git227/root/usr/bin:/opt/rh/rh-dotnet31/root/usr/bin:/opt/rh/rh-dotnet31/root/usr/sbin:/opt/rh/httpd24/root/usr/bin:/opt/rh/httpd24/root/usr/sbin:/opt/rh/devtoolset-11/root/usr/bin:/home/oci/bin:/opt/gradle/gradle-7.4.2/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/user_xyz/.composer/vendor/bin:/opt/yarn-v1.22.17/bin:/home/user_xyz/.dotnet/tools
    ```

    ```shell
    $ java -version
    
    java version "17.0.4" 2022-07-19 LTS   
    Java(TM) SE Runtime Environment GraalVM EE 22.2.0 (build 17.0.4+11-LTS-jvmci-22.2-b05)   
    Java HotSpot(TM) 64-Bit Server VM GraalVM EE 22.2.0 (build 17.0.4+11-LTS-jvmci-22.2-b05, mixed mode, sharing)
    ```

    ```shell
    $ native-image --version
    
    GraalVM 22.2.0 Java 17 EE (Java Version 17.0.4+11-LTS-jvmci-22.2-b05)
    ```

    ```shell
    $ mvn --version

    Apache Maven 3.6.1 (Red Hat 3.6.1-6.3)
    Maven home: /opt/rh/rh-maven36/root/usr/share/maven
    Java version: 17.0.4, vendor: Oracle Corporation, runtime: /usr/lib64/graalvm/graalvm22-ee-java17   
    Default locale: en_US, platform encoding: ANSI_X3.4-1968
    OS name: "linux", version: "4.14.35-2047.513.2.2.el7uek.x86_64", arch: "amd64", family: "unix"
    ```


## Steps
1. Git clone this repo.

2. Build a JAR for the sample app.

    ```shell
    mvn clean package
    ```

3. Run the JAR using:

    ```shell
    java -jar target/my-app-1.0-SNAPSHOT.jar
    ```

    The output should be similar to:
    ```
    Hello World!
    ```

4. Run the GraalVM Native Image build to produce a native executable.

    ```shell
    mvn clean -Pnative -DskipTests package
    ```

    4.1) **Option 1:** With **Quick Build disabled** in the pom.xml, the output should be similar to:

    ```
    ...
    ##### PLACEHOLDER FOR GRAALVM - TO BE CONFIRMED WITH THE CLOUD SHELL TEAM #####
    ================================================================================================
    GraalVM Native Image: Generating 'my-app' (executable)...
    ================================================================================================
    [1/7] Initializing...                                                            (5.0s @ 0.23GB)
    Version info: 'GraalVM 22.2.0 Java 17 EE'
    C compiler: cc (apple, x86_64, 12.0.0)
    Garbage collector: Serial GC
    [2/7] Performing analysis...  [******]                                           (5.4s @ 0.34GB)
    1,812 (62.61%) of  2,894 classes reachable
    1,591 (45.63%) of  3,487 fields reachable
    7,487 (36.89%) of 20,295 methods reachable
        19 classes,     0 fields, and   281 methods registered for reflection
        48 classes,    33 fields, and    47 methods registered for JNI access
    [3/7] Building universe...                                                       (0.5s @ 0.46GB)
    [4/7] Parsing methods...      [*]                                                (0.6s @ 0.60GB)
    [5/7] Inlining methods...     [****]                                             (1.0s @ 0.97GB)
    [[6/7] Compiling methods...    [****]                                            (14.2s @ 0.98GB)
    [7/7] Creating image...                                                          (1.2s @ 1.17GB)
    2.52MB (46.55%) for code area:    3,593 compilation units
    2.43MB (45.01%) for image heap:     908 classes and 37,618 objects
    467.68KB ( 8.45%) for other data
    5.41MB in total
    ------------------------------------------------------------------------------------------------
    Top 10 packages in code area:                   Top 10 object types in image heap:
    323.40KB java.lang                              509.63KB byte[] for code metadata
    202.37KB java.util                              327.21KB byte[] for java.lang.String
    190.41KB com.oracle.svm.jni                     265.10KB java.lang.Class
    167.81KB com.oracle.svm.core.reflect            263.06KB java.lang.String
    136.51KB com.oracle.svm.core.code               215.28KB byte[] for general heap data
    118.18KB com.oracle.svm.core.genscavenge        111.71KB char[]
    115.42KB java.util.concurrent                    70.78KB c.o.svm.core.hub.DynamicHubCompanion
    78.68KB jdk.proxy4                              61.74KB byte[] for reflection metadata
    73.94KB java.math                               55.47KB java.lang.reflect.Method
    72.35KB com.oracle.svm.jni.functions            47.53KB java.util.HashMap$Node
        ... 91 additional packages                      ... 475 additional object types
                                (use GraalVM Dashboard to see all)
    ------------------------------------------------------------------------------------------------
                0.7s (2.3% of total time) in 17 GCs | Peak RSS: 2.98GB | CPU load: 3.21
    ------------------------------------------------------------------------------------------------
    Produced artifacts:
    /Users/graal/gvm-yum-install/target/my-app (executable)
    /Users/graal/gvm-yum-install/target/my-app.build_artifacts.txt
    ================================================================================================
    Finished generating 'my-app' in 28.8s.
    ...
    ```

    4.2) **Option 2:** With **Quick Build enabled** in the pom.xml, the output should be similar to:

    ```
    ##### PLACEHOLDER FOR GRAALVM - TO BE CONFIRMED WITH THE CLOUD SHELL TEAM #####
   ...
    You enabled -Ob for this image build. This will configure some optimizations to reduce image build time.
    This feature should only be used during development and never for deployment.
    ================================================================================================
    GraalVM Native Image: Generating 'my-app' (executable)...
    ================================================================================================
    [1/7] Initializing...                                                            (8.0s @ 0.22GB)
    Version info: 'GraalVM 22.2.0 Java 17 EE'
    C compiler: cc (apple, x86_64, 12.0.0)
    Garbage collector: Serial GC
    [2/7] Performing analysis...  [******]                                           (5.6s @ 0.30GB)
    1,773 (63.50%) of  2,792 classes reachable
    1,555 (45.24%) of  3,437 fields reachable
    7,492 (37.88%) of 19,778 methods reachable
        19 classes,     0 fields, and   281 methods registered for reflection
        48 classes,    33 fields, and    47 methods registered for JNI access
    [3/7] Building universe...                                                       (0.7s @ 0.43GB)
    [4/7] Parsing methods...      [*]                                                (0.6s @ 0.55GB)
    [5/7] Inlining methods...     [****]                                             (0.9s @ 0.93GB)
    [[6/7] Compiling methods...    [**]                                               (3.2s @ 1.08GB)
    [7/7] Creating image...                                                          (1.0s @ 1.23GB)
    1.74MB (38.61%) for code area:    4,376 compilation units
    2.22MB (49.44%) for image heap:     902 classes and 37,317 objects
    549.94KB (11.95%) for other data
    4.50MB in total
    ------------------------------------------------------------------------------------------------
    Top 10 packages in code area:                   Top 10 object types in image heap:
    205.16KB com.oracle.svm.jni                     363.07KB byte[] for code metadata
    186.64KB java.lang                              324.47KB byte[] for java.lang.String
    163.17KB com.oracle.svm.core.code               261.02KB java.lang.String
    144.41KB java.util                              260.02KB java.lang.Class
    100.31KB com.oracle.svm.core.reflect            213.83KB byte[] for general heap data
    91.98KB com.oracle.svm.core.genscavenge        111.71KB char[]
    78.09KB java.util.concurrent                    69.26KB c.o.svm.core.hub.DynamicHubCompanion
    53.65KB java.math                               60.30KB byte[] for reflection metadata
    46.17KB com.oracle.svm.jni.functions            55.47KB java.lang.reflect.Method
    33.42KB jdk.proxy4                              47.53KB java.util.HashMap$Node
        ... 93 additional packages                      ... 475 additional object types
                                (use GraalVM Dashboard to see all)
    ------------------------------------------------------------------------------------------------
                0.5s (2.5% of total time) in 13 GCs | Peak RSS: 1.82GB | CPU load: 2.49
    ------------------------------------------------------------------------------------------------
    Produced artifacts:
    /Users/graal/gvm-yum-install/target/my-app (executable)
    /Users/graal/gvm-yum-install/target/my-app.build_artifacts.txt
    ================================================================================================
    Finished generating 'my-app' in 20.7s.
    ...
    ```

5. Run the native executable using:

    ```shell
    ./target/my-app
    ```

    The output should be similar to:
    ```
    Hello World!
    ```
