# Building JavaCard Applet with Ant-JavaCard

⚠️ **Note:** This guide only supports on  Windows.
## Install Ant

1. Download Apache Ant from here: https://ant.apache.org/
2. Then unpack apache-ant-xxxz into a tools directory like so: D:\SOFTWARE\apache ant--1.10.15
3. Add the environment variable: ANT_HOME: D:\SOFTWARE\apache ant--1.10.15, and
4. Add %ANT_HOME%\bin to PATH

## Install JDK

1. Download, OpenJDK, Standard Edition 11 from https://jdk.java.net/java-se-ri/11-MR3
2. Then unpack it into your custom directory, such as: D:\SOFTWARE\OpenJDK\jdk-11.0.0.2
3. Add the environment variable: JAVA_HOME = D:\SOFTWARE\OpenJDK\jdk-11.0.0.2
4. Add %JAVA_HOME%\bin and %JAVA_HOME%\jre\bin to PATH

## Install JavaCard SDK

1. Download JavaCard SDK from https://github.com/ibaibing/javacard_sdks to **your_project/libs/jcsdks**, or

2. Set https://github.com/ibaibing/javacard_sdks as your project subfolder.
```bat
cd your_project
git submodule add https://github.com/ibaibing/javacard_sdks.git libs/jcsdks
or
git submodule add git@github.com:ibaibing/globalplatform-apis.git libs/jcsdks

```
## Install GlobalPlatform API

1. If you need to use GlobalPlatform API, download GlobalPlatform from to **your_project/libs/gpapis**, or 
1. Add [globalplatform-apis](https://github.com/ibaibing/globalplatform-apis.git) as  submodule in your project.
```bat
cd your_project
git submodule add https://github.com/ibaibing/globalplatform-apis.git libs/gpapis
or
git submodule add git@github.com:ibaibing/globalplatform-apis.git libs/gpapis
```

## Install Custom API

1. If you have your own API, Copy the directory to **your_project/libs/custom/** directory.

## Install ant-javacard

1. Download the latest version of ant-javacard.jar from:
    https://github.com/martinpaljak/ant-javacard/releases/latest/download/ant-javacard.jar
2. Copy it into **your_project/libs** directory.

## Config Build.xml

1.Edit the classpath of ant-javacard

```xml
<taskdef name="javacard" classname="pro.javacard.ant.JavaCard" classpath="lib/ant-javacard.jar" />
```
2.You can edit the CAP version, SDK version like this:
```xml
<target name="build">
    <javacard jckit="libs/jcsdks/jc310b43_kit">
	    <cap aid="54657374506B67" version="1.0" sources="src" ints="true" strip="true" debug="true" verify="true">
		    <applet class="id.example.Applet" aid="5465737441707000" />
			<import exps="libs/gpapis/CORE/1.5/exports" jar="libs/gpapis/CORE/1.5/gpapi-globalplatform.jar"/>
			<import exps="libs/custom/export_files" jar="libs/custom/com.custom.api.jar"/>
		</cap>
    </javacard>
    <javacard jckit="libs/jcsdks/jc320v24.1_kit">
	    <cap aid="54657374506B67" version="1.1" sources="src" ints="true" strip="true" debug="true" verify="true">
		    <applet class="id.example.Applet" aid="5465737441707000" />
			<import exps="libs/gpapis/CORE/1.5/exports" jar="libs/gpapis/CORE/1.5/gpapi-globalplatform.jar"/>
			<import exps="libs/custom/export_files" jar="libs/custom/com.custom.api.jar"/>
		</cap>
    </javacard>
</target>

```

## Run Ant   

```bash
cd your_project
ant
```

