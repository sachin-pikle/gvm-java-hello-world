# gvm-yum-install

## Prerequisites

To run this sample on local you need the following software:

1. GraalVM Enterprise or Community 22.1 Java 17 - JDK and Native Image
2. Maven


## Steps
1. Git clone this repo.

2. Run `mvn clean package`

3. Run the java program using:

    ```shell
    java -jar target/my-app-1.0-SNAPSHOT.jar
    ```

    The output should be similar to:
    ```
    Hello World!
    ```

4. Run the GraalVM Native Image build.

    ```shell
    mvn clean -Pnative -DskipTests package
    ```

    The output should be similar to:

    ```
    ...
    You enabled -Ob for this image build. This will configure some optimizations to reduce image build time.
    This feature should only be used during development and never for deployment.
    ===============================================================================================
    GraalVM Native Image: Generating 'my-app' (executable)...
    ===============================================================================================
    [1/7] Initializing...                                                           (4.8s @ 0.18GB)
    Version info: 'GraalVM 22.1.0 Java 17 CE'
    C compiler: cc (apple, x86_64, 12.0.0)
    Garbage collector: Serial GC
    [2/7] Performing analysis...  [******]                                         (10.1s @ 0.93GB)
    2,837 (74.64%) of  3,801 classes reachable
    3,374 (50.78%) of  6,645 fields reachable
    12,855 (44.56%) of 28,848 methods reachable
        28 classes,     0 fields, and   346 methods registered for reflection
        57 classes,    59 fields, and    51 methods registered for JNI access
    [3/7] Building universe...                                                      (0.9s @ 1.12GB)
    [4/7] Parsing methods...      [*]                                               (0.9s @ 0.51GB)
    [5/7] Inlining methods...     [****]                                            (1.1s @ 1.24GB)
    [6/7] Compiling methods...    [[6/7] Compiling methods...    [***]                                             (6.8s @ 2.01GB)
    [7/7] Creating image...                                                         (1.9s @ 2.35GB)
    4.44MB (36.16%) for code area:    7,564 compilation units
    6.98MB (56.85%) for image heap:   1,698 classes and 92,832 objects
    879.97KB ( 6.99%) for other data
    12.29MB in total
    -----------------------------------------------------------------------------------------------
    Top 10 packages in code area:                  Top 10 object types in image heap:
    672.95KB java.util                             993.71KB byte[] for code metadata
    331.61KB java.lang                             974.49KB byte[] for general heap data
    272.23KB java.text                             875.38KB java.lang.String
    237.94KB java.util.regex                       642.23KB java.lang.Class
    233.21KB com.oracle.svm.jni                    530.28KB byte[] for java.lang.String
    186.82KB java.util.concurrent                  387.75KB java.util.HashMap$Node
    161.46KB com.oracle.svm.core.reflect           221.64KB c.o.svm.core.hub.DynamicHubCompanion
    154.43KB java.math                             167.69KB java.util.HashMap$Node[]
    102.69KB com.oracle.svm.core.genscavenge       162.71KB java.lang.String[]
    98.44KB java.util.logging                     156.05KB j.u.c.ConcurrentHashMap$Node
        ... 116 additional packages                    ... 772 additional object types
                                (use GraalVM Dashboard to see all)
    -----------------------------------------------------------------------------------------------
                2.0s (7.1% of total time) in 16 GCs | Peak RSS: 2.94GB | CPU load: 2.92
    -----------------------------------------------------------------------------------------------
    Produced artifacts:
    /Users/user1/gvm-yum-install/target/my-app (executable)
    /Users/user1/gvm-yum-install/target/my-app.build_artifacts.txt
    ===============================================================================================
    Finished generating 'my-app' in 27.8s.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  30.321 s
    [INFO] Finished at: 2022-05-24T15:38:56+05:30
    [INFO] ------------------------------------------------------------------------
    ```

5. Run the native executable using:

    ```shell
    ./target/my-app
    ```

    The output should be similar to:
    ```
    Hello World!
    ```
