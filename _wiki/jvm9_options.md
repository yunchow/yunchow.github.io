---
layout: wiki
title: Java9 参数大全
categories: wiki
description: Java9 参数大全
keywords: Java9 参数大全
---

# java

[源文地址](http://docs.oracle.com/javase/9/tools/java.htm#JSWOR624)

<div>

<span>You can use the `java` command to launch a Java application.</span>

<div class="section">

Synopsis

To run a class:

<pre dir="ltr">java [<span class="variable">options</span>] <span class="variable">classname</span> [<span class="variable">args...</span>] 
</pre>

To execute a JAR file:

<pre dir="ltr">java [<span class="variable">options</span>] -jar <span class="variable">filename</span> [<span class="variable">args...</span>]
</pre>

To execute the main class in a module:

<pre dir="ltr">java [<span class="variable">options</span>] -p <span class="variable">modulepath</span> -m <span class="variable">modulename</span>[/<span class="variable">mainclass</span>] [<span class="variable">args...</span>]
</pre>
<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-059E2BD9-881B-421D-9170-6AA051434E10"><!-- --></a>`<span class="variable">options</span>`</dt>
<dd>

Specifies command-line options separated by spaces. See [Overview of Java Options](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__CBBIJCHG) for a description of available options.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F01F6A46-F2B5-4F6E-9680-21E3EF310453"><!-- --></a>`<span class="variable">classname</span>`</dt>
<dd>

Specifies the name of the class to be launched. Command-line entries following `<span class="variable">classname</span>` are the arguments for the main method.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-308F58A7-B518-4D13-9634-CD1A8126F0B7"><!-- --></a>`<span class="variable">filename</span>`</dt>
<dd>

Specifies the name of the Java Archive (JAR) file to be called.; used only with the `-jar` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-A15AD269-AE5C-4D92-972E-83E5DA7AA5D7"><!-- --></a>`<span class="variable">modulepath</span>`</dt>
<dd>

Provides a semicolon-separated list of directories in which each directory is a directory of modules; used only with the `-p` option. See [Standard Options for Java](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABDJJFI).

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5F97A247-EF3F-4381-84C4-AC88723DBDED"><!-- --></a>`<span class="variable">modulename</span>[/<span class="variable">mainclass</span>]`</dt>
<dd>

Specifies the name of the initial module to resolve and, if it isn’t specified by the module, then specifies the name of the main class to execute.; used only with the `-m` option. See [Standard Options for Java](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABDJJFI).

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-556F694E-7775-46E3-91D3-A5459DF83C03"><!-- --></a>`<span class="variable">args</span>`</dt>
<dd>

Specifies the arguments passed to the `main` method separated by spaces.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section">

Description

The `java` command starts a Java application. It does this by starting the Java Runtime Environment (JRE), loading the specified class, and calling that class's `main()` method. The method must be declared `public` and `static`, it must not return any value, and it must accept a `String` array as a parameter. The method declaration has the following form:

<pre dir="ltr">public static void main(String[] args)
</pre>

In JDK 9, a new launcher environment variable, `JDK_JAVA_OPTIONS`, has been introduced that prepends its content to the actual command line to the `java` launcher. See [Using the JDK_JAVA_OPTIONS Launcher Environment Variable](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__USINGTHEJDK_JAVA_OPTIONSLAUNCHERENV-F3C0E3BA).

The `java` command can be used to launch a JavaFX application by loading a class that either has a `main()` method or that extends the `javafx.application.Application`. In the latter case, the launcher constructs an instance of the `Application` class, calls its `init()` method, and then calls the `start(javafx.stage.Stage)` method.

By default, the first argument that isn’t an option of the `java` command is the fully qualified name of the class to be called. If the `-jar` option is specified, then its argument is the name of the JAR file containing class and resource files for the application. The startup class must be indicated by the `Main-Class` manifest header in its manifest file.

Arguments after the class file name or the JAR file name are passed to the `main()` method.

<span class="bold">Windows:</span> The `javaw` command is identical to `java`, except that with `javaw` there’s no associated console window. Use `javaw` when you don’t want a command prompt window to appear. The `javaw` launcher will, however, display a dialog box with error information if a launch fails.

</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__USINGTHEJDK_JAVA_OPTIONSLAUNCHERENV-F3C0E3BA">

Using the JDK_JAVA_OPTIONS Launcher Environment Variable

`JDK_JAVA_OPTIONS` prepends its content to the options parsed from the command line. The content of the `JDK_JAVA_OPTIONS` environment variable is a list of arguments separated by white-space characters (as determined by `isspace()`). These are prepended to the command line arguments passed to `java` launcher. The encoding requirement for the environment variable is the same as the `java` command line on the system. `JDK_JAVA_OPTIONS` environment variable content is treated in the same manner as that specified in the command line.

Single (`'`) or double (`"`) quotes can be used to enclose arguments that&nbsp;contain whitespace characters. All content between the open quote and the first matching close quote are preserved by simply removing the pair of quotes. In case a matching quote is not found, the launcher will abort with an error message. `@<span class="variable">files</span>` are supported as they are specified in the command line. However, as in `@<span class="variable">files</span>`, use of a wildcard is not supported. In order to mitigate potential misuse of `JDK_JAVA_OPTIONS` behavior, options that specify the main class (such as `-jar`) or cause the `java` launcher to exit without executing the main class (such as `-h`) are disallowed in the environment variable. If any of these options appear in the environment variable, the launcher will abort with an error message. When `JDK_JAVA_OPTIONS` is set, the launcher prints a message to stderr as a reminder.

<span class="bold">Example:</span>

<pre dir="ltr">export JDK_JAVA_OPTIONS='-g @file1 -Dprop=value @file2 -Dws.prop="white spaces”' 
$ java -Xint @file3
</pre>

is equivalent to the command line:

<pre dir="ltr">java -g @file1 -Dprop=value @file2 -Dws.prop="white spaces" -Xint @file3
</pre></div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__CBBIJCHG">

Overview of Java Options

The `java` command supports a wide range of options in the following categories:

*   [Standard Options for Java](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABDJJFI): Options guaranteed to be supported by all implementations of the Java Virtual Machine (JVM). They’re used for common actions, such as checking the version of the JRE, setting the class path, enabling verbose output, and so on.

*   [Extra Options for Java](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABHDABI): General purpose options that are specific to the Java HotSpot Virtual Machine. They aren’t guaranteed to be supported by all JVM implementations, and are subject to change. These options start with `-X`.

The advanced options aren’t recommended for casual use. These are developer options used for tuning specific areas of the Java HotSpot Virtual Machine operation that often have specific system requirements and may require privileged access to system configuration parameters. Several examples of performance tuning are provided in [Performance Tuning Examples](java.htm#GUID-EE6BD9FA-EF6D-4C3E-AC5C-30B8762CDC1B "You can use the Java advanced runtime options to optimize the performance of your applications."). These options aren’t guaranteed to be supported by all JVM implementations and are subject to change. Advanced options start with `-XX`.

*   [Advanced Runtime Options for Java](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABCBGHF): Control the runtime behavior of the Java HotSpot VM.

*   [Advanced JIT Compiler Options for java](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABDDFII): Control the dynamic just-in-time (JIT) compilation performed by the Java HotSpot VM.

*   [Advanced Serviceability Options for Java](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABFJDIC): Enable gathering system information and performing extensive debugging.

*   [Advanced Garbage Collection Options for Java](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABFAFAE): Control how garbage collection (GC) is performed by the Java HotSpot

Boolean options are used to either enable a feature that’s disabled by default or disable a feature that’s enabled by default. Such options don’t require a parameter. Boolean `-XX` options are enabled using the plus sign (`-XX:+<span class="variable">OptionName</span>`) and disabled using the minus sign (`-XX:-<span class="variable">OptionName</span>`).

For options that require an argument, the argument may be separated from the option name by a space, a colon (:), or an equal sign (=), or the argument may directly follow the option (the exact syntax differs for each option). If you’re expected to specify the size in bytes, then you can use no suffix, or use the suffix `k` or `K` for kilobytes (KB), `m` or `M` for megabytes (MB), or `g` or `G` for gigabytes (GB). For example, to set the size to 8 GB, you can specify either `8g`, `8192m`, `8388608k`, or `8589934592` as the argument. If you are expected to specify the percentage, then use a number from 0 to 1. For example, specify `0.25` for 25%.

The following sections describe the options that are obsolete, deprecated, and removed in JDK 9:

*   [Obsolete Java Options](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__OBSOLETEJAVAOPTIONS-A4E7030A): Accepted but ignored. A warning is issued when they’re used.

*   [Deprecated Java Options](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__DEPRECATEDJAVAOPTIONS-A4E6FB83): Accepted and acted upon. A warning is issued when they’re used.

*   [Removed Java Options](java.htm#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__REMOVEDJAVAOPTIONS-A4E6F213): Removed in JDK 9. Using them results in an error.
</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABDJJFI">

Standard Options for Java

These are the most commonly used options supported by all implementations of the JVM:

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-37DE51D5-4B5C-4348-BB23-F039273ABAA4"><!-- --></a><span class="bold">`-agentlib:<span class="variable">libname</span>[=<span class="variable">options</span>]`</span></dt>
<dd>

Loads the specified native agent library. After the library name, a comma-separated list of options specific to the library can be used.

*   <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> If the option `-agentlib:foo` is specified, then the JVM attempts to load the library named `libfoo.so` in the location specified by the `LD_LIBRARY_PATH` system variable (on OS X this variable is `DYLD_LIBRARY_PATH`).

*   <span class="bold">Windows:</span> If the option `-agentlib:foo` is specified, then the JVM attempts to load the library named `foo.dll` in the location specified by the `PATH` system variable.

    The following example shows how to load the Java Debug Wire Protocol (JDWP) library and listen for the socket connection on port 8000, suspending the JVM before the main class loads:

    <pre dir="ltr">-agentlib:jdwp=transport=dt_socket,server=y,address=8000
</pre>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E3E7D982-EB89-4582-9CAD-C665B6E956F0"><!-- --></a><span class="bold">`-agentpath:<span class="variable">pathname</span>[=<span class="variable">options</span>]`</span></dt>
<dd>

Loads the native agent library specified by the absolute path name. This option is equivalent to `-agentlib` but uses the full path and file name of the library.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-8CAE5D46-28F3-49B8-AAF5-EE79DC092E5D"><!-- --></a><span class="bold">`--class-path <span class="variable">classpath</span>`, `-classpath <span class="variable">classpath</span>` , or `-cp <span class="variable">classpath</span>`</span></dt>
<dd>

A semicolon (`;`) separated list of directories, JAR archives, and ZIP archives to search for class files.

Specifying `<span class="variable">classpath</span>` overrides any setting of the `CLASSPATH` environment variable. If the class path option isn’t used and `<span class="variable">classpath</span>` isn’t set, then the user class path consists of the current directory (.).

As a special convenience, a class path element that contains a base name of an asterisk (*) is considered equivalent to specifying a list of all the files in the directory with the extension `.jar` or `.JAR` . A Java program can’t tell the difference between the two invocations. For example, if the directory `mydir` contains `a.jar` and `b.JAR`, then the class path element `mydir/*` is expanded to `A.jar:b.JAR`, except that the order of JAR files is unspecified. All `.jar` files in the specified directory, even hidden ones, are included in the list. A class path entry consisting of an asterisk (*) expands to a list of all the jar files in the current directory. The `CLASSPATH` environment variable, where defined, is similarly expanded. Any class path wildcard expansion that occurs before the Java VM is started. Java programs never see wildcards that aren’t expanded except by querying the environment, such as by calling `System.getenv("CLASSPATH")`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-30E227AA-D105-45A1-98F8-66B48B960391"><!-- --></a><span class="bold">`--module-path <span class="variable">module path</span>...` or `-p <span class="variable">module path</span>`</span></dt>
<dd>

Searches for directories from a semicolon-separated (;) list of directories. Each directory is a directory of modules.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-D924FD85-9E53-414B-85F4-4254E138799B"><!-- --></a><span class="bold">`--upgrade-module-path <span class="variable">module path</span>...`</span></dt>
<dd>

Searches for directories from a semicolon-separated (;) list of directories. Each directory is a directory of modules that replace upgradeable modules in the runtime image.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2C78296B-24C7-424A-ACD1-329691D4203E"><!-- --></a><span class="bold">`--module <span class="variable">modulename</span>[/<span class="variable">mainclass</span>]` or `-m <span class="variable">module</span>[/<span class="variable">mainclass</span>]`</span></dt>
<dd>

Specifies the initial module to resolve and the name of the main class to execute if not specified by the module.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-688B46ED-0337-4066-B40C-499731FE7F4B"><!-- --></a><span class="bold">`--add-modules <span class="variable">modulename</span>[,<span class="variable">modulename</span>...]`</span></dt>
<dd>

Specifies the root modules to resolve in addition to the initial module. `<span class="variable">modulename</span>` also can be `ALL-DEFAULT`, `ALL-SYSTEM`, and `ALL-MODULE-PATH`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1EA80D43-ACFC-41AE-8278-1960A83CC5E0"><!-- --></a><span class="bold">`--limit-modules <span class="variable">modulename</span>[,<span class="variable">modulename</span>...]`</span></dt>
<dd>

Specifies the limit of the universe of observable modules.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-8A27E339-DA5C-4179-BFFE-FAA4604DB9A5"><!-- --></a><span class="bold">`--list-modules [<span class="variable">modulename</span>[,<span class="variable">modulename</span>...]]`</span></dt>
<dd>

Lists the observable modules and exits.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-FFEF90E4-F22E-4B53-91CB-8F15B5CDDB30"><!-- --></a>`--dry-run`</dt>
<dd>

Creates the VM but doesn’t execute the main method. This `--dry-run` option may be useful for validating the command-line options such as the module system configuration.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F6A19E5C-0E9C-4376-BA5C-11571082F509"><!-- --></a><span class="bold">`-D<span class="variable">property</span>=<span class="variable">value</span>`</span></dt>
<dd>

Sets a system property value. The `<span class="variable">property</span>` variable is a string with no spaces that represents the name of the property. The `<span class="variable">value</span>` variable is a string that represents the value of the property. If `<span class="variable">value</span>` is a string with spaces, then enclose it in quotation marks (for example `-Dfoo="foo bar"`).

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2327FFCA-5148-466F-B5D3-F3BEC8726B94"><!-- --></a><span class="bold">`-disableassertions[:[<span class="variable">packagename</span>]...|:<span class="variable">classname</span>]` or `-da[:[<span class="variable">packagename</span>]...|:<span class="variable">classname</span>]`</span></dt>
<dd>

Disables assertions. By default, assertions are disabled in all packages and classes. With no arguments, `-disableassertions` (`-da`) disables assertions in all packages and classes. With the `<span class="variable">packagename</span>` argument ending in `...`, the switch disables assertions in the specified package and any subpackages. If the argument is simply `...`, then the switch disables assertions in the unnamed package in the current working directory. With the `<span class="variable">classname</span>` argument, the switch disables assertions in the specified class.

The `-disableassertions` (`-da`) option applies to all class loaders and to system classes (which don’t have a class loader). There’s one exception to this rule: If the option is provided with no arguments, then it doesn’t apply to system classes. This makes it easy to disable assertions in all classes except for system classes. The `-disablesystemassertions` option enables you to disable assertions in all system classes. To explicitly enable assertions in specific packages or classes, use the `-enableassertions` (`-ea`) option. Both options can be used at the same time. For example, to run the `MyClass` application with assertions enabled in the package `com.wombat.fruitbat` (and any subpackages) but disabled in the class `com.wombat.fruitbat.Brickbat`, use the following command:

<pre dir="ltr">java -ea:com.wombat.fruitbat... -da:com.wombat.fruitbat.Brickbat MyClass
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-985A5BDE-6111-49BC-8CD9-7A1EE971EDA1"><!-- --></a><span class="bold">`-disablesystemassertions` or `-dsa`</span></dt>
<dd>

Disables assertions in all system classes.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-D152AEF8-E151-422E-9389-2F2D5BBD85B2"><!-- --></a><span class="bold">`-enableassertions[:[<span class="variable">packagename</span>]...|:<span class="variable">classname</span>]` or `-ea[:[<span class="variable">packagename</span>]...|:<span class="variable">classname</span>]`</span></dt>
<dd>

Enables assertions. By default, assertions are disabled in all packages and classes. With no arguments, `-enableassertions` (`-ea`) enables assertions in all packages and classes. With the `<span class="variable">packagename</span>` argument ending in `...`, the switch enables assertions in the specified package and any subpackages. If the argument is simply `...`, then the switch enables assertions in the unnamed package in the current working directory. With the `<span class="variable">classname</span>` argument, the switch enables assertions in the specified class.

The `-enableassertions` (`-ea`) option applies to all class loaders and to system classes (which don’t have a class loader). There’s one exception to this rule: If the option is provided with no arguments, then it doesn’t apply to system classes. This makes it easy to enable assertions in all classes except for system classes. The `-enablesystemassertions` option provides a separate switch to enable assertions in all system classes. To explicitly disable assertions in specific packages or classes, use the `-disableassertions` (`-da`) option. If a single command contains multiple instances of these switches, then they’re processed in order, before loading any classes. For example, to run the `MyClass` application with assertions enabled only in the package `com.wombat.fruitbat` (and any subpackages) but disabled in the class `com.wombat.fruitbat.Brickbat`, use the following command:

<pre dir="ltr">java -ea:com.wombat.fruitbat... -da:com.wombat.fruitbat.Brickbat MyClass
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-77DBFA33-F57D-49BB-840C-C176AE754A78"><!-- --></a><span class="bold">`-enablesystemassertions` or `-esa`</span></dt>
<dd>

Enables assertions in all system classes.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-B3A5F675-7921-4FAA-A0E7-49283D1F9A74"><!-- --></a><span class="bold">`-help` or `-?`</span></dt>
<dd>

Prints the help message to the error stream.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0DDAB3BE-09B9-41AA-B5E8-3AFB2BB71FE8"><!-- --></a><span class="bold">`--help`</span></dt>
<dd>

Prints the help message to the output stream.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0F559BD8-4BB2-4365-9F9C-610B3D0FB07A"><!-- --></a><span class="bold">`-jar <span class="variable">filename</span>`</span></dt>
<dd>

Executes a program encapsulated in a JAR file. The `<span class="variable">filename</span>` argument is the name of a JAR file with a manifest that contains a line in the form `Main-Class:<span class="variable">classname</span>` that defines the class with the `public static void main(String[] args)` method that serves as your application's starting point. When you use the `-jar` option, the specified JAR file is the source of all user classes, and other class path settings are ignored. If you’re using JAR files, then see: [jar](jar.htm#GUID-51C11B76-D9F6-4BC2-A805-3C847E857867 "You can use the jar command to create an archive for classes and resources, and to manipulate or restore individual classes or resources from an archive.")

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-6041C1F1-4A31-4339-AFD9-083F4802BECE"><!-- --></a><span class="bold">`-javaagent:<span class="variable">jarpath</span>[=<span class="variable">options</span>]`</span></dt>
<dd>

Loads the specified Java programming language agent.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-82096EC7-EB76-4435-8CBC-0243B455A488"><!-- --></a>`--illegal-access=<span class="variable">parameter</span>`</dt>
<dd>
<div class="infobox-note" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C26F2127-DFD3-42DC-973D-32F3958B0791">

Note:

This option is supported only in JDK 9.

</div>

When present at run time, `--illegal-access=` takes a keyword `<span class="variable">parameter</span>` to specify a mode of operation:

<div class="infobox-note" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-9A6D0C4C-A024-428F-8769-3FB7B687B055">

Note:

Illegal-access operations to internal APIs from code on the class path are allowed by default in JDK 9.

</div>

*   `permit`: This mode opens packages in JDK 9 that existed in JDK 8 to code on the class path. This allows code on class path that relies on the use of setAccessible to break into JDK internals, or to do other illegal access on members of classes in these packages, to work as per previous releases. This enables both static access (such as, by compiled bytecode) and deep reflective access. Deep reflective access is accomplished through the platform's reflection APIs. The first reflective-access operation to any such package causes a warning to be issued. However, no warnings are issued after the first occurrence. This single warning describes how to enable further warnings. This mode is the default for JDK 9 but will change in a future release.

*   `warn`: This mode is identical to `permit` except that a warning message is issued for each illegal reflective-access operation.

*   `debug`: This mode is identical to `warn` except that both a warning message and a stack trace are issued for each illegal reflective-access operation.

*   `deny`: This mode disables all illegal-access operations except for those enabled by other command-line options, such as`--add-opens`. This mode will become the default in a future release.

The default mode, `--illegal-access=permit`, is intended to make you aware of code on the class path that reflectively accesses any JDK-internal APIs at least once. To learn about all such accesses, you can use the `warn` or the `debug` modes. For each library or framework on the class path that requires illegal access, you have two options:

*   If the component's maintainers have already released a fixed version that no longer uses JDK-internal APIs then you can consider upgrading to that version.

*   If the component still needs to be fixed, then you can contact its maintainers and ask them to replace their use of JDK-internal APIs with the proper exported APIs.

If you must continue to use a component that requires illegal access, then you can eliminate the warning messages by using one or more `--add-opens` options to open only those internal packages to which access is required.

To verify that your application is ready for a future version of the JDK, run it with `--illegal-access=deny` along with any necessary `--add-opens` options. Any remaining illegal-access errors will most likely be due to static references from compiled code to JDK-internal APIs. You can identify those by running the [jdeps](jdeps.htm#GUID-A543FEBE-908A-49BF-996C-39499367ADB4 "You use the jdeps command to launch the Java class dependency analyzer.") tool with the `--jdk-internals` option. For performance reasons, JDK 9 does not issue warnings for illegal static-access operations.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-D010431B-4ABF-4DEB-A0E6-F5B2B3FBF562"><!-- --></a><span class="bold">`--show-version` or `-showversion`</span></dt>
<dd>

Displays version information and continues execution of the application. This option is equivalent to the `-version` option except that the latter instructs the JVM to exit after displaying version information.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-38DE3394-D6AD-4F49-9A68-E15D9F88C460"><!-- --></a><span class="bold">`-splash:<span class="variable">imgname</span>`</span></dt>
<dd>

Shows the splash screen with the image specified by `<span class="variable">imgname</span>`. HiDPI scaled images are automatically supported and used if available. The unscaled image file name, such as `image.ext`, should always be passed as the argument to the `-splash` option. The most appropriate scaled image provided is picked up automatically.

For example, to show the `splash.gif` file from the `images` directory when starting your application, use the following option:

<pre dir="ltr">-splash:images/splash.gif
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1C3A14C6-9D80-40C7-BA58-00308195430C"><!-- --></a><span class="bold">`-verbose:class`</span></dt>
<dd>

Displays information about each loaded class.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-224B9F78-A997-4717-B09F-0403FEE4DD6A"><!-- --></a><span class="bold">`-verbose:gc`</span></dt>
<dd>

Displays information about each garbage collection (GC) event.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E33A5786-740C-4B9B-8D7D-2138AC6369ED"><!-- --></a><span class="bold">`-verbose:jni`</span></dt>
<dd>

Displays information about the use of native methods and other Java Native Interface (JNI) activity.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-621B6C2B-EDC9-4978-ACA0-ABD8BE193364"><!-- --></a><span class="bold">`--version` or `-version`</span></dt>
<dd>

Displays version information and then exits. This option is equivalent to the `-showversion` option except that the latter doesn’t instruct the JVM to exit after displaying version information.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-BAA01DEF-3FBE-4D82-95F0-677A022CF3C7"><!-- --></a><span class="bold">`-X`</span></dt>
<dd>

Prints the help on extra options to the error stream.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0A4E2480-48C8-4BEB-8538-59A16E1C8D66"><!-- --></a><span class="bold">`--help-extra`</span></dt>
<dd>

Prints the help on extra options to the output stream.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-36C0C35E-403B-4A05-9C54-0CBE7D237C1C"><!-- --></a><span class="bold">`@<span class="variable">argument files</span>`</span></dt>
<dd>

Specifies one or more argument files prefixed by `@` used by the `java` command. It isn’t uncommon for the `java` command line to be very long because of the `.jar` files needed in the classpath. The `@<span class="variable">argument files</span>` option overcomes command-line length limitations by enabling the launcher to expand the contents of argument files after shell expansion, but before argument processing. Contents in the argument files are expanded because otherwise, they would be specified on the command line until the `-Xdisable-@files` option was encountered.

The argument files can also contain the main class name and all options. If an argument file contains all of the options required by the `java` command, then the command line could simply be:

`java @<span class="variable">argument files</span>`

See [java Command-Line Argument Files](java.htm#GUID-4856361B-8BFD-4964-AE84-121F5F6CF111 "You can shorten or simplify the java command by using @argument files to specify a text file that contains arguments, such as options and class names, passed to the java command. This let’s you to create java commands of any length on any operating system.") for a description and examples of using `@<span class="variable">argument files</span>` .

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABHDABI">

Extra Options for Java

The following `java` options are general purpose options that are specific to the Java HotSpot Virtual Machine.

<dl class="1.08* 2.92*">
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-EAC0AB71-BC2D-4A56-AA38-985A897C2784"><!-- --></a><span class="bold">`-Xbatch`</span></dt>
<dd>

Disables background compilation. By default, the JVM compiles the method as a background task, running the method in interpreter mode until the background compilation is finished. The `-Xbatch` flag disables background compilation so that compilation of all methods proceeds as a foreground task until completed. This option is equivalent to `-XX:-BackgroundCompilation`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-99B61470-01FA-416C-A492-64C0E6861290"><!-- --></a><span class="bold">`-Xbootclasspath/a:<span class="variable">path</span>`</span></dt>
<dd>

Specifies a list of directories, JAR files, and ZIP archives to append to the end of the default bootstrap class path.

<span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> Colons (`:`) separate entities in this list.

<span class="bold">Windows:</span> Semicolons (`;`) separate entities in this list.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-48F38B87-44D1-4F89-81EB-79AFD4565187"><!-- --></a><span class="bold">`-Xcheck:jni`</span></dt>
<dd>

Performs additional checks for Java Native Interface (JNI) functions. Specifically, it validates the parameters passed to the JNI function and the runtime environment data before processing the JNI request. It also checks for pending exceptions between JNI calls. Any invalid data encountered indicates a problem in the native code, and the JVM terminates with an irrecoverable error in such cases. Expect a performance degradation when this option is used.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-98BE7332-FB29-4389-979B-B065ACD9EC63"><!-- --></a><span class="bold">`-Xcomp`</span></dt>
<dd>

Forces compilation of methods on first invocation. By default, the Client VM (`-client`) performs 1,000 interpreted method invocations and the Server VM (`-server`) performs 10,000 interpreted method invocations to gather information for efficient compilation. Specifying the `-Xcomp` option disables interpreted method invocations to increase compilation performance at the expense of efficiency. You can also change the number of interpreted method invocations before compilation using the `-XX:CompileThreshold` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-110FFF6B-F9CE-497C-A695-282E57CA7532"><!-- --></a><span class="bold">`-Xdebug`</span></dt>
<dd>

Does nothing. Provided for backward compatibility.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F829EDAD-34E1-4612-A202-CBA86E724A43"><!-- --></a><span class="bold">`-Xdiag`</span></dt>
<dd>

Shows additional diagnostic messages.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-53C518C0-9D36-40B3-9285-2D2864120802"><!-- --></a><span class="bold">`-Xdiag:resolver`</span></dt>
<dd>

Shows the resolver diagnostic messages.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5D19634F-82FC-4646-88CF-228C5A5EB58D"><!-- --></a><span class="bold">`-Xfuture`</span></dt>
<dd>

Enables strict class-file format checks that enforce close conformance to the class-file format specification. Developers should use this flag when developing new code. Stricter checks may become the default in future releases.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-795FE7B7-E1F2-4D76-A64C-A16FC787E6EE"><!-- --></a><span class="bold">`-Xint`</span></dt>
<dd>

Runs the application in interpreted-only mode. Compilation to native code is disabled, and all bytecode is executed by the interpreter. The performance benefits offered by the just-in-time (JIT) compiler aren’t present in this mode.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F444A4AF-327C-4664-9B1E-E7AF650EED7E"><!-- --></a><span class="bold">`-Xinternalversion`</span></dt>
<dd>

Displays more detailed JVM version information than the `-version` option, and then exits.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-B4E93115-D032-4B17-8136-AFC03957F2E9"><!-- --></a><span class="bold">`-Xloggc:<span class="variable">option</span>`</span></dt>
<dd>

Enables the JVM unified logging framework. Logs GC status to a file with time stamps.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0EBCD5B3-CFED-4FD8-81E4-62B64E74DAA3"><!-- --></a><span class="bold">`-Xlog:help`</span></dt>
<dd>

Prints a short option reference with examples, and then exits the VM. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E8EFD8B3-87E5-49F4-B37E-37AAC17CEAE2"><!-- --></a><span class="bold">`-Xmixed`</span></dt>
<dd>

Executes all bytecode by the interpreter except for hot methods, which are compiled to native code.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-462EA549-0BFB-4221-A803-412C63D6BA5F"><!-- --></a><span class="bold">`-Xmn <span class="variable">size</span>`</span></dt>
<dd>

Sets the initial and maximum size (in bytes) of the heap for the young generation (nursery). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. The young generation region of the heap is used for new objects. GC is performed in this region more often than in other regions. If the size for the young generation is too small, then a lot of minor garbage collections are performed. If the size is too large, then only full garbage collections are performed, which can take a long time to complete. Oracle recommends that you keep the size for the young generation greater than 25% and less than 50% of the overall heap size. The following examples show how to set the initial and maximum size of young generation to 256 MB using various units:

<pre dir="ltr">-Xmn256m
-Xmn262144k
-Xmn268435456
</pre>

Instead of the `-Xmn` option to set both the initial and maximum size of the heap for the young generation, you can use `-XX:NewSize` to set the initial size and `-XX:MaxNewSize` to set the maximum size.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-897FA8D4-BC24-4443-99C2-D0F3D6BE1A77"><!-- --></a><span class="bold">`-Xms <span class="variable">size</span>`</span></dt>
<dd>

Sets the initial size (in bytes) of the heap. This value must be a multiple of 1024 and greater than 1 MB. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The following examples show how to set the size of allocated memory to 6 MB using various units:

<pre dir="ltr">-Xms6291456
-Xms6144k
-Xms6m
</pre>

If you don’t set this option, then the initial size is set as the sum of the sizes allocated for the old generation and the young generation. The initial size of the heap for the young generation can be set using the `-Xmn` option or the `-XX:NewSize` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-98AC4535-A539-406D-9AC5-390C1AF143F0"><!-- --></a><span class="bold">`-Xmx <span class="variable">size</span>`</span></dt>
<dd>

Specifies the maximum size (in bytes) of the memory allocation pool in bytes. This value must be a multiple of 1024 and greater than 2 MB. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. The default value is chosen at runtime based on system configuration. For server deployments, `-Xms` and `-Xmx` are often set to the same value. The following examples show how to set the maximum allowed size of allocated memory to 80 MB using various units:

<pre dir="ltr">-Xmx83886080
-Xmx81920k
-Xmx80m
</pre>

The `-Xmx` option is equivalent to `-XX:MaxHeapSize`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-FF8D817B-78F6-48E8-AC5E-3FE84D03C8F2"><!-- --></a><span class="bold">`-Xnoclassgc`</span></dt>
<dd>

Disables garbage collection (GC) of classes. This can save some GC time, which shortens interruptions during the application run. When you specify `-Xnoclassgc` at startup, the class objects in the application are left untouched during GC and are always be considered live. This can result in more memory being permanently occupied which, if not used carefully, throws an out-of-memory exception.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-6DD65C8C-F64E-4248-8316-42BEB1102702"><!-- --></a><span class="bold">`-Xprof`</span></dt>
<dd>

Profiles the running program and sends profiling data to standard output. This option is provided as a utility that’s useful in program development and isn’t intended to be used in production systems.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-A0E69D30-6CFF-49CB-A237-F709C9AD7FA6"><!-- --></a><span class="bold">`-Xrs`</span></dt>
<dd>

Reduces the use of operating system signals by the JVM. Shutdown hooks enable the orderly shutdown of a Java application by running user cleanup code (such as closing database connections) at shutdown, even if the JVM terminates abruptly.

*   <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span>

        *   The JVM catches signals to implement shutdown hooks for unexpected termination. The JVM uses `SIGHUP`, `SIGINT`, and `SIGTERM` to initiate the running of shutdown hooks.

        *   The JVM catches signals to implement shutdown hooks for unexpected termination. The JVM uses `SIGHUP`, `SIGINT`, and `SIGTERM` to initiate the running of shutdown hooks.

        *   Applications embedding the JVM frequently need to trap signals such as `SIGINT` or `SIGTERM`, which can lead to interference with the JVM signal handlers. The `-Xrs` option is available to address this issue. When `-Xrs` is used, the signal masks for `SIGINT`, `SIGTERM`, `SIGHUP`, and `SIGQUIT` aren’t changed by the JVM, and signal handlers for these signals aren’t installed.

*   <span class="bold">Windows:</span>

        *   The JVM watches for console control events to implement shutdown hooks for unexpected termination. Specifically, the JVM registers a console control handler that begins shutdown-hook processing and returns `TRUE` for `CTRL_C_EVENT`, `CTRL_CLOSE_EVENT`, `CTRL_LOGOFF_EVENT`, and `CTRL_SHUTDOWN_EVENT`.

        *   The JVM uses a similar mechanism to implement the feature of dumping thread stacks for debugging purposes. The JVM uses `CTRL_BREAK_EVENT` to perform thread dumps.

        *   If the JVM is run as a service (for example, as a servlet engine for a web server), then it can receive `CTRL_LOGOFF_EVENT` but shouldn’t initiate shutdown because the operating system doesn’t actually terminate the process. To avoid possible interference such as this, the `-Xrs` option can be used. When the `-Xrs` option is used, the JVM doesn’t install a console control handler, implying that it doesn’t watch for or process `CTRL_C_EVENT`, `CTRL_CLOSE_EVENT`, `CTRL_LOGOFF_EVENT`, or `CTRL_SHUTDOWN_EVENT`.

There are two consequences of specifying `-Xrs`:

*   <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> `SIGQUIT` thread dumps aren’t available.

*   <span class="bold">Windows:</span> Ctrl + Break thread dumps aren’t available.

User code is responsible for causing shutdown hooks to run, for example, by calling the `System.exit()` when the JVM is to be terminated.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-DD586FCF-F78E-45C7-AE43-5338813970E9"><!-- --></a><span class="bold">`-Xshare:<span class="variable">mode</span>`</span></dt>
<dd>

Sets the class data sharing (CDS) mode.

Possible `<span class="variable">mode</span>` arguments for this option include the following:

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-721EFB11-C2D1-46EF-BF14-B05029270139"><!-- --></a><span class="bold">`auto`</span></dt>
<dd>

Uses CDS if possible. This is the default value for Java HotSpot 32-Bit Client VM.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-7EA73CEA-E7C3-4BAE-8A1A-57B6555FFE02"><!-- --></a><span class="bold">`on`</span></dt>
<dd>

Requires the use of CDS. This option prints an error message and exits if class data sharing can’t be used.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3AD20DBB-8141-4A4D-8BC3-C6B9961144C9"><!-- --></a><span class="bold">`off`</span></dt>
<dd>

Instructs not to use CDS.

</dd>
</dl>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0A6BB1F6-1AC6-4F23-B767-746785F3167B"><!-- --></a><span class="bold">`-XshowSettings:<span class="variable">category</span>`</span></dt>
<dd>

Shows settings and continues. Possible `<span class="variable">category</span>` arguments for this option include the following:

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3FCCD7BA-A516-424D-A7F1-CF7AB16CAC22"><!-- --></a><span class="bold">`all`</span></dt>
<dd>

Shows all categories of settings. This is the default value.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-FDBEE5DB-226A-4616-9019-0AFFFA1B4AC1"><!-- --></a><span class="bold">`locale`</span></dt>
<dd>

Shows settings related to locale.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5FEAC6B0-171C-45AC-8FC0-C92DD0560F39"><!-- --></a><span class="bold">`properties`</span></dt>
<dd>

Shows settings related to system properties.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3230B631-E249-4A3A-AD91-449FD0C336F8"><!-- --></a><span class="bold">`vm`</span></dt>
<dd>

Shows the settings of the JVM.

</dd>
</dl>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-72BC3B70-49FF-4588-979F-7F8A32FEE6DA"><!-- --></a><span class="bold">`-Xss <span class="variable">size</span>`</span></dt>
<dd>

Sets the thread stack size (in bytes). Append the letter `k` or `K` to indicate KB, `m` or `M` to indicate MB, or `g` or `G` to indicate GB. The default value depends on the platform:

*   Linux/ARM (32-bit): 320 KB

*   Linux/ARM (64-bit): 1024 KB

*   Linux/x64 (64-bit): 1024 KB

*   OS X (64-bit): 1024 KB

*   Oracle Solaris/i386 (32-bit): 320 KB

*   Oracle Solaris/x64 (64-bit): 1024 KB

*   Windows: The default value depends on virtual memory

The following examples set the thread stack size to 1024 KB in different units:

<pre dir="ltr">-Xss1m
-Xss1024k
-Xss1048576
</pre>

This option is similar to `-XX:ThreadStackSize`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-868FC876-08B4-4B21-A09F-B867511EC1ED"><!-- --></a><span class="bold">`-Xverify:<span class="variable">mode</span>`</span></dt>
<dd>

Sets the mode of the bytecode verifier. Bytecode verification ensures that class files are properly formed and satisfy the constraints listed in [Verification of Class Files](../security/java-security-overview1.htm#JSSEC-GUID-65C96219-B2AB-4205-808E-5B41CB2AD694) in the <cite>The Java Virtual Machine Specification</cite>.

Don’t turn off verification because this reduces the protection provided by Java and could cause problems due to ill-formed class files.

Possible `<span class="variable">mode</span>` arguments for this option include the following:

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-FFF0D2FE-31E9-4AF0-B1DD-B6F5F33C424A"><!-- --></a><span class="bold">`remote`</span></dt>
<dd>

Verifies those classes that aren’t loaded by the bootstrap class loader. This is the default behavior if you don’t specify the `-Xverify` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-B641843A-29CA-43ED-AAD2-3BEFC9A8F336"><!-- --></a><span class="bold">`all`</span></dt>
<dd>

Enables verification of all bytecodes.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C4D51CF7-4927-4240-874A-EE40EE76FB94"><!-- --></a><span class="bold">`none`</span></dt>
<dd>

Disables verification of all bytecodes. Use of `-Xverify:none` is unsupported.

</dd>
</dl>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-AC22D7B8-E3C4-4554-AE49-A62C9421AA7C"><!-- --></a><span class="bold">`--add-reads <span class="variable">module</span>=<span class="variable">target-module</span>(,<span class="variable">target-module</span>)*`</span></dt>
<dd>

Updates `<span class="variable">module</span>` to read the `<span class="variable">target-module</span>`, regardless of the module declaration. `<span class="variable">target-module</span>` can be all unnamed to read all unnamed modules.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-9C0CBDB7-C149-46EB-8595-831EF3EB0623"><!-- --></a><span class="bold">`--add-exports <span class="variable">module</span>/<span class="variable">package</span>=<span class="variable">target-module</span>(,<span class="variable">target-module</span>)*`</span></dt>
<dd>

Updates `<span class="variable">module</span>` to export `<span class="variable">package</span>` to `<span class="variable">target-module</span>`, regardless of module declaration. The `<span class="variable">target-module</span>` can be all unnamed to export to all unnamed modules.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-26C77A7F-58B6-429B-A397-799E2DB5DF65"><!-- --></a><span class="bold">`--add-opens <span class="variable">module</span>/<span class="variable">package</span>=<span class="variable">target-module</span>(,<span class="variable">target-module</span>)*`</span></dt>
<dd>

Updates `<span class="variable">module</span>` to open `<span class="variable">package</span>` to `<span class="variable">target-module</span>`, regardless of module declaration.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5363532A-5EFC-4084-A7E1-4A4DADA6ED62"><!-- --></a><span class="bold">`--disable-@files`</span></dt>
<dd>

Can be used anywhere on the command line, including in an argument file, to prevent further `@filename` expansion. This option stops expanding `@argfiles` after the option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-ABA4F32C-222A-48BC-BE92-3B167D3134AD"><!-- --></a><span class="bold">`--patch-module <span class="variable">module</span>=<span class="variable">file</span>(;<span class="variable">file</span>)*`</span></dt>
<dd>

Overrides or augments a module with classes and resources in JAR files or directories.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABCBGHF">

Advanced Runtime Options for Java

These `java` options control the runtime behavior of the Java HotSpot VM.

<dl class="1.07* 2.93*">
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-A66FEEF0-B572-4935-A95B-3289D15E9BF5"><!-- --></a>`-XX:+CheckEndorsedAndExtDirs`</dt>
<dd>

Enables the option to prevent the `java` command from running a Java application if any of these directories exists and isn't empty:

*   `lib/endorsed`

*   `lib/ext`

*   The systemwide platform-specific extension directory

The endorsed standards override mechanism and the extension mechanism are no longer supported.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-CBE7EDFC-034B-4C96-B43A-C0C4879DEF22"><!-- --></a>`-XX:-CompactStrings`</dt>
<dd>

Disables the Compact Strings feature. By default, this option is enabled. When this option is enabled, Java Strings containing only single-byte characters are internally represented and stored as single-byte-per-character Strings using ISO-8859-1 / Latin-1 encoding. This reduces, by 50%, the amount of space required for Strings containing only single-byte characters. For Java Strings containing at least one multibyte character: these are represented and stored as 2 bytes per character using UTF-16 encoding. Disabling the Compact Strings feature forces the use of UTF-16 encoding as the internal representation for all Java Strings.

Cases where it may be beneficial to disable Compact Strings include the following:

*   When it’s known that an application overwhelmingly will be allocating multibyte character Strings

*   In the unexpected event where a performance regression is observed in migrating from Java SE 8 to Java SE 9 and an analysis shows that Compact Strings introduces the regression

In both of these scenarios, disabling Compact Strings makes sense.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-63E10752-DD85-4225-B6BA-B17692E7013F"><!-- --></a>`-XX:CompilerDirectivesFile=file`</dt>
<dd>

Adds directives from a file to the directives stack when a program starts. See [Compiler Directives and the Command Line](../vm/commands-work-directive-files.htm#JSJVM-GUID-3517007F-824A-4079-B374-188C107D9AB2).

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C99D717F-834C-4CF9-8BBB-B89B140C47C3"><!-- --></a>`-XX:CompilerDirectivesPrint`</dt>
<dd>

Prints the directives stack when the program starts or when a new directive is added..

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1901A243-FFBB-405C-BEC0-8113BF34900F"><!-- --></a>`-XX:ConcGCThreads=<span class="variable">n</span>`</dt>
<dd>

Sets the number of parallel marking threads. Sets `<span class="variable">n</span>` to approximately 1/4 of the number of parallel garbage collection threads (ParallelGCThreads).

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3804B90A-CF86-4E27-8C06-E1B2AC510305"><!-- --></a>`-XX:+DisableAttachMechanism`</dt>
<dd>

Disables the mechanism that lets tools attach to the JVM. By default, this option is disabled, meaning that the attach mechanism is enabled and you can use diagnostics and troubleshooting tools such as `jcmd`, `jstack`, `jmap`, and `jinfo`.

<div class="infobox-note" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-AA72FDE6-E319-404E-BE51-18956E60F74C">

Note:

The tools such as [jcmd](jcmd.htm#GUID-59153599-875E-447D-8D98-0078A5778F05 "You use the jcmd utility to send diagnostic command requests to a running Java Virtual Machine (JVM)."), [jinfo](jinfo.htm#GUID-69246B58-28C4-477D-B375-278F5F9830A5 "You use the jinfo command to generate Java configuration information for a specified Java process. This command is experimental and unsupported."), [jmap](jmap.htm#GUID-D2340719-82BA-4077-B0F3-2803269B7F41 "You use the jmap command to print details of a specified process. This command is experimental and unsupported."), and [jstack](jstack.htm#GUID-721096FC-237B-473C-A461-DBBBB79E4F6A "You use the jstack command to print Java stack traces of Java threads for a specified Java process. This command is experimental and unsupported.") shipped with the JDK aren’t supported when using the tools from one JDK version to troubleshoot a different JDK version.

</div>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-EC34275B-B906-450E-A5A9-D5807311F844"><!-- --></a>`-XX:ErrorFile=<span class="variable">filename</span>`</dt>
<dd>

Specifies the path and file name to which error data is written when an irrecoverable error occurs. By default, this file is created in the current working directory and named hs_err_pid pid.log where `<span class="variable">pid</span>` is the identifier of the process that caused the error.

The following example shows how to set the default log file (note that the identifier of the process is specified as `%p`):

<pre dir="ltr">-XX:ErrorFile=./hs_err_pid%p.log
</pre>

*   <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> The following example shows how to set the error log to `/var/log/java/java_error.log`:

    <pre dir="ltr">-XX:ErrorFile=/var/log/java/java_error.log
</pre>
*   <span class="bold">Windows:</span> The following example shows how to set the error log file to `C:/log/java/java_error.log`:

    <pre dir="ltr">-XX:ErrorFile=C:/log/java/java_error.log
</pre>

If the file can ‘t be created in the specified directory (due to insufficient space, permission problem, or another issue), then the file is created in the temporary directory for the operating system:

*   <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> The temporary directory is `/tmp`.

*   <span class="bold">Windows:</span> The temporary directory is specified by the value of the `TMP` environment variable; if that environment variable isn’t defined, then the value of the `TEMP` environment variable is used.
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F76A697B-5E05-41BC-93A1-1236CA7A13E1"><!-- --></a>`-XX:+FailOverToOldVerifier`</dt>
<dd>

Enables automatic failover to the old verifier when the new type checker fails. By default, this option is disabled and it’s ignored (that is, treated as disabled) for classes with a recent bytecode version. You can enable it for classes with older versions of the bytecode.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5363F9BA-CBBF-4011-A497-567A8D258EE1"><!-- --></a>`-XX:+FlightRecorder`</dt>
<dd>

.Enables the use of the Java Flight Recorder (JFR) during the runtime of the application. This is a commercial feature that requires that you also specify the `-XX:+UnlockCommercialFeatures` option as follows:

<pre dir="ltr">java -XX:+UnlockCommercialFeatures -XX:+FlightRecorder
</pre>
<div class="infobox-note" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0EEC3AE8-97E3-4C5B-8F76-3F0DA4B21A65">

Note:

The `-XX:+FlightRecorder` option is no longer required to use JFR. This was a change made in JDK 8u40.</div>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-6D79EEF1-52D7-4038-820C-EDD46208A664"><!-- --></a>`-XX:FlightRecorderOptions=<span class="variable">parameter</span>=<span class="variable">value</span>`</dt>
<dd>

Sets the parameters that control the behavior of JFR. This is a commercial feature that works in conjunction with the `-XX:+UnlockCommercialFeatures` option. The `-XX:+FlightRecorder` option is no longer required to use JFR. This was a change made in JDK 8u40.

The following list contains all available JFR parameters:

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-049C36AE-D896-4BB0-89F3-868A1AE79D95"><!-- --></a><span class="bold">`globalbuffersize=<span class="variable">size</span>`</span></dt>
<dd>

Specifies the total amount of primary memory (in bytes) used for data retention. Append `k` or `K`, to specify the size in KB, `m` or `M` to specify the size in MB, or `g` or `G` to specify the size in GB. By default, the size is set to 462848 bytes.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-B173CE0C-7D04-49D5-B521-D1C3B532B660"><!-- --></a><span class="bold">`loglevel={quiet|error|warning|info|debug|trace}`</span></dt>
<dd>

Specify the amount of data written to the log file by JFR. By default, it’s set to `info`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-01EB2D0C-E5D2-490D-B235-732E8C6765D0"><!-- --></a><span class="bold">`maxchunksize=<span class="variable">size</span>`</span></dt>
<dd>

Specifies the maximum size (in bytes) of the data chunks in a recording. Append `k` or `K`, to specify the size in KB, or `m` or `M` to specify the size in MB, or `g` or `G` to specify the size in GB. By default, the maximum size of data chunks is set to 12 MB.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-4742CBC0-D22A-4BCA-AA76-01BB2589BFEF"><!-- --></a><span class="bold">`memorysize=<span class="variable">size</span>`</span></dt>
<dd>

Determines how much buffer memory should be used.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-DFFF8414-4E5B-4B5E-B31B-77FD3E4D72F6"><!-- --></a><span class="bold">`repository=<span class="variable">path</span>`</span></dt>
<dd>

Specifies the repository (a directory) for temporary disk storage. By default, the system's temporary directory is used.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-72E4C472-2E11-44D0-9145-52C601725AC0"><!-- --></a><span class="bold">`samplethreads={true|false}`</span></dt>
<dd>

Specifies whether thread sampling is enabled. Thread sampling occurs only if the sampling event is enabled along with this parameter. By default, this parameter is enabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1B89962C-86D8-4C6D-8CF1-2BED57E860CA"><!-- --></a><span class="bold">`stackdepth=<span class="variable">depth</span>`</span></dt>
<dd>

Stack depth for stack traces by JFR. By default, the depth is set to 64 method calls. The maximum is 2048, and the minimum is 1.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-35F1BB7E-056A-4438-98DF-A30364933176"><!-- --></a><span class="bold">`threadbuffersize=<span class="variable">size</span>`</span></dt>
<dd>

Specifies the per-thread local buffer size (in bytes). Append `k` or `K`, to specify the size in KB, or `m` or `M` to specify the size in MB, `g` or `G` to specify the size in GB. Higher values for this parameter allow more data gathering without contention to flush it to the global storage. It can increase an application footprint in a thread-rich environment. By default, the local buffer size is set to 5 KB.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-751050B7-00AC-4CB5-814F-E7FAF351570F"><!-- --></a><span class="bold">`transform={true|false}`</span></dt>
<dd>

Specifies if event classes should be retransformed using JVMTI. If false, instrumentation will be added when event classes are loaded. By default it is true.

</dd>
</dl>

You can specify values for multiple parameters by separating them with a comma.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-B8F2A794-FB96-4003-B3D3-A53E8FDB92A1"><!-- --></a>`-XX:InitiatingHeapOccupancyPercent=<span class="variable">45</span>`</dt>
<dd>

Sets the Java heap occupancy threshold that triggers a marking cycle. The default occupancy is 45 percent of the entire Java heap.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-15557713-2E7E-4D0B-901F-958AA628E7F7"><!-- --></a>`-XX:LargePageSizeInBytes=<span class="variable">size</span>`</dt>
<dd>

<span class="bold">Oracle Solaris</span>: Sets the maximum size (in bytes) for large pages used for the Java heap. The `<span class="variable">size</span>` argument must be a power of 2 (2, 4, 8, 16, and so on). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. By default, the size is set to 0, meaning that the JVM chooses the size for large pages automatically. See [Large Pages](java.htm#GUID-7BE7CD55-3AC3-4A96-BBDD-E4D9FC4FCCCB "You use large pages, also known as huge pages, as memory pages that are significantly larger than the standard memory page size (which varies depending on the processor and operating system). Large pages optimize processor Translation-Lookaside Buffers.").

The following example describes how to set the large page size to 4 megabytes (MB):

<pre dir="ltr">-XX:LargePageSizeInBytes=4m
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2E02B495-5C36-4C93-8597-0020EFDC9A9C"><!-- --></a>`-XX:MaxDirectMemorySize=<span class="variable">size</span>`</dt>
<dd>

Sets the maximum total size (in bytes) of the `java.nio` package, direct-buffer allocations. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. By default, the size is set to 0, meaning that the JVM chooses the size for NIO direct-buffer allocations automatically.

The following examples illustrate how to set the NIO size to 1024 KB in different units:

<pre dir="ltr">-XX:MaxDirectMemorySize=1m
-XX:MaxDirectMemorySize=1024k
-XX:MaxDirectMemorySize=1048576
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-B5DB91C4-E6D2-4415-B9DD-C88E1D7AC115"><!-- --></a>`-XX:-MaxFDLimit`</dt>
<dd>

Disables the attempt to set the soft limit for the number of open file descriptors to the hard limit. By default, this option is enabled on all platforms, but is ignored on Windows. The only time that you may need to disable this is on Mac OS, where its use imposes a maximum of 10240, which is lower than the actual system maximum.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2951C196-DB50-4E12-8BBF-27A85C66D35C"><!-- --></a>`-XX:MaxGCPauseMillis=<span class="variable">200</span>`</dt>
<dd>

Sets a target value for the desired maximum pause time. The default value is 200 milliseconds. The specified value doesn’t adapt to your heap size.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-40EE5C9C-5D85-4397-A8D2-636CABBE3FBB"><!-- --></a>`-XX:NativeMemoryTracking=<span class="variable">mode</span>`</dt>
<dd>

Specifies the mode for tracking JVM native memory usage. Possible <span class="variable">mode</span> arguments for this option include the following:

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2CDA6C67-05F5-447D-8A31-D6FE792FD10C"><!-- --></a><span class="bold">`off`</span></dt>
<dd>

Instructs not to track JVM native memory usage. This is the default behavior if you don’t specify the `-XX:NativeMemoryTracking` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-FB9FFB02-8D45-43CF-AA12-ACCEDF733B31"><!-- --></a><span class="bold">`summary`</span></dt>
<dd>

Tracks memory usage only by JVM subsystems, such as Java heap, class, code, and thread.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E2477824-9786-4813-A769-B2CB4E0F58A6"><!-- --></a><span class="bold">`detail`</span></dt>
<dd>

In addition to tracking memory usage by JVM subsystems, track memory usage by individual `CallSite`, individual virtual memory region and its committed regions.

</dd>
</dl>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2261D7D0-4CC0-471F-A325-3585E1099748"><!-- --></a>`-XX:ObjectAlignmentInBytes=<span class="variable">alignment</span>`</dt>
<dd>

Sets the memory alignment of Java objects (in bytes). By default, the value is set to 8 bytes. The specified value should be a power of 2, and must be within the range of 8 and 256 (inclusive). This option makes it possible to use compressed pointers with large Java heap sizes.

The heap size limit in bytes is calculated as:

<pre dir="ltr">4GB * ObjectAlignmentInBytes
</pre>
<div class="infobox-note" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-07E18076-0A66-41A9-9230-5A11DC21E1EC">

Note:

As the alignment value increases, the unused space between objects also increases. As a result, you may not realize any benefits from using compressed pointers with large Java heap sizes.

</div>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-04AEB619-927F-4118-A6D3-3167DD0F6BED"><!-- --></a>`-XX:OnError=<span class="variable">string</span>`</dt>
<dd>

Sets a custom command or a series of semicolon-separated commands to run when an irrecoverable error occurs. If the string contains spaces, then it must be enclosed in quotation marks.

*   <span class="bold">Oracle Solaris, Linux, and OS X:</span> The following example shows how the `-XX:OnError` option can be used to run the `gcore` command to create the core image, and the debugger is started to attach to the process in case of an irrecoverable error (the `%p` designates the current process):

    <pre dir="ltr">-XX:OnError="gcore %p;dbx - %p"
</pre>
*   <span class="bold">Windows:</span> The following example shows how the `-XX:OnError` option can be used to run the `userdump.exe` utility to obtain a crash dump in case of an irrecoverable error (the `%p` designates the current process). This example assumes that the path to the `userdump.exe` utility is specified in the `PATH` environment variable:

    <pre dir="ltr">-XX:OnError="userdump.exe %p"
</pre>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-01F4A72B-DB45-4EFB-BDF0-DB03DC056C66"><!-- --></a>`-XX:OnOutOfMemoryError=<span class="variable">string</span>`</dt>
<dd>

Sets a custom command or a series of semicolon-separated commands to run when an `OutOfMemoryError` exception is first thrown. If the string contains spaces, then it must be enclosed in quotation marks. For an example of a command string, see the description of the `-XX:OnError` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E3E557B7-AA19-4CD5-8385-767B053C3397"><!-- --></a>`-XX:ParallelGCThreads=<span class="variable">n</span>`</dt>
<dd>

Sets the value of the STW worker threads. Sets the value of n to the number of logical processors. The value of `<span class="variable">n</span>` is the same as the number of logical processors up to a value of 8. If there are more than 8 logical processors, then this option sets the value of `n` to approximately 5/8 of the logical processors. This works in most cases except for larger SPARC systems where the value of `n` can be approximately 5/16 of the logical processors.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E013C919-B1A7-482F-8883-9B38D049FE9F"><!-- --></a>`-XX:+PerfDataSaveToFile`</dt>
<dd>

If enabled, saves [jstat](jstat.htm#GUID-5F72A7F9-5D5A-4486-8201-E1D1BA8ACCB5 "You use the jstat command to monitor JVM statistics. This command is experimental and unsupported.") binary data when the Java application exits. This binary data is saved in a file named `hsperfdata_``<span class="variable">pid</span>`, where `<span class="variable">pid</span>` is the process identifier of the Java application that you ran. Use the`<span class="variable">jstat</span>` command to display the performance data contained in this file as follows:

<pre dir="ltr">jstat -class file:///<span class="variable">path</span>/hsperfdata_<span class="variable">pid</span>
</pre>
<pre dir="ltr">jstat -gc file:///<span class="variable">path</span>/hsperfdata_<span class="variable">pid</span>
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-021FB56C-6B3A-48B7-95A4-697C7CB77E6A"><!-- --></a>`-XX:+PrintCommandLineFlags`</dt>
<dd>

Enables printing of ergonomically selected JVM flags that appeared on the command line. It can be useful to know the ergonomic values set by the JVM, such as the heap space size and the selected garbage collector. By default, this option is disabled and flags aren’t printed.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-90D07719-207A-4903-B1A8-CFC3817FC7D7"><!-- --></a>`-XX:+PreserveFramePointer&nbsp;`</dt>
<dd>

Selects between using the RBP register as a general purpose register`&nbsp;(-XX:-PreserveFramePointer)` and using the RBP register to hold the frame pointer of the currently executing method `(-XX:+PreserveFramePointer).` If the frame pointer is available, then external profiling tools&nbsp;(for example, Linux perf) can construct more accurate stack traces.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-6778E677-2616-435B-AFBD-1EDC4F1830BD"><!-- --></a>`-XX:+PrintNMTStatistics`</dt>
<dd>

Enables printing of collected native memory tracking data at JVM exit when native memory tracking is enabled (see `-XX:NativeMemoryTracking`). By default, this option is disabled and native memory tracking data isn’t printed.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-490DB6FB-7DD7-4527-A12E-240D134803C0"><!-- --></a>`-XX:+RelaxAccessControlCheck`</dt>
<dd>

Decreases the amount of access control checks in the verifier. By default, this option is disabled, and it’s ignored (that is, treated as disabled) for classes with a recent bytecode version. You can enable it for classes with older versions of the bytecode.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-A8BF79EE-48B1-4D9F-ACED-1800944BEB1F"><!-- --></a>`-XX:+ResourceManagement`</dt>
<dd>

Enables the use of Resource Management during the runtime of the application.

This is a commercial feature that requires you to also specify the `-XX:+UnlockCommercialFeatures` option as follows:

<pre dir="ltr">java -XX:+UnlockCommercialFeatures -XX:+ResourceManagement
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-9117D8FB-91BB-4949-B0C7-02412CC7DCDF"><!-- --></a>`-XX:ResourceManagementSampleInterval=<span class="variable">value in milliseconds</span>`</dt>
<dd>

Sets the parameter that controls the sampling interval for Resource Management measurements, in milliseconds.

This option can be used only when Resource Management is enabled (that is, the `-XX:+ResourceManagement` option is specified).

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-A37C3055-BF73-4D9B-920A-75562650F9EE"><!-- --></a>`-XX:SharedArchiveFile=<span class="variable">path</span>`</dt>
<dd>

Specifies the path and name of the class data sharing (CDS) archive file

See [Application Class Data Sharing](java.htm#GUID-31503FCE-93D0-4175-9B4F-F6A738B2F4C4 "Application Class Data Sharing (AppCDS) extends class data sharing to enable application classes to be placed in the shared archive.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-D5E35460-2790-4F3E-997F-1546674827CD"><!-- --></a>`-XX:SharedArchiveConfigFile=<span class="variable">shared_config_file</span>`</dt>
<dd>

Specifies additional shared data added to the archive file.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-B7B30A0B-47D9-44C6-9D75-EA6087514004"><!-- --></a>`-XX:SharedClassListFile=<span class="variable">file_name</span>`</dt>
<dd>

Specifies the text file that contains the names of the class files to store in the class data sharing (CDS) archive. This file contains the full name of one class file per line, except slashes (`/`) replace dots (`.`). For example, to specify the classes `java.lang.Object` and `hello.Main`, create a text file that contains the following two lines:

<pre dir="ltr">java/lang/Object
hello/Main
</pre>

The class files that you specify in this text file should include the classes that are commonly used by the application. They may include any classes from the application, extension, or bootstrap class paths.

See [Application Class Data Sharing](java.htm#GUID-31503FCE-93D0-4175-9B4F-F6A738B2F4C4 "Application Class Data Sharing (AppCDS) extends class data sharing to enable application classes to be placed in the shared archive.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-FD377576-9B3A-4833-9DDD-9B36BB209E97"><!-- --></a>`-XX:+ShowMessageBoxOnError`</dt>
<dd>

Enables the display of a dialog box when the JVM experiences an irrecoverable error. This prevents the JVM from exiting and keeps the process active so that you can attach a debugger to it to investigate the cause of the error. By default, this option is disabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-348FD717-B699-481B-AD14-9F765CCB9759"><!-- --></a>`-XX:StartFlightRecording=<span class="variable">parameter</span>=<span class="variable">value</span>`</dt>
<dd>

Starts a JFR recording for the Java application. This is a commercial feature that works in conjunction with the `-XX:+UnlockCommercialFeatures` option. This option is equivalent to the `JFR.start` diagnostic command that starts a recording during runtime. You can set the following parameters when starting a JFR recording:

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2B9B0171-929E-4844-BBD3-285925981932"><!-- --></a><span class="bold">`delay=<span class="variable">time</span>`</span></dt>
<dd>

Specifies the delay between the Java application launch time and the start of the recording. Append `s` to specify the time in seconds, `m` for minutes, `h` for hours, or `d` for days (for example, specifying `10m` means 10 minutes). By default, there’s no delay, and this parameter is set to 0.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2C8C7A66-A0DA-4546-A770-383B0F16BE37"><!-- --></a><span class="bold">`duration=<span class="variable">time</span>`</span></dt>
<dd>

Specifies the duration of the recording. Append `s` to specify the time in seconds, `m` for minutes, `h` for hours, or `d` for days (for example, specifying `5h` means 5 hours). By default, the duration isn’t limited, and this parameter is set to 0.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-D68FEE63-71AD-48F8-A576-C40733660A3C"><!-- --></a><span class="bold">`filename=<span class="variable">path</span>`</span></dt>
<dd>

Specifies the path and name of the JFR recording log file.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-62DBC6A7-7A09-4A48-B7B9-95F5AC725B0D"><!-- --></a><span class="bold">`name=<span class="variable">identifier</span>`</span></dt>
<dd>

Takes both the name and the identifier of a recording.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-07092AD3-C073-42E7-B14D-4242B739FFA5"><!-- --></a><span class="bold">`maxage=<span class="variable">time</span>`</span></dt>
<dd>

Specifies the maximum age of disk data to keep for the default recording. Append `s` to specify the time in seconds, `m` for minutes, `h` for hours, or `d` for days (for example, specifying `30s` means 30 seconds). By default, the maximum age is set to 15 minutes (`15m`).

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E577A56E-83A6-4C04-85D3-D51BF3590D68"><!-- --></a><span class="bold">`maxsize=<span class="variable">size</span>`</span></dt>
<dd>

Specifies the maximum size (in bytes) of disk data to keep for the default recording. Append `k` or `K`, to specify the size in KB, `m` or `M` to specify the size in MB, or `g` or `G` to specify the size in GB. By default, the maximum size of disk data isn’t limited, and this parameter is set to 0.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C6D19A12-255E-454D-BE58-65C82E083989"><!-- --></a><span class="bold">`settings=<span class="variable">path</span>`</span></dt>
<dd>

Specifies the path and name of the event settings file (of type JFC). By default, the `default.jfc` file is used, which is located in `JAVA_HOME/jre/lib/jfr`.

</dd>
</dl>

You can specify values for multiple parameters by separating them with a comma.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-EFC23CFC-A104-49C1-97BE-5E00A9241A5E"><!-- --></a>`-XX:ThreadStackSize=<span class="variable">size</span>`</dt>
<dd>

Sets the Java thread stack size (in kilobytes). Use of a scaling suffix, such as `k`, results in the scaling of the kilobytes value so that `-XX:ThreadStackSize=1k` sets the Java thread stack size&nbsp;to 1024*1024 bytes or 1 megabyte. The default value depends on the platform:

*   Linux/ARM (32-bit): 320 KB

*   Linux/ARM (64-bit): 1024 KB

*   Linux/i386 (32-bit): 320 KB

*   Linux/x64 (64-bit): 1024 KB

*   OS X (64-bit): 1024 KB

*   Oracle Solaris/x64 (64-bit): 1024 KB

*   Windows: The default value depends on the virtual memory.

The following examples show how to set the thread stack size to 1 megabyte in different units:

<pre dir="ltr">-XX:ThreadStackSize=1k
-XX:ThreadStackSize=1024
</pre>

This option is similar to `-Xss`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0C7A9590-BC3D-4967-8689-1E27FB686811"><!-- --></a>`-XX:+UnlockCommercialFeatures`</dt>
<dd>

Enables the use of commercial features. Commercial features are included with Oracle Java SE Advanced or Oracle Java SE Suite packages, as defined in the[Oracle Java SE and Oracle Java Embedded Products](http://www.oracle.com/pls/topic/lookup?ctx=javase9&amp;id=javase_embedded_products) page.

By default, this option is disabled and the JVM runs without the commercial features. After they're enabled for a JVM process, it isn’t possible to disable their use for that process.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-7D644D1F-8603-45EA-ABDB-3638D983184A"><!-- --></a>`-XX:+UseAppCDS`</dt>
<dd>

Enables application class data sharing (AppCDS). To use AppCDS, you must also specify values for the options `-XX:SharedClassListFile` and `-XX:SharedArchiveFile` during both CDS dump time (see the option `-Xshare:dump`) and application run time.

This is a commercial feature that requires you to also specify the `-XX:+UnlockCommercialFeatures` option. This is also an experimental feature; it may change in future releases.

See [Application Class Data Sharing](java.htm#GUID-31503FCE-93D0-4175-9B4F-F6A738B2F4C4 "Application Class Data Sharing (AppCDS) extends class data sharing to enable application classes to be placed in the shared archive.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2A02B104-E2AE-45AA-B10D-CEAACADAE219"><!-- --></a>`-XX:-UseBiasedLocking`</dt>
<dd>

Disables the use of biased locking. Some applications with significant amounts of uncontended synchronization may attain significant speedups with this flag enabled, but applications with certain patterns of locking may see slowdowns. .

By default, this option is enabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5E8FB0EE-1058-4F53-A54A-C2247FE19FD5"><!-- --></a>`-XX:-UseCompressedOops`</dt>
<dd>

Disables the use of compressed pointers. By default, this option is enabled, and compressed pointers are used when Java heap sizes are less than 32 GB. When this option is enabled, object references are represented as 32-bit offsets instead of 64-bit pointers, which typically increases performance when running the application with Java heap sizes of less than 32 GB. This option works only for 64-bit JVMs.

It’s also possible to use compressed pointers when Java heap sizes are greater than 32 GB. See the `-XX:ObjectAlignmentInBytes` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-ED4A27EA-AEDE-472F-978D-F65578839E33"><!-- --></a>`XX:+UseGCLogRotation`</dt>
<dd>

Handles large log files. This option must be used with `-Xloggc:<span class="variable">filename</span>.`

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-DBB99499-527D-49F5-A3B0-568B3FFE9C6B"><!-- --></a>`-XX:NumberOfGClogFiles=<span class="variable">number of files</span>`</dt>
<dd>

Handles large log files. The `<span class="variable">number of files</span>` must be greater than or equal to `1`. The default is `1`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E739DA8F-6EC5-4881-A293-4204E75FDD16"><!-- --></a>`-XX:GCLogFileSize=<span class="variable">number</span>`</dt>
<dd>

Handles large log files. The `<span class="variable">number</span>` can be in the form of `<span class="variable">number</span>M` or `<span class="variable">number</span>K`. The default is set to `512K`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C6A677D3-D1D9-4DEF-8298-B80CC946654E"><!-- --></a>`-XX:+UseHugeTLBFS`</dt>
<dd>

<span class="bold">Linux only:</span> This option is the equivalent of specifying `-XX:+UseLargePages`. This option is disabled by default. This option pre-allocates all large pages up-front, when memory is reserved; consequently the JVM can’t dynamically grow or shrink large pages memory areas; see `-XX:UseTransparentHugePages` if you want this behavior.

See [Large Pages](java.htm#GUID-7BE7CD55-3AC3-4A96-BBDD-E4D9FC4FCCCB "You use large pages, also known as huge pages, as memory pages that are significantly larger than the standard memory page size (which varies depending on the processor and operating system). Large pages optimize processor Translation-Lookaside Buffers.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-51E05566-8DA5-4DBE-8300-B3DC28B1D3BB"><!-- --></a>`-XX:+UseLargePages`</dt>
<dd>

Enables the use of large page memory. By default, this option is disabled and large page memory isn’t used.

See [Large Pages](java.htm#GUID-7BE7CD55-3AC3-4A96-BBDD-E4D9FC4FCCCB "You use large pages, also known as huge pages, as memory pages that are significantly larger than the standard memory page size (which varies depending on the processor and operating system). Large pages optimize processor Translation-Lookaside Buffers.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-800C2D61-DA7D-4755-B507-687634FFB601"><!-- --></a>`-XX:+UseMembar`</dt>
<dd>

Enables issuing of membars on thread-state transitions. This option is disabled by default on all platforms except ARM servers, where it’s enabled. (It’s recommended that you don’t disable this option on ARM servers.)

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E89486E9-FE76-4C30-9A8C-08A0E5224DFE"><!-- --></a>`-XX:+UsePerfData`</dt>
<dd>

Enables the `perfdata` feature. This option is enabled by default to allow JVM monitoring and performance testing. Disabling it suppresses the creation of the `hsperfdata_userid` directories. To disable the `perfdata` feature, specify `-XX:-UsePerfData`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-83E85C0B-F13D-4669-BBDE-8E63705D5D30"><!-- --></a>`-XX:+UseTransparentHugePages`</dt>
<dd>

<span class="bold">Linux only:</span> Enables the use of large pages that can dynamically grow or shrink. This option is disabled by default. You may encounter performance problems with transparent huge pages as the OS moves other pages around to create huge pages; this option is made available for experimentation.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-471A9FC9-CABB-4CC9-B327-BA66A560C2FC"><!-- --></a>`-XX:+AllowUserSignalHandlers`</dt>
<dd>

Enables installation of signal handlers by the application. By default, this option is disabled and the application isn’t allowed to install signal handlers.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1E3C83E8-B1F1-49D6-824B-B4CE1E22F088"><!-- --></a>`-XX:VMOptionsFile=<span class="variable">filename</span>`</dt>
<dd>

Allows user to specify VM options in a file, for example, `java -XX:VMOptionsFile=/var/my_vm_options HelloWorld`.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABDDFII">

Advanced JIT Compiler Options for java

These `java` options control the dynamic just-in-time (JIT) compilation performed by the Java HotSpot VM.

<dl class="1.00* 3.00*">
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-74288A34-3018-4050-9713-236DFEF0F54B"><!-- --></a><span class="bold">`-XX:+AggressiveOpts`</span></dt>
<dd>

Enables the use of aggressive performance optimization features. By default, this option is disabled and experimental performance features aren’t used.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-CDC345C8-0417-49AB-9F04-D32666D4BBBA"><!-- --></a><span class="bold">`-XX:AllocateInstancePrefetchLines=<span class="variable">lines</span>`</span></dt>
<dd>

Sets the number of lines to prefetch ahead of the instance allocation pointer. By default, the number of lines to prefetch is set to 1:

<pre dir="ltr">-XX:AllocateInstancePrefetchLines=1
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-6A20A6D6-5641-4084-9BBD-475E69624556"><!-- --></a><span class="bold">`-XX:AllocatePrefetchDistance=<span class="variable">size</span>`</span></dt>
<dd>

Sets the size (in bytes) of the prefetch distance for object allocation. Memory about to be written with the value of new objects is prefetched up to this distance starting from the address of the last allocated object. Each Java thread has its own allocation point.

Negative values denote that prefetch distance is chosen based on the platform. Positive values are bytes to prefetch. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. The default value is set to -1.

The following example shows how to set the prefetch distance to 1024 bytes:

<pre dir="ltr">-XX:AllocatePrefetchDistance=1024
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3F6467A4-634A-4FB4-91F3-A5E73FCED3BB"><!-- --></a><span class="bold">`-XX:AllocatePrefetchInstr=<span class="variable">instruction</span>`</span></dt>
<dd>

Sets the prefetch instruction to prefetch ahead of the allocation pointer. Only the Java HotSpot Server VM supports this option. Possible values are from 0 to 3. The actual instructions behind the values depend on the platform. By default, the prefetch instruction is set to 0:

<pre dir="ltr">-XX:AllocatePrefetchInstr=0
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-EF2F09D4-65DE-4AC8-A822-05134DB84BCE"><!-- --></a><span class="bold">`-XX:AllocatePrefetchLines=<span class="variable">lines</span>`</span></dt>
<dd>

Sets the number of cache lines to load after the last object allocation by using the prefetch instructions generated in compiled code. The default value is 1 if the last allocated object was an instance, and 3 if it was an array.

The following example shows how to set the number of loaded cache lines to 5:

<pre dir="ltr">-XX:AllocatePrefetchLines=5
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-74CDE8DD-9511-44E0-9012-77E91F3D9BCF"><!-- --></a><span class="bold">`-XX:AllocatePrefetchStepSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the step size (in bytes) for sequential prefetch instructions. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. By default, the step size is set to 16 bytes:

<pre dir="ltr">-XX:AllocatePrefetchStepSize=16
</pre>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C16A1109-E2C1-4A44-B0B2-77FB179B5748"><!-- --></a><span class="bold">`-XX:AllocatePrefetchStyle=<span class="variable">style</span>`</span></dt>
<dd>

Sets the generated code style for prefetch instructions. The `<span class="variable">style</span>` argument is an integer from 0 to 3:

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-49E6F329-6FA6-47E8-8C5A-0DF7CCEAFE44"><!-- --></a><span class="bold">`0`</span></dt>
<dd>

Don’t generate prefetch instructions.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-EC049189-F132-4F8A-B5BE-DFF321EC864C"><!-- --></a><span class="bold">`1`</span></dt>
<dd>

Execute prefetch instructions after each allocation. This is the default parameter.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F6212D64-E8AC-4CE8-A3A9-4B42DE03D5A1"><!-- --></a><span class="bold">`2`</span></dt>
<dd>

Use the thread-local allocation block (TLAB) watermark pointer to determine when prefetch instructions are executed.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2E35459A-46BE-4171-B40A-ADF229E48BEA"><!-- --></a><span class="bold">`3`</span></dt>
<dd>

Use BIS instruction on SPARC for allocation prefetch.

</dd>
</dl>

Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-7D2D88A8-2D90-4C8E-A10D-7BACA052D5DA"><!-- --></a><span class="bold">`-XX:+BackgroundCompilation`</span></dt>
<dd>

Enables background compilation. This option is enabled by default. To disable background compilation, specify `-XX:-BackgroundCompilation` (this is equivalent to specifying `-Xbatch`).

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-7DAB6574-7606-47D6-973E-C73171F0F786"><!-- --></a><span class="bold">`-XX:CICompilerCount=<span class="variable">threads</span>`</span></dt>
<dd>

Sets the number of compiler threads to use for compilation. By default, the number of threads is set to 2 for the server JVM, to 1 for the client JVM, and it scales to the number of cores if tiered compilation is used. The following example shows how to set the number of threads to 2:

<pre dir="ltr">-XX:CICompilerCount=2
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-EEFF3F9F-FB7B-432A-A2FD-D1E45662116E"><!-- --></a><span class="bold">`-XX:CompileCommand=<span class="variable">command</span>,<span class="variable">method</span>[,<span class="variable">option</span>]`</span></dt>
<dd>

Specifies a command to perform on a method. For example, to exclude the `indexOf()` method of the `String` class from being compiled, use the following:

<pre dir="ltr">-XX:CompileCommand=exclude,java/lang/String.indexOf
</pre>

Note that the full class name is specified, including all packages and subpackages separated by a slash (`/`). For easier cut-and-paste operations, it’s also possible to use the method name format produced by the `-XX:+PrintCompilation` and `-XX:+LogCompilation` options:

<pre dir="ltr">-XX:CompileCommand=exclude,java.lang.String::indexOf
</pre>

If the method is specified without the signature, then the command isapplied to all methods with the specified name. However, you can also specify the signature of the method in the class file format. In this case, you should enclose the arguments in quotation marks, because otherwise the shell treats the semicolon as a command end. For example, if you want to exclude only the `indexOf(String)` method of the `String` class from being compiled, use the following:

<pre dir="ltr">-XX:CompileCommand="exclude,java/lang/String.indexOf,(Ljava/lang/String;)I"
</pre>

You can also use the asterisk (*) as a wildcard for class and method names. For example, to exclude all `indexOf()` methods in all classes from being compiled, use the following:

<pre dir="ltr">-XX:CompileCommand=exclude,*.indexOf
</pre>

The commas and periods are aliases for spaces, making it easier to pass compiler commands through a shell. You can pass arguments to `-XX:CompileCommand` using spaces as separators by enclosing the argument in quotation marks:

<pre dir="ltr">-XX:CompileCommand="exclude java/lang/String indexOf"
</pre>

Note that after parsing the commands passed on the command line using the `-XX:CompileCommand` options, the JIT compiler then reads commands from the `.hotspot_compiler` file. You can add commands to this file or specify a different file using the `-XX:CompileCommandFile` option.

To add several commands, either specify the `-XX:CompileCommand` option multiple times, or separate each argument with the new line separator (`\n`). The following commands are available:

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-78217B83-C102-4489-9DE4-ABAFF133EC72"><!-- --></a><span class="bold">`break`</span></dt>
<dd>

Sets a breakpoint when debugging the JVM to stop at the beginning of compilation of the specified method.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-42240DD6-1E02-448A-A00B-950F7DDF8E44"><!-- --></a><span class="bold">`compileonly`</span></dt>
<dd>

Excludes all methods from compilation except for the specified method. As an alternative, you can use the `-XX:CompileOnly` option, which lets you specify several methods.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-951223F7-88B6-4F89-98C8-0E19652351CC"><!-- --></a><span class="bold">`dontinline`</span></dt>
<dd>

Prevents inlining of the specified method.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-59F82668-7F74-4880-A065-A85AA9C47035"><!-- --></a><span class="bold">`exclude`</span></dt>
<dd>

Excludes the specified method from compilation.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-985ADB6D-3338-4FB2-8AFE-88A568B6672A"><!-- --></a><span class="bold">`help`</span></dt>
<dd>

Prints a help message for the `-XX:CompileCommand` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1C60AC1A-5EB6-4284-B879-3441F2CF3343"><!-- --></a><span class="bold">`inline`</span></dt>
<dd>

Attempts to inline the specified method.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-31A550BD-585F-493D-B884-A4EE059B5E52"><!-- --></a><span class="bold">`log`</span></dt>
<dd>

Excludes compilation logging (with the `-XX:+LogCompilation` option) for all methods except for the specified method. By default, logging is performed for all compiled methods.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C9B8F7AB-F229-467E-8A97-1B6EF4E50A28"><!-- --></a><span class="bold">`option`</span></dt>
<dd>

Passes a JIT compilation option to the specified method in place of the last argument (`<span class="variable">option</span>`). The compilation option is set at the end, after the method name. For example, to enable the `BlockLayoutByFrequency` option for the `append()` method of the `StringBuffer` class, use the following:

<pre dir="ltr">-XX:CompileCommand=option,java/lang/StringBuffer.append,BlockLayoutByFrequency
</pre>

You can specify multiple compilation options, separated by commas or spaces.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F5D59766-D26D-4553-9797-A0118A7501CB"><!-- --></a><span class="bold">`print`</span></dt>
<dd>

Prints generated assembler code after compilation of the specified method.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-12905E67-19B6-4620-A614-D77FFD21CA7D"><!-- --></a><span class="bold">`quiet`</span></dt>
<dd>

Instructs not to print the compile commands. By default, the commands that you specify with the -`XX:CompileCommand` option are printed; for example, if you exclude from compilation the `indexOf()` method of the `String` class, then the following is printed to standard output:

<pre dir="ltr">CompilerOracle: exclude java/lang/String.indexOf
</pre>

You can suppress this by specifying the `-XX:CompileCommand=quiet` option before other `-XX:CompileCommand` options.

</dd>
</dl>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C392678F-67D9-4B4C-8C24-3AA55C3451EC"><!-- --></a><span class="bold">`-XX:CompileCommandFile=<span class="variable">filename</span>`</span></dt>
<dd>

Sets the file from which JIT compiler commands are read. By default, the `.hotspot_compiler` file is used to store commands performed by the JIT compiler.

Each line in the command file represents a command, a class name, and a method name for which the command is used. For example, this line prints assembly code for the `toString()` method of the `String` class:

<pre dir="ltr">print java/lang/String toString
</pre>

If you’re using commands for the JIT compiler to perform on methods, then see the `-XX:CompileCommand` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-DCA373B1-DAF0-40E8-9840-321896DD3988"><!-- --></a><span class="bold">`-XX:CompileOnly=<span class="variable">methods</span>`</span></dt>
<dd>

Sets the list of methods (separated by commas) to which compilation should be restricted. Only the specified methods are compiled. Specify each method with the full class name (including the packages and subpackages). For example, to compile only the `length()` method of the `String` class and the `size()` method of the `List` class, use the following:

<pre dir="ltr">-XX:CompileOnly=java/lang/String.length,java/util/List.size
</pre>

Note that the full class name is specified, including all packages and subpackages separated by a slash (`/`). For easier cut and paste operations, it’s also possible to use the method name format produced by the `-XX:+PrintCompilation` and `-XX:+LogCompilation` options:

<pre dir="ltr">-XX:CompileOnly=java.lang.String::length,java.util.List::size
</pre>

Although wildcards aren’t supported, you can specify only the class or package name to compile all methods in that class or package, as well as specify just the method to compile methods with this name in any class:

<pre dir="ltr">-XX:CompileOnly=java/lang/String
-XX:CompileOnly=java/lang
-XX:CompileOnly=.length
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-BC0E8E98-5F8B-4430-B09F-CD3DEA753E14"><!-- --></a><span class="bold">`-XX:CompileThreshold=<span class="variable">invocations</span>`</span></dt>
<dd>

Sets the number of interpreted method invocations before compilation. By default, in the server JVM, the JIT compiler performs 10,000 interpreted method invocations to gather information for efficient compilation. For the client JVM, the default setting is 1,500 invocations. This option is ignored when tiered compilation is enabled; see the option `-XX:-TieredCompilation`. The following example shows how to set the number of interpreted method invocations to 5,000:

<pre dir="ltr">-XX:CompileThreshold=5000
</pre>

You can completely disable interpretation of Java methods before compilation by specifying the `-Xcomp` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-91EBC542-755A-481C-A6E0-C5F905C1A003"><!-- --></a><span class="bold">`-XX:CompileThresholdScaling=<span class="variable">scale</span>`</span></dt>
<dd>

Provides unified control of first compilation. This option controls when methods are first compiled for both the tiered and the nontiered modes of operation. The `CompileThresholdScaling` option has an integer value between 0 and +Inf and scales the thresholds corresponding to the current mode of operation (both tiered and nontiered). Setting `CompileThresholdScaling` to a value less than 1.0 results in earlier compilation while values greater than 1.0 delay compilation. Setting `CompileThresholdScaling` to 0 is equivalent to disabling compilation.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F28BD5C4-1DD8-479C-96F4-F872AC1F0672"><!-- --></a><span class="bold">`-XX:+DoEscapeAnalysis`</span></dt>
<dd>

Enables the use of escape analysis. This option is enabled by default. To disable the use of escape analysis, specify `-XX:-DoEscapeAnalysis`. Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1E08AD37-3DB9-41FD-9895-EE8B4F0B022D"><!-- --></a><span class="bold">`-XX:InitialCodeCacheSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the initial code cache size (in bytes). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. The default value is set to 500 KB. The initial code cache size shouldn’t be less than the system's minimal memory page size. The following example shows how to set the initial code cache size to 32 KB:

<pre dir="ltr">-XX:InitialCodeCacheSize=32k
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1A963FA2-BD3E-4D80-8249-5397D40BE969"><!-- --></a><span class="bold">`-XX:+Inline`</span></dt>
<dd>

Enables method inlining. This option is enabled by default to increase performance. To disable method inlining, specify `-XX:-Inline`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-09A613D3-69CC-4108-B7AB-08E73B078894"><!-- --></a><span class="bold">`-XX:InlineSmallCode=<span class="variable">size</span>`</span></dt>
<dd>

Sets the maximum code size (in bytes) for compiled methods that should be inlined. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. Only compiled methods with the size smaller than the specified size is inlined. By default, the maximum code size is set to 1000 bytes:

<pre dir="ltr">-XX:InlineSmallCode=1000
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-58C5D621-40D7-4B01-ADA3-4BBEF50D8412"><!-- --></a><span class="bold">`-XX:+LogCompilation`</span></dt>
<dd>

Enables logging of compilation activity to a file named `hotspot.log` in the current working directory. You can specify a different log file path and name using the `-XX:LogFile` option.

By default, this option is disabled and compilation activity isn’t logged. The `-XX:+LogCompilation` option has to be used together with the `-XX:UnlockDiagnosticVMOptions` option that unlocks diagnostic JVM options.

You can enable verbose diagnostic output with a message printed to the console every time a method is compiled by using the `-XX:+PrintCompilation` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-BF1B5BDD-3512-427A-BE8B-CA72BCAD780B"><!-- --></a><span class="bold">`-XX:MaxInlineSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the maximum bytecode size (in bytes) of a method to be inlined. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. By default, the maximum bytecode size is set to 35 bytes:

<pre dir="ltr">-XX:MaxInlineSize=35
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-BF44D05B-13CF-43A6-BFD6-229E7A80D63B"><!-- --></a><span class="bold">`-XX:MaxNodeLimit=<span class="variable">nodes</span>`</span></dt>
<dd>

Sets the maximum number of nodes to be used during single method compilation. By default, the maximum number of nodes is set to 65,000:

<pre dir="ltr">-XX:MaxNodeLimit=65000
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F93786D2-159C-4522-ADDB-9AC1F331F0C7"><!-- --></a><span class="bold">`-XX:NonNMethodCodeHeapSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the size in bytes of the code segment containing nonmethod code.

A nonmethod code segment containing nonmethod code, such as compiler buffers and the bytecode interpreter. This code type stays in the code cache forever. This flag is used only if `—XX:SegmentedCodeCache` is enabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5085D923-C63B-449F-97A2-78FF46824F5C"><!-- --></a><span class="bold">`—XX:NonProfiledCodeHeapSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the size in bytes of the code segment containing nonprofiled methods. This flag is used only if `—XX:SegmentedCodeCache` is enabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-B0BD6950-B293-48C0-8349-3594D9497972"><!-- --></a><span class="bold">`-XX:MaxTrivialSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the maximum bytecode size (in bytes) of a trivial method to be inlined. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. By default, the maximum bytecode size of a trivial method is set to 6 bytes:

<pre dir="ltr">-XX:MaxTrivialSize=6
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2087CC3B-E7F0-4DAA-B42D-4F9FE3A8419B"><!-- --></a><span class="bold">`-XX:+OptimizeStringConcat`</span></dt>
<dd>

Enables the optimization of `String` concatenation operations. This option is enabled by default. To disable the optimization of `String` concatenation operations, specify `-XX:-OptimizeStringConcat`. Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-CA098685-2F70-4D8C-8F75-D003CFE62022"><!-- --></a>`-XX:+PrintAssembly`</dt>
<dd>

Enables printing of assembly code for bytecoded and native methods by using the external `hsdis-&lt;arch&gt;.so` or `.dll` library. For 64-bit VM on Windows, it’s `hsdis-amd64.dll`. This let’s you to see the generated code, which may help you to diagnose performance issues.

By default, this option is disabled and assembly code isn’t printed. The `-XX:+PrintAssembly` option has to be used together with the `-XX:UnlockDiagnosticVMOptions` option that unlocks diagnostic JVM options.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-D9263E0B-6613-40A9-A37D-CB35FF5CC983"><!-- --></a><span class="bold">`-XX:ProfiledCodeHeapSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the size in bytes of the code segment containing profiled methods. This flag is used only if `—XX:SegmentedCodeCache` is enabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-499709B1-E4A1-4C39-88B2-5ABFCC20831B"><!-- --></a><span class="bold">`-XX:+PrintCompilation`</span></dt>
<dd>

Enables verbose diagnostic output from the JVM by printing a message to the console every time a method is compiled. This let’s you to see which methods actually get compiled. By default, this option is disabled and diagnostic output isn’t printed.

You can also log compilation activity to a file by using the `-XX:+LogCompilation` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F72125D2-B44F-4A91-8BFE-16795A2A40B5"><!-- --></a><span class="bold">`-XX:+PrintInlining`</span></dt>
<dd>

Enables printing of inlining decisions. This let’s you to see which methods are getting inlined.

By default, this option is disabled and inlining information isn’t printed. The `-XX:+PrintInlining` option has to be used together with the `-XX:+UnlockDiagnosticVMOptions` option that unlocks diagnostic JVM options.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3CFE74F1-EBAF-4FDA-8472-9C4D583D2723"><!-- --></a><span class="bold">`-XX:ReservedCodeCacheSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the maximum code cache size (in bytes) for JIT-compiled code. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. The default maximum code cache size is 240 MB; if you disable tiered compilation with the option `-XX:-TieredCompilation`, then the default size is 48 MB. This option has a limit of 2 GB; otherwise, an error is generated. The maximum code cache size shouldn’t be less than the initial code cache size; see the option `-XX:InitialCodeCacheSize`. This option is equivalent to `-Xmaxjitcodesize`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-7B470A4F-E63F-4614-83AF-3018D36E9509"><!-- --></a><span class="bold">`-XX:RTMAbortRatio=<span class="variable">abort_ratio</span>`</span></dt>
<dd>

Specifies the RTM abort ratio is specified as a percentage (%) of all executed RTM transactions. If a number of aborted transactions becomes greater than this ratio, then the compiled code is deoptimized. This ratio is used when the `-XX:+UseRTMDeopt` option is enabled. The default value of this option is 50. This means that the compiled code is deoptimized if 50% of all transactions are aborted.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-24C7711F-3C49-4007-BF16-002B28A8F4B2"><!-- --></a><span class="bold">`-XX:+SegmentedCodeCache`</span></dt>
<dd>

Enables segmentation of the code cache. Without the `—XX:+SegmentedCodeCache`, the code cache consists of one large segment. With `—XX:+SegmentedCodeCache`, we have separate segments for nonmethod, profiled method, and nonprofiled method code. These segments aren’t resized at runtime. The feature is enabled by default if tiered compilation is enabled (`-XX:+TieredCompilation` ) and `-XX:ReservedCodeCacheSize` &gt;= 240 MB. The advantages are better control of the memory footprint, reduced code fragmentation, and better iTLB/iCache behavior due to improved locality. iTLB/iCache is a CPU-specific term meaning Instruction Translation Lookaside Buffer (ITLB). ICache is an instruction cache in theCPU. The implementation of the code cache can be found in the file: `/share/vm/code/codeCache.cpp`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-208BCEC0-ABCE-4B75-88C7-52682B06738A"><!-- --></a><span class="bold">`-XX:StartAggressiveSweepingAt=<span class="variable">percent</span>`</span></dt>
<dd>

Forces stack scanning of active methods to aggressively remove unused code when only the given percentage of the code cache is free. The default value is 10%.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-AED1A461-3648-4F24-8504-2652E4009D0B"><!-- --></a><span class="bold">`-XX:RTMRetryCount=<span class="variable">number_of_retries</span>`</span></dt>
<dd>

Specifies the number of times that the RTM locking code is retried, when it is aborted or busy, before falling back to the normal locking mechanism. The default value for this option is 5. The `-XX:UseRTMLocking` option must be enabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-D15D1A39-DED8-4409-9FBA-BE06E23E8C38"><!-- --></a><span class="bold">`-XX:-TieredCompilation`</span></dt>
<dd>

Disables the use of tiered compilation. By default, this option is enabled. Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-89946F0C-73DE-4DFC-B137-A86FD94EDAD6"><!-- --></a><span class="bold">`-XX:+UseAES`</span></dt>
<dd>

Enables hardware-based AES intrinsics for Intel, AMD, and SPARC hardware. Intel Westmere (2010 and newer), AMD Bulldozer (2011 and newer), and SPARC (T4 and newer) are the supported hardware. The `-XX:+UseAES` is used in conjunction with UseAESIntrinsics. Flags that control intrinsics now require the option `-XX:+UnlockDiagnosticVMOptions`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-184F07C5-102C-4DD0-B11B-EE344F8E82ED"><!-- --></a><span class="bold">`-XX:+UseAESIntrinsics`</span></dt>
<dd>

Enables `-XX:+UseAES` and `-XX:+UseAESIntrinsics` flags by default and are supported only for the Java HotSpot Server VM 32-bit and 64-bit. To disable hardware-based AES intrinsics, specify `-XX:-UseAES -XX:-UseAESIntrinsics`. For example, to enable hardware AES, use the following flags:

<pre dir="ltr">-XX:+UseAES -XX:+UseAESIntrinsics
</pre>

Flags that control intrinsics now require the option `-XX:+UnlockDiagnosticVMOptions`. To support UseAES and UseAESIntrinsics flags for 32-bit and 64-bit, use the `-server` option to select the Java HotSpot Server VM. These flags aren’t supported on Client VM.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-57898B09-AB39-47DF-B044-216DB70F5064"><!-- --></a><span class="bold">`-XX:+UseCMoveUnconditionally`</span></dt>
<dd>

Generates CMove (scalar and vector) instructions regardless of profitability analysis.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1FB47631-E505-44C4-9228-1DDEE9641CA4"><!-- --></a><span class="bold">`-XX:+UseCodeCacheFlushing`</span></dt>
<dd>

Enables flushing of the code cache before shutting down the compiler. This option is enabled by default. To disable flushing of the code cache before shutting down the compiler, specify `-XX:-UseCodeCacheFlushing`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-DDFF278B-0065-4D45-BDD7-9AADEE3582B9"><!-- --></a><span class="bold">`-XX:+UseCondCardMark`</span></dt>
<dd>

Enables checking if the card is already marked before updating the card table. This option is disabled by default. It should be used only on machines with multiple sockets, where it increases the performance of Java applications that rely on concurrent operations. Only the Java HotSpot Server VM supports this option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F4A3D081-4CB5-48E6-8D4E-AB37CBFBD868"><!-- --></a><span class="bold">`-XX:+UseCountedLoopSafepoints`</span></dt>
<dd>

Keeps safepoints in counted loops. Its default value is false.&nbsp;

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-4518DD05-D6D1-4FAD-9D35-E124580D689A"><!-- --></a><span class="bold">`-XX:+UseFMA`</span></dt>
<dd>

Enables hardware-based FMA intrinsics for hardware where FMA instructions are available (such as, Intel, SPARC, and ARM64). FMA intrinsics are generated for the `java.lang.Math.fma(<span class="variable">a</span>, <span class="variable">b</span>, <span class="variable">c</span>)` methods that calculate the value of (`<span class="variable">a</span> * <span class="variable">b</span> + <span class="variable">c</span>`) expressions.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-FDDF35E6-A336-447D-9E9D-C23FD995E066"><!-- --></a><span class="bold">`-XX:+UseRTMDeopt`</span></dt>
<dd>

Autotunes RTM locking depending on the abort ratio. This ratio is specified by the `-XX:RTMAbortRatio` option. If the number of aborted transactions exceeds the abort ratio, then the method containing the lock is deoptimized and recompiled with all locks as normal locks. This option is disabled by default. The `-XX:+UseRTMLocking` option must be enabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F702DC4A-EA68-4CF9-9A6B-D8440D5639B1"><!-- --></a><span class="bold">`-XX:+UseRTMLocking`</span></dt>
<dd>

Generates Restricted Transactional Memory (RTM) locking code for all inflated locks, with the normal locking mechanism as the fallback handler. This option is disabled by default. Options related to RTM are available only for the Java HotSpot Server VM on x86 CPUs that support Transactional Synchronization Extensions (TSX).

RTM is part of Intel's TSX, which is an x86 instruction set extension and facilitates the creation of multithreaded applications. RTM introduces the new instructions `XBEGIN`, `XABORT`, `XEND`, and `XTEST`. The `XBEGIN` and `XEND` instructions enclose a set of instructions to run as a transaction. If no conflict is found when running the transaction, then the memory and register modifications are committed together at the `XEND` instruction. The `XABORT` instruction can be used to explicitly abort a transaction and the `XEND` instruction checks if a set of instructions is being run in a transaction.

A lock on a transaction is inflated when another thread tries to access the same transaction, thereby blocking the thread that didn’t originally request access to the transaction. RTM requires that a fallback set of operations be specified in case a transaction aborts or fails. An RTM lock is a lock that has been delegated to the TSX's system.

RTM improves performance for highly contended locks with low conflict in a critical region (which is code that must not be accessed by more than one thread concurrently). RTM also improves the performance of coarse-grain locking, which typically doesn’t perform well in multithreaded applications. (Coarse-grain locking is the strategy of holding locks for long periods to minimize the overhead of taking and releasing locks, while fine-grained locking is the strategy of trying to achieve maximum parallelism by locking only when necessary and unlocking as soon as possible.) Also, for lightly contended locks that are used by different threads, RTM can reduce false cache line sharing, also known as cache line ping-pong. This occurs when multiple threads from different processors are accessing different resources, but the resources share the same cache line. As a result, the processors repeatedly invalidate the cache lines of other processors, which forces them to read from main memory instead of their cache.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C8FC7000-89CD-4E47-93D4-0D23908BFFD4"><!-- --></a><span class="bold">`-XX:+UseSHA`</span></dt>
<dd>

Enables hardware-based intrinsics for SHA crypto hash functions for SPARC hardware. The `UseSHA` option is used in conjunction with the `UseSHA1Intrinsics`, `UseSHA256Intrinsics`, and `UseSHA512Intrinsics` options.

The `UseSHA` and `UseSHA*Intrinsics` flags are enabled by default, and are supported only for Java HotSpot Server VM 64-bit on SPARC T4 and newer.

This feature is applicable only when using the `sun.security.provider.Sun` provider for SHA operations. Flags that control intrinsics now require the option `-XX:+UnlockDiagnosticVMOptions`.

To disable all hardware-based SHA intrinsics, specify the `-XX:-UseSHA`. To disable only a particular SHA intrinsic, use the appropriate corresponding option. For example: `-XX:-UseSHA256Intrinsics`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-9C9914C4-F4F6-4E65-B52F-C3D8F76D096B"><!-- --></a><span class="bold">`-XX:+UseSHA1Intrinsics`</span></dt>
<dd>

Enables intrinsics for SHA-1 crypto hash function. Flags that control intrinsics now require the option`-XX:+UnlockDiagnosticVMOptions`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0B136A5A-0867-46F7-A592-99D53AAC8212"><!-- --></a><span class="bold">`-XX:+UseSHA256Intrinsics`</span></dt>
<dd>

Enables intrinsics for SHA-224 and SHA-256 crypto hash functions. Flags that control intrinsics now require the option`-XX:+UnlockDiagnosticVMOptions`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-00ABA50D-4CD0-4968-8E42-12001A11BDE2"><!-- --></a><span class="bold">`-XX:+UseSHA512Intrinsics`</span></dt>
<dd>

Enables intrinsics for SHA-384 and SHA-512 crypto hash functions. Flags that control intrinsics now require the option`-XX:+UnlockDiagnosticVMOptions`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-7E9D63AE-254C-4A5D-B9CF-A66FF4755FAE"><!-- --></a><span class="bold">`-XX:+UseSuperWord`</span></dt>
<dd>

Enables the transformation of scalar operations into superword operations. Superword is a vectorization optimization. This option is enabled by default. To disable the transformation of scalar operations into superword operations, specify `-XX:-UseSuperWord`. Only the Java HotSpot Server VM supports this option.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABFJDIC">

Advanced Serviceability Options for Java

These `java` options provide the ability to gather system information and perform extensive debugging.

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-65231785-2AB8-43C4-BB64-72E4B391DF67"><!-- --></a><span class="bold">`-XX:+ExtendedDTraceProbes`</span></dt>
<dd>

<span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> Enables additional `dtrace` tool probes that affect the performance. By default, this option is disabled and `dtrace` performs only standard probes.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-54771326-1CB1-4D43-A22E-7D75C4ADC153"><!-- --></a><span class="bold">`-XX:+HeapDumpOnOutOfMemoryError`</span></dt>
<dd>

Enables the dumping of the Java heap to a file in the current directory by using the heap profiler (HPROF) when a `java.lang.OutOfMemoryError` exception is thrown. You can explicitly set the heap dump file path and name using the `-XX:HeapDumpPath` option. By default, this option is disabled and the heap isn’t dumped when an `OutOfMemoryError` exception is thrown.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-38364443-118C-4536-8AB5-DF0933997159"><!-- --></a><span class="bold">`-XX:HeapDumpPath=<span class="variable">path</span>`</span></dt>
<dd>

Sets the path and file name for writing the heap dump provided by the heap profiler (HPROF) when the `-XX:+HeapDumpOnOutOfMemoryError` option is set. By default, the file is created in the current working directory, and it’s named `java_pid``pid``.hprof` where `pid` is the identifier of the process that caused the error. The following example shows how to set the default file explicitly (`%p` represents the current process identifier):

<pre dir="ltr">-XX:HeapDumpPath=./java_pid%p.hprof
</pre>

*   <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> The following example shows how to set the heap dump file to `/var/log/java/java_heapdump.hprof`:

    <pre dir="ltr">-XX:HeapDumpPath=/var/log/java/java_heapdump.hprof
</pre>
*   <span class="bold">Windows:</span> The following example shows how to set the heap dump file to `C:/log/java/java_heapdump.log`:

    <pre dir="ltr">-XX:HeapDumpPath=C:/log/java/java_heapdump.log
</pre>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0A32E900-3405-49B3-9BCE-A2B7598A1FA8"><!-- --></a><span class="bold">`-XX:LogFile=<span class="variable">path</span>`</span></dt>
<dd>

Sets the path and file name to where log data is written. By default, the file is created in the current working directory, and it’s named `hotspot.log`.

*   <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> The following example shows how to set the log file to `/var/log/java/hotspot.log`:

    <pre dir="ltr">-XX:LogFile=/var/log/java/hotspot.log
</pre>
*   <span class="bold">Windows:</span> The following example shows how to set the log file to `C:/log/java/hotspot.log`:

    <pre dir="ltr">-XX:LogFile=C:/log/java/hotspot.log
</pre>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-A19C2751-E79A-40FA-AFCA-89BF5326BA8E"><!-- --></a><span class="bold">`-XX:+PrintClassHistogram`</span></dt>
<dd>

Enables printing of a class instance histogram after one of the following events:

*   <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> `Control+Break`

*   <span class="bold">Windows:</span> `Control+C` (`SIGTERM`)

By default, this option is disabled.

Setting this option is equivalent to running the `jmap -histo` command, or the `jcmd <span class="variable">pid</span> GC.class_histogram` command, where `<span class="variable">pid</span>` is the current Java process identifier.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-EA8F2041-B541-45D7-A9FC-D9F7BEAD6B3C"><!-- --></a><span class="bold">`-XX:+PrintConcurrentLocks`</span></dt>
<dd>

Enables printing of `java.util.concurrent` locks after one of the following events:

*   <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span> `Control+Break`

*   <span class="bold">Windows:</span> `Control+C` (`SIGTERM`)

By default, this option is disabled.

Setting this option is equivalent to running the `jstack -l` command or the`jcmd <span class="variable">pid</span> Thread.print -l` command, where `<span class="variable">pid</span>` is the current Java process identifier.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3C2AF1D0-CCDA-481E-A697-1AC875347BC2"><!-- --></a><span class="bold">`-XX:+PrintFlagsRanges`</span></dt>
<dd>

Prints the range specified and allows automatic testing of the values. See [Validate Java Virtual Machine Flag Arguments](java.htm#GUID-9569449C-525F-4474-972C-4C1F63D5C357 "You use values provided to all Java Virtual Machine (JVM) command-line flags for validation and, if the input value is invalid or out-of-range, then an appropriate error message is displayed.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E0FD8F37-0D2A-4686-B59A-F0C83CC196A4"><!-- --></a><span class="bold">`-XX:+UnlockDiagnosticVMOptions`</span></dt>
<dd>

Unlocks the options intended for diagnosing the JVM. By default, this option is disabled and diagnostic options aren’t available.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__BABFAFAE">

Advanced Garbage Collection Options for Java

These `java` options control how garbage collection (GC) is performed by the Java HotSpot VM.

<dl>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-6C4E6A3C-05B5-4037-B812-A7912712ED43"><!-- --></a><span class="bold">`-XX:+AggressiveHeap`</span></dt>
<dd>

Enables Java heap optimization. This sets various parameters to be optimal for long-running jobs with intensive memory allocation, based on the configuration of the computer (RAM and CPU). By default, the option is disabled and the heap isn’t optimized.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-228D6C58-31A4-488D-810E-296FED374B59"><!-- --></a><span class="bold">`-XX:+AlwaysPreTouch`</span></dt>
<dd>

Enables touching of every page on the Java heap during JVM initialization. This gets all pages into memory before entering the `main()` method. The option can be used in testing to simulate a long-running system with all virtual memory mapped to physical memory. By default, this option is disabled and all pages are committed as JVM heap space fills.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-31D382AC-4730-45BE-A921-C0C4A05BBAF8"><!-- --></a><span class="bold">`-XX:+CMSClassUnloadingEnabled`</span></dt>
<dd>

Enables class unloading when using the concurrent mark-sweep (CMS) garbage collector. This option is enabled by default. To disable class unloading for the CMS garbage collector, specify `-XX:-CMSClassUnloadingEnabled`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-24323CF9-88AF-4814-96F2-0D94ECBA8406"><!-- --></a><span class="bold">`-XX:CMSExpAvgFactor=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage of time (0 to 100) used to weight the current sample when computing exponential averages for the concurrent collection statistics. By default, the exponential averages factor is set to 25%. The following example shows how to set the factor to 15%:

<pre dir="ltr">-XX:CMSExpAvgFactor=15
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-8E1AC53B-4CC0-4A89-A300-4C31DF5013A2"><!-- --></a><span class="bold">`-XX:CMSIncrementalDutyCycle=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage (0 to 100) of time between minor collections that the CMS collector is allowed to run. If `CMSIncrementalPacing` is enabled, then this is just the initial value. The default value is 10.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-9A8166B1-96B3-4A33-8BAB-D73843793EA8"><!-- --></a><span class="bold">`-XX:CMSIncrementalDutyCycleMin=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage (0 to 100) that’s the lower bound on the duty cycle when `CMSIncrementalPacing` is enabled. The default value is 0.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-75D457E3-D16E-4A8F-912F-5EE8988722BD"><!-- --></a><span class="bold">`-XX:CMSIncrementalDutySafetyFactor=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage (0 to 100) used to add conservatism when computing the duty cycle. The default value is 10.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-430EC636-B612-4FB0-A418-AF19A54BD061"><!-- --></a><span class="bold">`-XX:CMSIncrementalOffset=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage (0 to 100) by which the incremental mode duty cycle is shifted to the right within the period between minor collections. The default value is 0.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-28DDBD36-EFA5-4497-8DE4-5F33955E33F1"><!-- --></a><span class="bold">`-XX:+CMSIncrementalPacing`</span></dt>
<dd>

Enables automatic pacing. The incremental mode duty cycle is automatically adjusted based on statistics collected while the JVM is running. By default, this option is disabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5EC52715-B974-482D-A6DC-515F27C3B504"><!-- --></a><span class="bold">`-XX:+CMSScavengeBeforeRemark`</span></dt>
<dd>

Enables scavenging attempts before the CMS remark step. By default, this option is disabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3F8A5BF0-67F5-4B55-A84C-BB34B95DE180"><!-- --></a><span class="bold">`-XX:CMSTriggerRatio=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage (0 to 100) of the value specified by the option `-XX:MinHeapFreeRatio` that’s allocated before a CMS collection cycle commences. The default value is set to 80%.

The following example shows how to set the occupancy fraction to 75%:

<pre dir="ltr">-XX:CMSTriggerRatio=75
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-EDAC2B01-CBD6-40DF-A345-0CCC1D90E5A6"><!-- --></a><span class="bold">`-XX:ConcGCThreads=<span class="variable">threads</span>`</span></dt>
<dd>

Sets the number of threads used for concurrent GC. Sets <span class="italic">`threads`</span> to approximately 1/4 of the number of parallel garbage collection threads. The default value depends on the number of CPUs available to the JVM.

For example, to set the number of threads for concurrent GC to 2, specify the following option:

<pre dir="ltr">-XX:ConcGCThreads=2
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C1BBE082-B974-4605-9F1E-A84F5A35C2F6"><!-- --></a><span class="bold">`-XX:+DisableExplicitGC`</span></dt>
<dd>

Enables the option that disables processing of calls to the `System.gc()` method. This option is disabled by default, meaning that calls to `System.gc()` are processed. If processing of calls to `System.gc()` is disabled, then the JVM still performs GC when necessary.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3C6DF7A2-77AB-486F-BFEC-59A9FC7B638B"><!-- --></a><span class="bold">`-XX:+ExplicitGCInvokesConcurrent`</span></dt>
<dd>

Enables invoking of concurrent GC by using the `System.gc()` request. This option is disabled by default and can be enabled only together with the `-XX:+UseConcMarkSweepGC` and `-XX:+UseG1GC` options.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-6281356F-EABB-4568-9BAE-9A0BB1CE0133"><!-- --></a><span class="bold">`-XX:+ExplicitGCInvokesConcurrentAndUnloadsClasses`</span></dt>
<dd>

Enables invoking of concurrent GC by using the `System.gc()` request and unloading of classes during the concurrent GC cycle. This option is disabled by default and can be enabled only together with the `-XX:+UseConcMarkSweepGC` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-B9F3FC5E-F03C-46D5-A0C7-1A0699CFD7D2"><!-- --></a><span class="bold">`-XX:G1HeapRegionSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the size of the regions into which the Java heap is subdivided when using the garbage-first (G1) collector. The value is a power of 2 and can range from 1 MB to 32 MB. The goal is to have around 2048 regions based on the minimum Java heap size. The default region size is determined ergonomically based on the heap size.

The following example sets the size of the subdivisions to 16 MB:

<pre dir="ltr">-XX:G1HeapRegionSize=16m
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5E738BEB-BD20-4ADC-B21E-E32595C4F2C1"><!-- --></a><span class="bold">`-XX:G1HeapWastePercent=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage of heap that you’re willing to waste. The Java HotSpot VM doesn’t initiate the mixed garbage collection cycle when the reclaimable percentage is less than the heap waste percentage. The default is 5 percent.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-23C38E1D-2DDF-42CD-BC3C-A36E065B1650"><!-- --></a><span class="bold">`-XX:G1MaxNewSizePercent=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage of the heap size to use as the maximum for the young generation size. The default value is 60 percent of your Java heap.

This is an experimental flag. This setting replaces the `-XX:DefaultMaxNewGenPercent` setting.

This setting isn’t available in Java HotSpot VM build 23 or earlier.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-364D09E1-D236-41A3-BE6A-6D291559372C"><!-- --></a><span class="bold">`-XX:G1MixedGCCountTarget=<span class="variable">number</span>`</span></dt>
<dd>

Sets the target number of mixed garbage collections after a marking cycle to collect old regions with at most `G1MixedGCLIveThresholdPercent` live data. The default is 8 mixed garbage collections. The goal for mixed collections is to be within this target number.

This setting isn’t available in Java HotSpot VM build 23 or earlier.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-BBD43EC1-BF18-49DB-B20E-53DEE08A904B"><!-- --></a><span class="bold">`-XX:G1MixedGCLiveThresholdPercent=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the occupancy threshold for an old region to be included in a mixed garbage collection cycle. The default occupancy is 85 percent.

This is an experimental flag. This setting replaces the `-XX:G1OldCSetRegionLiveThresholdPercent` setting.

This setting isn’t available in Java HotSpot VM build 23 or earlier.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-4CFBD26E-B890-471C-9299-5395355FE105"><!-- --></a><span class="bold">`-XX:G1NewSizePercent=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage of the heap to use as the minimum for the young generation size. The default value is 5 percent of your Java heap.

This is an experimental flag. This setting replaces the `-XX:DefaultMinNewGenPercent` setting.

This setting isn’t available in Java HotSpot VM build 23 or earlier.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3B41E8F6-36B3-4686-AD2B-DA877FD3EFF3"><!-- --></a><span class="bold">`-XX:G1OldCSetRegionThresholdPercent=<span class="variable">percent</span>`</span></dt>
<dd>

Sets an upper limit on the number of old regions to be collected during a mixed garbage collection cycle. The default is 10 percent of the Java heap.

This setting isn’t available in Java HotSpot VM build 23 or earlier.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-A1CE758F-6246-470D-93E5-59085756150D"><!-- --></a><span class="bold">`-XX:G1ReservePercent=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage of the heap (0 to 50) that’s reserved as a false ceiling to reduce the possibility of promotion failure for the G1 collector. When you increase or decrease the percentage, ensure that you adjust the total Java heap by the same amount. By default, this option is set to 10%.

The following example sets the reserved heap to 20%:

<pre dir="ltr">-XX:G1ReservePercent=20
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-2CAEFBDD-3B79-443C-9AC3-53DC051673A7"><!-- --></a><span class="bold">`-XX:InitialHeapOccupancyPercent=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the Java heap occupancy threshold that triggers a marking cycle. The default occupancy is 45 percent of the entire Java heap.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-77CCA623-9925-432D-8F74-1F8FD08C0D98"><!-- --></a><span class="bold">`-XX:InitialHeapSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the initial size (in bytes) of the memory allocation pool. This value must be either 0, or a multiple of 1024 and greater than 1 MB. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. The default value is selected at run time based on the system configuration.

The following examples show how to set the size of allocated memory to 6 MB using various units:

<pre dir="ltr">-XX:InitialHeapSize=6291456
-XX:InitialHeapSize=6144k
-XX:InitialHeapSize=6m
</pre>

If you set this option to 0, then the initial size is set as the sum of the sizes allocated for the old generation and the young generation. The size of the heap for the young generation can be set using the `-XX:NewSize` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-DE80CD5E-729C-46D5-AABB-D05D249AB804"><!-- --></a><span class="bold">`-XX:InitialSurvivorRatio=<span class="variable">ratio</span>`</span></dt>
<dd>

Sets the initial survivor space ratio used by the throughput garbage collector (which is enabled by the `-XX:+UseParallelGC` and/or -`XX:+UseParallelOldGC` options). Adaptive sizing is enabled by default with the throughput garbage collector by using the `-XX:+UseParallelGC` and `-XX:+UseParallelOldGC` options, and the survivor space is resized according to the application behavior, starting with the initial value. If adaptive sizing is disabled (using the `-XX:-UseAdaptiveSizePolicy` option), then the `-XX:SurvivorRatio` option should be used to set the size of the survivor space for the entire execution of the application.

The following formula can be used to calculate the initial size of survivor space (S) based on the size of the young generation (Y), and the initial survivor space ratio (R):

<pre dir="ltr">S=Y/(R+2)
</pre>

The 2 in the equation denotes two survivor spaces. The larger the value specified as the initial survivor space ratio, the smaller the initial survivor space size.

By default, the initial survivor space ratio is set to 8. If the default value for the young generation space size is used (2 MB), then the initial size of the survivor space is 0.2 MB.

The following example shows how to set the initial survivor space ratio to 4:

<pre dir="ltr">-XX:InitialSurvivorRatio=4
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-5D50EEFC-FF82-4444-8E0E-957B26B52E5D"><!-- --></a><span class="bold">`-XX:InitiatingHeapOccupancyPercent=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage of the heap occupancy (0 to 100) at which to start a concurrent GC cycle. It’s used by garbage collectors that trigger a concurrent GC cycle based on the occupancy of the entire heap, not just one of the generations (for example, the G1 garbage collector).

By default, the initiating value is set to 45%. A value of 0 implies nonstop GC cycles. The following example shows how to set the initiating heap occupancy to 75%:

<pre dir="ltr">-XX:InitiatingHeapOccupancyPercent=75
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-BC9C47F0-2C8F-4423-B298-A524D3BE28EF"><!-- --></a><span class="bold">`-XX:MaxGCPauseMillis=<span class="variable">time</span>`</span></dt>
<dd>

Sets a target for the maximum GC pause time (in milliseconds). This is a soft goal, and the JVM will make its best effort to achieve it. The specified value doesn’t adapt to your heap size. By default, there’s no maximum pause time value.

The following example shows how to set the maximum target pause time to 500 ms:

<pre dir="ltr">-XX:MaxGCPauseMillis=500
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-16B1303C-0604-40AE-B3AC-D5A1BC08C9B1"><!-- --></a><span class="bold">`-XX:MaxHeapSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the maximum size (in byes) of the memory allocation pool. This value must be a multiple of 1024 and greater than 2 MB. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. The default value is selected at run time based on the system configuration. For server deployments, the options `-XX:InitialHeapSize` and `-XX:MaxHeapSize` are often set to the same value.

The following examples show how to set the maximum allowed size of allocated memory to 80 MB using various units:

<pre dir="ltr">-XX:MaxHeapSize=83886080
-XX:MaxHeapSize=81920k
-XX:MaxHeapSize=80m
</pre>

On Oracle Solaris 7 and Oracle Solaris 8 SPARC platforms, the upper limit for this value is approximately 4,000 MB minus overhead amounts. On Oracle Solaris 2.6 and x86 platforms, the upper limit is approximately 2,000 MB minus overhead amounts. On Linux platforms, the upper limit is approximately 2,000 MB minus overhead amounts.

The `-XX:MaxHeapSize` option is equivalent to `-Xmx`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1188B661-AF65-425F-A583-62ABD7BC0B51"><!-- --></a><span class="bold">`-XX:MaxHeapFreeRatio=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the maximum allowed percentage of free heap space (0 to 100) after a GC event. If free heap space expands above this value, then the heap is shrunk. By default, this value is set to 70%.

Minimize the Java heap size by lowering the values of the parameters `MaxHeapFreeRatio` (default value is 70%) and `MinHeapFreeRatio` (default value is 40%) with the command-line options `-XX:MaxHeapFreeRatio` and `-XX:MinHeapFreeRatio`. Lowering `MaxHeapFreeRatio` to as low as 10% and `MinHeapFreeRatio` to 5% has successfully reduced the heap size without too much performance regression; however, results may vary greatly depending on your application. Try different values for these parameters until they’re as low as possible yet still retain acceptable performance.

<pre dir="ltr">-XX:MaxHeapFreeRatio=10 -XX:MinHeapFreeRatio=5 
</pre>

Customers trying to keep the heap small should also add the option `-XX:-ShrinkHeapInSteps`. See [Performance Tuning Examples](java.htm#GUID-EE6BD9FA-EF6D-4C3E-AC5C-30B8762CDC1B "You can use the Java advanced runtime options to optimize the performance of your applications.") for a description of using this option to keep the Java heap small by reducing the dynamic footprint for embedded applications.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-00745992-626E-483C-9895-381D8FA5A4DB"><!-- --></a><span class="bold">`-XX:MaxMetaspaceSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the maximum amount of native memory that can be allocated for class metadata. By default, the size isn’t limited. The amount of metadata for an application depends on the application itself, other running applications, and the amount of memory available on the system.

The following example shows how to set the maximum class metadata size to 256 MB:

<pre dir="ltr">-XX:MaxMetaspaceSize=256m
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-06251108-3903-4241-808F-F22328FA8B1E"><!-- --></a><span class="bold">`-XX:MaxNewSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the maximum size (in bytes) of the heap for the young generation (nursery). The default value is set ergonomically.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-7D91A1EF-EF14-4AB9-A2A5-383C85BC2D48"><!-- --></a><span class="bold">`-XX:MaxTenuringThreshold=<span class="variable">threshold</span>`</span></dt>
<dd>

Sets the maximum tenuring threshold for use in adaptive GC sizing. The largest value is 15. The default value is 15 for the parallel (throughput) collector, and 6 for the CMS collector.

The following example shows how to set the maximum tenuring threshold to 10:

<pre dir="ltr">-XX:MaxTenuringThreshold=10
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-06646B3F-3F78-4EEF-A12C-B5049C1D6C2C"><!-- --></a><span class="bold">`-XX:MetaspaceSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the size of the allocated class metadata space that triggers a garbage collection the first time it’s exceeded. This threshold for a garbage collection is increased or decreased depending on the amount of metadata used. The default size depends on the platform.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-AF59549A-80B0-4A65-9B51-9499366745E5"><!-- --></a><span class="bold">`-XX:MinHeapFreeRatio=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the minimum allowed percentage of free heap space (0 to 100) after a GC event. If free heap space falls below this value, then the heap is expanded. By default, this value is set to 40%.

Minimize Java heap size by lowering the values of the parameters `MaxHeapFreeRatio` (default value is 70%) and `MinHeapFreeRatio` (default value is 40%) with the command-line options `-XX:MaxHeapFreeRatio` and `-XX:MinHeapFreeRatio`. Lowering `MaxHeapFreeRatio` to as low as 10% and `MinHeapFreeRatio` to 5% has successfully reduced the heap size without too much performance regression; however, results may vary greatly depending on your application. Try different values for these parameters until they’re as low as possible, yet still retain acceptable performance.

<pre dir="ltr">-XX:MaxHeapFreeRatio=10 -XX:MinHeapFreeRatio=5 
</pre>

Customers trying to keep the heap small should also add the option `-XX:-ShrinkHeapInSteps`. See [Performance Tuning Examples](java.htm#GUID-EE6BD9FA-EF6D-4C3E-AC5C-30B8762CDC1B "You can use the Java advanced runtime options to optimize the performance of your applications.") for a description of using this option to keep the Java heap small by reducing the dynamic footprint for embedded applications.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-F9708DDE-19BD-4001-B7EF-559BDF24CBA2"><!-- --></a><span class="bold">`-XX:NewRatio=<span class="variable">ratio</span>`</span></dt>
<dd>

Sets the ratio between young and old generation sizes. By default, this option is set to 2. The following example shows how to set the young-to-old ratio to 1:

<pre dir="ltr">-XX:NewRatio=1
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-7CA92AEC-F9D1-4965-A0EE-6474FFAD8EC1"><!-- --></a><span class="bold">`-XX:NewSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the initial size (in bytes) of the heap for the young generation (nursery). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes.

The young generation region of the heap is used for new objects. GC is performed in this region more often than in other regions. If the size for the young generation is too low, then a large number of minor GCs are performed. If the size is too high, then only full GCs are performed, which can take a long time to complete. Oracle recommends that you keep the size for the young generation greater than 25% and less than 50% of the overall heap size.

The following examples show how to set the initial size of the young generation to 256 MB using various units:

<pre dir="ltr">-XX:NewSize=256m
-XX:NewSize=262144k
-XX:NewSize=268435456
</pre>

The `-XX:NewSize` option is equivalent to `-Xmn`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-A0EED983-31F3-4A6C-996D-C446F9B6E90B"><!-- --></a><span class="bold">`-XX:ParallelGCThreads=<span class="variable">threads</span>`</span></dt>
<dd>

Sets the value of the stop-the-world (STW) worker threads. This option sets the value of `<span class="variable">threads</span>` to the number of logical processors. The value of `<span class="variable">threads</span>` is the same as the number of logical processors up to a value of 8.

If there are more than 8 logical processors, then this option sets the value of `<span class="variable">threads</span>` to approximately 5/8 of the logical processors. This works in most cases except for larger SPARC systems where the value of `<span class="variable">threads</span>` can be approximately 5/16 of the logical processors.

The default value depends on the number of CPUs available to the JVM.

For example, to set the number of threads for parallel GC to 2, specify the following option:

<pre dir="ltr">-XX:ParallelGCThreads=2
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C7FC81DF-4472-4B29-98F1-937A7CA1B720"><!-- --></a><span class="bold">`-XX:+ParallelRefProcEnabled`</span></dt>
<dd>

Enables parallel reference processing. By default, this option is disabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-7B83E274-2B09-4F60-A1EC-524E7CD2BE9F"><!-- --></a><span class="bold">`-XX:+PrintAdaptiveSizePolicy`</span></dt>
<dd>

Enables printing of information about adaptive-generation sizing. By default, this option is disabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3F5BF92A-3F8B-4390-BA31-15BF6A7FEBFF"><!-- --></a><span class="bold">`-XX:+ScavengeBeforeFullGC`</span></dt>
<dd>

Enables GC of the young generation before each full GC. This option is enabled by default. Oracle recommends that you <span class="italic">don’t</span> disable it, because scavenging the young generation before a full GC can reduce the number of objects reachable from the old generation space into the young generation space. To disable GC of the young generation before each full GC, specify the option `-XX:-ScavengeBeforeFullGC`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E2E96805-AC1A-4BCD-91DC-D210750AC08E"><!-- --></a><span class="bold">`-XX:-ShrinkHeapInSteps`</span></dt>
<dd>

Incrementally reduces the Java heap to the target size, specified by the option `—XX:MaxHeapFreeRatio`. This option is enabled by default. If disabled, then it immediately reduces the Java heap to the target size instead of requiring multiple garbage collection cycles. Disable this option if you want to minimize the Java heap size. You will likely encounter performance degradation when this option is disabled.

See [Performance Tuning Examples](java.htm#GUID-EE6BD9FA-EF6D-4C3E-AC5C-30B8762CDC1B "You can use the Java advanced runtime options to optimize the performance of your applications.") for a description of using the `MaxHeapFreeRatio` option to keep the Java heap small by reducing the dynamic footprint for embedded applications.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-3A53F101-8D5D-451F-A726-C1BF5A8F7198"><!-- --></a><span class="bold">`–XX:StringDeduplicationAgeThreshold=<span class="variable">threshold</span>`</span></dt>
<dd>

Identifies `String` objects reaching the specified age that are considered candidates for deduplication. An object's age is a measure of how many times it has survived garbage collection. This is sometimes referred to as tenuring. See the `-XX:+PrintTenuringDistribution` option.

<div class="infobox-note" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-A5A66696-4BC6-4187-A261-274F57228288">

Note:

`String` objects that are promoted to an old heap region before this age has been reached are always considered candidates for deduplication. The default value for this option is `3`. See the `-XX:+UseStringDeduplication` option.

</div>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0B776EEF-4A67-46A7-AED3-397F8CE4A229"><!-- --></a><span class="bold">`-XX:SurvivorRatio=<span class="variable">ratio</span>`</span></dt>
<dd>

Sets the ratio between eden space size and survivor space size. By default, this option is set to 8. The following example shows how to set the eden/survivor space ratio to 4:

<pre dir="ltr">-XX:SurvivorRatio=4
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-4FB69536-35AA-4121-BEDA-E686AC962B88"><!-- --></a><span class="bold">`-XX:TargetSurvivorRatio=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the desired percentage of survivor space (0 to 100) used after young garbage collection. By default, this option is set to 50%.

The following example shows how to set the target survivor space ratio to 30%:

<pre dir="ltr">-XX:TargetSurvivorRatio=30
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-D69A24EE-5341-489E-8CE2-B5B0753AD828"><!-- --></a><span class="bold">`-XX:TLABSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the initial size (in bytes) of a thread-local allocation buffer (TLAB). Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. If this option is set to 0, then the JVM selects the initial size automatically.

The following example shows how to set the initial TLAB size to 512 KB:

<pre dir="ltr">-XX:TLABSize=512k
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-8F90F247-0DD1-4DE4-B252-E941A6EB375C"><!-- --></a><span class="bold">`-XX:+UseAdaptiveSizePolicy`</span></dt>
<dd>

Enables the use of adaptive generation sizing. This option is enabled by default. To disable adaptive generation sizing, specify `-XX:-UseAdaptiveSizePolicy` and set the size of the memory allocation pool explicitly. See the `-XX:SurvivorRatio` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-1A0FD6E8-0AC1-4E8A-B1F0-6BC88E503458"><!-- --></a><span class="bold">`-XX:+UseCMSInitiatingOccupancyOnly`</span></dt>
<dd>

Enables the use of the occupancy value as the only criterion for initiating the CMS collector. By default, this option is disabled and other criteria may be used.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-146724E9-C9AF-4F88-8F7A-BA6419F20CAB"><!-- --></a><span class="bold">`-XX:+UseG1GC`</span></dt>
<dd>

Enables the use of the garbage-first (G1) garbage collector. It’s a server-style garbage collector, targeted for multiprocessor machines with a large amount of RAM. This option meets GC pause time goals with high probability, while maintaining good throughput. The G1 collector is recommended for applications requiring large heaps (sizes of around 6 GB or larger) with limited GC latency requirements (a stable and predictable pause time below 0.5 seconds). By default, this option is enabled and G1 is used as the default garbage collector.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-729376EB-FC4A-4BAA-A743-9A0BA7E3619D"><!-- --></a><span class="bold">`-XX:+UseGCOverheadLimit`</span></dt>
<dd>

Enables the use of a policy that limits the proportion of time spent by the JVM on GC before an `OutOfMemoryError` exception is thrown. This option is enabled, by default, and the parallel GC will throw an `OutOfMemoryError` if more than 98% of the total time is spent on garbage collection and less than 2% of the heap is recovered. When the heap is small, this feature can be used to prevent applications from running for long periods of time with little or no progress. To disable this option, specify the option `-XX:-UseGCOverheadLimit`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-0CF18151-BF1E-46AE-B63E-755EB573D155"><!-- --></a><span class="bold">`-XX:+UseNUMA`</span></dt>
<dd>

Enables performance optimization of an application on a machine with nonuniform memory architecture (NUMA) by increasing the application's use of lower latency memory. By default, this option is disabled and no optimization for NUMA is made. The option is available only when the parallel garbage collector is used (`-XX:+UseParallelGC`).

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-DD9FE0C9-E505-45D7-B4F3-D6E0A08430D7"><!-- --></a><span class="bold">`-XX:+UseParallelGC`</span></dt>
<dd>

Enables the use of the parallel scavenge garbage collector (also known as the throughput collector) to improve the performance of your application by leveraging multiple processors.

By default, this option is disabled and the collector is chosen automatically based on the configuration of the machine and type of the JVM. If it’s enabled, then the `-XX:+UseParallelOldGC` option is automatically enabled, unless you explicitly disable it.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-69880687-4B41-490B-A3B8-4832C9441D7A"><!-- --></a><span class="bold">`-XX:+UseParallelOldGC`</span></dt>
<dd>

Enables the use of the parallel garbage collector for full GCs. By default, this option is disabled. Enabling it automatically enables the `-XX:+UseParallelGC` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-29F4121C-BD9F-47B5-B043-C801F79102A5"><!-- --></a><span class="bold">`-XX:+UseSerialGC`</span></dt>
<dd>

Enables the use of the serial garbage collector. This is generally the best choice for small and simple applications that don’t require any special functionality from garbage collection. By default, this option is disabled and the collector is selected automatically based on the configuration of the machine and type of the JVM.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-E867E13D-00EF-403F-A808-72CEE66F1AF7"><!-- --></a><span class="bold">`-XX:+UseSHM`</span></dt>
<dd>

<span class="bold">Linux only:</span> Enables the JVM to use shared memory to set up large pages.

See [Large Pages](java.htm#GUID-7BE7CD55-3AC3-4A96-BBDD-E4D9FC4FCCCB "You use large pages, also known as huge pages, as memory pages that are significantly larger than the standard memory page size (which varies depending on the processor and operating system). Large pages optimize processor Translation-Lookaside Buffers.") for setting up large pages.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-C6B373BC-6312-45EC-A3CB-2AA8164BEF2F"><!-- --></a><span class="bold">`-XX:+UseStringDeduplication`</span></dt>
<dd>

Enables string deduplication. By default, this option is disabled. To use this option, you must enable the garbage-first (G1) garbage collector.

String deduplication reduces the memory footprint of `String` objects on the Java heap by taking advantage of the fact that many `String` objects are identical. Instead of each `String` object pointing to its own character array, identical `String` objects can point to and share the same character array.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-DEE9762E-3E49-4705-A0C5-8251A6CF6135"><!-- --></a><span class="bold">`-XX:+UseTLAB`</span></dt>
<dd>

Enables the use of thread-local allocation blocks (TLABs) in the young generation space. This option is enabled by default. To disable the use of TLABs, specify the option `-XX:-UseTLAB`.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__OBSOLETEJAVAOPTIONS-A4E7030A">

Obsolete Java Options

These `java` options are still accepted but ignored, and a warning is issued when they’re used.

<dl class="0.99* 3.01*">
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-66DDEAD6-E436-4000-B784-14DDECFC538F"><!-- --></a><span class="bold">`-Xusealtsigs / -XX:+UseAltSigs`</span></dt>
<dd>

<span class="bold">Oracle Solaris only:</span> Use alternative signals instead of SIGUSR1 and SIGUSR2 for JVM internal signals. Since Solaris 10, two dedicated signals have been made available to the VM and so, since JDK 6, these flags have been documented as having no effect. The flags have now been made obsolete, and their use generates a warning. In a future release these flags will be removed completely.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__DEPRECATEDJAVAOPTIONS-A4E6FB83">

Deprecated Java Options

These `java` options are <span>deprecated and might be removed in a future JDK release.</span> They’re still accepted and acted upon, but a warning is issued when they’re used.

<dl class="0.99* 3.01*">
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__D32JDK-8168826DOCUMENTDEPRECATIONOF-A4E3081A"><!-- --></a><span class="bold">`-d32`</span></dt>
<dd>

<span class="bold">Oracle Solaris, Linux, and OS X:</span> Runs the application in a 32-bit environment. If a 32-bit environment isn’t installed or isn’t supported, then an error is reported. By default, the application is run in a 32-bit environment unless a 64-bit system is used.

<div class="infobox-note" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__THE-D32AND-D64OPTIONSWEREADDEDTOALL-A4E30839">

Note:

The `-d32` and `-d64` options were added to allow multiple architectures (data model) JDKs and JREs to coexist on the same system. The user could invoke the other data model by using these launcher options. Oracle Solaris was the only platform supporting these options, and the 32-bit JDKs/JREs are no longer supported. Therefore, these options are obsolete and will be removed in a future release.

</div>
</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__D64DEPRECATEDORACLESOLARISLINUXANDO-A4E30862"><!-- --></a><span class="bold">`-d64`</span></dt>
<dd>

<span class="bold">Oracle Solaris, Linux, and OS X:</span> Runs the application in a 64-bit environment. If a 64-bit environment isn’t installed or isn’t supported, then an error is reported. By default, the application is run in a 32-bit environment unless a 64-bit system is used.

Only the Java HotSpot Server VM supports 64-bit operation and the `-server` option is implicit with the use of `-d64`. The `-client` option is ignored with the use of `-d64`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XLOGGCGARBAGE-COLLECTION.LOGINJDK9X-A4E3089D"><!-- --></a><span class="bold">`-Xloggc:garbage-collection.log`</span></dt>
<dd>

Sets the file to which verbose GC events information should be redirected for logging. The information written to this file is similar to the output of `-verbose:gc` with the time elapsed since the first GC event preceding each logged event. The `-Xloggc` option overrides `-verbose:gc` if both are given with the same java command.

Example:

<pre dir="ltr">-Xlog:gc:garbage-collection.log
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXCMSINITIATINGOCCUPANCYFRACTIONPER-A4E307F3"><!-- --></a><span class="bold">`-XX:CMSInitiatingOccupancyFraction=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage of the old generation occupancy (0 to 100) at which to start a CMS collection cycle. The default value is set to -1. Any negative value (including the default) implies that the option `-XX:CMSTriggerRatio` is used to define the value of the initiating occupancy fraction.

The following example shows how to set the occupancy fraction to 20%:

<pre dir="ltr">-XX:CMSInitiatingOccupancyFraction=20
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXCMSINITIATINGPERMOCCUPANCYFRACTIO-A4E30802"><!-- --></a><span class="bold">`-XX:CMSInitiatingPermOccupancyFraction=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage of the permanent generation occupancy (0 to 100) at which to start a GC. This option was deprecated in JDK 8 with no replacement.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXG1PRINTHEAPREGIONSINJDK9THISISADE-A4E307EB"><!-- --></a><span class="bold">`-XX:+G1PrintHeapRegions`</span></dt>
<dd>

Enables the printing of information about which regions are allocated and which are reclaimed by the G1 collector. By default, this option is disabled. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXMAXPERMSIZESIZEDEPRECATEDSETSTHEM-A4E307DD"><!-- --></a><span class="bold">`-XX:MaxPermSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the maximum permanent generation space size (in bytes). This option was deprecated in JDK 8 and superseded by the `-XX:MaxMetaspaceSize` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPERMSIZESIZEDEPRECATEDSETSTHESPAC-A4E3074F"><!-- --></a><span class="bold">`-XX:PermSize=<span class="variable">size</span>`</span></dt>
<dd>

Sets the space (in bytes) allocated to the permanent generation that triggers a garbage collection if it’s exceeded. This option was deprecated in JDK 8 and superseded by the `-XX:MetaspaceSize` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPRINTGCINJDK9THISISADEPRECATEDOPT-A4E307D5"><!-- --></a><span class="bold">`-XX:+PrintGC`</span></dt>
<dd>

Enables printing of messages at every GC. By default, this option is disabled. If you’re using this flag, then see [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework."). In JDK 9, this option is deprecated.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPRINTGCAPPLICATIONCONCURRENTTIMEI-A4E308BF"><!-- --></a><span class="bold">`-XX:+PrintGCApplicationConcurrentTime`</span></dt>
<dd>

Enables printing of how much time elapsed since the last pause (for example, a GC pause). By default, this option is deprecated.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPRINTGCAPPLICATIONSTOPPEDTIMEINJD-A4E308B8"><!-- --></a><span class="bold">`-XX:+PrintGCApplicationStoppedTime`</span></dt>
<dd>

Enables printing of how much time the pause (for example, a GC pause) lasted. By default, this option is deprecated

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPRINTGCDATESTAMPSINJDK9THISISADEP-A4E307C1"><!-- --></a><span class="bold">`-XX:+PrintGCDateStamps`</span></dt>
<dd>

Enables printing of a date stamp at every GC. By default, this option is deprecated.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPRINTGCDETAILSINJDK9THISISADEPREC-A4E307B9"><!-- --></a><span class="bold">`-XX:+PrintGCDetails`</span></dt>
<dd>

Enables printing of detailed messages at every GC. By default, this option is disabled. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPRINTGCTASKTIMESTAMPSINJDK9THISIS-A4E30857"><!-- --></a><span class="bold">`-XX:+PrintGCTaskTimeStamps`</span></dt>
<dd>

Enables printing of time stamps for every individual GC worker thread task. By default, this option is disabled. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPRINTGCTIMESTAMPSINJDK9THISISADEP-A4E308B1"><!-- --></a><span class="bold">`-XX:+PrintGCTimeStamps`</span></dt>
<dd>

Enables printing of time stamps at every GC. By default, this option is disabled. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPRINTSTRINGDEDUPLICATIONSTATISTIC-A4E307C7"><!-- --></a><span class="bold">`-XX:+PrintStringDeduplicationStatistics`</span></dt>
<dd>

Prints detailed deduplication statistics. By default, this option is disabled. See the `-XX:+UseStringDeduplication` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXPRINTTENURINGDISTRIBUTIONINJDK9TH-A4E307FB"><!-- --></a><span class="bold">`-XX:+PrintTenuringDistribution`</span></dt>
<dd>

Enables printing of tenuring age information. The following is an example of the output:

<pre dir="ltr">Desired survivor size 48286924 bytes, new threshold 10 (max 10)
- age 1: 28992024 bytes, 28992024 total
- age 2: 1366864 bytes, 30358888 total
- age 3: 1425912 bytes, 31784800 total
...
</pre>

Age 1 objects are the youngest survivors (they were created after the previous scavenge, survived the latest scavenge, and moved from eden to survivor space). Age 2 objects have survived two scavenges (during the second scavenge they were copied from one survivor space to the next). This pattern is repeated for all objects in the output.

In the preceding example, 28,992,024bytes survived one scavenge and were copied from eden to survivor space, 1,366,864 bytes are occupied by age 2 objects, and so on. The third value in each row is the cumulative size of objects of age <span class="variable">n</span> or less.

By default, this option is disabled.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXSOFTREFLRUPOLICYMSPERMBTIMEADDEDT-A4E30812"><!-- --></a><span class="bold">`-XX:SoftRefLRUPolicyMSPerMB=<span class="variable">time</span>`</span></dt>
<dd>

Sets the amount of time (in milliseconds) a softly reachable object is kept active on the heap after the last time it was referenced. The default value is one second of lifetime per free megabyte in the heap. The `-XX:SoftRefLRUPolicyMSPerMB` option accepts integer values representing milliseconds per one megabyte of the current heap size (for Java HotSpot Client VM) or the maximum possible heap size (for Java HotSpot Server VM). This difference means that the Client VM tends to flush soft references rather than grow the heap, whereas the Server VM tends to grow the heap rather than flush soft references. In the latter case, the value of the `-Xmx` option has a significant effect on how quickly soft references are garbage collected.

The following example shows how to set the value to 2.5 seconds:

<pre dir="ltr">-XX:SoftRefLRUPolicyMSPerMB=2500
</pre></dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-85A9CF82-09DB-4AC8-81EC-2404A8DC8660"><!-- --></a><span class="bold">`-XX:+TraceClassLoading`</span></dt>
<dd>

Enables tracing of classes as they are loaded. By default, this option is disabled and classes aren’t traced.

The replacement Unified Logging syntax is `-Xlog:class+load=<span class="variable">level</span>`. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.")

Use `<span class="variable">level</span>`=`info` for regular information, or `<span class="variable">level</span>`=`debug` for additional information. In Unified Logging syntax, `-verbose:class` equals `-Xlog:class+load=info,class+unload=info`..

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-FF7B6056-45D5-46CB-B45C-20A7F12FDA68"><!-- --></a><span class="bold">`-XX:+TraceClassLoadingPreorder`</span></dt>
<dd>

Enables tracing of all loaded classes in the order in which they’re referenced. By default, this option is disabled and classes aren’t traced.

The replacement Unified Logging syntax is `-Xlog:class+preorder=debug`. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-17D5AEFA-D964-4A0F-AB08-F718B74AE53F"><!-- --></a><span class="bold">`-XX:+TraceClassResolution`</span></dt>
<dd>

Enables tracing of constant pool resolutions. By default, this option is disabled and constant pool resolutions aren’t traced.

The replacement Unified Logging syntax is `-Xlog:class+resolve=debug`. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-21107EDD-4B6A-402E-8BDD-0C47142E9192"><!-- --></a><span class="bold">`-XX:+TraceClassUnloading`</span></dt>
<dd>

Enables tracing of classes as they’re unloaded. By default, this option is disabled and classes aren’t traced.

The replacement Unified Logging syntax is `-Xlog:class+unload=<span class="variable">level</span>`. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.").

Use `<span class="variable">level</span>`=`info` for regular information, and `<span class="variable">level</span>`=`trace` for additional information. In Unified Logging syntax, `-verbose:class` equals `-Xlog:class+unload=info,class+unload=info` .

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-DB8FC218-1200-43D5-962F-75DAEA2E0D7B"><!-- --></a><span class="bold">`-XX:+TraceLoaderConstraints`</span></dt>
<dd>

Enables tracing of the loader constraints recording. By default, this option is disabled and loader constraints recording isn’t traced.

The replacement Unified Logging syntax is `-Xlog:class+loader+constraints=info`. See [Enable Logging with the JVM Unified Logging Framework](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5 "You use the -Xlog option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.").

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__GUID-88A2F5D3-F598-469A-BC57-5B7D040FAFA4"><!-- --></a><span class="bold">`-XX:+UseConcMarkSweepGC`</span></dt>
<dd>

Enables the use of the CMS garbage collector for the old generation. CMS is an alternative to the default garbage collector (G1), which also focuses on meeting application latency requirements. By default, this option is disabled and the collector is selected automatically based on the configuration of the machine and type of the JVM. In JDK 9, the CMS garbage collector is deprecated.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXUSEPARNEWGCADDED-XXUSEPARNEWGCASA-A4E308A3"><!-- --></a><span class="bold">`-XX:+UseParNewGC`</span></dt>
<dd>

Enables the use of parallel threads for collection in the young generation. By default, this option is disabled. It’s automatically enabled when you set the `-XX:+UseConcMarkSweepGC` option. Using the `-XX:+UseParNewGC` option without the `-XX:+UseConcMarkSweepGC` option was deprecated in JDK 8. Starting with JDK 9, all uses of the `-XX:+UseParNewGC` option are deprecated. Using the option without `-XX:+UseConcMarkSweepGC` isn’t possible.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXUSESPLITVERIFIERDEPRECATEDENABLES-A4E30755"><!-- --></a><span class="bold">`-XX:+UseSplitVerifier`</span></dt>
<dd>

Enables splitting the verification process. By default, this option was enabled in the previous releases, and verification was split into two phases: type referencing (performed by the compiler) and type checking (performed by the JVM runtime). Verification is now split by default without a way to disable it.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section" id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__REMOVEDJAVAOPTIONS-A4E6F213">

Removed Java Options

<div class="p">These `java` options were removed in JDK 9 and using them results in an error of:
<pre dir="ltr">Unrecognized VM option <span class="variable">option-name</span>
</pre></div>
<dl class="0.99* 3.01*">
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XINCGCINJDK9THE-XINCGCOPTIONNOTAVAI-A4E386AB"><!-- --></a><span class="bold">`-Xincgc`</span></dt>
<dd>

Enabled incremental garbage collection. This option and the GC mode are removed in JDK 9.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XMAXJITCODESIZESIZEREMOVEDJDK-80421-A4E386C0"><!-- --></a><span class="bold">`-Xmaxjitcodesize=<span class="variable">size</span>`</span></dt>
<dd>

Specified the maximum code cache size (in bytes) for JIT-compiled code. Appended the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. The default maximum code cache size is 240 MB; if you disable tiered compilation with the option `-XX:-TieredCompilation`, then the default size is 48 MB:

<pre dir="ltr">-Xmaxjitcodesize=240m
</pre>

This option is equivalent to `-XX:ReservedCodeCacheSize`.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XRUNLIBNAMEREMOVEDLOADSTHESPECIFIED-A4E3872D"><!-- --></a><span class="bold">`-Xrun<span class="variable">libname</span>`</span></dt>
<dd>

Loaded the specified debugging/profiling library. This option was superseded by the `-agentlib` option.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXCMSINCREMENTALDUTYCYCLEPERCENTINJ-A4E385C1"><!-- --></a><span class="bold">`-XX:CMSIncrementalDutyCycle=<span class="variable">percent</span>`</span></dt>
<dd>

Set the percentage of time (0 to 100) between minor collections that the concurrent collector was allowed to run.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXCMSINCREMENTALDUTYCYCLEMINPERCENT-A4E3861C"><!-- --></a><span class="bold">`-XX:CMSIncrementalDutyCycleMin=<span class="variable">percent</span>`</span></dt>
<dd>

Sets the percentage of time (0 to 100) between minor collections that was the lower bound for the duty cycle when `-XX:+CMSIncrementalPacing` option was enabled. This option was deprecated in JDK 8 with no replacement, following the deprecation of the `-XX:+CMSIncrementalMode` option. The option was removed in JDK 9, because the entire incremental mode was removed.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXCMSINCREMENTALMODEINJDK9THISISADE-A4E385CC"><!-- --></a><span class="bold">`-XX:+CMSIncrementalMode`</span></dt>
<dd>

Enabled incremental mode. Note that the CMS collector must also be enabled (with `-XX:+UseConcMarkSweepGC)` for this option to work. The option was removed in JDK 9, because the entire incremental mode was removed.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXCMSINCREMENTALOFFSETPERCENTINPUTS-A4E385D6"><!-- --></a><span class="bold">`-XX:CMSIncrementalOffset=<span class="variable">percent</span>`</span></dt>
<dd>

Set the percentage of time (0 to 100) by which the incremental mode duty cycle was shifted to the right within the period between minor collections.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXCMSINCREMENTALPACINGINPUTSFROMSTE-A4E385E0"><!-- --></a><span class="bold">`-XX:+CMSIncrementalPacing`</span></dt>
<dd>

Enabled automatic adjustment of the incremental mode duty cycle based on statistics collected while the JVM was running. This option was deprecated with no replacement, following the deprecation of the `-XX:+CMSIncrementalMode` option. The option was removed, because the entire incremental mode was removed.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXCMSINCREMENTALSAFETYFACTORPERCENT-A4E385EA"><!-- --></a><span class="bold">`-XX:CMSIncrementalSafetyFactor=<span class="variable">percent</span>`</span></dt>
<dd>

Set the percentage of time (0 to 100) used to add conservatism when computing the duty cycle. This option was deprecated in JDK 8 with no replacement, following the deprecation of the `-XX:+CMSIncrementalMode` option. The option was removed, because the entire incremental mode was removed.

</dd>
<dt class="dlterm"><a id="GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__XXCODECACHEMINIMUMFREESPACESIZEJDK--A4E3866D"><!-- --></a><span class="bold">`-XX:CodeCacheMinimumFreeSpace=<span class="variable">size</span>`</span></dt>
<dd>

Set the minimum free space (in bytes) required for compilation. Appended the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, or `g` or `G` to indicate gigabytes. When less than the minimum free space remained, compiling stopped. By default, this option was set to 500 KB.

</dd>
</dl>
</div>
<!-- class="section" --></div>
<div class="sect2"><a id="GUID-4856361B-8BFD-4964-AE84-121F5F6CF111"></a>

## java Command-Line Argument Files

<div>

You can shorten or simplify the `java` command by using `@<span class="variable">argument files</span>` to specify a text file that contains arguments, such as options and class names, passed to the `java` command. This let’s you to create `java` commands of any length on any operating system.

<div class="section"></div>
<!-- class="section" -->
<div class="section">

In the command line, use the at sign (`@`) prefix to identify an argument file that contains `java` options and class names. When the `java` command encounters a file beginning with the at sign (`@`) , it expands the contents of that file into an argument list just as they would be specified on the command line.

The `java` launcher expands the argument file contents until it encounters the `-Xdisable-@files` option. You can use the `-Xdisable-@files` option anywhere on the command line, including in an argument file, to stop `@{argument files}` expansion.

The following items describe the syntax of `java` argument files:

*   The argument file must contain only ASCII characters or characters in system default encoding that’s ASCII friendly, such as UTF-8.

*   The argument file size must not exceed MAXINT (2,147,483,647) bytes.

*   The launcher doesn’t expand wildcards that are present within an argument file.

*   Use white space or new line characters to separate arguments included in the file.

*   White space includes a white space character, `\t`, `\n`, `\r`, and `\f`.

    For example, it is possible to have a path with a space, such as `c:\Program Files` that can be specified as either `"c:\\Program Files"` or, to avoid an escape, `c:\Program" "Files`.

*   Any option that contains spaces, such as a path component, must be within quotation marks using quotation ('"') characters in its entirety.

*   A string within quotation marks may contain the characters `\n`, `\r`, `\t`, and `\f`. They are converted to their respective ASCII codes. &nbsp;

*   If a file name contains embedded spaces, then put the whole file name in double quotation marks.

*   File names in an argument file are relative to the current directory, not to the location of the argument file.

*   Use the number sign `#` in the argument file to identify comments. All characters following the`#` are ignored until the end of line.

*   Additional at sign `@` prefixes to `@` prefixed options act as an escape, (the first `@` is removed and the rest of the arguments are presented to the launcher literally).

*   Lines may be continued using the continuation character (`\`) at the end-of-line. The two lines are concatenated with the leading white spaces trimmed. To prevent trimming the &nbsp;leading white spaces, a continuation character (`\`) may be placed at the first column.

*   Because backslash (\) is an escape character, a backslash character&nbsp;must be escaped with another backslash character.

*   Partial quote is allowed and is closed by an end-of-file.

*   An open quote stops at end-of-line unless `\` is the last character, which then joins the next line by removing all leading white space characters.

*   Wildcards (*) aren’t allowed in these lists (such as specifying `*.java`).

*   Use of the at sign (`@`) to recursively interpret files isn’t supported.
</div>
<!-- class="section" -->
<div class="section">

Example of Open or Partial Quotes in an Argument File

In the argument file,

<pre dir="ltr">-cp "lib/
cool/
app/
jars
</pre>

this is interpreted as:

<pre dir="ltr">-cp lib/cool/app/jars  
</pre></div>
<!-- class="section" -->
<div class="section">

Example of a Backslash Character&nbsp;Escaped with Another Backslash Character in an Argument File

To output the following:

`-cp c:\Program Files (x86)\Java\jre\lib\ext;c:\Program Files\Java\jre9\lib\ext`

The backslash character must be specified in the argument file as:

`-cp &nbsp;"c:\\Program Files (x86)\\Java\\jre\\lib\\ext;c:\\Program Files\\Java\\jre9\\lib\\ext"`

</div>
<!-- class="section" -->
<div class="section">

Example of an EOL Escape Used to Force Concatenation of Lines in an Argument File

In the argument file,

<pre dir="ltr">-cp "/lib/cool app/jars:\
    /lib/another app/jars"
</pre>

This is interpreted as:

<pre dir="ltr">-cp /lib/cool app/jars:/lib/another app/jars  
</pre></div>
<!-- class="section" -->
<div class="section">

Example of Line Continuation with Leading Spaces in an Argument File

In the argument file,

<pre dir="ltr">-cp "/lib/cool\ 
\app/jars” 
</pre>

This is interpreted as:

`-cp /lib/cool app/jars` &nbsp;

</div>
<!-- class="section" -->
<div class="section">

Examples of Using Single Argument File

You can use a single argument file, such as `myargumentfile` in the following example, to hold all required `java` arguments:

<pre dir="ltr">java @myargumentfile
</pre></div>
<!-- class="section" -->
<div class="section">

Examples of Using Argument Files with Paths

You can include relative paths in argument files; however, they’re relative to the current working directory and not to the paths of the argument files themselves. In the following example, `path1/options` and `path2/options` represent argument files with different paths. Any relative paths that they contain are relative to the current working directory and not to the argument files:

<pre dir="ltr">java @path1/options @path2/classes
</pre></div>
<!-- class="section" --></div>
</div>
<div class="sect2"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5"></a>

## Enable Logging with the JVM Unified Logging Framework

<div>

You use the `-Xlog` option to configure or enable logging with the Java Virtual Machine (JVM) unified logging framework.

<div class="section">

Synopsis

<pre dir="ltr">-Xlog[:[<span class="variable">what</span>][:[<span class="variable">output</span>][:[<span class="variable">decorators</span>][:<span class="variable">output-options</span> [,...]]]]]
</pre>
<dl>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__XLOGHELPPRINTS-XLOGUSAGESYNTAXANDAV-A7A3FE7C"><!-- --></a>`<span class="variable">what</span>`</dt>
<dd>

Specifies a combination of tags and levels of the form `<span class="variable">tag1</span>`[+`<span class="variable">tag2</span>`...][`*`][=`<span class="variable">level</span>`][,...]. Unless the wildcard (`*`) is specified, only log messages tagged with exactly the tags specified are matched. See [-Xlog Tags and Levels](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__TAGSANDLEVELS-A7A4A0DF).

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-ED271C85-4EA8-4A7C-ABF1-DE4618B44B1A"><!-- --></a>`<span class="variable">output</span>`</dt>
<dd>

Sets the type of output. Omitting the `<span class="variable">output</span>` type defaults to `stdout`. See [-Xlog Output](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__OUTPUTS-A7A49D8E).

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-3AE387D4-BBE2-4500-9A05-872E583A2C0A"><!-- --></a>`<span class="variable">decorators</span>`</dt>
<dd>

Configures the output to use a custom set of decorators. Omitting `<span class="variable">decorators</span>` defaults to `uptime`, `level`, and `tags`. See [Decorations](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__DECORATIONS-A7A4AD31).

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-062AD496-C390-43A7-B5F9-C241F52B9C33"><!-- --></a>`<span class="variable">output-options</span>`</dt>
<dd>

Sets the `-Xlog` logging output options.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section">

Description

The Java Virtual Machine (JVM) unified logging framework provides a common logging system for all components of the JVM. GC logging for the JVM has been changed to use the new logging framework. The mapping of old GC flags to the corresponding new Xlog configuration is described in [Convert GC Logging Flags to Xlog](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__CONVERTGCLOGGINGFLAGSTOXLOG-A5046BD1). In addition, runtime logging has also been changed to use the JVM unified logging framework. The mapping of legacy runtime logging flags to the corresponding new Xlog configuration is described in [Convert Runtime Logging Flags to Xlog](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__CONVERTRUNTIMELOGGINGFLAGSTOXLOG-A504703E).

The following provides quick reference to the `-Xlog` command and syntax for options:

<dl>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-EFAA771C-7ACA-4BC0-9D03-21CBD578386E"><!-- --></a>`-Xlog`</dt>
<dd>

Enables JVM logging on an `info` level.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-7749EEC4-8FEE-4D91-B296-D058D1F78E6D"><!-- --></a>`-Xlog:help`</dt>
<dd>

Prints `-Xlog` usage syntax and available tags, levels, and decorators along with example command lines with explanations.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-A4AC2771-EB79-423F-93C2-546CF5277C8E"><!-- --></a>`-Xlog:disable`</dt>
<dd>

Turns off all logging and clears all configuration of the logging framework including the default configuration for warnings and errors.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-61EE25F9-F19C-4531-9DBE-C343F270643A"><!-- --></a>`-Xlog[:<span class="variable">option</span>]`</dt>
<dd>

Applies multiple arguments in the order that they appear on the command line. Multiple `-Xlog` arguments for the same output override each other in their given order.

The `<span class="variable">option</span>` is set as:
<pre dir="ltr">[<span class="variable">tag selection</span>][:[<span class="variable">output</span>][:[<span class="variable">decorators</span>][:<span class="variable">output-options</span>]]]
</pre>

Omitting the `<span class="variable">tag selection</span>` defaults to a tag-set of `all` and a level of `info`.

<pre dir="ltr"><span class="variable">tag</span>[+...] all
</pre>

The `all` tag is a meta tag consisting of all tag-sets available. The asterisk `*` in a tag set definition denotes a wildcard tag match. Matching with a wildcard selects all tag sets that contain <span class="italic">at least</span> the specified tags. Without the wildcard, only exact matches of the specified tag sets are selected.

`<span class="variable">output_options</span>` is
<pre dir="ltr">filecount=<span class="variable">file count</span> filesize=<span class="variable">file size with optional K, M or G suffix</span>
</pre></dd>
</dl>
</div>
<!-- class="section" -->
<div class="section">

Default Configuration

When the `-Xlog`option and nothing else is specified on the command line, the default configuration is used. The default configuration logs all messages with a level that matches either the warning or error regardless of what tags the message is associated with. The default configuration is equivalent to entering the following on the command line:

<pre dir="ltr">-Xlog:all=warning:stdout:uptime,level,tags
</pre></div>
<!-- class="section" -->
<div class="section">

Controlling Logging at Runtime

Logging can also be controlled at run time through Diagnostic Commands (with the `jcmd` utility). Everything that can be specified on the command line can also be specified dynamically with the `VM.log` command. As the diagnostic commands are automatically exposed as MBeans, you can use JMX to change logging configuration at run time.

</div>
<!-- class="section" -->
<div class="section" id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__TAGSANDLEVELS-A7A4A0DF">

-Xlog Tags and Levels

Each log message has a level and a tag set associated with it. The level of the message corresponds to its details, and the tag set corresponds to what the message contains or which JVM component it involves (such as, GC, compiler, or threads). Mapping GC flags to the Xlog configuration is described in [Convert GC Logging Flags to Xlog](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__CONVERTGCLOGGINGFLAGSTOXLOG-A5046BD1). Mapping legacy runtime logging flags to the corresponding Xlog configuration is described in [Convert Runtime Logging Flags to Xlog](java.htm#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__CONVERTRUNTIMELOGGINGFLAGSTOXLOG-A504703E).

<dl>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-1D43F04B-7CCF-421B-9FB6-A0B08F489EC1"><!-- --></a>Available log levels:</dt>
<dd>

*   `off`

*   `trace`

*   `debug`

*   `info`

*   `warning`

*   `error`
</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-05C7ACDF-518C-4F00-AEBD-F8854A3E3ADB"><!-- --></a>Available log tags:</dt>
<dd>

The following are the available log tags. Specifying `all` instead of a tag combination matches all tag combinations.

*   `add`

*   `age`

*   `alloc`

*   `annotation`

*   `aot`

*   `arguments`

*   `attach`

*   `barrier`

*   `biasedlocking`

*   `blocks`

*   `bot`

*   `breakpoint`

*   `bytecode`

*   `census`

*   `class`

*   `classhisto`

*   `cleanup`

*   `compaction`

*   `comparator`

*   `constraints`

*   `constantpool`

*   `coops`

*   `cpu`

*   `cset`

*   `data`

*   `defaultmethods`

*   `dump`

*   `ergo`

*   `event`

*   `exceptions`

*   `exit`

*   `fingerprint`

*   `freelist`

*   `gc`

*   `hashtables`

*   `heap`

*   `humongous`

*   `ihop`

*   `iklass`

*   `init`

*   `itables`

*   `jfr`

*   `jni`

*   `jvmti`

*   `liveness`

*   `load`

*   `loader`

*   `logging`

*   `mark`

*   `marking`

*   `metadata`

*   `metaspace`

*   `method`

*   `mmu`

*   `modules`

*   `monitorinflation`

*   `monitormismatch`

*   `nmethod`

*   `normalize`

*   `objecttagging`

*   `obsolete`

*   `oopmap`

*   `os`

*   `pagesize`

*   `parser`

*   `patch`

*   `path`

*   `phases`

*   `plab`

*   `preorder`

*   `promotion`

*   `protectiondomain`

*   `purge`

*   `redefine`

*   `ref`

*   `refine`

*   `region`

*   `remset`

*   `resolve`

*   `safepoint`

*   `scavenge`

*   `scrub`

*   `setting`

*   `stackmap`

*   `stacktrace`

*   `stackwalk`

*   `start`

*   `startuptime`

*   `state`

*   `stats`

*   `stringdedup`

*   `stringtable`

*   `subclass`

*   `survivor`

*   `sweep`

*   `system`

*   `task`

*   `thread`

*   `time`

*   `timer`

*   `tlab`

*   `unload`

*   `update`

*   `verification`

*   `verify`

*   `vmoperation`

*   `vtables`

*   `workgang`
</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section" id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__OUTPUTS-A7A49D8E">

-Xlog Output

The `-Xlog` option supports the following types of outputs:

*   `stdout` — Sends output to stdout

*   `stderr` — Sends output to stderr

*   `file=<span class="variable">filename</span>` — Sends output to text file(s).

When using `file=<span class="variable">filename</span>`, specifying `%p` and/or `%t` in the file name expands to the JVM's PID and startup timestamp, respectively. You can also configure text files to handle file rotation based on file size and a number of files to rotate. For example, to rotate the log file every 10 MB and keep 5 files in rotation, specify the options `filesize=10M, filecount=5`. The target size of the files isn’t guaranteed to be exact, it’s just an approximate value. Files are rotated by default with up to 5 rotated files of target size 20 MB, unless configured otherwise. Specifying `filecount=0` means that the log file shouldn’t be rotated. There’s a possibility of the pre-existing log file getting overwritten.

</div>
<!-- class="section" -->
<div class="section" id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__DECORATIONS-A7A4AD31">

Decorations

<div class="p">Logging messages are decorated with information about the message. You can configure each output to use a custom set of decorators. The order of the output is always the same as listed in the table. You can configure the decorations to be used at run time. Decorations are prepended to the log message. For example:
<pre dir="ltr">[6.567s][info][gc,old] Old collection complete
</pre></div>

Omitting `<span class="variable">decorators</span>` defaults to `uptime`, `level`, and `tags`. The `none` decorator is special and is used to turn off all decorations.

`time` (`t`), `utctime` (`utc`), `uptime` (`u`), `timemillis` (`tm`), `uptimemillis` (`um`), `timenanos` (`tn`), `uptimenanos` (`un`), `hostname` (`hn`), `pid` (`p`), `tid` (`ti`), `level` (`l`), `tags` (`tg`) decorators can also be specified as `none` for no decoration.

<div class="tblformalwide" id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__THEFOLLOWINGTABLEDESCRIBESALISTOFTH-4A1B5B9F">
<table class="cellalignment186" summary="The following table describes a list of the possible decorations.">
<thead>
<tr class="cellalignment180">
<th class="cellalignment196" id="d11430e7535">Decorations</th>
<th class="cellalignment196" id="d11430e7537">Description</th>
</tr>
</thead>
<tbody>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`time` or `t`</td>
<td class="cellalignment180" headers="d11430e7537">

Current time and date in ISO-8601 format.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`utctime` or `utc`</td>
<td class="cellalignment180" headers="d11430e7537">

Universal Time Coordinated or Coordinated Universal Time.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`uptime` or `u`</td>
<td class="cellalignment180" headers="d11430e7537">

Time since the start of the JVM in seconds and milliseconds. For example, 6.567s.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`timemillis` or `tm`</td>
<td class="cellalignment180" headers="d11430e7537">

The same value as generated by `System.currentTimeMillis()`.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`uptimemillis` or `um`</td>
<td class="cellalignment180" headers="d11430e7537">

Milliseconds since the JVM started.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`timenanos` or `tn`</td>
<td class="cellalignment180" headers="d11430e7537">

The same value generated by `System.nanoTime()`.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`uptimenanos` or `un`</td>
<td class="cellalignment180" headers="d11430e7537">

Nanoseconds since the JVM started.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`hostname` or `hn`</td>
<td class="cellalignment180" headers="d11430e7537">

The host name.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`pid` or `p`</td>
<td class="cellalignment180" headers="d11430e7537">

The process identifier.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`tid` or `ti`</td>
<td class="cellalignment180" headers="d11430e7537">

The thread identifier.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`level` or `l`</td>
<td class="cellalignment180" headers="d11430e7537">

The level associated with the log message.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7535">`tags` or `tg`</td>
<td class="cellalignment180" headers="d11430e7537">

The tag-set associated with the log message.

</td>
</tr>
</tbody>
</table>
</div>
<!-- class="inftblhruleinformal" --></div>
<!-- class="section" -->
<div class="section" id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__CONVERTGCLOGGINGFLAGSTOXLOG-A5046BD1">

Convert GC Logging Flags to Xlog

<div class="tblformalwide" id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-615C95D0-B715-4924-BF23-6915455D15DB">

Table 2-1 Mapping Legacy Garbage Collection Logging Flags to the Xlog Configuration

<table class="cellalignment186" title="Mapping Legacy Garbage Collection Logging Flags to the Xlog Configuration" summary=" The following table describes the mapping of legacy Garbage Collection (GC) logging flags to the corresponding new Xlog configuration.">
<thead>
<tr class="cellalignment180">
<th class="cellalignment196" id="d11430e7682">Legacy Garbage Collection (GC) Flag</th>
<th class="cellalignment196" id="d11430e7684">Xlog Configuration</th>
<th class="cellalignment196" id="d11430e7686">Comment</th>
</tr>
</thead>
<tbody>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`G1PrintHeapRegions`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:gc+region=trace`

</td>
<td class="cellalignment180" headers="d11430e7686">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`GCLogFileSize`</td>
<td class="cellalignment180" headers="d11430e7684">

No configuration available

</td>
<td class="cellalignment180" headers="d11430e7686">Log rotation is handled by the framework.</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`NumberOfGCLogFiles`</td>
<td class="cellalignment180" headers="d11430e7684">

Not Applicable

</td>
<td class="cellalignment180" headers="d11430e7686">Log rotation is handled by the framework.</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintAdaptiveSizePolicy`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:ergo*=<span class="variable">level</span>`

</td>
<td class="cellalignment180" headers="d11430e7686">

Use a `<span class="variable">level</span>` of `debug` for most of the information, or a `<span class="variable">level</span>` of `trace` for all of what was logged for `PrintAdaptiveSizePolicy`.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintGC`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:gc`

</td>
<td class="cellalignment180" headers="d11430e7686">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintGCApplicationConcurrentTime`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:safepoint`

</td>
<td class="cellalignment180" headers="d11430e7686">

Note that `PrintGCApplicationConcurrentTime` and `PrintGCApplicationStoppedTime` are logged on the same tag and aren’t separated in the new logging.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintGCApplicationStoppedTime`</td>
<td class="cellalignment180" headers="d11430e7684">
<div class="p">
<pre dir="ltr">-Xlog:safepoint
</pre></div>
</td>
<td class="cellalignment180" headers="d11430e7686">

Note that `PrintGCApplicationConcurrentTime` and `PrintGCApplicationStoppedTime` are logged on the same tag and not separated in the new logging.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintGCCause`</td>
<td class="cellalignment180" headers="d11430e7684">

Not Applicable

</td>
<td class="cellalignment180" headers="d11430e7686">GC cause is now always logged.</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintGCDateStamps`</td>
<td class="cellalignment180" headers="d11430e7684">

Not Applicable

</td>
<td class="cellalignment180" headers="d11430e7686">

Date stamps are logged by the framework.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintGCDetails`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:gc*`

</td>
<td class="cellalignment180" headers="d11430e7686">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintGCID`</td>
<td class="cellalignment180" headers="d11430e7684">

Not Applicable

</td>
<td class="cellalignment180" headers="d11430e7686">

GC ID is now always logged.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintGCTaskTimeStamps`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:task*=debug`

</td>
<td class="cellalignment180" headers="d11430e7686">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintGCTimeStamps`</td>
<td class="cellalignment180" headers="d11430e7684">

Not Applicable

</td>
<td class="cellalignment180" headers="d11430e7686">

Time stamps are logged by the framework.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintHeapAtGC`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:gc+heap=trace`

</td>
<td class="cellalignment180" headers="d11430e7686">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintReferenceGC`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:ref*=debug`

</td>
<td class="cellalignment180" headers="d11430e7686">

Note that in the old logging, `PrintReferenceGC` had an effect only if `PrintGCDetails` was also enabled.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintStringDeduplicationStatistics`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:stringdedup*=debug`

</td>
<td class="cellalignment180" headers="d11430e7686">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`PrintTenuringDistribution`</td>
<td class="cellalignment180" headers="d11430e7684">

`-Xlog:age*=<span class="variable">level</span>`

</td>
<td class="cellalignment180" headers="d11430e7686">

Use a `<span class="variable">level</span>` of `debug` for the most relevant information, or a `<span class="variable">level</span>` of `trace` for all of what was logged for `PrintTenuringDistribution`.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7682">`UseGCLogFileRotation`</td>
<td class="cellalignment180" headers="d11430e7684">

Not Applicable

</td>
<td class="cellalignment180" headers="d11430e7686">

What was logged for `PrintTenuringDistribution`.

</td>
</tr>
</tbody>
</table>
</div>
<!-- class="inftblhruleinformal" --></div>
<!-- class="section" -->
<div class="section" id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__CONVERTRUNTIMELOGGINGFLAGSTOXLOG-A504703E">

Convert Runtime Logging Flags to Xlog

<div class="tblformalwide" id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__MAPPINGLEGACYGARBAGECOLLECTIONLOGGI-A486C349">

Table 2-2 Mapping Runtime Logging Flags to the Xlog Configuration

<table class="cellalignment186" title="Mapping Runtime Logging Flags to the Xlog Configuration" summary=" The following table describes the mapping of legacy runtime logging flags to the corresponding new Xlog configuration.">
<thead>
<tr class="cellalignment180">
<th class="cellalignment196" id="d11430e7952">Legacy Runtime Flag</th>
<th class="cellalignment196" id="d11430e7954">Xlog Configuration</th>
<th class="cellalignment196" id="d11430e7956">Comment</th>
</tr>
</thead>
<tbody>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceExceptions`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:exceptions=info`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceClassLoading`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:class+load=<span class="variable">level</span>`

</td>
<td class="cellalignment180" headers="d11430e7956">

Use `<span class="variable">level</span>=info` for regular information, or `<span class="variable">level</span>=debug` for additional information. In Unified Logging syntax, `-verbose:class` equals `-Xlog:class+load=info,class+unload=info`.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceClassLoadingPreorder`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:class+preorder=debug`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceClassUnloading`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:class+unload=<span class="variable">level</span>`

</td>
<td class="cellalignment180" headers="d11430e7956">

Use `<span class="variable">level</span>=info` for regular information, or `<span class="variable">level</span>=trace` for additional information. In Unified Logging syntax, `-verbose:class` equals `-Xlog:class+load=info,class+unload=info`.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`VerboseVerification`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:verification=info`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceClassPaths`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:class+path=info`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceClassResolution`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:class+resolve=debug`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceClassInitialization`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:class+init=info`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceLoaderConstraints`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:class+loader+constraints=info`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceClassLoaderData`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:class+loader+data=<span class="variable">level</span>`

</td>
<td class="cellalignment180" headers="d11430e7956">

Use `level=debug` for regular information or `level=trace` for additional information.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceSafepointCleanupTime`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:safepoint+cleanup=info`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceSafepoint`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:safepoint=debug`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceMonitorInflation`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:monitorinflation=debug`

</td>
<td class="cellalignment180" headers="d11430e7956">

Not Applicable

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceBiasedLocking`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:biasedlocking=<span class="variable">level</span>`

</td>
<td class="cellalignment180" headers="d11430e7956">

Use `level=info` for regular information, or `level=trace` for additional information.

</td>
</tr>
<tr class="cellalignment180">
<td class="cellalignment180" headers="d11430e7952">`TraceRedefineClasses`</td>
<td class="cellalignment180" headers="d11430e7954">

`-Xlog:redefine+class*=<span class="variable">level</span>`

</td>
<td class="cellalignment180" headers="d11430e7956">

`level=info`, `=debug`, and `=trace` provide increasing amounts of information.

</td>
</tr>
</tbody>
</table>
</div>
<!-- class="inftblhruleinformal" --></div>
<!-- class="section" -->
<div class="section">

-Xlog Usage Examples

The following are `-Xlog` examples.

<dl>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-7A56566A-44F3-4D4F-90D9-F31A5968059B"><!-- --></a>`-Xlog`</dt>
<dd>

Logs all messages by using the `info`level to `stdout` with `uptime`, `levels`, and `tags` decorations. This is equivalent to using:

<pre dir="ltr">-Xlog:all=info:stdout:uptime,levels,tags
</pre></dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-E685CE51-86BF-4EF0-851B-05F40F6898F5"><!-- --></a>`-Xlog:gc`</dt>
<dd>

Logs messages tagged with the `gc` tag using `info` level to `stdout`. The default configuration for all other messages at level `warning` is in effect.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-55445A32-BFBC-424F-B9C0-090ECF203926"><!-- --></a>`-Xlog:gc,safepoint`</dt>
<dd>

Logs messages tagged either with the `gc` or `safepoint` tags, both using the `info` level, to `stdout`, with default decorations. Messages tagged with both `gc` and `safepoint`won’t be logged.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-1736B765-CAF5-4A55-9FD2-B3921FF407B1"><!-- --></a>`-Xlog:gc+ref=debug`</dt>
<dd>

Logs messages tagged with both `gc` and `ref` tags, using the `debug` level to `stdout`, with default decorations. Messages tagged only with one of the two tags won’t be logged.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-0CFF7A78-5FE2-4892-A414-51576BBA2323"><!-- --></a>`-Xlog:gc=debug:file=gc.txt:none`</dt>
<dd>

Logs messages tagged with the `gc` tag using the `debug` level to a file called `gc.txt` with no decorations. The default configuration for all other messages at level `warning` is still in effect.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-6387517C-8601-4F75-9C2C-CC675E287DC3"><!-- --></a>`-Xlog:gc=trace:file=gctrace.txt:uptimemillis,pids:filecount=5,filesize=1024`</dt>
<dd>

Logs messages tagged with the `gc` tag using the `trace` level to a rotating file set with 5 files with size 1 MB with the base name `gctrace.txt` and uses decorations `uptimemillis` and `pid`.

The default configuration for all other messages at level`warning` is still in effect.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-B02B8F49-5E24-4A38-B723-0370BE75A7B6"><!-- --></a>`-Xlog:gc::uptime,tid`</dt>
<dd>

Logs messages tagged with the `gc` tag using the default 'info' level to default the output`stdout` and uses decorations `uptime` and `tid`. The default configuration for all other messages at level`warning` is still in effect.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-01D8E833-3611-4CF4-9571-3D3547BB5143"><!-- --></a>`-Xlog:gc*=info,safepoint*=off`</dt>
<dd>

Logs messages tagged with at least `gc` using the `info` level, but turns off logging of messages tagged with `safepoint`. Messages tagged with both `gc` and `safepoint` won’t be logged.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-65F007AE-7757-4EDD-8CC0-31933EBE9D64"><!-- --></a>`-Xlog:disable -Xlog:safepoint=trace:safepointtrace.txt`</dt>
<dd>

Turns off all logging, including warnings and errors, and then enables messages tagged with `safepoint`using `trace`level to the file `safepointtrace.txt`. The default configuration doesn’t apply, because the command line started with `-Xlog:disable`.

</dd>
</dl>
</div>
<!-- class="section" -->
<div class="section">

Complex -Xlog Usage Examples

The following describes a few complex examples of using the `-Xlog` option.

<dl>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-D843C19D-D6CA-437E-AE7E-CAE5E16B7B22"><!-- --></a>`-Xlog:gc+class*=debug`</dt>
<dd>

Logs messages tagged with at least `gc` and `class` tags using the `debug` level to `stdout`. The default configuration for all other messages at the level `warning` is still in effect

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-AA440A9C-C4FE-4B5D-BBA9-18FCBA040CEA"><!-- --></a>`-Xlog:gc+meta*=trace,class*=off:file=gcmetatrace.txt`</dt>
<dd>

Logs messages tagged with at least the `gc` and `meta` tags using the`trace` level to the file `metatrace.txt` but turns off all messages tagged with `class`. Messages tagged with `gc`, `meta`, and`class` aren’t be logged as`class*` is set to off. The default configuration for all other messages at level `warning` is in effect except for those that include `class`.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-49527B3D-FF04-434A-B01F-6CF786C17158"><!-- --></a>`-Xlog:gc+meta=trace`</dt>
<dd>

Logs messages tagged with exactly the `gc` and `meta` tags using the `trace` level to `stdout`. The default configuration for all other messages at level `warning` is still be in effect.

</dd>
<dt class="dlterm"><a id="GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5__GUID-A7FC1C13-00D1-4242-9BD4-30A85F81E598"><!-- --></a>`-Xlog:gc+class+heap*=debug,meta*=warning,threads*=off`</dt>
<dd>

Logs messages tagged with at least `gc`, `class`, and `heap` tags using the `trace` level to `stdout` but only log messages tagged with `meta` with level. The default configuration for all other messages at the level `warning` is in effect except for those that include `threads`.

</dd>
</dl>
</div>
<!-- class="section" --></div>
</div>
<div class="sect2"><a id="GUID-9569449C-525F-4474-972C-4C1F63D5C357"></a>

## Validate Java Virtual Machine Flag Arguments

<div>

You use values provided to all Java Virtual Machine (JVM) command-line flags for validation and, if the input value is invalid or out-of-range, then an appropriate error message is displayed.

<div class="section">Whether they’re set ergonomically, in a command line, by an input tool, or through the APIs (for example, classes contained in the package `java.lang.management`) the values provided to all Java Virtual Machine (JVM) command-line flags are validated. Ergonomics are described in <span><cite>Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide</cite></span>.

Range and constraints are validated either when all flags have their values set during JVM initialization or a flag’s value is changed during runtime (for example using the `jcmd` tool). The JVM is terminated if a value violates either the range or constraint check and an appropriate error message is printed on the error stream.

</div>
<!-- class="section" -->
<div class="section">For example, if a flag violates a range or a constraint check, then the JVM exits with an error:
<pre dir="ltr">java -XX:AllocatePrefetchStyle=5 -version   
intx AllocatePrefetchStyle=5 is outside the allowed range [ 0 ... 3 ]   
Improperly specified VM option 'AllocatePrefetchStyle=5'   
Error: Could not create the Java Virtual Machine.  
Error: A fatal exception has occurred. Program will exit.
</pre>

The flag `-XX:+PrintFlagsRanges` prints the range of all the flags. This flag allows automatic testing of the flags by the values provided by the ranges. For the flags that have the ranges specified, the type, name, and the actual range is printed in the output.

<div class="p">For example,
<pre dir="ltr">intx   ThreadStackSize [ 0 ... 9007199254740987 ] {pd product}
</pre>
For the flags that don’t have the range specified, the values aren’t displayed in the print out. For example,:
<pre dir="ltr">size_t NewSize         [   ...                  ] {product}
</pre>
This helps to identify the flags that need to be implemented. The automatic testing framework can skip those flags that don’t have values and aren’t implemented.</div>
</div>
<!-- class="section" --></div>
</div>
<div class="sect2"><a id="GUID-7BE7CD55-3AC3-4A96-BBDD-E4D9FC4FCCCB"></a>

## Large Pages

<div>

You use large pages, also known as huge pages, as memory pages that are significantly larger than the standard memory page size (which varies depending on the processor and operating system). Large pages optimize processor Translation-Lookaside Buffers.

<div class="section">

A Translation-Lookaside Buffer (TLB) is a page translation cache that holds the most-recently used virtual-to-physical address translations. A TLB is a scarce system resource. A TLB miss can be costly because the processor must then read from the hierarchical page table, which may require multiple memory accesses. By using a larger memory page size, a single TLB entry can represent a larger memory range. This results in less pressure on a TLB, and memory-intensive applications may have better performance.

However, large pages page memory can negatively affect system performance. For example, when a large mount of memory is pinned by an application, it may create a shortage of regular memory and cause excessive paging in other applications and slow down the entire system. Also, a system that has been up for a long time could produce excessive fragmentation, which could make it impossible to reserve enough large page memory. When this happens, either the OS or JVM reverts to using regular pages.

</div>
<!-- class="section" -->
<div class="section">

</div>
<!-- class="section" -->
<div class="section">

Large Pages Support

Oracle Solaris, Linux, and Windows Server 2003 support large pages.

</div>
<!-- class="section" -->
<div class="section">

Large Pages Support for Oracle Solaris

Oracle Solaris 9 and later include Multiple Page Size Support (MPSS). No additional configuration is necessary. See [&nbsp;Features and Benefits - Scalability](http://www.oracle.com/technetwork/server-storage/solaris10/overview/solaris9-features-scalability-135663.html).

</div>
<!-- class="section" -->
<div class="section">

Large Pages Support for Linux

The 2.6 kernel supports large pages. Some vendors have backported the code to their 2.4-based releases. To check if your system can support large page memory, try the following:

<pre dir="ltr"># cat /proc/meminfo | grep Huge
HugePages_Total: 0
HugePages_Free: 0
Hugepagesize: 2048 kB
</pre>

If the output shows the three "Huge" variables, then your system can support large page memory but it needs to be configured. If the command prints nothing, then your system doesn’t support large pages. To configure the system to use large page memory, login as `root`, and then follow these steps:

1.  If you’re using the option `-XX:+UseSHM` (instead of `-XX:+UseHugeTLBFS`), then increase the `SHMMAX` value. It must be larger than the Java heap size. On a system with 4 GB of physical RAM (or less), the following makes all the memory sharable:

    <pre dir="ltr"># echo 4294967295 &gt; /proc/sys/kernel/shmmax
</pre>
2.  If you’re using the option `-XX:+UseSHM` or `-XX:+UseHugeTLBFS`, then specify the number of large pages. In the following example, 3 GB of a 4 GB system are reserved for large pages (assuming a large page size of 2048kB, then 3 GB = 3 * 1024 MB = 3072 MB = 3072 * 1024 kB = 3145728 kB and 3145728 kB / 2048 kB = 1536):

    <pre dir="ltr"># echo 1536 &gt; /proc/sys/vm/nr_hugepages
</pre>
<div class="infobox-note" id="GUID-7BE7CD55-3AC3-4A96-BBDD-E4D9FC4FCCCB__GUID-F3CD1677-37DC-4141-992A-804E03320981">

Note:

*   Note that the values contained in `/proc` resets after you reboot your system, so may want to set them in an initialization script (for example, `rc.local` or `sysctl.conf`).

*   If you configure (or resize) the OS kernel parameters `/proc/sys/kernel/shmmax` or `/proc/sys/vm/nr_hugepages`, Java processes may allocate large pages for areas in addition to the Java heap. These steps can allocate large pages for the following areas:

        *   Java heap

        *   Code cache

        *   The marking bitmap data structure for the parallel GC

Consequently, if you configure the `nr_hugepages` parameter to the size of the Java heap, then the JVM can fail in allocating the code cache areas on large pages because these areas are quite large in size.
</div>
</div>
<!-- class="section" -->
<div class="section">

Large Pages Support for Windows Server 2003

Only Windows Server 2003 supports large pages. To use this feature, the administrator must first assign additional privileges to the user who’s running the application:

1.  Select <span class="bold">Control Panel</span>, <span class="bold">Administrative Tools</span>, and then <span class="bold">Local Security Policy</span>.

2.  Select <span class="bold">Local Policies</span> and then <span class="bold">User Rights Assignment</span>.

3.  Double-click <span class="bold">Lock pages in memory</span>, then add users and/or groups.

4.  Reboot your system.

Note that these steps are required even if it’s the administrator who’s running the application, because administrators by default don’t have the privilege to lock pages in memory.

</div>
<!-- class="section" --></div>
</div>
<div class="sect2"><a id="GUID-31503FCE-93D0-4175-9B4F-F6A738B2F4C4"></a>

## Application Class Data Sharing

<div>

Application Class Data Sharing (AppCDS) extends class data sharing to enable application classes to be placed in the shared archive.

<div class="section" id="GUID-31503FCE-93D0-4175-9B4F-F6A738B2F4C4__APP_CLASS_DATA_SHARING">

In addition to the core library classes, AppCDS supports [Class Data Sharing](../vm/class-data-sharing.htm#JSJVM-GUID-7EAA3411-8CF0-4D19-BD05-DF5E1780AA91) from the following locations:

*   Platform classes from the module image

*   Application classes from the module image

*   Application classes from `-cp` path
<div class="infobox-note" id="GUID-31503FCE-93D0-4175-9B4F-F6A738B2F4C4__GUID-714EC0FD-6C0E-4F87-80C2-AEA7386E342F">

Note:

In JDK 9, application classes from module path is not supported by AppCDS.

</div>

When running multiple JVM processes, AppCDS reduces the runtime footprint with memory sharing for read-only metadata.

This is a commercial feature that requires you to specify `-XX:+UnlockCommercialFeatures`.

</div>
<!-- class="section" -->
<div class="section">

Creating a Shared Archive File and Using It to Run an Application

The following steps create a shared archive file that contains all the classes used by the `test.Hello` application. The last step runs the application with the shared archive file.

1.  Create a list of all classes used by the `test.Hello` application. The following command creates a file named `hello.classlist` that contains a list of all classes used by this application:

    <pre dir="ltr">java -Xshare:off -XX:+UnlockCommercialFeatures -XX:DumpLoadedClassList=hello.classlist -XX:+UseAppCDS -cp hello.jar test.Hello
</pre>

Note that the `-cp` parameter must contain only JAR files; the `-XX:+UseAppCDS` option doesn’t support class paths that contain directory names.

2.  Create a shared archive, named `hello.jsa`, that contains all the classes in `hello.classlist`:

    <pre dir="ltr">java -XX:+UnlockCommercialFeatures -Xshare:dump -XX:+UseAppCDS -XX:SharedArchiveFile=hello.jsa -XX:SharedClassListFile=hello.classlist -cp hello.jar
</pre>

Note that the `-cp` parameter used at archive creation time must be the same as (or a prefix of) the `-cp` used at run time.

3.  Run the application `test.Hello` with the shared archive `hello.jsa`:

    <pre dir="ltr">java -XX:+UnlockCommercialFeatures -Xshare:on -XX:+UseAppCDS -XX:SharedArchiveFile=hello.jsa -cp hello.jar test.Hello
</pre>

Ensure that you have specified the option `-Xshare:on` or `-Xshare:auto`. If the option is not specified,`-Xshare:auto` is the default .

4.  <span class="bold">Optional</span>: Verify that the `test.Hello` application is using the class contained in the `hello.jsa` shared archive:

    <pre dir="ltr">java -XX:+UnlockCommercialFeatures -Xshare:on -XX:+UseAppCDS -XX:SharedArchiveFile=hello.jsa -cp hello.jar -verbose:class test.Hello
</pre>

The output of this command should contain the following text:

    <pre dir="ltr">Loaded test.Hello from shared objects file by sun/misc/Launcher$AppClassLoader
</pre>
</div>
<!-- class="section" -->
<div class="section">

Sharing a Shared Archive Across Multiple Application Processes

You can share the same archive file across multiple applications processes. This reduces memory usage because the archive is memory-mapped into the address space of the processes. The operating system automatically shares the read-only pages across these processes.

The following steps demonstrate how to create a common archive that can be shared by different applications. Only the classes from `common.jar` are archived in the `common.jsa` (step 3). Classes from `hello.jar` and `hi.jar` are not archived in this particular example because they are not in the `-cp` path during the archiving step (step 3).

To include classes from `hello.jar` and `hi.jar`, the `.jar` files must be added to the `-cp` path.

1.  Create a list of all classes used by the `Hello` application and another list for the `Hi` application:

    <pre dir="ltr">java -XX:+UnlockCommercialFeatures -XX:DumpLoadedClassList=hello.classlist -XX:+UseAppCDS -cp common.jar:hello.jar Hello
</pre>
<pre dir="ltr">java -XX:+UnlockCommercialFeatures -XX:DumpLoadedClassList=hi.classlist -XX:+UseAppCDS -cp common.jar:hi.jar Hi
</pre>
2.  Create a single list of classes used by all the applications that will share the shared archive file.

    <span class="bold"><span>Oracle Solaris, Linux, and OS X</span>:</span>: The following commands combine the files `hello.classlist` and `hi.classlist` into one file, `common.classlist`:

    <pre dir="ltr">cat hello.classlist hi.classlist &gt; common.classlist
</pre>

<span class="bold">Windows</span>: The following commands combine the files `hello.classlist` and `hi.classlist` into one file, `common.classlist`:

    <pre dir="ltr">type hello.classlist hi.classlist &gt; common.classlist
</pre>
3.  Create a shared archive, named `common.jsa`, that contains all the classes in `common.classlist`:

    <pre dir="ltr"> java -XX:+UnlockCommercialFeatures -Xshare:dump -XX:SharedArchiveFile=common.jsa -XX:+UseAppCDS -XX:SharedClassListFile=common.classlist -cp common.jar:hello.jar:hi.jar
</pre>

The value of the `-cp` parameter is the common class path prefix shared by the `Hello` and `Hi` applications.

4.  Run the `Hello` and `Hi` applications with the same shared archive:

    <pre dir="ltr"> java -XX:+UnlockCommercialFeatures -Xshare:on -XX:SharedArchiveFile=common.jsa -XX:+UseAppCDS -cp common.jar:hello.jar:hi.jar Hello</pre>
<pre dir="ltr">java -XX:+UnlockCommercialFeatures -Xshare:on -XX:SharedArchiveFile=common.jsa -XX:+UseAppCDS -cp common.jar:hello.jar:hi.jar Hi
</pre>
</div>
<!-- class="section" -->
<div class="section">

Specifying Additional Shared Data Added to an Archive File

<pre dir="ltr"> -XX:SharedArchiveConfigFile=<span class="variable">shared_config_file</span>
</pre>

The option is used to specify additional shared data added to the archive file. In JDK 9, it supports strings and symbols. The string data and symbol data should be generated by the `jcmd` tool attaching to a running JVM process. See [jcmd](jcmd.htm#GUID-59153599-875E-447D-8D98-0078A5778F05 "You use the jcmd utility to send diagnostic command requests to a running Java Virtual Machine (JVM).").

The following is an example of the string and symbol dumping command in `jcmd`:&nbsp;

<pre dir="ltr"> jcmd <span class="variable">pid</span> VM.stringtable -verbose  
 jcmd <span class="variable">pid</span> VM.symboltable -verbose
</pre>

The following is an example of a configuration file:

<pre dir="ltr">VERSION: 1.0
@SECTION: String
7: test123
1: *
8: segments
@SECTION: Symbol
10 -1: linkMethod
</pre>

In the configuration file example:

*   The string entries under `@SECTION: String` use the following format:

    <pre dir="ltr"><span class="variable">length: string </span></pre>
*   The `@SECTION: Symbol` entry uses the following format:

    <pre dir="ltr"> length refcount: symbol
</pre>

The `<span class="variable">refcount</span>` for a shared symbol is always `-1`.

`@SECTION` specifies the type of the section that follows it. All data within the section must be the same type that's specified by `@SECTION`. Different types of data can’t be mixed. Multiple separated data sections for the same type specified by different `@SECTION` are allowed within one `<span class="variable">shared_config_file</span>` .

</div>
<!-- class="section" --></div>
</div>
<div class="sect2"><a id="GUID-EE6BD9FA-EF6D-4C3E-AC5C-30B8762CDC1B"></a>

## Performance Tuning Examples

<div>

You can use the Java advanced runtime options to optimize the performance of your applications.

<div class="section">

Tuning for Higher Throughput

Use the following commands and advanced runtime options to achieve higher throughput performance for your application:

<pre dir="ltr">java -d64 -server -XX:+UseParallelGC -XX:+AggressiveOpts -XX:+UseLargePages -Xmn10g  -Xms26g -Xmx26g
</pre></div>
<!-- class="section" -->
<div class="section">

Tuning for Lower Response Time

Use the following commands and advanced runtime options to achieve lower response times for your application:

<pre dir="ltr">java -d64 -XX:+UseG1GC -Xms26g Xmx26g -XX:MaxGCPauseMillis=500 -XX:+PrintGCTimeStamp 
</pre></div>
<!-- class="section" -->
<div class="section">

Keeping the Java Heap Small and Reducing the Dynamic Footprint of Embedded Applications

Use the following advanced runtime options to keep the Java heap small and reduce the dynamic footprint of embedded applications:

<pre dir="ltr">-XX:MaxHeapFreeRatio=10 -XX:MinHeapFreeRatio=5
</pre>
<div class="infobox-note" id="GUID-EE6BD9FA-EF6D-4C3E-AC5C-30B8762CDC1B__GUID-55866564-4AA3-4283-9A0C-19D23D308633">

Note:

The defaults for these two options are 70% and 40% respectively. Because performance sacrifices can occur when using these small settings, you should optimize for a small footprint by reducing these settings as much as possible without introducing unacceptable performance degradation.

</div>
</div>
<!-- class="section" --></div>
</div>
<div class="sect2"><a id="GUID-741FC470-AA3E-494A-8D2B-1B1FE4A990D1"></a>

## Exit Status

<div>


The following exit values are typically returned by the launcher when the launcher is called with the wrong arguments, serious errors, or exceptions thrown by the JVM. However, a Java application may choose to return any value by using the API call `System.exit(exitValue)`. The values are:

*   `0`: Successful completion

*   `&gt;0`: An error occurred


