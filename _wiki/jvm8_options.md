---
layout: wiki
title: Java8 JVM 参数大全
categories: wiki
description: Java8 JVM 参数大全
keywords: Java8 JVM 参数大全
---

[源文地址](http://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html)

# java 8 参数大全
Launches a Java application.


## Description

The `java` command starts a Java application. It does this by starting the Java Runtime Environment (JRE), loading the specified class, and calling that class's `main()` method. The method must be declared _public_ and _static_, it must not return any value, and it must accept a `String` array as a parameter. The method declaration has the following form:

<pre dir="ltr" xml:space="preserve">public static void main(String[] args)
</pre>

The `java` command can be used to launch a JavaFX application by loading a class that either has a `main()` method or that extends `javafx.application.Application`. In the latter case, the launcher constructs an instance of the `Application` class, calls its `init()` method, and then calls the `start(javafx.stage.Stage)` method.

By default, the first argument that is not an option of the `java` command is the fully qualified name of the class to be called. If the `-jar` option is specified, its argument is the name of the JAR file containing class and resource files for the application. The startup class must be indicated by the `Main-Class` manifest header in its source code.

The JRE searches for the startup class (and other classes used by the application) in three sets of locations: the bootstrap class path, the installed extensions, and the user's class path.

Arguments after the class file name or the JAR file name are passed to the `main()` method.


## Options

The `java` command supports a wide range of options that can be divided into the following categories:

</a>

<a id="CBBIJCHG" name="CBBIJCHG">
</a>*   <a id="CBBIJCHG" name="CBBIJCHG">
</a>

    <a id="CBBIJCHG" name="CBBIJCHG"></a>[Standard Options](#BABDJJFI)

*   [Non-Standard Options](#BABHDABI)

*   [Advanced Runtime Options](#BABCBGHF)

*   [Advanced JIT Compiler Options](#BABDDFII)

*   [Advanced Serviceability Options](#BABFJDIC)

*   [Advanced Garbage Collection Options](#BABFAFAE)

Standard options are guaranteed to be supported by all implementations of the Java Virtual Machine (JVM). They are used for common actions, such as checking the version of the JRE, setting the class path, enabling verbose output, and so on.

Non-standard options are general purpose options that are specific to the Java HotSpot Virtual Machine, so they are not guaranteed to be supported by all JVM implementations, and are subject to change. These options start with `-X`.

Advanced options are not recommended for casual use. These are developer options used for tuning specific areas of the Java HotSpot Virtual Machine operation that often have specific system requirements and may require privileged access to system configuration parameters. They are also not guaranteed to be supported by all JVM implementations, and are subject to change. Advanced options start with `-XX`.

To keep track of the options that were deprecated or removed in the latest release, there is a section named [Deprecated and Removed Options](#BABDCEGG) at the end of the document.

Boolean options are used to either enable a feature that is disabled by default or disable a feature that is enabled by default. Such options do not require a parameter. Boolean `-XX` options are enabled using the plus sign (`-XX:+`_OptionName_) and disabled using the minus sign (`-XX:-`_OptionName_).

For options that require an argument, the argument may be separated from the option name by a space, a colon (:), or an equal sign (=), or the argument may directly follow the option (the exact syntax differs for each option). If you are expected to specify the size in bytes, you can use no suffix, or use the suffix `k` or `K` for kilobytes (KB), `m` or `M` for megabytes (MB), `g` or `G` for gigabytes (GB). For example, to set the size to 8 GB, you can specify either `8g`, `8192m`, `8388608k`, or `8589934592` as the argument. If you are expected to specify the percentage, use a number from 0 to 1 (for example, specify `0.25` for 25%).

<div><a id="BABDJJFI" name="BABDJJFI">

### Standard Options

These are the most commonly used options that are supported by all implementations of the JVM.

</a><dl><a id="BABDJJFI" name="BABDJJFI">
<dt>-agentlib:_libname_[=_options_]</dt>
</a><dd><a id="BABDJJFI" name="BABDJJFI">

Loads the specified native agent library. After the library name, a comma-separated list of options specific to the library can be used.

If the option `-agentlib:foo` is specified, then the JVM attempts to load the library named `libfoo.so` in the location specified by the `LD_LIBRARY_PATH` system variable (on OS X this variable is `DYLD_LIBRARY_PATH`).

The following example shows how to load the heap profiling tool (HPROF) library and get sample CPU information every 20 ms, with a stack depth of 3:

<pre dir="ltr" xml:space="preserve">-agentlib:hprof=cpu=samples,interval=20,depth=3
</pre>

The following example shows how to load the Java Debug Wire Protocol (JDWP) library and listen for the socket connection on port 8000, suspending the JVM before the main class loads:

<pre dir="ltr" xml:space="preserve">-agentlib:jdwp=transport=dt_socket,server=y,address=8000
</pre>

For more information about the native agent libraries, refer to the following:

</a>

<a id="BABDJJFI" name="BABDJJFI">
</a>*   <a id="BABDJJFI" name="BABDJJFI">
</a>

    <a id="BABDJJFI" name="BABDJJFI">The `java.lang.instrument` package description at </a>`[http://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html](http://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html)`

*   Agent Command Line Options in the JVM Tools Interface guide at `[http://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html#starting](http://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html#starting)`
</dd>
<dt>-agentpath:_pathname_[=_options_]</dt>
<dd>

Loads the native agent library specified by the absolute path name. This option is equivalent to `-agentlib` but uses the full path and file name of the library.

</dd>
<dt>-client</dt>
<dd>

Selects the Java HotSpot Client VM. The 64-bit version of the Java SE Development Kit (JDK) currently ignores this option and instead uses the Server JVM.

For default JVM selection, see Server-Class Machine Detection at

`[http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html)`

</dd>
<dt>-D_property_=_value_</dt>
<dd>

Sets a system property value. The _property_ variable is a string with no spaces that represents the name of the property. The _value_ variable is a string that represents the value of the property. If _value_ is a string with spaces, then enclose it in quotation marks (for example `-Dfoo="foo bar"`).

</dd>
<dt>-d32</dt>
<dd>

Runs the application in a 32-bit environment. If a 32-bit environment is not installed or is not supported, then an error will be reported. By default, the application is run in a 32-bit environment unless a 64-bit system is used.

</dd>
<dt>-d64</dt>
<dd>

Runs the application in a 64-bit environment. If a 64-bit environment is not installed or is not supported, then an error will be reported. By default, the application is run in a 32-bit environment unless a 64-bit system is used.

Currently only the Java HotSpot Server VM supports 64-bit operation, and the `-server` option is implicit with the use of `-d64`. The `-client` option is ignored with the use of `-d64`. This is subject to change in a future release.

</dd>
<dt>-disableassertions[:[_packagename_]...|:_classname_]</dt>
<dt>-da[:[_packagename_]...|:_classname_]</dt>
<dd>

Disables assertions. By default, assertions are disabled in all packages and classes.

With no arguments, `-disableassertions` (`-da`) disables assertions in all packages and classes. With the _packagename_ argument ending in `...`, the switch disables assertions in the specified package and any subpackages. If the argument is simply `...`, then the switch disables assertions in the unnamed package in the current working directory. With the _classname_ argument, the switch disables assertions in the specified class.

The `-disableassertions` (`-da`) option applies to all class loaders and to system classes (which do not have a class loader). There is one exception to this rule: if the option is provided with no arguments, then it does not apply to system classes. This makes it easy to disable assertions in all classes except for system classes. The `-disablesystemassertions` option enables you to disable assertions in all system classes.

To explicitly enable assertions in specific packages or classes, use the `-enableassertions` (`-ea`) option. Both options can be used at the same time. For example, to run the `MyClass` application with assertions enabled in package `com.wombat.fruitbat` (and any subpackages) but disabled in class `com.wombat.fruitbat.Brickbat`, use the following command:

<pre dir="ltr" xml:space="preserve">java -ea:com.wombat.fruitbat... -da:com.wombat.fruitbat.Brickbat MyClass
</pre></dd>
<dt>-disablesystemassertions</dt>
<dt>-dsa</dt>
<dd>

Disables assertions in all system classes.

</dd>
<dt>-enableassertions[:[_packagename_]...|:_classname_]</dt>
<dt>-ea[:[_packagename_]...|:_classname_]</dt>
<dd>

Enables assertions. By default, assertions are disabled in all packages and classes.

With no arguments, `-enableassertions` (`-ea`) enables assertions in all packages and classes. With the _packagename_ argument ending in `...`, the switch enables assertions in the specified package and any subpackages. If the argument is simply `...`, then the switch enables assertions in the unnamed package in the current working directory. With the _classname_ argument, the switch enables assertions in the specified class.

The `-enableassertions` (`-ea`) option applies to all class loaders and to system classes (which do not have a class loader). There is one exception to this rule: if the option is provided with no arguments, then it does not apply to system classes. This makes it easy to enable assertions in all classes except for system classes. The `-enablesystemassertions` option provides a separate switch to enable assertions in all system classes.

To explicitly disable assertions in specific packages or classes, use the `-disableassertions` (`-da`) option. If a single command contains multiple instances of these switches, then they are processed in order before loading any classes. For example, to run the `MyClass` application with assertions enabled only in package `com.wombat.fruitbat` (and any subpackages) but disabled in class `com.wombat.fruitbat.Brickbat`, use the following command:

<pre dir="ltr" xml:space="preserve">java -ea:com.wombat.fruitbat... -da:com.wombat.fruitbat.Brickbat MyClass
</pre></dd>
<dt>-enablesystemassertions</dt>
<dt>-esa</dt>
<dd>

Enables assertions in all system classes.

</dd>
<dt>-help</dt>
<dt>-?</dt>
<dd>

Displays usage information for the `java` command without actually running the JVM.

</dd>
<dt>-jar _filename_</dt>
<dd>

Executes a program encapsulated in a JAR file. The _filename_ argument is the name of a JAR file with a manifest that contains a line in the form `Main-Class:``<span>classname</span>` that defines the class with the `public static void main(String[] args)` method that serves as your application's starting point.

When you use the `-jar` option, the specified JAR file is the source of all user classes, and other class path settings are ignored.

For more information about JAR files, see the following resources:

*   [`jar`(1)](jar.html#BGBEJEEG)

*   The Java Archive (JAR) Files guide at `[http://docs.oracle.com/javase/8/docs/technotes/guides/jar/index.html](http://docs.oracle.com/javase/8/docs/technotes/guides/jar/index.html)`

*   Lesson: Packaging Programs in JAR Files at

    `[http://docs.oracle.com/javase/tutorial/deployment/jar/index.html](http://docs.oracle.com/javase/tutorial/deployment/jar/index.html)`
</dd>
<dt>-javaagent:_jarpath_[=_options_]</dt>
<dd>

Loads the specified Java programming language agent. For more information about instrumenting Java applications, see the `java.lang.instrument` package description in the Java API documentation at `[http://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html](http://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html)`

</dd>
<dt>-jre-restrict-search</dt>
<dd>

Includes user-private JREs in the version search.

</dd>
<dt>-no-jre-restrict-search</dt>
<dd>

Excludes user-private JREs from the version search.

</dd>
<dt>-server</dt>
<dd>

Selects the Java HotSpot Server VM. The 64-bit version of the JDK supports only the Server VM, so in that case the option is implicit.

For default JVM selection, see Server-Class Machine Detection at

`[http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html)`

</dd>
<dt>-showversion</dt>
<dd>

Displays version information and continues execution of the application. This option is equivalent to the `-version` option except that the latter instructs the JVM to exit after displaying version information.

</dd>
<dt>-splash:_imgname_</dt>
<dd>

Shows the splash screen with the image specified by _imgname_. For example, to show the `splash.gif` file from the `images` directory when starting your application, use the following option:

<pre dir="ltr" xml:space="preserve">-splash:images/splash.gif
</pre></dd>
<dt>-verbose:class</dt>
<dd>

Displays information about each loaded class.

</dd>
<dt>-verbose:gc</dt>
<dd>

Displays information about each garbage collection (GC) event.

</dd>
<dt>-verbose:jni</dt>
<dd>

Displays information about the use of native methods and other Java Native Interface (JNI) activity.

</dd>
<dt>-version</dt>
<dd>

Displays version information and then exits. This option is equivalent to the `-showversion` option except that the latter does not instruct the JVM to exit after displaying version information.

</dd>
<dt>-version:_release_</dt>
<dd>

Specifies the release version to be used for running the application. If the version of the `java` command called does not meet this specification and an appropriate implementation is found on the system, then the appropriate implementation will be used.

The _release_ argument specifies either the exact version string, or a list of version strings and ranges separated by spaces. A _version string_ is the developer designation of the version number in the following form: `1.`_x_`.0_`_u_ (where _x_ is the major version number, and _u_ is the update version number). A _version range_ is made up of a version string followed by a plus sign (`+`) to designate this version or later, or a part of a version string followed by an asterisk (`*`) to designate any version string with a matching prefix. Version strings and ranges can be combined using a space for a logical _OR_ combination, or an ampersand (`&amp;`) for a logical _AND_ combination of two version strings/ranges. For example, if running the class or JAR file requires either JRE 6u13 (1.6.0_13), or any JRE 6 starting from 6u10 (1.6.0_10), specify the following:

<pre dir="ltr" xml:space="preserve">-version:"1.6.0_13 1.6* &amp; 1.6.0_10+"
</pre>

Quotation marks are necessary only if there are spaces in the _release_ parameter.

For JAR files, the preference is to specify version requirements in the JAR file manifest rather than on the command line.

</dd>
</dl>
</div>

<div><a id="BABHDABI" name="BABHDABI">

### Non-Standard Options

These options are general purpose options that are specific to the Java HotSpot Virtual Machine.

</a><dl><a id="BABHDABI" name="BABHDABI">
<dt>-X</dt>
<dd>

Displays help for all available `-X` options.

</dd>
<dt>-Xbatch</dt>
<dd>

Disables background compilation. By default, the JVM compiles the method as a background task, running the method in interpreter mode until the background compilation is finished. The `-Xbatch` flag disables background compilation so that compilation of all methods proceeds as a foreground task until completed.

This option is equivalent to `-XX:-BackgroundCompilation`.

</dd>
<dt>-Xbootclasspath:_path_</dt>
<dd>

Specifies a list of directories, JAR files, and ZIP archives separated by colons (:) to search for boot class files. These are used in place of the boot class files included in the JDK.

Do not deploy applications that use this option to override a class in `rt.jar`, because this violates the JRE binary code license.

</dd>
<dt>-Xbootclasspath/a:_path_</dt>
<dd>

Specifies a list of directories, JAR files, and ZIP archives separated by colons (:) to append to the end of the default bootstrap class path.

Do not deploy applications that use this option to override a class in `rt.jar`, because this violates the JRE binary code license.

</dd>
<dt>-Xbootclasspath/p:_path_</dt>
<dd>

Specifies a list of directories, JAR files, and ZIP archives separated by colons (:) to prepend to the front of the default bootstrap class path.

Do not deploy applications that use this option to override a class in `rt.jar`, because this violates the JRE binary code license.

</dd>
<dt>-Xcheck:jni</dt>
<dd>

Performs additional checks for Java Native Interface (JNI) functions. Specifically, it validates the parameters passed to the JNI function and the runtime environment data before processing the JNI request. Any invalid data encountered indicates a problem in the native code, and the JVM will terminate with an irrecoverable error in such cases. Expect a performance degradation when this option is used.

</dd>
<dt>-Xcomp</dt>
<dd>

Forces compilation of methods on first invocation. By default, the Client VM (`-client`) performs 1,000 interpreted method invocations and the Server VM (`-server`) performs 10,000 interpreted method invocations to gather information for efficient compilation. Specifying the `-Xcomp` option disables interpreted method invocations to increase compilation performance at the expense of efficiency.

You can also change the number of interpreted method invocations before compilation using the `-XX:CompileThreshold` option.

</dd>
<dt>-Xdebug</dt>
<dd>

Does nothing. Provided for backward compatibility.

</dd>
<dt>-Xdiag</dt>
<dd>

Shows additional diagnostic messages.

</dd>
<dt>-Xfuture</dt>
<dd>

Enables strict class-file format checks that enforce close conformance to the class-file format specification. Developers are encouraged to use this flag when developing new code because the stricter checks will become the default in future releases.

</dd>
<dt>-Xint</dt>
<dd>

Runs the application in interpreted-only mode. Compilation to native code is disabled, and all bytecode is executed by the interpreter. The performance benefits offered by the just in time (JIT) compiler are not present in this mode.

</dd>
<dt>-Xinternalversion</dt>
<dd>

Displays more detailed JVM version information than the `-version` option, and then exits.

</dd>
<dt>-Xloggc:_filename_</dt>
<dd>

Sets the file to which verbose GC events information should be redirected for logging. The information written to this file is similar to the output of `-verbose:gc` with the time elapsed since the first GC event preceding each logged event. The `-Xloggc` option overrides `-verbose:gc` if both are given with the same `java` command.

Example:

<pre dir="ltr" xml:space="preserve">-Xloggc:garbage-collection.log
</pre></dd>
<dt>-Xmaxjitcodesize=_size_</dt>
<dd>

Specifies the maximum code cache size (in bytes) for JIT-compiled code. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The default maximum code cache size is 240 MB; if you disable tiered compilation with the option `-XX:-TieredCompilation`, then the default size is 48 MB:

<pre dir="ltr" xml:space="preserve">-Xmaxjitcodesize=240m
</pre>

This option is equivalent to `-XX:ReservedCodeCacheSize`.

</dd>
<dt>-Xmixed</dt>
<dd>

Executes all bytecode by the interpreter except for hot methods, which are compiled to native code.

</dd>
<dt>-Xmn_size_</dt>
<dd>

Sets the initial and maximum size (in bytes) of the heap for the young generation (nursery). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes.

The young generation region of the heap is used for new objects. GC is performed in this region more often than in other regions. If the size for the young generation is too small, then a lot of minor garbage collections will be performed. If the size is too large, then only full garbage collections will be performed, which can take a long time to complete. Oracle recommends that you keep the size for the young generation between a half and a quarter of the overall heap size.

The following examples show how to set the initial and maximum size of young generation to 256 MB using various units:

<pre dir="ltr" xml:space="preserve">-Xmn256m
-Xmn262144k
-Xmn268435456
</pre>

Instead of the `-Xmn` option to set both the initial and maximum size of the heap for the young generation, you can use `-XX:NewSize` to set the initial size and `-XX:MaxNewSize` to set the maximum size.

</dd>
<dt>-Xms_size_</dt>
<dd>

Sets the initial size (in bytes) of the heap. This value must be a multiple of 1024 and greater than 1 MB. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes.

The following examples show how to set the size of allocated memory to 6 MB using various units:

<pre dir="ltr" xml:space="preserve">-Xms6291456
-Xms6144k
-Xms6m
</pre>

If you do not set this option, then the initial size will be set as the sum of the sizes allocated for the old generation and the young generation. The initial size of the heap for the young generation can be set using the `-Xmn` option or the `-XX:NewSize` option.

</dd>
<dt>-Xmx_size_</dt>
</a><dd><a id="BABHDABI" name="BABHDABI">
</a>

<a id="BABHDABI" name="BABHDABI">Specifies the maximum size (in bytes) of the memory allocation pool in bytes. This value must be a multiple of 1024 and greater than 2 MB. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The default value is chosen at runtime based on system configuration. For server deployments, `-Xms` and `-Xmx` are often set to the same value. See the section "Ergonomics" in _Java SE HotSpot Virtual Machine Garbage Collection Tuning Guide_ at </a>`[http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html)`.

The following examples show how to set the maximum allowed size of allocated memory to 80 MB using various units:

<pre dir="ltr" xml:space="preserve">-Xmx83886080
-Xmx81920k
-Xmx80m
</pre>

The `-Xmx` option is equivalent to `-XX:MaxHeapSize`.

</dd>
<dt>-Xnoclassgc</dt>
<dd>

Disables garbage collection (GC) of classes. This can save some GC time, which shortens interruptions during the application run.

When you specify `-Xnoclassgc` at startup, the class objects in the application will be left untouched during GC and will always be considered live. This can result in more memory being permanently occupied which, if not used carefully, will throw an out of memory exception.

</dd>
<dt>-Xprof</dt>
<dd>

Profiles the running program and sends profiling data to standard output. This option is provided as a utility that is useful in program development and is not intended to be used in production systems.

</dd>
<dt>-Xrs</dt>
<dd>

Reduces the use of operating system signals by the JVM.

Shutdown hooks enable orderly shutdown of a Java application by running user cleanup code (such as closing database connections) at shutdown, even if the JVM terminates abruptly.

The JVM catches signals to implement shutdown hooks for unexpected termination. The JVM uses `SIGHUP`, `SIGINT`, and `SIGTERM` to initiate the running of shutdown hooks.

The JVM uses a similar mechanism to implement the feature of dumping thread stacks for debugging purposes. The JVM uses `SIGQUIT` to perform thread dumps.

Applications embedding the JVM frequently need to trap signals such as `SIGINT` or `SIGTERM`, which can lead to interference with the JVM signal handlers. The `-Xrs` option is available to address this issue. When `-Xrs` is used, the signal masks for `SIGINT`, `SIGTERM`, `SIGHUP`, and `SIGQUIT` are not changed by the JVM, and signal handlers for these signals are not installed.

There are two consequences of specifying `-Xrs`:

*   `SIGQUIT` thread dumps are not available.

*   User code is responsible for causing shutdown hooks to run, for example, by calling `System.exit()` when the JVM is to be terminated.
</dd>
<dt>-Xshare:_mode_</dt>
<dd>

Sets the class data sharing (CDS) mode. Possible _mode_ arguments for this option include the following:

<dl>
<dt>auto</dt>
<dd>

Use CDS if possible. This is the default value for Java HotSpot 32-Bit Client VM.

</dd>
<dt>on</dt>
<dd>

Require the use of CDS. Print an error message and exit if class data sharing cannot be used.

</dd>
<dt>off</dt>
<dd>

Do not use CDS. This is the default value for Java HotSpot 32-Bit Server VM, Java HotSpot 64-Bit Client VM, and Java HotSpot 64-Bit Server VM.

</dd>
<dt>dump</dt>
<dd>

Manually generate the CDS archive. Specify the application class path as described in ["Setting the Class Path"](classpath.html#CBHHCGFB).

You should regenerate the CDS archive with each new JDK release.

</dd>
</dl>
</dd>
<dt>-XshowSettings:_category_</dt>
<dd>

Shows settings and continues. Possible _category_ arguments for this option include the following:

<dl>
<dt>all</dt>
<dd>

Shows all categories of settings. This is the default value.

</dd>
<dt>locale</dt>
<dd>

Shows settings related to locale.

</dd>
<dt>properties</dt>
<dd>

Shows settings related to system properties.

</dd>
<dt>vm</dt>
<dd>

Shows the settings of the JVM.

</dd>
</dl>
</dd>
<dt>-Xss_size_</dt>
<dd>

Sets the thread stack size (in bytes). Append the letter `k` or `K` to indicate KB, `m` or `M` to indicate MB, `g` or `G` to indicate GB. The default value depends on the platform:

*   Linux/ARM (32-bit): 320 KB

*   Linux/i386 (32-bit): 320 KB

*   Linux/x64 (64-bit): 1024 KB

*   OS X (64-bit): 1024 KB

*   Oracle Solaris/i386 (32-bit): 320 KB

*   Oracle Solaris/x64 (64-bit): 1024 KB

The following examples set the thread stack size to 1024 KB in different units:

<pre dir="ltr" xml:space="preserve">-Xss1m
-Xss1024k
-Xss1048576
</pre>

This option is equivalent to `-XX:ThreadStackSize`.

</dd>
<dt>-Xusealtsigs</dt>
<dd>

Use alternative signals instead of `SIGUSR1` and `SIGUSR2` for JVM internal signals. This option is equivalent to `-XX:+UseAltSigs`.

</dd>
<dt>-Xverify:_mode_</dt>
<dd>

Sets the mode of the bytecode verifier. Bytecode verification ensures that class files are properly formed and satisfy the constraints listed in _The Java Virtual Machine Specification_; see section 4.10, "Verification of `class` Files":

`[https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html#jvms-4.10](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html#jvms-4.10)`

Do not turn off verification when running dynamically generated or untrusted class files as this reduces the protection provided by Java and makes diagnosing problems with ill-formed bytecodes more difficult.

Possible _mode_ arguments for this option include the following:

<dl>
<dt>remote</dt>
<dd>

Verifies all bytecodes not loaded by the bootstrap class loader. This is the default behavior.

</dd>
<dt>all</dt>
<dd>

Enables verification of all bytecodes.

</dd>
<dt>none</dt>
<dd>

Disables verification of all bytecodes.

</dd>
</dl>
</dd>
</dl>
</div>

<div><a id="BABCBGHF" name="BABCBGHF">

### Advanced Runtime Options

These options control the runtime behavior of the Java HotSpot VM.

</a><dl><a id="BABCBGHF" name="BABCBGHF">
<dt>-XX:+CheckEndorsedAndExtDirs</dt>
<dd>

Enables the option to prevent the `java` command from running a Java application if it uses the endorsed-standards override mechanism or the extension mechanism. This option checks if an application is using one of these mechanisms by checking the following:

*   The `java.ext.dirs` or `java.endorsed.dirs` system property is set.

*   The `lib/endorsed` directory exists and is not empty.

*   The `lib/ext` directory contains any JAR files other than those of the JDK.

*   The system-wide platform-specific extension directory contains any JAR files.
</dd>
<dt>-XX:+DisableAttachMechanism</dt>
<dd>

Enables the option that disables the mechanism that lets tools attach to the JVM. By default, this option is disabled, meaning that the attach mechanism is enabled and you can use tools such as `jcmd`, `jstack`, `jmap`, and `jinfo`.

</dd>
<dt>-XX:ErrorFile=_filename_</dt>
<dd>

Specifies the path and file name to which error data is written when an irrecoverable error occurs. By default, this file is created in the current working directory and named `hs_err_pid`_pid_`.log` where _pid_ is the identifier of the process that caused the error. The following example shows how to set the default log file (note that the identifier of the process is specified as `%p`):

<pre dir="ltr" xml:space="preserve">-XX:ErrorFile=./hs_err_pid%p.log
</pre>

The following example shows how to set the error log to `/var/log/java/java_error.log`:

<pre dir="ltr" xml:space="preserve">-XX:ErrorFile=/var/log/java/java_error.log
</pre>

If the file cannot be created in the specified directory (due to insufficient space, permission problem, or another issue), then the file is created in the temporary directory for the operating system. The temporary directory is `/tmp`.

</dd>
<dt>-XX:+FailOverToOldVerifier</dt>
<dd>

Enables automatic failover to the old verifier when the new type checker fails. By default, this option is disabled and it is ignored (that is, treated as disabled) for classes with a recent bytecode version. You can enable it for classes with older versions of the bytecode.

</dd>
<dt>-XX:+FlightRecorder</dt>
<dd>

Enables the use of the Java Flight Recorder (JFR) during the runtime of the application. This is a commercial feature that works in conjunction with the `-XX:+UnlockCommercialFeatures` option as follows:

<pre dir="ltr" xml:space="preserve">java -XX:+UnlockCommercialFeatures -XX:+FlightRecorder
</pre>

If this option is not provided, Java Flight Recorder can still be enabled in a running JVM by providing the appropriate `jcmd` diagnostic commands.

</dd>
<dt>-XX:-FlightRecorder</dt>
<dd>

Disables the use of the Java Flight Recorder (JFR) during the runtime of the application. This is a commercial feature that works in conjunction with the `-XX:+UnlockCommercialFeatures` option as follows:

<pre dir="ltr" xml:space="preserve">java -XX:+UnlockCommercialFeatures -XX:-FlightRecorder
</pre>

If this option is provided, Java Flight Recorder cannot be enabled in a running JVM.

</dd>
<dt>-XX:FlightRecorderOptions=_parameter_=_value_</dt>
<dd>

Sets the parameters that control the behavior of JFR. This is a commercial feature that works in conjunction with the `-XX:+UnlockCommercialFeatures` option. This option can be used only when JFR is enabled (that is, the `-XX:+FlightRecorder` option is specified).

The following list contains all available JFR parameters:

<dl>
<dt>defaultrecording={true|false}</dt>
<dd>

Specifies whether the recording is a continuous background recording or if it runs for a limited time. By default, this parameter is set to `false` (recording runs for a limited time). To make the recording run continuously, set the parameter to `true`.

</dd>
<dt>disk={true|false}</dt>
<dd>

Specifies whether JFR should write a continuous recording to disk. By default, this parameter is set to `false` (continuous recording to disk is disabled). To enable it, set the parameter to `true`, and also set `defaultrecording=true`.

</dd>
<dt>dumponexit={true|false}</dt>
<dd>

Specifies whether a dump file of JFR data should be generated when the JVM terminates in a controlled manner. By default, this parameter is set to `false` (dump file on exit is not generated). To enable it, set the parameter to `true`, and also set `defaultrecording=true`.

The dump file is written to the location defined by the `dumponexitpath` parameter.

</dd>
<dt>dumponexitpath=_path_</dt>
<dd>

Specifies the path and name of the dump file with JFR data that is created when the JVM exits in a controlled manner if you set the `dumponexit=true` parameter. Setting the path makes sense only if you also set `defaultrecording=true`.

If the specified path is a directory, the JVM assigns a file name that shows the creation date and time. If the specified path includes a file name and if that file already exists, the JVM creates a new file by appending the date and time stamp to the specified file name.

</dd>
<dt>globalbuffersize=_size_</dt>
<dd>

Specifies the total amount of primary memory (in bytes) used for data retention. Append `k` or `K`, to specify the size in KB, `m` or `M` to specify the size in MB, `g` or `G` to specify the size in GB. By default, the size is set to 462848 bytes.

</dd>
<dt>loglevel={quiet|error|warning|info|debug|trace}</dt>
<dd>

Specify the amount of data written to the log file by JFR. By default, it is set to `info`.

</dd>
<dt>maxage=_time_</dt>
<dd>

Specifies the maximum age of disk data to keep for the default recording. Append `s` to specify the time in seconds, `m` for minutes, `h` for hours, or `d` for days (for example, specifying `30s` means 30 seconds). By default, the maximum age is set to 15 minutes (`15m`).

This parameter is valid only if you set the `disk=true` parameter.

</dd>
<dt>maxchunksize=_size_</dt>
<dd>

Specifies the maximum size (in bytes) of the data chunks in a recording. Append `k` or `K`, to specify the size in KB, `m` or `M` to specify the size in MB, `g` or `G` to specify the size in GB. By default, the maximum size of data chunks is set to 12 MB.

</dd>
<dt>maxsize=_size_</dt>
<dd>

Specifies the maximum size (in bytes) of disk data to keep for the default recording. Append `k` or `K`, to specify the size in KB, `m` or `M` to specify the size in MB, `g` or `G` to specify the size in GB. By default, the maximum size of disk data is not limited, and this parameter is set to 0.

This parameter is valid only if you set the `disk=true` parameter.

</dd>
<dt>repository=_path_</dt>
<dd>

Specifies the repository (a directory) for temporary disk storage. By default, the system's temporary directory is used.

</dd>
<dt>samplethreads={true|false}</dt>
<dd>

Specifies whether thread sampling is enabled. Thread sampling occurs only if the sampling event is enabled along with this parameter. By default, this parameter is enabled.

</dd>
<dt>settings=_path_</dt>
<dd>

Specifies the path and name of the event settings file (of type JFC). By default, the `default.jfc` file is used, which is located in `JAVA_HOME/jre/lib/jfr`.

</dd>
<dt>stackdepth=_depth_</dt>
<dd>

Stack depth for stack traces by JFR. By default, the depth is set to 64 method calls. The maximum is 2048, minimum is 1.

</dd>
<dt>threadbuffersize=_size_</dt>
<dd>

Specifies the per-thread local buffer size (in bytes). Append `k` or `K`, to specify the size in KB, `m` or `M` to specify the size in MB, `g` or `G` to specify the size in GB. Higher values for this parameter allow more data gathering without contention to flush it to the global storage. It can increase application footprint in a thread-rich environment. By default, the local buffer size is set to 5 KB.

</dd>
</dl>

You can specify values for multiple parameters by separating them with a comma. For example, to instruct JFR to write a continuous recording to disk, and set the maximum size of data chunks to 10 MB, specify the following:

<pre dir="ltr" xml:space="preserve">-XX:FlightRecorderOptions=defaultrecording=true,disk=true,maxchunksize=10M
</pre></dd>
<dt>-XX:LargePageSizeInBytes=_size_</dt>
<dd>

On Solaris, sets the maximum size (in bytes) for large pages used for Java heap. The _size_ argument must be a power of 2 (2, 4, 8, 16, ...). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. By default, the size is set to 0, meaning that the JVM chooses the size for large pages automatically.

The following example illustrates how to set the large page size to 4 megabytes (MB):

<pre dir="ltr" xml:space="preserve">-XX:LargePageSizeInBytes=4m
</pre></dd>
<dt>-XX:MaxDirectMemorySize=_size_</dt>
<dd>

Sets the maximum total size (in bytes) of the New I/O (the `java.nio` package) direct-buffer allocations. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. By default, the size is set to 0, meaning that the JVM chooses the size for NIO direct-buffer allocations automatically.

The following examples illustrate how to set the NIO size to 1024 KB in different units:

<pre dir="ltr" xml:space="preserve">-XX:MaxDirectMemorySize=1m
-XX:MaxDirectMemorySize=1024k
-XX:MaxDirectMemorySize=1048576
</pre></dd>
<dt>-XX:NativeMemoryTracking=_mode_</dt>
<dd>

Specifies the mode for tracking JVM native memory usage. Possible _mode_ arguments for this option include the following:

<dl>
<dt>off</dt>
<dd>

Do not track JVM native memory usage. This is the default behavior if you do not specify the `-XX:NativeMemoryTracking` option.

</dd>
<dt>summary</dt>
<dd>

Only track memory usage by JVM subsystems, such as Java heap, class, code, and thread.

</dd>
<dt>detail</dt>
<dd>

In addition to tracking memory usage by JVM subsystems, track memory usage by individual `CallSite`, individual virtual memory region and its committed regions.

</dd>
</dl>
</dd>
<dt>-XX:ObjectAlignmentInBytes=_alignment_</dt>
<dd>

Sets the memory alignment of Java objects (in bytes). By default, the value is set to 8 bytes. The specified value should be a power of two, and must be within the range of 8 and 256 (inclusive). This option makes it possible to use compressed pointers with large Java heap sizes.

The heap size limit in bytes is calculated as:

`4GB * ObjectAlignmentInBytes`

Note: As the alignment value increases, the unused space between objects will also increase. As a result, you may not realize any benefits from using compressed pointers with large Java heap sizes.

</dd>
<dt>-XX:OnError=_string_</dt>
<dd>

Sets a custom command or a series of semicolon-separated commands to run when an irrecoverable error occurs. If the string contains spaces, then it must be enclosed in quotation marks.

The following example shows how the `-XX:OnError` option can be used to run the `gcore` command to create the core image, and the debugger is started to attach to the process in case of an irrecoverable error (the `%p` designates the current process):

<pre dir="ltr" xml:space="preserve">-XX:OnError="gcore %p;dbx - %p"
</pre></dd>
<dt>-XX:OnOutOfMemoryError=_string_</dt>
<dd>

Sets a custom command or a series of semicolon-separated commands to run when an `OutOfMemoryError` exception is first thrown. If the string contains spaces, then it must be enclosed in quotation marks. For an example of a command string, see the description of the `-XX:OnError` option.

</dd>
<dt>-XX:+PerfDataSaveToFile</dt>
</a><dd><a id="BABCBGHF" name="BABCBGHF">
</a>

<a id="BABCBGHF" name="BABCBGHF">If enabled, saves </a>[`jstat`(1)](jstat.html#BEHBBBDJ) binary data when the Java application exits. This binary data is saved in a file named `hsperfdata_``<span>&lt;pid&gt;</span>`, where `<span>&lt;pid&gt;</span>` is the process identifier of the Java application you ran. Use `jstat` to display the performance data contained in this file as follows:

<pre dir="ltr" xml:space="preserve">jstat -class file:///<span>&lt;path&gt;</span>/hsperfdata_<span>&lt;pid&gt;</span>
jstat -gc file:///<span>&lt;path&gt;</span>/hsperfdata_<span>&lt;pid&gt;</span>
</pre></dd>
<dt>-XX:+PrintCommandLineFlags</dt>
<dd>

Enables printing of ergonomically selected JVM flags that appeared on the command line. It can be useful to know the ergonomic values set by the JVM, such as the heap space size and the selected garbage collector. By default, this option is disabled and flags are not printed.

</dd>
<dt>-XX:+PrintNMTStatistics</dt>
<dd>

Enables printing of collected native memory tracking data at JVM exit when native memory tracking is enabled (see `-XX:NativeMemoryTracking`). By default, this option is disabled and native memory tracking data is not printed.

</dd>
<dt>-XX:+RelaxAccessControlCheck</dt>
<dd>

Decreases the amount of access control checks in the verifier. By default, this option is disabled, and it is ignored (that is, treated as disabled) for classes with a recent bytecode version. You can enable it for classes with older versions of the bytecode.

</dd>
<dt>-XX:+ResourceManagement</dt>
<dd>

Enables the use of Resource Management during the runtime of the application.

This is a commercial feature that requires you to also specify the `-XX:+UnlockCommercialFeatures` option as follows:

`java -XX:+UnlockCommercialFeatures -XX:+ResourceManagement`

</dd>
<dt>-XX:ResourceManagementSampleInterval=_value_ (milliseconds)</dt>
<dd>

Sets the parameter that controls the sampling interval for Resource Management measurements, in milliseconds.

This option can be used only when Resource Management is enabled (that is, the `-XX:+ResourceManagement` option is specified).

</dd>
<dt>-XX:SharedArchiveFile=_path_</dt>
<dd>

Specifies the path and name of the class data sharing (CDS) archive file

</dd>
<dt>-XX:SharedClassListFile=_file_name_</dt>
<dd>

Specifies the text file that contains the names of the class files to store in the class data sharing (CDS) archive. This file contains the full name of one class file per line, except slashes (`/`) replace dots (`.`). For example, to specify the classes `java.lang.Object` and `hello.Main`, create a text file that contains the following two lines:

<pre dir="ltr" xml:space="preserve">java/lang/Object
hello/Main
</pre>

The class files that you specify in this text file should include the classes that are commonly used by the application. They may include any classes from the application, extension, or bootstrap class paths.

</dd>
<dt>-XX:+ShowMessageBoxOnError</dt>
<dd>

Enables displaying of a dialog box when the JVM experiences an irrecoverable error. This prevents the JVM from exiting and keeps the process active so that you can attach a debugger to it to investigate the cause of the error. By default, this option is disabled.

</dd>
<dt>-XX:StartFlightRecording=_parameter_=_value_</dt>
<dd>

Starts a JFR recording for the Java application. This is a commercial feature that works in conjunction with the `-XX:+UnlockCommercialFeatures` option. This option is equivalent to the `JFR.start` diagnostic command that starts a recording during runtime. You can set the following parameters when starting a JFR recording:

<dl>
<dt>compress={true|false}</dt>
<dd>

Specifies whether to compress the JFR recording log file (of type JFR) on the disk using the `gzip` file compression utility. This parameter is valid only if the `filename` parameter is specified. By default it is set to `false` (recording is not compressed). To enable compression, set the parameter to `true`.

</dd>
<dt>defaultrecording={true|false}</dt>
<dd>

Specifies whether the recording is a continuous background recording or if it runs for a limited time. By default, this parameter is set to `false` (recording runs for a limited time). To make the recording run continuously, set the parameter to `true`.

</dd>
<dt>delay=_time_</dt>
<dd>

Specifies the delay between the Java application launch time and the start of the recording. Append `s` to specify the time in seconds, `m` for minutes, `h` for hours, or `d` for days (for example, specifying `10m` means 10 minutes). By default, there is no delay, and this parameter is set to 0.

</dd>
<dt>dumponexit={true|false}</dt>
<dd>

Specifies whether a dump file of JFR data should be generated when the JVM terminates in a controlled manner. By default, this parameter is set to `false` (dump file on exit is not generated). To enable it, set the parameter to `true`.

The dump file is written to the location defined by the `filename` parameter.

Example:

<pre dir="ltr" xml:space="preserve">-XX:StartFlightRecording=name=test,filename=D:\test.jfr,dumponexit=true
</pre></dd>
<dt>duration=_time_</dt>
<dd>

Specifies the duration of the recording. Append `s` to specify the time in seconds, `m` for minutes, `h` for hours, or `d` for days (for example, specifying `5h` means 5 hours). By default, the duration is not limited, and this parameter is set to 0.

</dd>
<dt>filename=_path_</dt>
<dd>

Specifies the path and name of the JFR recording log file.

</dd>
<dt>name=_identifier_</dt>
<dd>

Specifies the identifier for the JFR recording. By default, it is set to `Recording x`.

</dd>
<dt>maxage=_time_</dt>
<dd>

Specifies the maximum age of disk data to keep for the default recording. Append `s` to specify the time in seconds, `m` for minutes, `h` for hours, or `d` for days (for example, specifying `30s` means 30 seconds). By default, the maximum age is set to 15 minutes (`15m`).

</dd>
<dt>maxsize=_size_</dt>
<dd>

Specifies the maximum size (in bytes) of disk data to keep for the default recording. Append `k` or `K`, to specify the size in KB, `m` or `M` to specify the size in MB, `g` or `G` to specify the size in GB. By default, the maximum size of disk data is not limited, and this parameter is set to 0.

</dd>
<dt>settings=_path_</dt>
<dd>

Specifies the path and name of the event settings file (of type JFC). By default, the `default.jfc` file is used, which is located in `JAVA_HOME/jre/lib/jfr`.

</dd>
</dl>

You can specify values for multiple parameters by separating them with a comma. For example, to save the recording to test.jfr in the current working directory, and instruct JFR to compress the log file, specify the following:

<pre dir="ltr" xml:space="preserve">-XX:StartFlightRecording=filename=test.jfr,compress=true
</pre></dd>
<dt>-XX:ThreadStackSize=_size_</dt>
<dd>

Sets the thread stack size (in bytes). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The default value depends on the platform:

*   Linux/ARM (32-bit): 320 KB

*   Linux/i386 (32-bit): 320 KB

*   Linux/x64 (64-bit): 1024 KB

*   OS X (64-bit): 1024 KB

*   Oracle Solaris/i386 (32-bit): 320 KB

*   Oracle Solaris/x64 (64-bit): 1024 KB

The following examples show how to set the thread stack size to 1024 KB in different units:

<pre dir="ltr" xml:space="preserve">-XX:ThreadStackSize=1m
-XX:ThreadStackSize=1024k
-XX:ThreadStackSize=1048576
</pre>

This option is equivalent to `-Xss`.

</dd>
<dt>-XX:+TraceClassLoading</dt>
<dd>

Enables tracing of classes as they are loaded. By default, this option is disabled and classes are not traced.

</dd>
<dt>-XX:+TraceClassLoadingPreorder</dt>
<dd>

Enables tracing of all loaded classes in the order in which they are referenced. By default, this option is disabled and classes are not traced.

</dd>
<dt>-XX:+TraceClassResolution</dt>
<dd>

Enables tracing of constant pool resolutions. By default, this option is disabled and constant pool resolutions are not traced.

</dd>
<dt>-XX:+TraceClassUnloading</dt>
<dd>

Enables tracing of classes as they are unloaded. By default, this option is disabled and classes are not traced.

</dd>
<dt>-XX:+TraceLoaderConstraints</dt>
<dd>

Enables tracing of the loader constraints recording. By default, this option is disabled and loader constraints recording is not traced.

</dd>
<dt>-XX:+UnlockCommercialFeatures</dt>
<dd>

Enables the use of commercial features. Commercial features are included with Oracle Java SE Advanced or Oracle Java SE Suite packages, as defined on the _Java SE Products_ page at `[http://www.oracle.com/technetwork/java/javase/terms/products/index.html](http://www.oracle.com/technetwork/java/javase/terms/products/index.html)`

By default, this option is disabled and the JVM runs without the commercial features. Once they were enabled for a JVM process, it is not possible to disable their use for that process.

If this option is not provided, commercial features can still be unlocked in a running JVM by using the appropriate `jcmd` diagnostic commands.

</dd>
<dt>-XX:+UseAltSigs</dt>
<dd>

Enables the use of alternative signals instead of `SIGUSR1` and `SIGUSR2` for JVM internal signals. By default, this option is disabled and alternative signals are not used. This option is equivalent to `-Xusealtsigs`.

</dd>
<dt>-XX:+UseAppCDS</dt>
<dd>

Enables application class data sharing (AppCDS). To use AppCDS, you must also specify values for the options `-XX:SharedClassListFile` and `-XX:SharedArchiveFile` during both CDS dump time (see the option `-Xshare:dump`) and application run time.

This is a commercial feature that requires you to also specify the `-XX:+UnlockCommercialFeatures` option. This is also an experimental feature; it may change in future releases.

See ["Application Class Data Sharing"](#app_class_data_sharing).

</dd>
<dt>-XX:-UseBiasedLocking</dt>
<dd>

Disables the use of biased locking. Some applications with significant amounts of uncontended synchronization may attain significant speedups with this flag enabled, whereas applications with certain patterns of locking may see slowdowns. For more information about the biased locking technique, see the example in Java Tuning White Paper at `[http://www.oracle.com/technetwork/java/tuning-139912.html#section4.2.5](http://www.oracle.com/technetwork/java/tuning-139912.html#section4.2.5)`

By default, this option is enabled.

</dd>
<dt>-XX:-UseCompressedOops</dt>
<dd>

Disables the use of compressed pointers. By default, this option is enabled, and compressed pointers are used when Java heap sizes are less than 32 GB. When this option is enabled, object references are represented as 32-bit offsets instead of 64-bit pointers, which typically increases performance when running the application with Java heap sizes less than 32 GB. This option works only for 64-bit JVMs.

It is also possible to use compressed pointers when Java heap sizes are greater than 32GB. See the `-XX:ObjectAlignmentInBytes` option.

</dd>
<dt>-XX:+UseHugeTLBFS</dt>
<dd>

This option for Linux is the equivalent of specifying `-XX:+UseLargePages`. This option is disabled by default. This option pre-allocates all large pages up-front, when memory is reserved; consequently the JVM cannot dynamically grow or shrink large pages memory areas; see `-XX:UseTransparentHugePages` if you want this behavior.

For more information, see ["Large Pages"](#large_pages).

</dd>
<dt>-XX:+UseLargePages</dt>
<dd>

Enables the use of large page memory. By default, this option is disabled and large page memory is not used.

For more information, see ["Large Pages"](#large_pages).

</dd>
<dt>-XX:+UseMembar</dt>
<dd>

Enables issuing of membars on thread state transitions. This option is disabled by default on all platforms except ARM servers, where it is enabled. (It is recommended that you do not disable this option on ARM servers.)

</dd>
<dt>-XX:+UsePerfData</dt>
<dd>

Enables the `perfdata` feature. This option is enabled by default to allow JVM monitoring and performance testing. Disabling it suppresses the creation of the `hsperfdata_userid` directories. To disable the `perfdata` feature, specify `-XX:-UsePerfData`.

</dd>
<dt>-XX:+UseTransparentHugePages</dt>
<dd>

On Linux, enables the use of large pages that can dynamically grow or shrink. This option is disabled by default. You may encounter performance problems with transparent huge pages as the OS moves other pages around to create huge pages; this option is made available for experimentation.

For more information, see ["Large Pages"](#large_pages).

</dd>
<dt>-XX:+AllowUserSignalHandlers</dt>
<dd>

Enables installation of signal handlers by the application. By default, this option is disabled and the application is not allowed to install signal handlers.

</dd>
</dl>
</div>

<div><a id="BABDDFII" name="BABDDFII">

### Advanced JIT Compiler Options

These options control the dynamic just-in-time (JIT) compilation performed by the Java HotSpot VM.

<dl>
<dt>-XX:+AggressiveOpts</dt>
<dd>

Enables the use of aggressive performance optimization features, which are expected to become default in upcoming releases. By default, this option is disabled and experimental performance features are not used.

</dd>
<dt>-XX:AllocateInstancePrefetchLines=_lines_</dt>
<dd>

Sets the number of lines to prefetch ahead of the instance allocation pointer. By default, the number of lines to prefetch is set to 1:

<pre dir="ltr" xml:space="preserve">-XX:AllocateInstancePrefetchLines=1
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:AllocatePrefetchDistance=_size_</dt>
<dd>

Sets the size (in bytes) of the prefetch distance for object allocation. Memory about to be written with the value of new objects is prefetched up to this distance starting from the address of the last allocated object. Each Java thread has its own allocation point.

Negative values denote that prefetch distance is chosen based on the platform. Positive values are bytes to prefetch. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The default value is set to -1.

The following example shows how to set the prefetch distance to 1024 bytes:

<pre dir="ltr" xml:space="preserve">-XX:AllocatePrefetchDistance=1024
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:AllocatePrefetchInstr=_instruction_</dt>
<dd>

Sets the prefetch instruction to prefetch ahead of the allocation pointer. Only the Java HotSpot Server VM supports this option. Possible values are from 0 to 3. The actual instructions behind the values depend on the platform. By default, the prefetch instruction is set to 0:

<pre dir="ltr" xml:space="preserve">-XX:AllocatePrefetchInstr=0
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:AllocatePrefetchLines=_lines_</dt>
<dd>

Sets the number of cache lines to load after the last object allocation by using the prefetch instructions generated in compiled code. The default value is 1 if the last allocated object was an instance, and 3 if it was an array.

The following example shows how to set the number of loaded cache lines to 5:

<pre dir="ltr" xml:space="preserve">-XX:AllocatePrefetchLines=5
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:AllocatePrefetchStepSize=_size_</dt>
<dd>

Sets the step size (in bytes) for sequential prefetch instructions. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. By default, the step size is set to 16 bytes:

<pre dir="ltr" xml:space="preserve">-XX:AllocatePrefetchStepSize=16
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:AllocatePrefetchStyle=_style_</dt>
<dd>

Sets the generated code style for prefetch instructions. The _style_ argument is an integer from 0 to 3:

<dl>
<dt>0</dt>
<dd>

Do not generate prefetch instructions.

</dd>
<dt>1</dt>
<dd>

Execute prefetch instructions after each allocation. This is the default parameter.

</dd>
<dt>2</dt>
<dd>

Use the thread-local allocation block (TLAB) watermark pointer to determine when prefetch instructions are executed.

</dd>
<dt>3</dt>
<dd>

Use BIS instruction on SPARC for allocation prefetch.

</dd>
</dl>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:+BackgroundCompilation</dt>
<dd>

Enables background compilation. This option is enabled by default. To disable background compilation, specify `-XX:-BackgroundCompilation` (this is equivalent to specifying `-Xbatch`).

</dd>
<dt>-XX:CICompilerCount=_threads_</dt>
<dd>

Sets the number of compiler threads to use for compilation. By default, the number of threads is set to 2 for the server JVM, to 1 for the client JVM, and it scales to the number of cores if tiered compilation is used. The following example shows how to set the number of threads to 2:

<pre dir="ltr" xml:space="preserve">-XX:CICompilerCount=2
</pre></dd>
<dt>-XX:CodeCacheMinimumFreeSpace=_size_</dt>
<dd>

Sets the minimum free space (in bytes) required for compilation. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. When less than the minimum free space remains, compiling stops. By default, this option is set to 500 KB. The following example shows how to set the minimum free space to 1024 MB:

<pre dir="ltr" xml:space="preserve">-XX:CodeCacheMinimumFreeSpace=1024m
</pre></dd>
<dt>-XX:CompileCommand=_command_,_method_[,_option_]</dt>
<dd>

Specifies a command to perform on a method. For example, to exclude the `indexOf()` method of the `String` class from being compiled, use the following:

<pre dir="ltr" xml:space="preserve">-XX:CompileCommand=exclude,java/lang/String.indexOf
</pre>

Note that the full class name is specified, including all packages and subpackages separated by a slash (`/`). For easier cut and paste operations, it is also possible to use the method name format produced by the `-XX:+PrintCompilation` and `-XX:+LogCompilation` options:

<pre dir="ltr" xml:space="preserve">-XX:CompileCommand=exclude,java.lang.String::indexOf
</pre>

If the method is specified without the signature, the command will be applied to all methods with the specified name. However, you can also specify the signature of the method in the class file format. In this case, you should enclose the arguments in quotation marks, because otherwise the shell treats the semicolon as command end. For example, if you want to exclude only the `indexOf(String)` method of the `String` class from being compiled, use the following:

<pre dir="ltr" xml:space="preserve">-XX:CompileCommand="exclude,java/lang/String.indexOf,(Ljava/lang/String;)I"
</pre>

You can also use the asterisk (*) as a wildcard for class and method names. For example, to exclude all `indexOf()` methods in all classes from being compiled, use the following:

<pre dir="ltr" xml:space="preserve">-XX:CompileCommand=exclude,*.indexOf
</pre>

The commas and periods are aliases for spaces, making it easier to pass compiler commands through a shell. You can pass arguments to `-XX:CompileCommand` using spaces as separators by enclosing the argument in quotation marks:

<pre dir="ltr" xml:space="preserve">-XX:CompileCommand="exclude java/lang/String indexOf"
</pre>

Note that after parsing the commands passed on the command line using the `-XX:CompileCommand` options, the JIT compiler then reads commands from the `.hotspot_compiler` file. You can add commands to this file or specify a different file using the `-XX:CompileCommandFile` option.

To add several commands, either specify the `-XX:CompileCommand` option multiple times, or separate each argument with the newline separator (`\n`). The following commands are available:

<dl>
<dt>break</dt>
<dd>

Set a breakpoint when debugging the JVM to stop at the beginning of compilation of the specified method.

</dd>
<dt>compileonly</dt>
<dd>

Exclude all methods from compilation except for the specified method. As an alternative, you can use the `-XX:CompileOnly` option, which allows to specify several methods.

</dd>
<dt>dontinline</dt>
<dd>

Prevent inlining of the specified method.

</dd>
<dt>exclude</dt>
<dd>

Exclude the specified method from compilation.

</dd>
<dt>help</dt>
<dd>

Print a help message for the `-XX:CompileCommand` option.

</dd>
<dt>inline</dt>
<dd>

Attempt to inline the specified method.

</dd>
<dt>log</dt>
<dd>

Exclude compilation logging (with the `-XX:+LogCompilation` option) for all methods except for the specified method. By default, logging is performed for all compiled methods.

</dd>
<dt>option</dt>
<dd>

This command can be used to pass a JIT compilation option to the specified method in place of the last argument (_option_). The compilation option is set at the end, after the method name. For example, to enable the `BlockLayoutByFrequency` option for the `append()` method of the `StringBuffer` class, use the following:

<pre dir="ltr" xml:space="preserve">-XX:CompileCommand=option,java/lang/StringBuffer.append,BlockLayoutByFrequency
</pre>

You can specify multiple compilation options, separated by commas or spaces.

</dd>
<dt>print</dt>
<dd>

Print generated assembler code after compilation of the specified method.

</dd>
<dt>quiet</dt>
<dd>

Do not print the compile commands. By default, the commands that you specify with the -`XX:CompileCommand` option are printed; for example, if you exclude from compilation the `indexOf()` method of the `String` class, then the following will be printed to standard output:

<pre dir="ltr" xml:space="preserve">CompilerOracle: exclude java/lang/String.indexOf
</pre>

You can suppress this by specifying the `-XX:CompileCommand=quiet` option before other `-XX:CompileCommand` options.

</dd>
</dl>
</dd>
<dt>-XX:CompileCommandFile=_filename_</dt>
<dd>

Sets the file from which JIT compiler commands are read. By default, the `.hotspot_compiler` file is used to store commands performed by the JIT compiler.

Each line in the command file represents a command, a class name, and a method name for which the command is used. For example, this line prints assembly code for the `toString()` method of the `String` class:

<pre dir="ltr" xml:space="preserve">print java/lang/String toString
</pre>

For more information about specifying the commands for the JIT compiler to perform on methods, see the `-XX:CompileCommand` option.

</dd>
<dt>-XX:CompileOnly=_methods_</dt>
<dd>

Sets the list of methods (separated by commas) to which compilation should be restricted. Only the specified methods will be compiled. Specify each method with the full class name (including the packages and subpackages). For example, to compile only the `length()` method of the `String` class and the `size()` method of the `List` class, use the following:

<pre dir="ltr" xml:space="preserve">-XX:CompileOnly=java/lang/String.length,java/util/List.size
</pre>

Note that the full class name is specified, including all packages and subpackages separated by a slash (`/`). For easier cut and paste operations, it is also possible to use the method name format produced by the `-XX:+PrintCompilation` and `-XX:+LogCompilation` options:

<pre dir="ltr" xml:space="preserve">-XX:CompileOnly=java.lang.String::length,java.util.List::size
</pre>

Although wildcards are not supported, you can specify only the class or package name to compile all methods in that class or package, as well as specify just the method to compile methods with this name in any class:

<pre dir="ltr" xml:space="preserve">-XX:CompileOnly=java/lang/String
-XX:CompileOnly=java/lang
-XX:CompileOnly=.length
</pre></dd>
<dt>-XX:CompileThreshold=_invocations_</dt>
<dd>

Sets the number of interpreted method invocations before compilation. By default, in the server JVM, the JIT compiler performs 10,000 interpreted method invocations to gather information for efficient compilation. For the client JVM, the default setting is 1,500 invocations. This option is ignored when tiered compilation is enabled; see the option `-XX:+TieredCompilation`. The following example shows how to set the number of interpreted method invocations to 5,000:

<pre dir="ltr" xml:space="preserve">-XX:CompileThreshold=5000
</pre>

You can completely disable interpretation of Java methods before compilation by specifying the `-Xcomp` option.

</dd>
<dt>-XX:+DoEscapeAnalysis</dt>
<dd>

Enables the use of escape analysis. This option is enabled by default. To disable the use of escape analysis, specify `-XX:-DoEscapeAnalysis`. Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:InitialCodeCacheSize=_size_</dt>
<dd>

Sets the initial code cache size (in bytes). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The default value is set to 500 KB. The initial code cache size should be not less than the system's minimal memory page size. The following example shows how to set the initial code cache size to 32 KB:

<pre dir="ltr" xml:space="preserve">-XX:InitialCodeCacheSize=32k
</pre></dd>
<dt>-XX:+Inline</dt>
<dd>

Enables method inlining. This option is enabled by default to increase performance. To disable method inlining, specify `-XX:-Inline`.

</dd>
<dt>-XX:InlineSmallCode=_size_</dt>
<dd>

Sets the maximum code size (in bytes) for compiled methods that should be inlined. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. Only compiled methods with the size smaller than the specified size will be inlined. By default, the maximum code size is set to 1000 bytes:

<pre dir="ltr" xml:space="preserve">-XX:InlineSmallCode=1000
</pre></dd>
<dt>-XX:+LogCompilation</dt>
<dd>

Enables logging of compilation activity to a file named `hotspot.log` in the current working directory. You can specify a different log file path and name using the `-XX:LogFile` option.

By default, this option is disabled and compilation activity is not logged. The `-XX:+LogCompilation` option has to be used together with the `-XX:UnlockDiagnosticVMOptions` option that unlocks diagnostic JVM options.

You can enable verbose diagnostic output with a message printed to the console every time a method is compiled by using the `-XX:+PrintCompilation` option.

</dd>
<dt>-XX:MaxInlineSize=_size_</dt>
<dd>

Sets the maximum bytecode size (in bytes) of a method to be inlined. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. By default, the maximum bytecode size is set to 35 bytes:

<pre dir="ltr" xml:space="preserve">-XX:MaxInlineSize=35
</pre></dd>
<dt>-XX:MaxNodeLimit=_nodes_</dt>
<dd>

Sets the maximum number of nodes to be used during single method compilation. By default, the maximum number of nodes is set to 65,000:

<pre dir="ltr" xml:space="preserve">-XX:MaxNodeLimit=65000
</pre></dd>
<dt>-XX:MaxTrivialSize=_size_</dt>
<dd>

Sets the maximum bytecode size (in bytes) of a trivial method to be inlined. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. By default, the maximum bytecode size of a trivial method is set to 6 bytes:

<pre dir="ltr" xml:space="preserve">-XX:MaxTrivialSize=6
</pre></dd>
<dt>-XX:+OptimizeStringConcat</dt>
<dd>

Enables the optimization of `String` concatenation operations. This option is enabled by default. To disable the optimization of `String` concatenation operations, specify `-XX:-OptimizeStringConcat`. Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:+PrintAssembly</dt>
<dd>

Enables printing of assembly code for bytecoded and native methods by using the external `disassembler.so` library. This enables you to see the generated code, which may help you to diagnose performance issues.

By default, this option is disabled and assembly code is not printed. The `-XX:+PrintAssembly` option has to be used together with the `-XX:UnlockDiagnosticVMOptions` option that unlocks diagnostic JVM options.

</dd>
<dt>-XX:+PrintCompilation</dt>
<dd>

Enables verbose diagnostic output from the JVM by printing a message to the console every time a method is compiled. This enables you to see which methods actually get compiled. By default, this option is disabled and diagnostic output is not printed.

You can also log compilation activity to a file by using the `-XX:+LogCompilation` option.

</dd>
<dt>-XX:+PrintInlining</dt>
<dd>

Enables printing of inlining decisions. This enables you to see which methods are getting inlined.

By default, this option is disabled and inlining information is not printed. The `-XX:+PrintInlining` option has to be used together with the `-XX:+UnlockDiagnosticVMOptions` option that unlocks diagnostic JVM options.

</dd>
<dt>-XX:ReservedCodeCacheSize=_size_</dt>
<dd>

Sets the maximum code cache size (in bytes) for JIT-compiled code. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The default maximum code cache size is 240 MB; if you disable tiered compilation with the option `-XX:-TieredCompilation`, then the default size is 48 MB. This option has a limit of 2 GB; otherwise, an error is generated. The maximum code cache size should not be less than the initial code cache size; see the option `-XX:InitialCodeCacheSize`. This option is equivalent to `-Xmaxjitcodesize`.

</dd>
<dt>-XX:RTMAbortRatio=_abort_ratio_</dt>
<dd>

The RTM abort ratio is specified as a percentage (%) of all executed RTM transactions. If a number of aborted transactions becomes greater than this ratio, then the compiled code will be deoptimized. This ratio is used when the `-XX:+UseRTMDeopt` option is enabled. The default value of this option is 50. This means that the compiled code will be deoptimized if 50% of all transactions are aborted.

</dd>
<dt>-XX:RTMRetryCount=_number_of_retries_</dt>
<dd>

RTM locking code will be retried, when it is aborted or busy, the number of times specified by this option before falling back to the normal locking mechanism. The default value for this option is 5. The `-XX:UseRTMLocking` option must be enabled.

</dd>
<dt>-XX:-TieredCompilation</dt>
<dd>

Disables the use of tiered compilation. By default, this option is enabled. Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:+UseAES</dt>
<dd>

Enables hardware-based AES intrinsics for Intel, AMD, and SPARC hardware. Intel Westmere (2010 and newer), AMD Bulldozer (2011 and newer), and SPARC (T4 and newer) are the supported hardware. UseAES is used in conjunction with UseAESIntrinsics.

</dd>
<dt>-XX:+UseAESIntrinsics</dt>
<dd>

UseAES and UseAESIntrinsics flags are enabled by default and are supported only for Java HotSpot Server VM 32-bit and 64-bit. To disable hardware-based AES intrinsics, specify `-XX:-UseAES -XX:-UseAESIntrinsics`. For example, to enable hardware AES, use the following flags:

<pre dir="ltr" xml:space="preserve">-XX:+UseAES -XX:+UseAESIntrinsics
</pre>

To support UseAES and UseAESIntrinsics flags for 32-bit and 64-bit use `-server` option to choose Java HotSpot Server VM. These flags are not supported on Client VM.

</dd>
<dt>-XX:+UseCodeCacheFlushing</dt>
<dd>

Enables flushing of the code cache before shutting down the compiler. This option is enabled by default. To disable flushing of the code cache before shutting down the compiler, specify `-XX:-UseCodeCacheFlushing`.

</dd>
<dt>-XX:+UseCondCardMark</dt>
<dd>

Enables checking of whether the card is already marked before updating the card table. This option is disabled by default and should only be used on machines with multiple sockets, where it will increase performance of Java applications that rely heavily on concurrent operations. Only the Java HotSpot Server VM supports this option.

</dd>
<dt>-XX:+UseRTMDeopt</dt>
<dd>

Auto-tunes RTM locking depending on the abort ratio. This ratio is specified by `-XX:RTMAbortRatio` option. If the number of aborted transactions exceeds the abort ratio, then the method containing the lock will be deoptimized and recompiled with all locks as normal locks. This option is disabled by default. The `-XX:+UseRTMLocking` option must be enabled.

</dd>
<dt>-XX:+UseRTMLocking</dt>
<dd>

Generate Restricted Transactional Memory (RTM) locking code for all inflated locks, with the normal locking mechanism as the fallback handler. This option is disabled by default. Options related to RTM are only available for the Java HotSpot Server VM on x86 CPUs that support Transactional Synchronization Extensions (TSX).

RTM is part of Intel's TSX, which is an x86 instruction set extension and facilitates the creation of multithreaded applications. RTM introduces the new instructions `XBEGIN`, `XABORT`, `XEND`, and `XTEST`. The `XBEGIN` and `XEND` instructions enclose a set of instructions to run as a transaction. If no conflict is found when running the transaction, the memory and register modifications are committed together at the `XEND` instruction. The `XABORT` instruction can be used to explicitly abort a transaction and the `XEND` instruction to check if a set of instructions are being run in a transaction.

A lock on a transaction is inflated when another thread tries to access the same transaction, thereby blocking the thread that did not originally request access to the transaction. RTM requires that a fallback set of operations be specified in case a transaction aborts or fails. An RTM lock is a lock that has been delegated to the TSX's system.

RTM improves performance for highly contended locks with low conflict in a critical region (which is code that must not be accessed by more than one thread concurrently). RTM also improves the performance of coarse-grain locking, which typically does not perform well in multithreaded applications. (Coarse-grain locking is the strategy of holding locks for long periods to minimize the overhead of taking and releasing locks, while fine-grained locking is the strategy of trying to achieve maximum parallelism by locking only when necessary and unlocking as soon as possible.) Also, for lightly contended locks that are used by different threads, RTM can reduce false cache line sharing, also known as cache line ping-pong. This occurs when multiple threads from different processors are accessing different resources, but the resources share the same cache line. As a result, the processors repeatedly invalidate the cache lines of other processors, which forces them to read from main memory instead of their cache.

</dd>
<dt>-XX:+UseSHA</dt>
<dd>

Enables hardware-based intrinsics for SHA crypto hash functions for SPARC hardware. `UseSHA` is used in conjunction with the `UseSHA1Intrinsics`, `UseSHA256Intrinsics`, and `UseSHA512Intrinsics` options.

The `UseSHA` and `UseSHA*Intrinsics` flags are enabled by default, and are supported only for Java HotSpot Server VM 64-bit on SPARC T4 and newer.

This feature is only applicable when using the `sun.security.provider.Sun` provider for SHA operations.

To disable all hardware-based SHA intrinsics, specify `-XX:-UseSHA`. To disable only a particular SHA intrinsic, use the appropriate corresponding option. For example: `-XX:-UseSHA256Intrinsics`.

</dd>
<dt>-XX:+UseSHA1Intrinsics</dt>
<dd>

Enables intrinsics for SHA-1 crypto hash function.

</dd>
<dt>-XX:+UseSHA256Intrinsics</dt>
<dd>

Enables intrinsics for SHA-224 and SHA-256 crypto hash functions.

</dd>
<dt>-XX:+UseSHA512Intrinsics</dt>
<dd>

Enables intrinsics for SHA-384 and SHA-512 crypto hash functions.

</dd>
<dt>-XX:+UseSuperWord</dt>
<dd>

Enables the transformation of scalar operations into superword operations. This option is enabled by default. To disable the transformation of scalar operations into superword operations, specify `-XX:-UseSuperWord`. Only the Java HotSpot Server VM supports this option.

</dd>
</dl>
</a></div><a id="BABDDFII" name="BABDDFII">

</a><div><a id="BABDDFII" name="BABDDFII"></a><a id="BABFJDIC" name="BABFJDIC">

### Advanced Serviceability Options

These options provide the ability to gather system information and perform extensive debugging.

<dl>
<dt>-XX:+ExtendedDTraceProbes</dt>
<dd>

Enables additional `dtrace` tool probes that impact the performance. By default, this option is disabled and `dtrace` performs only standard probes.

</dd>
<dt>-XX:+HeapDumpOnOutOfMemory</dt>
<dd>

Enables the dumping of the Java heap to a file in the current directory by using the heap profiler (HPROF) when a `java.lang.OutOfMemoryError` exception is thrown. You can explicitly set the heap dump file path and name using the `-XX:HeapDumpPath` option. By default, this option is disabled and the heap is not dumped when an `OutOfMemoryError` exception is thrown.

</dd>
<dt>-XX:HeapDumpPath=_path_</dt>
<dd>

Sets the path and file name for writing the heap dump provided by the heap profiler (HPROF) when the `-XX:+HeapDumpOnOutOfMemoryError` option is set. By default, the file is created in the current working directory, and it is named `java_pid`_pid_`.hprof` where _pid_ is the identifier of the process that caused the error. The following example shows how to set the default file explicitly (`%p` represents the current process identificator):

<pre dir="ltr" xml:space="preserve">-XX:HeapDumpPath=./java_pid%p.hprof
</pre>

The following example shows how to set the heap dump file to `/var/log/java/java_heapdump.hprof`:

<pre dir="ltr" xml:space="preserve">-XX:HeapDumpPath=/var/log/java/java_heapdump.hprof
</pre></dd>
<dt>-XX:LogFile=_path_</dt>
<dd>

Sets the path and file name where log data is written. By default, the file is created in the current working directory, and it is named `hotspot.log`.

The following example shows how to set the log file to `/var/log/java/hotspot.log`:

<pre dir="ltr" xml:space="preserve">-XX:LogFile=/var/log/java/hotspot.log
</pre></dd>
<dt>-XX:+PrintClassHistogram</dt>
<dd>

Enables printing of a class instance histogram after a `Control+C` event (`SIGTERM`). By default, this option is disabled.

Setting this option is equivalent to running the `jmap -histo` command, or the `jcmd` _pid_ `GC.class_histogram` command, where _pid_ is the current Java process identifier.

</dd>
<dt>-XX:+PrintConcurrentLocks</dt>
<dd>

Enables printing of `java.util.concurrent` locks after a `Control+C` event (`SIGTERM`). By default, this option is disabled.

Setting this option is equivalent to running the `jstack -l` command or the `jcmd` _pid_ `Thread.print -l` command, where _pid_ is the current Java process identifier.

</dd>
<dt>-XX:+UnlockDiagnosticVMOptions</dt>
<dd>

Unlocks the options intended for diagnosing the JVM. By default, this option is disabled and diagnostic options are not available.

</dd>
</dl>
</a></div><a id="BABFJDIC" name="BABFJDIC">

</a><div><a id="BABFJDIC" name="BABFJDIC"></a><a id="BABFAFAE" name="BABFAFAE">

### Advanced Garbage Collection Options

These options control how garbage collection (GC) is performed by the Java HotSpot VM.

</a><dl><a id="BABFAFAE" name="BABFAFAE">
<dt>-XX:+AggressiveHeap</dt>
<dd>

Enables Java heap optimization. This sets various parameters to be optimal for long-running jobs with intensive memory allocation, based on the configuration of the computer (RAM and CPU). By default, the option is disabled and the heap is not optimized.

</dd>
<dt>-XX:+AlwaysPreTouch</dt>
<dd>

Enables touching of every page on the Java heap during JVM initialization. This gets all pages into the memory before entering the `main()` method. The option can be used in testing to simulate a long-running system with all virtual memory mapped to physical memory. By default, this option is disabled and all pages are committed as JVM heap space fills.

</dd>
<dt>-XX:+CMSClassUnloadingEnabled</dt>
<dd>

Enables class unloading when using the concurrent mark-sweep (CMS) garbage collector. This option is enabled by default. To disable class unloading for the CMS garbage collector, specify `-XX:-CMSClassUnloadingEnabled`.

</dd>
<dt>-XX:CMSExpAvgFactor=_percent_</dt>
<dd>

Sets the percentage of time (0 to 100) used to weight the current sample when computing exponential averages for the concurrent collection statistics. By default, the exponential averages factor is set to 25%. The following example shows how to set the factor to 15%:

<pre dir="ltr" xml:space="preserve">-XX:CMSExpAvgFactor=15
</pre></dd>
<dt>-XX:CMSInitiatingOccupancyFraction=_percent_</dt>
<dd>

Sets the percentage of the old generation occupancy (0 to 100) at which to start a CMS collection cycle. The default value is set to -1. Any negative value (including the default) implies that `-XX:CMSTriggerRatio` is used to define the value of the initiating occupancy fraction.

The following example shows how to set the occupancy fraction to 20%:

<pre dir="ltr" xml:space="preserve">-XX:CMSInitiatingOccupancyFraction=20
</pre></dd>
<dt>-XX:+CMSScavengeBeforeRemark</dt>
<dd>

Enables scavenging attempts before the CMS remark step. By default, this option is disabled.

</dd>
<dt>-XX:CMSTriggerRatio=_percent_</dt>
<dd>

Sets the percentage (0 to 100) of the value specified by `-XX:MinHeapFreeRatio` that is allocated before a CMS collection cycle commences. The default value is set to 80%.

The following example shows how to set the occupancy fraction to 75%:

<pre dir="ltr" xml:space="preserve">-XX:CMSTriggerRatio=75
</pre></dd>
<dt>-XX:ConcGCThreads=_threads_</dt>
<dd>

Sets the number of threads used for concurrent GC. The default value depends on the number of CPUs available to the JVM.

For example, to set the number of threads for concurrent GC to 2, specify the following option:

<pre dir="ltr" xml:space="preserve">-XX:ConcGCThreads=2
</pre></dd>
<dt>-XX:+DisableExplicitGC</dt>
<dd>

Enables the option that disables processing of calls to `System.gc()`. This option is disabled by default, meaning that calls to `System.gc()` are processed. If processing of calls to `System.gc()` is disabled, the JVM still performs GC when necessary.

</dd>
<dt>-XX:+ExplicitGCInvokesConcurrent</dt>
<dd>

Enables invoking of concurrent GC by using the `System.gc()` request. This option is disabled by default and can be enabled only together with the `-XX:+UseConcMarkSweepGC` option.

</dd>
<dt>-XX:+ExplicitGCInvokesConcurrentAndUnloadsClasses</dt>
<dd>

Enables invoking of concurrent GC by using the `System.gc()` request and unloading of classes during the concurrent GC cycle. This option is disabled by default and can be enabled only together with the `-XX:+UseConcMarkSweepGC` option.

</dd>
<dt>-XX:G1HeapRegionSize=_size_</dt>
<dd>

Sets the size of the regions into which the Java heap is subdivided when using the garbage-first (G1) collector. The value can be between 1 MB and 32 MB. The default region size is determined ergonomically based on the heap size.

The following example shows how to set the size of the subdivisions to 16 MB:

<pre dir="ltr" xml:space="preserve">-XX:G1HeapRegionSize=16m
</pre></dd>
<dt>-XX:+G1PrintHeapRegions</dt>
<dd>

Enables the printing of information about which regions are allocated and which are reclaimed by the G1 collector. By default, this option is disabled.

</dd>
<dt>-XX:G1ReservePercent=_percent_</dt>
<dd>

Sets the percentage of the heap (0 to 50) that is reserved as a false ceiling to reduce the possibility of promotion failure for the G1 collector. By default, this option is set to 10%.

The following example shows how to set the reserved heap to 20%:

<pre dir="ltr" xml:space="preserve">-XX:G1ReservePercent=20
</pre></dd>
<dt>-XX:InitialHeapSize=_size_</dt>
</a><dd><a id="BABFAFAE" name="BABFAFAE">
</a>

<a id="BABFAFAE" name="BABFAFAE">Sets the initial size (in bytes) of the memory allocation pool. This value must be either 0, or a multiple of 1024 and greater than 1 MB. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The default value is chosen at runtime based on system configuration. See the section "Ergonomics" in _Java SE HotSpot Virtual Machine Garbage Collection Tuning Guide_ at </a>`[http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html)`.

The following examples show how to set the size of allocated memory to 6 MB using various units:

<pre dir="ltr" xml:space="preserve">-XX:InitialHeapSize=6291456
-XX:InitialHeapSize=6144k
-XX:InitialHeapSize=6m
</pre>

If you set this option to 0, then the initial size will be set as the sum of the sizes allocated for the old generation and the young generation. The size of the heap for the young generation can be set using the `-XX:NewSize` option.

</dd>
<dt>-XX:InitialSurvivorRatio=_ratio_</dt>
<dd>

Sets the initial survivor space ratio used by the throughput garbage collector (which is enabled by the `-XX:+UseParallelGC` and/or -`XX:+UseParallelOldGC` options). Adaptive sizing is enabled by default with the throughput garbage collector by using the `-XX:+UseParallelGC` and `-XX:+UseParallelOldGC` options, and survivor space is resized according to the application behavior, starting with the initial value. If adaptive sizing is disabled (using the `-XX:-UseAdaptiveSizePolicy` option), then the `-XX:SurvivorRatio` option should be used to set the size of the survivor space for the entire execution of the application.

The following formula can be used to calculate the initial size of survivor space (S) based on the size of the young generation (Y), and the initial survivor space ratio (R):

<pre dir="ltr" xml:space="preserve">S=Y/(R+2)
</pre>

The 2 in the equation denotes two survivor spaces. The larger the value specified as the initial survivor space ratio, the smaller the initial survivor space size.

By default, the initial survivor space ratio is set to 8. If the default value for the young generation space size is used (2 MB), the initial size of the survivor space will be 0.2 MB.

The following example shows how to set the initial survivor space ratio to 4:

<pre dir="ltr" xml:space="preserve">-XX:InitialSurvivorRatio=4
</pre></dd>
<dt>-XX:InitiatingHeapOccupancyPercent=_percent_</dt>
<dd>

Sets the percentage of the heap occupancy (0 to 100) at which to start a concurrent GC cycle. It is used by garbage collectors that trigger a concurrent GC cycle based on the occupancy of the entire heap, not just one of the generations (for example, the G1 garbage collector).

By default, the initiating value is set to 45%. A value of 0 implies nonstop GC cycles. The following example shows how to set the initiating heap occupancy to 75%:

<pre dir="ltr" xml:space="preserve">-XX:InitiatingHeapOccupancyPercent=75
</pre></dd>
<dt>-XX:MaxGCPauseMillis=_time_</dt>
<dd>

Sets a target for the maximum GC pause time (in milliseconds). This is a soft goal, and the JVM will make its best effort to achieve it. By default, there is no maximum pause time value.

The following example shows how to set the maximum target pause time to 500 ms:

<pre dir="ltr" xml:space="preserve">-XX:MaxGCPauseMillis=500
</pre></dd>
<dt>-XX:MaxHeapSize=_size_</dt>
<dd>

Sets the maximum size (in byes) of the memory allocation pool. This value must be a multiple of 1024 and greater than 2 MB. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The default value is chosen at runtime based on system configuration. For server deployments, `-XX:InitialHeapSize` and `-XX:MaxHeapSize` are often set to the same value. See the section "Ergonomics" in _Java SE HotSpot Virtual Machine Garbage Collection Tuning Guide_ at `[http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html)`.

The following examples show how to set the maximum allowed size of allocated memory to 80 MB using various units:

<pre dir="ltr" xml:space="preserve">-XX:MaxHeapSize=83886080
-XX:MaxHeapSize=81920k
-XX:MaxHeapSize=80m
</pre>

On Oracle Solaris 7 and Oracle Solaris 8 SPARC platforms, the upper limit for this value is approximately 4,000 MB minus overhead amounts. On Oracle Solaris 2.6 and x86 platforms, the upper limit is approximately 2,000 MB minus overhead amounts. On Linux platforms, the upper limit is approximately 2,000 MB minus overhead amounts.

The `-XX:MaxHeapSize` option is equivalent to `-Xmx`.

</dd>
<dt>-XX:MaxHeapFreeRatio=_percent_</dt>
<dd>

Sets the maximum allowed percentage of free heap space (0 to 100) after a GC event. If free heap space expands above this value, then the heap will be shrunk. By default, this value is set to 70%.

The following example shows how to set the maximum free heap ratio to 75%:

<pre dir="ltr" xml:space="preserve">-XX:MaxHeapFreeRatio=75
</pre></dd>
<dt>-XX:MaxMetaspaceSize=_size_</dt>
<dd>

Sets the maximum amount of native memory that can be allocated for class metadata. By default, the size is not limited. The amount of metadata for an application depends on the application itself, other running applications, and the amount of memory available on the system.

The following example shows how to set the maximum class metadata size to 256 MB:

<pre dir="ltr" xml:space="preserve">-XX:MaxMetaspaceSize=256m
</pre></dd>
<dt>-XX:MaxNewSize=_size_</dt>
<dd>

Sets the maximum size (in bytes) of the heap for the young generation (nursery). The default value is set ergonomically.

</dd>
<dt>-XX:MaxTenuringThreshold=_threshold_</dt>
<dd>

Sets the maximum tenuring threshold for use in adaptive GC sizing. The largest value is 15. The default value is 15 for the parallel (throughput) collector, and 6 for the CMS collector.

The following example shows how to set the maximum tenuring threshold to 10:

<pre dir="ltr" xml:space="preserve">-XX:MaxTenuringThreshold=10
</pre></dd>
<dt>-XX:MetaspaceSize=_size_</dt>
<dd>

Sets the size of the allocated class metadata space that will trigger a garbage collection the first time it is exceeded. This threshold for a garbage collection is increased or decreased depending on the amount of metadata used. The default size depends on the platform.

</dd>
<dt>-XX:MinHeapFreeRatio=_percent_</dt>
<dd>

Sets the minimum allowed percentage of free heap space (0 to 100) after a GC event. If free heap space falls below this value, then the heap will be expanded. By default, this value is set to 40%.

The following example shows how to set the minimum free heap ratio to 25%:

<pre dir="ltr" xml:space="preserve">-XX:MinHeapFreeRatio=25
</pre></dd>
<dt>-XX:NewRatio=_ratio_</dt>
<dd>

Sets the ratio between young and old generation sizes. By default, this option is set to 2. The following example shows how to set the young/old ratio to 1:

<pre dir="ltr" xml:space="preserve">-XX:NewRatio=1
</pre></dd>
<dt>-XX:NewSize=_size_</dt>
<dd>

Sets the initial size (in bytes) of the heap for the young generation (nursery). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes.

The young generation region of the heap is used for new objects. GC is performed in this region more often than in other regions. If the size for the young generation is too low, then a large number of minor GCs will be performed. If the size is too high, then only full GCs will be performed, which can take a long time to complete. Oracle recommends that you keep the size for the young generation between a half and a quarter of the overall heap size.

The following examples show how to set the initial size of young generation to 256 MB using various units:

<pre dir="ltr" xml:space="preserve">-XX:NewSize=256m
-XX:NewSize=262144k
-XX:NewSize=268435456
</pre>

The `-XX:NewSize` option is equivalent to `-Xmn`.

</dd>
<dt>-XX:ParallelGCThreads=_threads_</dt>
<dd>

Sets the number of threads used for parallel garbage collection in the young and old generations. The default value depends on the number of CPUs available to the JVM.

For example, to set the number of threads for parallel GC to 2, specify the following option:

<pre dir="ltr" xml:space="preserve">-XX:ParallelGCThreads=2
</pre></dd>
<dt>-XX:+ParallelRefProcEnabled</dt>
<dd>

Enables parallel reference processing. By default, this option is disabled.

</dd>
<dt>-XX:+PrintAdaptiveSizePolicy</dt>
<dd>

Enables printing of information about adaptive generation sizing. By default, this option is disabled.

</dd>
<dt>-XX:+PrintGC</dt>
<dd>

Enables printing of messages at every GC. By default, this option is disabled.

</dd>
<dt>-XX:+PrintGCApplicationConcurrentTime</dt>
<dd>

Enables printing of how much time elapsed since the last pause (for example, a GC pause). By default, this option is disabled.

</dd>
<dt>-XX:+PrintGCApplicationStoppedTime</dt>
<dd>

Enables printing of how much time the pause (for example, a GC pause) lasted. By default, this option is disabled.

</dd>
<dt>-XX:+PrintGCDateStamps</dt>
<dd>

Enables printing of a date stamp at every GC. By default, this option is disabled.

</dd>
<dt>-XX:+PrintGCDetails</dt>
<dd>

Enables printing of detailed messages at every GC. By default, this option is disabled.

</dd>
<dt>-XX:+PrintGCTaskTimeStamps</dt>
<dd>

Enables printing of time stamps for every individual GC worker thread task. By default, this option is disabled.

</dd>
<dt>-XX:+PrintGCTimeStamps</dt>
<dd>

Enables printing of time stamps at every GC. By default, this option is disabled.

</dd>
<dt>-XX:+PrintStringDeduplicationStatistics</dt>
<dd>

Prints detailed deduplication statistics. By default, this option is disabled. See the `-XX:+UseStringDeduplication` option.

</dd>
<dt>-XX:+PrintTenuringDistribution</dt>
<dd>

Enables printing of tenuring age information. The following is an example of the output:

<pre dir="ltr" xml:space="preserve">Desired survivor size 48286924 bytes, new threshold 10 (max 10)
- age 1: 28992024 bytes, 28992024 total
- age 2: 1366864 bytes, 30358888 total
- age 3: 1425912 bytes, 31784800 total
...
</pre>

Age 1 objects are the youngest survivors (they were created after the previous scavenge, survived the latest scavenge, and moved from eden to survivor space). Age 2 objects have survived two scavenges (during the second scavenge they were copied from one survivor space to the next). And so on.

In the preceding example, 28 992 024 bytes survived one scavenge and were copied from eden to survivor space, 1 366 864 bytes are occupied by age 2 objects, etc. The third value in each row is the cumulative size of objects of age n or less.

By default, this option is disabled.

</dd>
<dt>-XX:+ScavengeBeforeFullGC</dt>
<dd>

Enables GC of the young generation before each full GC. This option is enabled by default. Oracle recommends that you _do not_ disable it, because scavenging the young generation before a full GC can reduce the number of objects reachable from the old generation space into the young generation space. To disable GC of the young generation before each full GC, specify `-XX:-ScavengeBeforeFullGC`.

</dd>
<dt>-XX:SoftRefLRUPolicyMSPerMB=_time_</dt>
<dd>

Sets the amount of time (in milliseconds) a softly reachable object is kept active on the heap after the last time it was referenced. The default value is one second of lifetime per free megabyte in the heap. The `-XX:SoftRefLRUPolicyMSPerMB` option accepts integer values representing milliseconds per one megabyte of the current heap size (for Java HotSpot Client VM) or the maximum possible heap size (for Java HotSpot Server VM). This difference means that the Client VM tends to flush soft references rather than grow the heap, whereas the Server VM tends to grow the heap rather than flush soft references. In the latter case, the value of the `-Xmx` option has a significant effect on how quickly soft references are garbage collected.

The following example shows how to set the value to 2.5 seconds:

<pre dir="ltr" xml:space="preserve">-XX:SoftRefLRUPolicyMSPerMB=2500
</pre></dd>
<dt>-XX:StringDeduplicationAgeThreshold=_threshold_</dt>
<dd>

`String` objects reaching the specified age are considered candidates for deduplication. An object's age is a measure of how many times it has survived garbage collection. This is sometimes referred to as tenuring; see the `-XX:+PrintTenuringDistribution` option. Note that `String` objects that are promoted to an old heap region before this age has been reached are always considered candidates for deduplication. The default value for this option is `3`. See the `-XX:+UseStringDeduplication` option.

</dd>
<dt>-XX:SurvivorRatio=_ratio_</dt>
<dd>

Sets the ratio between eden space size and survivor space size. By default, this option is set to 8. The following example shows how to set the eden/survivor space ratio to 4:

<pre dir="ltr" xml:space="preserve">-XX:SurvivorRatio=4
</pre></dd>
<dt>-XX:TargetSurvivorRatio=_percent_</dt>
<dd>

Sets the desired percentage of survivor space (0 to 100) used after young garbage collection. By default, this option is set to 50%.

The following example shows how to set the target survivor space ratio to 30%:

<pre dir="ltr" xml:space="preserve">-XX:TargetSurvivorRatio=30
</pre></dd>
<dt>-XX:TLABSize=_size_</dt>
<dd>

Sets the initial size (in bytes) of a thread-local allocation buffer (TLAB). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. If this option is set to 0, then the JVM chooses the initial size automatically.

The following example shows how to set the initial TLAB size to 512 KB:

<pre dir="ltr" xml:space="preserve">-XX:TLABSize=512k
</pre></dd>
<dt>-XX:+UseAdaptiveSizePolicy</dt>
<dd>

Enables the use of adaptive generation sizing. This option is enabled by default. To disable adaptive generation sizing, specify `-XX:-UseAdaptiveSizePolicy` and set the size of the memory allocation pool explicitly (see the `-XX:SurvivorRatio` option).

</dd>
<dt>-XX:+UseCMSInitiatingOccupancyOnly</dt>
<dd>

Enables the use of the occupancy value as the only criterion for initiating the CMS collector. By default, this option is disabled and other criteria may be used.

</dd>
<dt>-XX:+UseConcMarkSweepGC</dt>
<dd>

Enables the use of the CMS garbage collector for the old generation. Oracle recommends that you use the CMS garbage collector when application latency requirements cannot be met by the throughput (`-XX:+UseParallelGC`) garbage collector. The G1 garbage collector (`-XX:+UseG1GC`) is another alternative.

By default, this option is disabled and the collector is chosen automatically based on the configuration of the machine and type of the JVM. When this option is enabled, the `-XX:+UseParNewGC` option is automatically set and you should not disable it, because the following combination of options has been deprecated in JDK 8: `-XX:+UseConcMarkSweepGC -XX:-UseParNewGC`.

</dd>
<dt>-XX:+UseG1GC</dt>
<dd>

Enables the use of the garbage-first (G1) garbage collector. It is a server-style garbage collector, targeted for multiprocessor machines with a large amount of RAM. It meets GC pause time goals with high probability, while maintaining good throughput. The G1 collector is recommended for applications requiring large heaps (sizes of around 6 GB or larger) with limited GC latency requirements (stable and predictable pause time below 0.5 seconds).

By default, this option is disabled and the collector is chosen automatically based on the configuration of the machine and type of the JVM.

</dd>
<dt>-XX:+UseGCOverheadLimit</dt>
<dd>

Enables the use of a policy that limits the proportion of time spent by the JVM on GC before an `OutOfMemoryError` exception is thrown. This option is enabled, by default and the parallel GC will throw an `OutOfMemoryError` if more than 98% of the total time is spent on garbage collection and less than 2% of the heap is recovered. When the heap is small, this feature can be used to prevent applications from running for long periods of time with little or no progress. To disable this option, specify `-XX:-UseGCOverheadLimit`.

</dd>
<dt>-XX:+UseNUMA</dt>
<dd>

Enables performance optimization of an application on a machine with nonuniform memory architecture (NUMA) by increasing the application's use of lower latency memory. By default, this option is disabled and no optimization for NUMA is made. The option is only available when the parallel garbage collector is used (`-XX:+UseParallelGC`).

</dd>
<dt>-XX:+UseParallelGC</dt>
<dd>

Enables the use of the parallel scavenge garbage collector (also known as the throughput collector) to improve the performance of your application by leveraging multiple processors.

By default, this option is disabled and the collector is chosen automatically based on the configuration of the machine and type of the JVM. If it is enabled, then the `-XX:+UseParallelOldGC` option is automatically enabled, unless you explicitly disable it.

</dd>
<dt>-XX:+UseParallelOldGC</dt>
<dd>

Enables the use of the parallel garbage collector for full GCs. By default, this option is disabled. Enabling it automatically enables the `-XX:+UseParallelGC` option.

</dd>
<dt>-XX:+UseParNewGC</dt>
<dd>

Enables the use of parallel threads for collection in the young generation. By default, this option is disabled. It is automatically enabled when you set the `-XX:+UseConcMarkSweepGC` option. Using the `-XX:+UseParNewGC` option without the `-XX:+UseConcMarkSweepGC` option was deprecated in JDK 8.

</dd>
<dt>-XX:+UseSerialGC</dt>
<dd>

Enables the use of the serial garbage collector. This is generally the best choice for small and simple applications that do not require any special functionality from garbage collection. By default, this option is disabled and the collector is chosen automatically based on the configuration of the machine and type of the JVM.

</dd>
<dt>-XX:+UseSHM</dt>
<dd>

On Linux, enables the JVM to use shared memory to setup large pages.

For more information, see ["Large Pages"](#large_pages).

</dd>
<dt>-XX:+UseStringDeduplication</dt>
<dd>

Enables string deduplication. By default, this option is disabled. To use this option, you must enable the garbage-first (G1) garbage collector. See the `-XX:+UseG1GC` option.

_String deduplication_ reduces the memory footprint of `String` objects on the Java heap by taking advantage of the fact that many `String` objects are identical. Instead of each `String` object pointing to its own character array, identical `String` objects can point to and share the same character array.

</dd>
<dt>-XX:+UseTLAB</dt>
<dd>

Enables the use of thread-local allocation blocks (TLABs) in the young generation space. This option is enabled by default. To disable the use of TLABs, specify `-XX:-UseTLAB`.

</dd>
</dl>
</div>

<div><a id="BABDCEGG" name="BABDCEGG">

### Deprecated and Removed Options

These options were included in the previous release, but have since been considered unnecessary.

<dl>
<dt>-Xincgc</dt>
<dd>

Enables incremental garbage collection. This option was deprecated in JDK 8 with no replacement.

</dd>
<dt>-Xrun_libname_</dt>
<dd>

Loads the specified debugging/profiling library. This option was superseded by the `-agentlib` option.

</dd>
<dt>-XX:CMSIncrementalDutyCycle=_percent_</dt>
<dd>

Sets the percentage of time (0 to 100) between minor collections that the concurrent collector is allowed to run. This option was deprecated in JDK 8 with no replacement, following the deprecation of the `-XX:+CMSIncrementalMode` option.

</dd>
<dt>-XX:CMSIncrementalDutyCycleMin=_percent_</dt>
<dd>

Sets the percentage of time (0 to 100) between minor collections that is the lower bound for the duty cycle when `-XX:+CMSIncrementalPacing` is enabled. This option was deprecated in JDK 8 with no replacement, following the deprecation of the `-XX:+CMSIncrementalMode` option.

</dd>
<dt>-XX:+CMSIncrementalMode</dt>
<dd>

Enables the incremental mode for the CMS collector. This option was deprecated in JDK 8 with no replacement, along with other options that start with `CMSIncremental`.

</dd>
<dt>-XX:CMSIncrementalOffset=_percent_</dt>
<dd>

Sets the percentage of time (0 to 100) by which the incremental mode duty cycle is shifted to the right within the period between minor collections. This option was deprecated in JDK 8 with no replacement, following the deprecation of the `-XX:+CMSIncrementalMode` option.

</dd>
<dt>-XX:+CMSIncrementalPacing</dt>
<dd>

Enables automatic adjustment of the incremental mode duty cycle based on statistics collected while the JVM is running. This option was deprecated in JDK 8 with no replacement, following the deprecation of the `-XX:+CMSIncrementalMode` option.

</dd>
<dt>-XX:CMSIncrementalSafetyFactor=_percent_</dt>
<dd>

Sets the percentage of time (0 to 100) used to add conservatism when computing the duty cycle. This option was deprecated in JDK 8 with no replacement, following the deprecation of the `-XX:+CMSIncrementalMode` option.

</dd>
<dt>-XX:CMSInitiatingPermOccupancyFraction=_percent_</dt>
<dd>

Sets the percentage of the permanent generation occupancy (0 to 100) at which to start a GC. This option was deprecated in JDK 8 with no replacement.

</dd>
<dt>-XX:MaxPermSize=_size_</dt>
<dd>

Sets the maximum permanent generation space size (in bytes). This option was deprecated in JDK 8, and superseded by the `-XX:MaxMetaspaceSize` option.

</dd>
<dt>-XX:PermSize=_size_</dt>
<dd>

Sets the space (in bytes) allocated to the permanent generation that triggers a garbage collection if it is exceeded. This option was deprecated un JDK 8, and superseded by the `-XX:MetaspaceSize` option.

</dd>
<dt>-XX:+UseSplitVerifier</dt>
<dd>

Enables splitting of the verification process. By default, this option was enabled in the previous releases, and verification was split into two phases: type referencing (performed by the compiler) and type checking (performed by the JVM runtime). This option was deprecated in JDK 8, and verification is now split by default without a way to disable it.

</dd>
<dt>-XX:+UseStringCache</dt>
<dd>

Enables caching of commonly allocated strings. This option was removed from JDK 8 with no replacement.

</dd>
</dl>
</a></div><a id="BABDCEGG" name="BABDCEGG">
</a></div><a id="BABDCEGG" name="BABDCEGG">

</a><div><a id="BABDCEGG" name="BABDCEGG"></a><a id="sthref49" name="sthref49">

## Performance Tuning Examples

The following examples show how to use experimental tuning flags to either optimize throughput or to provide lower response time.

</a><dl><a id="sthref49" name="sthref49">
</a><dd><a id="sthref49" name="sthref49"></a><a id="JSSOR625" name="JSSOR625"></a><a id="sthref50" name="sthref50"></a></dd><a id="sthref50" name="sthref50">
<dt>Example 1 - Tuning for Higher Throughput</dt>
<dd>
<pre dir="ltr" xml:space="preserve">java -d64 -server -XX:+AggressiveOpts -XX:+UseLargePages -Xmn10g  -Xms26g -Xmx26g
</pre></dd>
</a><dd><a id="sthref50" name="sthref50"></a><a id="JSSOR626" name="JSSOR626"></a><a id="sthref51" name="sthref51"></a></dd><a id="sthref51" name="sthref51">
<dt>Example 2 - Tuning for Lower Response Time</dt>
<dd>
<pre dir="ltr" xml:space="preserve">java -d64 -XX:+UseG1GC -Xms26g Xmx26g -XX:MaxGCPauseMillis=500 -XX:+PrintGCTimeStamp
</pre></dd>
</a></dl><a id="sthref51" name="sthref51">
</a></div><a id="sthref51" name="sthref51">

</a><div><a id="sthref51" name="sthref51"></a><a id="large_pages" name="large_pages">

## Large Pages

Also known as huge pages, large pages are memory pages that are significantly larger than the standard memory page size (which varies depending on the processor and operating system). Large pages optimize processor Translation-Lookaside Buffers.

A Translation-Lookaside Buffer (TLB) is a page translation cache that holds the most-recently used virtual-to-physical address translations. TLB is a scarce system resource. A TLB miss can be costly as the processor must then read from the hierarchical page table, which may require multiple memory accesses. By using a larger memory page size, a single TLB entry can represent a larger memory range. There will be less pressure on TLB, and memory-intensive applications may have better performance.

However, large pages page memory can negatively affect system performance. For example, when a large mount of memory is pinned by an application, it may create a shortage of regular memory and cause excessive paging in other applications and slow down the entire system. Also, a system that has been up for a long time could produce excessive fragmentation, which could make it impossible to reserve enough large page memory. When this happens, either the OS or JVM reverts to using regular pages.

</a><div><a id="large_pages" name="large_pages"></a><a id="sthref52" name="sthref52">

### Large Pages Support

Solaris and Linux support large pages.

</a><div><a id="sthref52" name="sthref52"></a><a id="sthref53" name="sthref53">

Solaris

</a>

<a id="sthref53" name="sthref53">Solaris 9 and later include Multiple Page Size Support (MPSS); no additional configuration is necessary. See </a>`[http://www.oracle.com/technetwork/server-storage/solaris10/overview/solaris9-features-scalability-135663.html](http://www.oracle.com/technetwork/server-storage/solaris10/overview/solaris9-features-scalability-135663.html)`.

</div>

<div><a id="sthref54" name="sthref54">

Linux

The 2.6 kernel supports large pages. Some vendors have backported the code to their 2.4-based releases. To check if your system can support large page memory, try the following:

<pre dir="ltr" xml:space="preserve"># cat /proc/meminfo | grep Huge
HugePages_Total: 0
HugePages_Free: 0
Hugepagesize: 2048 kB
</pre>

If the output shows the three "Huge" variables, then your system can support large page memory but it needs to be configured. If the command prints nothing, then your system does not support large pages. To configure the system to use large page memory, login as `root`, and then follow these steps:

1.  If you are using the option `-XX:+UseSHM` (instead of `-XX:+UseHugeTLBFS`), then increase the `SHMMAX` value. It must be larger than the Java heap size. On a system with 4 GB of physical RAM (or less), the following will make all the memory sharable:

    <pre dir="ltr" xml:space="preserve"># echo 4294967295 &gt; /proc/sys/kernel/shmmax
</pre>
2.  If you are using the option `-XX:+UseSHM` or `-XX:+UseHugeTLBFS`, then specify the number of large pages. In the following example, 3 GB of a 4 GB system are reserved for large pages (assuming a large page size of 2048kB, then 3 GB = 3 * 1024 MB = 3072 MB = 3072 * 1024 kB = 3145728 kB and 3145728 kB / 2048 kB = 1536):

    <pre dir="ltr" xml:space="preserve"># echo 1536 &gt; /proc/sys/vm/nr_hugepages
</pre>
<div align="center">
<div>

<table border="1" cellpadding="3" cellspacing="0" dir="ltr" frame="hsides" rules="groups" summary="" width="80%">
<tbody>
<tr>
<td style="text-align:left">

**Note:**

*   Note that the values contained in `/proc` will reset after you reboot your system, so may want to set them in an initialization script (for example, `rc.local` or `sysctl.conf`).
*   If you configure (or resize) the OS kernel parameters `/proc/sys/kernel/shmmax` or `/proc/sys/vm/nr_hugepages`, Java processes may allocate large pages for areas in addition to the Java heap. These steps can allocate large pages for the following areas:

        *   Java heap

        *   Code cache

        *   The marking bitmap data structure for the parallel GC

Consequently, if you configure the `nr_hugepages` parameter to the size of the Java heap, then the JVM can fail in allocating the code cache areas on large pages because these areas are quite large in size.
</td>
</tr>
</tbody>
</table>

</div>
</div>
</a></div><a id="sthref54" name="sthref54">
</a></div><a id="sthref54" name="sthref54">
</a></div><a id="sthref54" name="sthref54">

</a><div><a id="sthref54" name="sthref54"></a><a id="app_class_data_sharing" name="app_class_data_sharing">

## Application Class Data Sharing

</a>

<a id="app_class_data_sharing" name="app_class_data_sharing">Application Class Data Sharing (AppCDS) extends CDS (see </a>`[https://docs.oracle.com/javase/8/docs/technotes/guides/vm/class-data-sharing.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/class-data-sharing.html)`) to enable classes from the standard extensions directories (specified by the system property `java.ext.dirs`; see `[https://docs.oracle.com/javase/8/docs/technotes/guides/extensions/spec.html](https://docs.oracle.com/javase/8/docs/technotes/guides/extensions/spec.html)`) and the application class path (see ["Setting the Class Path"](classpath.html#CBHHCGFB)) to be placed in the shared archive. AppCDS reduces the footprint and decreases start-up time of your applications provided that a substantial number of classes are loaded from the application class path.

This is a commercial feature that requires you to also specify the `-XX:+UnlockCommercialFeatures` option. This is also an experimental feature; it may change in future releases.

<div><a id="appcds_creating_running" name="appcds_creating_running">

### Creating a Shared Archive File, and Running an Application with It

The following steps create a shared archive file that contains all the classes used by the `test.Hello` application. The last step runs the application with the shared archive file.

1.  Create a list of all classes used by the `test.Hello` application. The following command creates a file named `hello.classlist` that contains a list of all classes used by this application:

    `java -Xshare:off -XX:+UnlockCommercialFeatures -XX:DumpLoadedClassList=hello.classlist -XX:+UseAppCDS -cp hello.jar test.Hello`

    Note that the `-cp` parameter must contain only JAR files; the `-XX:+UseAppCDS` option does not support class paths that contain directory names.

2.  Create a shared archive, named `hello.jsa`, that contains all the classes in `hello.classlist`:

    `java -XX:+UnlockCommercialFeatures -Xshare:dump -XX:+UseAppCDS -XX:SharedArchiveFile=hello.jsa -XX:SharedClassListFile=hello.classlist -cp hello.jar`

    Note that the `-cp` parameter used at archive creation time must be the same as (or a prefix of) the `-cp` used at run time.

3.  Run the application `test.Hello` with the shared archive `hello.jsa`:

    `java -XX:+UnlockCommercialFeatures -Xshare:on -XX:+UseAppCDS -XX:SharedArchiveFile=hello.jsa -cp hello.jar test.Hello`

    Ensure that you have specified the option `-Xshare:on` or -`Xshare:auto`.

4.  Verify that the `test.Hello` application is using the class contained in the `hello.jsa` shared archive:

    `java -XX:+UnlockCommercialFeatures -Xshare:on -XX:+UseAppCDS -XX:SharedArchiveFile=hello.jsa -cp hello.jar -verbose:class test.Hello`

    The output of this command should contain the following text:

    `Loaded test.Hello from shared objects file by sun/misc/Launcher$AppClassLoader`
</a></div><a id="appcds_creating_running" name="appcds_creating_running">

</a><div><a id="appcds_creating_running" name="appcds_creating_running"></a><a id="appcds_multiple_apps" name="appcds_multiple_apps">

### Sharing a Shared Archive across Multiple Application Processes

You can share the same archive file across multiple applications processes that have the exact same class path or share a common class path prefix. This reduces memory usage as the archive is memory-mapped into the address space of the processes. The operating system automatically shares the read-only pages across these processes.

The following steps create a shared archive that both applications `Hello` and `Hi` can use.

1.  Create a list of all classes used by the `Hello` application and another list for the `Hi` application:

    `java -XX:+UnlockCommercialFeatures -XX:DumpLoadedClassList=hello.classlist -XX:+UseAppCDS -cp common.jar:hello.jar Hello`

    `java -XX:+UnlockCommercialFeatures -XX:DumpLoadedClassList=hi.classlist -XX:+UseAppCDS -cp common.jar:hi.jar Hi`

    Note that because the `Hello` and `Hi` applications share a common class path prefix (both of their class paths start with `common.jar`), these two applications can share a shared archive file.

2.  Create a single list of classes used by all the applications that will share the shared archive file.

    The following commands combine the files `hello.classlist` and `hi.classlist` to one file, `common.classlist`:

    `cat hello.classlist hi.classlist &gt; common.classlist`

3.  Create a shared archive, named `common.jsa`, that contains all the classes in `common.classlist`:

    `java -XX:+UnlockCommercialFeatures -Xshare:dump -XX:SharedArchiveFile=common.jsa -XX:+UseAppCDS -XX:SharedClassListFile=common.classlist -cp common.jar`

    The value of the `-cp` parameter is the common class path prefix shared by the `Hello` and `Hi` applications.

4.  Run the `Hello` and `Hi` applications with the same shared archive:

    `java -XX:+UnlockCommercialFeatures -Xshare:on -XX:SharedArchiveFile=common.jsa -XX:+UseAppCDS -cp common.jar:hello.jar Hello`

    `java -XX:+UnlockCommercialFeatures -Xshare:on -XX:SharedArchiveFile=common.jsa -XX:+UseAppCDS -cp common.jar:hi.jar Hi`
</a></div><a id="appcds_multiple_apps" name="appcds_multiple_apps">
</a></div><a id="appcds_multiple_apps" name="appcds_multiple_apps">

</a><div><a id="appcds_multiple_apps" name="appcds_multiple_apps"></a><a id="sthref55" name="sthref55">

## Exit Status

The following exit values are typically returned by the launcher when the launcher is called with the wrong arguments, serious errors, or exceptions thrown by the JVM. However, a Java application may choose to return any value by using the API call `System.exit(exitValue)`. The values are:

*   `0`: Successful completion

*   `&gt;0`: An error occurred
</a></div><a id="sthref55" name="sthref55">

</a><div><a id="sthref55" name="sthref55"></a><a id="sthref56" name="sthref56">



