# gvm-yum-install

## Prerequisites

To run this sample on local, you need the following:

1. The latest GraalVM Enterprise 22.x for Java 17 components:
    - JDK, and
    - Native Image

2. Maven

3. Check the versions installed using:

    ```shell
    $ echo $JAVA_HOME

    /Library/Java/JavaVirtualMachines/graalvm-ee-java17-22.1.0/Contents/Home
    ```

    ```shell
    $ echo $PATH

    /Library/Java/JavaVirtualMachines/graalvm-ee-java17-22.1.0/Contents/Home/bin:/usr/local/Cellar/maven/3.8.4/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
   ```

    ```shell
    $ java -version
    
    java version "17.0.3" 2022-04-19 LTS
    Java(TM) SE Runtime Environment GraalVM EE 22.1.0 (build 17.0.3+8-LTS-jvmci-22.1-b05)
    Java HotSpot(TM) 64-Bit Server VM GraalVM EE 22.1.0 (build 17.0.3+8-LTS-jvmci-22.1-b05, mixed mode, sharing)
    ```

    ```shell
    $ native-image --version
    
    GraalVM 22.1.0 Java 17 EE (Java Version 17.0.3+8-LTS-jvmci-22.1-b05)
    ```

    ```shell
    $ mvn --version

    Apache Maven 3.8.4 (9b656c72d54e5bacbed989b64718c159fe39b537)
    Maven home: /usr/local/Cellar/maven/3.8.4/libexec
    Java version: 17.0.3, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/graalvm-ee-java17-22.1.0/Contents/Home
    Default locale: en_US, platform encoding: UTF-8
    OS name: "mac os x", version: "12.4", arch: "x86_64", family: "mac"
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
    ================================================================================================
    GraalVM Native Image: Generating 'my-app' (executable)...
    ================================================================================================
    [1/7] Initializing...                                                            (5.0s @ 0.23GB)
    Version info: 'GraalVM 22.1.0 Java 17 EE'
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
    ...
    You enabled -Ob for this image build. This will configure some optimizations to reduce image build time.
    This feature should only be used during development and never for deployment.
    ================================================================================================
    GraalVM Native Image: Generating 'my-app' (executable)...
    ================================================================================================
    [1/7] Initializing...                                                            (8.0s @ 0.22GB)
    Version info: 'GraalVM 22.1.0 Java 17 EE'
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
