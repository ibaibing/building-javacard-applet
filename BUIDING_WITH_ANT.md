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

   [!NOTE]

   Different JCSDK need different JDK, please see []() for more information.

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
<property name="package.name" value="HelloWorld"/>	
<property name="package.aid.ant" value="0xD0:0x48:0x65:0x6c:0x6c:0x6F:0x77:0x6F:0x72:0x6c:0x64"/><!--for ant-->
<property name="applet.name" value="HelloWorld"/>	
<property name="applet.aid.ant" value="0xD0:0x48:0x65:0x6c:0x6c:0x6F:0x77:0x6F:0x72:0x6c:0x64:0x00"/><!--for ant-->

<property name="src.dir" location="src"/>
<property name="output.dir" location="bin"/>

<property name="jc301.sdk" value="./libs/jcsdks/jc301_kit_ClassicEdition"/>
<property name="gpapi.jar" value="./libs/gpapis/CORE/1.0/gpapi-globalplatform.jar"/>
```
2.Edit specified jcxxx.xml like this

```xml
<project name="build-jc301" default="convert" basedir=".">
    
	<import file="build.properties.xml"/>
	
	<!-- Compile target using exec form -->
    <target name="compile">
        <!-- Create output directory -->
        <mkdir dir="${output.dir}/jc301"/>

        <!-- Compile JavaCard source files -->
        <exec executable="javac" failonerror="true">
            <arg value="-source"/><arg value="1.5"/>
            <arg value="-target"/><arg value="1.5"/>
            <arg value="-d"/><arg value="${output.dir}/jc301"/>
            <arg value="-classpath"/>
            <arg value="${jc301.sdk}/lib/api_classic.jar;${output.dir}/jc301;${gpapi.jar}"/>
            <arg value="${src.dir}/${package.name}/*.java"/>
        </exec>
    </target>
	
	<!-- Convert target using exec form -->
    <target name="convert" depends="compile">
	    <!-- Run JavaCard converter using specific JDK -->
        <exec executable="${jdk11.java}" failonerror="true">

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
3. Config the JCSDK you need in your project in build.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="building-javacard-applet" default="convert" basedir=".">
    <import file="build.properties.xml"/>
    <target name="convert">
        <subant target="convert">
            <fileset dir=".">
			    <!-- Import all build targets for each JCSDK version -->
                <include name="build.jc301.xml"/>
                <!-- include name="build.jc320v241.xml"/-->
                <!-- include name="build.jc320v250.xml"/-->
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
2. Build with specified JCSDK
```bash
cd your_project
ant -f build.jc301.xml
or
ant -f build.jc301.xml convert
```



[!Note]

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
