# MAVEN-MVND-DEMO-561

This project is mainly used to reproduce the issues #561 of maven-mvnd.

## Test environment:

MySQL:

```shell
mysql --version
mysql  Ver 14.14 Distrib 5.7.31, for macos10.14 (x86_64) using  EditLine wrapper
```

Linux:

```shell
mvnd -version
mvnd native client 0.7.1-linux-amd64 (97c587c11383a67b5bd0ff8388bd94c694b91c1e)
Terminal: org.jline.terminal.impl.PosixSysTerminal with pty org.jline.terminal.impl.jansi.linux.LinuxNativePty
Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
Maven home: /home/suninformation/.sdkman/candidates/mvnd/0.7.1/mvn
Java version: 11.0.13, vendor: GraalVM Community, runtime: /home/suninformation/Downloads/graalvm-ce-java11-21.3.0
Default locale: zh_CN, platform encoding: UTF-8
OS name: "linux", version: "5.13.0-30-generic", arch: "amd64", family: "unix"
```

Mac:

```shell
mvnd --version
mvnd native client 0.7.1-darwin-amd64 (97c587c11383a67b5bd0ff8388bd94c694b91c1e)
Terminal: org.jline.terminal.impl.PosixSysTerminal with pty org.jline.terminal.impl.jansi.osx.OsXNativePty
Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
Maven home: /Users/suninformation/.sdkman/candidates/mvnd/current/mvn
Java version: 1.8.0_271, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk1.8.0_271.jdk/Contents/Home/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "mac os x", version: "10.16", arch: "x86_64", family: "mac"
```

## STEP 1:

You need to configure the connection information of MySQL 5.7 database.

Edit lines 161, 164, and 167 in the "ymp-conf.properties" file, for example:

```properties
# 数据库连接字符串, 必填参数
ymp.configs.persistence.jdbc.ds.default.connection_url=jdbc:mysql://10.211.55.3:3306/drc?useUnicode=true&useSSL=false&characterEncoding=UTF-8

# 数据库访问用户名称, 必填参数
ymp.configs.persistence.jdbc.ds.default.username=clientuser

# 数据库访问密码, 可选参数, 经过默认密码处理器加密后的admin字符串为wRI2rASW58E
ymp.configs.persistence.jdbc.ds.default.password=admin
```

Save the file!

## STEP 2:

Execute the maven command:

```shell
mvnd ymate:entity -DshowOnly=true
```

The same build fails in the second run [#561](https://github.com/apache/maven-mvnd/issues/561), and the console output is as follows:

```shell
[INFO] Processing build on daemon 33c62ea9
[INFO] Scanning for projects...
[INFO] BuildTimeEventSpy is registered.
[INFO] Task segments : [ymate:entity]
[INFO] Build maximum degree of concurrency is 1
[INFO] Total number of projects is 1
[INFO] 
[INFO] -----------------< net.ymate.demo:maven-mvnd-demo-561 >-----------------
[INFO] Building maven-mvnd-demo-561 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- ymate-maven-plugin:1.0.0:entity (default-cli) @ maven-mvnd-demo-561 ---
[INFO] Found and load the config file: /home/suninformation/Workspace/maven-mvnd-demo-561/src/main/resources/ymp-conf.properties
[INFO] Initializing ymate-platform-core-2.1.0-Release build-2022-02-20T10:00:47Z - debug:true - env:dev - PID:8167
[INFO] Initializing ymate-platform-persistence-jdbc-2.1.0-Release build-2022-02-20T10:00:47Z 
[INFO] RecycleHelper has registered the number of resources to be recycled: 0
[INFO] Initialization completed, Total time: 2ms
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.072 s
[INFO] Finished at: 2022-02-21T08:23:44-08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal net.ymate.maven.plugins:ymate-maven-plugin:1.0.0:entity (default-cli) on project maven-mvnd-demo-561: No suitable driver found for jdbc:mysql://10.211.55.3:3306/drc?useUnicode=true&useSSL=false&characterEncoding=UTF-8 -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
```
