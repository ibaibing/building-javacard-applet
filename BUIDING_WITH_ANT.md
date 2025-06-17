# Building JavaCard Applet with Ant

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

> [!NOTE]
> Different JCSDK need different JDK, please see [javacard_sdks](https://github.com/ibaibing/javacard_sdks/blob/master/README.FORK.md) for more information.

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

If you have your own API, Copy the directory to **your_project/libs/custom/** directory. and then you should edit the jcxxxx.xml file to support compile and covert.

## Config

1.Define the properties, edit **build.property.xml**

```xml
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
2.Edit specified jcxxx.xml like this

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="build-jc301" default="convert" basedir=".">
    
	<import file="build.properties.xml"/>
	<import file="build.dateinfo.xml"/>
	
	<property name="jdk.java"  value="${jdk11.java}"/>
	<property name="jdk.javac" value="${jdk11.javac}"/>
	
	<!-- Compile target using exec form -->
    <target name="compile" depends="generate-info-file">
	    <echo message="compile ${jc301.sdk}..." />
		
        <!-- Create output directory -->
        <mkdir dir="${output.dir}/jc301"/>

        <!-- Compile JavaCard source files -->
        <exec executable="${jdk.javac}" failonerror="true">
            <arg value="-source"/><arg value="1.6"/>
            <arg value="-target"/><arg value="1.6"/>
            <arg value="-d"/><arg value="${output.dir}/jc301"/>
            <arg value="-classpath"/>
            <arg value="${jc301.sdk}/lib/api_classic.jar;${output.dir}/jc301;${gpapi.jar}"/>
            <arg value="${src.dir}/${package.name}/*.java"/>
        </exec>
    </target>
	
	<!-- Convert target using exec form -->
    <target name="convert" depends="compile">
	    <echo message="convert ${jc301.sdk}..." />
		
	    <!-- Run JavaCard converter using specific JDK -->
        <exec executable="${jdk.java}" failonerror="true">

			<!-- Set JVM property jc.home -->
			<arg value="-Djc.home=${jc301.sdk}"/>
			
			<!-- Set classpath to include JavaCard converter tool -->
			<arg value="-cp"/>
			<arg value="${jc301.sdk}/lib/tools.jar"/>
			
			<!-- Main class to run -->
			<arg value="com.sun.javacard.converter.Main"/>

			<!-- Specify export path -->
			<arg value="-exportpath"/>
			<arg value="${output.dir}/jc301;${jc301.sdk}/api_export_files;./libs/gpapis/CORE/1.0/exports;./export"/>


			<!-- Output formats -->
            <arg value="-out"/>
            <arg value="JCA"/>
            <arg value="CAP"/>
            <arg value="EXP"/>
			
			<!-- Compiled class files directory -->
            <arg value="-classdir"/>
            <arg value="${output.dir}/jc301"/>

			<!-- Output directory for generated files -->
            <arg value="-d"/>
            <arg value="${output.dir}/jc301"/>
			
			<!-- support int -->
            <arg value="-i"/>
			
			<!-- Applet information -->
            <arg value="-applet"/>
            <arg value="${package.aid.ant}"/>
            <arg value="${package.name}"/>
            <arg value="${applet.name}"/>
            <arg value="${applet.aid.ant}"/>

			<!-- Applet version -->
            <arg value="${version}"/>
        </exec>
    </target>
</project>


```
3. Config the JCSDK which you need in your project in build.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="building-javacard-applet" default="convert" basedir=".">
    <import file="ant/build.properties.xml"/>
    <target name="convert">
        <subant target="convert">
            <fileset dir=".">
			    <!-- Import all build targets for each JCSDK version -->
				<!--include name="build.jc21.xml"/-->
				<include name="ant/build.jc211.xml"/>
				<include name="ant/build.jc212.xml"/>
				<!--include name="ant/build.jc220.xml"/-->
				<include name="ant/build.jc221.xml"/>
				<include name="ant/build.jc320v250.xml"/>
				<include name="ant/build.jc222.xml"/>
				<include name="ant/build.jc301.xml"/>
				<include name="ant/build.jc302.xml"/>
				<include name="ant/build.jc303.xml"/>
				<include name="ant/build.jc304.xml"/>
				<include name="ant/build.jc305u1.xml"/>
				<include name="ant/build.jc305u2.xml"/>
				<include name="ant/build.jc305u3.xml"/>
				<include name="ant/build.jc305u4.xml"/>
				<include name="ant/build.jc310b43.xml"/>
				<include name="ant/build.jc310r20210706.xml"/>
				<include name="ant/build.jc320v240.xml"/>
				<include name="ant/build.jc320v241.xml"/>
            </fileset>
        </subant>
    </target>
</project>


```
## BUILD
1. Build with all JCSKDs
```bash
cd your_project
ant
or
ant convert
or
ant -f build.xml convert
```
## SDK and Converter Version

| SDK                      | Converter Version | Support   |
| ------------------------ | ----------------- | --------- |
| jc20_kit                 | N/A               | N/A       |
| jc21_kit                 | 1.0               | No        |
| jc211_kit                | 1.1               | Yes(JDK8) |
| jc212_kit                | 1.2               | Yes(JDK8) |
| jc220_kit                | 1.3               | No        |
| jc221_kit                | 1.3               | Yes(JDK8) |
| jc222_kit                | 1.3               | Yes(JDK8) |
| jc301_kit_ClassicEdition | v3.0.1            | Yes       |
| jc302_kit_ClassicEdition | v3.0.2            | Yes       |
| jc303_kit                | v3.0.3            | Yes       |
| jc304_kit                | v3.0.4            | Yes       |
| jc305u1_kit              | v3.0.5            | Yes       |
| jc305u2_kit              | v3.0.5            | Yes       |
| jc305u3_kit              | v3.0.5            | Yes       |
| jc305u4_kit              | v3.0.5            | Yes       |
| jc310b43_kit             | v3.1.0            | Yes       |
| jc310r20210706_kit       | v3.1.0            | Yes       |
| jc320v24.0_kit           | v3.2.0            | Yes       |
| jc320v24.1_kit           | v24.1             | Yes       |
| jc320v25.0_kit           | v25.0             | Yes       |
