# Building JavaCard Applet with Ant-JavaCard

>![Note] This guide only tested on Windows.

## Requirements

Please see [README](README.md) for details.

## Install Ant

1. Download Apache Ant from here: https://ant.apache.org/
2. Then unpack apache-ant-xxxz into a tools directory like so: D:\SOFTWARE\apache ant--1.10.15
3. Add the environment variable: ANT_HOME: D:\SOFTWARE\apache ant--1.10.15, and
4. Add %ANT_HOME%\bin to PATH

## Install ant-javacard

1. Download the latest version of ant-javacard.jar from:
    https://github.com/martinpaljak/ant-javacard/releases/latest/download/ant-javacard.jar
2. Copy it into **your_project/libs** directory.

## Config build.ant.javacard.xml

1.Define the properties, edit **ant/build.property.xml**

```bash
<!-- build.properties.xml -->
<project name="build-properties" default="noop" basedir=".">

    <!-- Dummy target to avoid "no default target" warning -->
    <target name="noop"/>
	<property name="ant.javacard" value="../libs/ant-javacard.jar"/>

    <!-- Global properties -->
    <property name="version" value="1.0"/>
    <property name="package.name" value="HelloWorld"/>
    <property name="package.aid" value="D0:48:65:6c:6c:6F:77:6F:72:6c:64"/>
    <property name="package.aid.ant" value="0xD0:0x48:0x65:0x6c:0x6c:0x6F:0x77:0x6F:0x72:0x6c:0x64"/>
    <property name="applet.name" value="HelloWorld"/>
    <property name="applet.aid" value="D0:48:65:6c:6c:6F:77:6F:72:6c:64:00"/>
    <property name="applet.aid.ant" value="0xD0:0x48:0x65:0x6c:0x6c:0x6F:0x77:0x6F:0x72:0x6c:0x64:0x00"/>

    <!-- Directory settings -->
    <property name="src.dir" location="../src"/>
    <property name="output.dir" location="../bin"/>

    <!-- JDK -->
	<property name="jdk8" value="F:/SOFTWARE/OpenJDK/java-se-8u44-ri/bin"/>
	<property name="jdk11" value="F:/SOFTWARE/OpenJDK/jdk-11.0.0.2/bin"/>
	
    <property name="jdk11.javac" value="${jdk11}/javac.exe"/>
	<property name="jdk11.java" value="${jdk11}/java.exe"/>
	<property name="jdk8.javac" value="${jdk8}/javac.exe"/>
	<property name="jdk8.java" value="${jdk8}/java.exe"/>
	
	<!-- JCSDK paths -->
    <property name="jc21.sdk" value="../libs/jcsdks/jc21_kit"/>
    <property name="jc211.sdk" value="../libs/jcsdks/jc211_kit"/>
    <property name="jc212.sdk" value="../libs/jcsdks/jc212_kit"/>
    <property name="jc220.sdk" value="../libs/jcsdks/jc220_kit"/>
    <property name="jc221.sdk" value="../libs/jcsdks/jc221_kit"/>
    <property name="jc222.sdk" value="../libs/jcsdks/jc222_kit"/>
    <property name="jc301.sdk" value="../libs/jcsdks/jc301_kit_ClassicEdition"/>
    <property name="jc302.sdk" value="../libs/jcsdks/jc302_kit_ClassicEdition"/>
    <property name="jc303.sdk" value="../libs/jcsdks/jc303_kit"/>
    <property name="jc304.sdk" value="../libs/jcsdks/jc304_kit"/>
    <property name="jc305u1.sdk" value="../libs/jcsdks/jc305u1_kit"/>
    <property name="jc305u2.sdk" value="../libs/jcsdks/jc305u2_kit"/>
    <property name="jc305u3.sdk" value="../libs/jcsdks/jc305u3_kit"/>
    <property name="jc305u4.sdk" value="../libs/jcsdks/jc305u4_kit"/>
    <property name="jc310b43.sdk" value="../libs/jcsdks/jc310b43_kit"/>
    <property name="jc310r20210706.sdk" value="../libs/jcsdks/jc310r20210706_kit"/>
    <property name="jc320v240.sdk" value="../libs/jcsdks/jc320v24.0_kit"/>
    <property name="jc320v241.sdk" value="../libs/jcsdks/jc320v24.1_kit"/>
    <property name="jc320v250.sdk" value="../libs/jcsdks/jc320v25.0_kit"/>

    <!-- GP API -->
    <property name="gpapi.jar" value="../libs/gpapis/CORE/1.0/gpapi-globalplatform.jar"/>

</project>
```

2.Edit the classpath of ant-javacard in ant/build.ant.javacard.xml

```xml
<taskdef name="javacard" classname="pro.javacard.ant.JavaCard" classpath="${ant.javacard}"/>
```
3.You can edit outfile name

```bash
<property name="output.fmt" value="${package.name}_%a_%h_v${version}_${date.strs}_JC%j_%J.cap"/>
```
4.You can change different jcsdk from here

  ```bash
  <property name="jc.sdk" value="${jc302.sdk}"/>
  ```

## Run Ant   

```bash
cd your_project
ant convert_ant_jc
```

>![NOTE]
>ant-javacard doesn't support jc301, throw **error: invalid flag -useproxyclass**, because this converter version doesn't support the option.
