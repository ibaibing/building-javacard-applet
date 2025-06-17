# Building JavaCard Applet with Shell

⚠️ **Note:** This guide only supports on  Windows.
## Install JDK

1. Download, OpenJDK, Standard Edition 11 from https://jdk.java.net/java-se-ri/11-MR3

2. Then unpack it into your custom directory, such as: D:\SOFTWARE\OpenJDK\jdk-11.0.0.2

3. Add the environment variable: JAVA_HOME = D:\SOFTWARE\OpenJDK\jdk-11.0.0.2

4. Add %JAVA_HOME%\bin and %JAVA_HOME%\jre\bin to PATH

   Note: Different JCSDK need different JDK, please see the  table in [ss](ss)

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

## Build

Use JCSDK directly like this:

```bash
cd your_project
```

```bash
javac 
-classpath .\libs\jcsdks\jc301_kit_ClassicEdition\lib\api_classic.jar;.\bin\Release;.\libs\gpapis\CORE\1.0\gpapi-globalplatform.jar; 
-target 1.5 
-source 1.5 
-d .\bin\Release 
-g src\HelloWorld\*.java
```

```bash
java 
-Djc.home=.\libs\jcsdks\jc301_kit_ClassicEdition  
-classpath .\libs\jcsdks\jc301_kit_ClassicEdition\lib\tools.jar com.sun.javacard.converter.Main 
-exportpath .\bin\Release;.\libs\jcsdks\jc301_kit_ClassicEdition\api_export_files;.\libs\gpapis\CORE\1.0\exports 
-out JCA CAP EXP  
-classdir .\bin\Release  
-d .\bin\Release 
-i 
-applet 0xD0:0x48:0x65:0x6c:0x6c:0x6F:0x77:0x6F:0x72:0x6c:0x64 HelloWorld HelloWorld 0xD0:0x48:0x65:0x6c:0x6c:0x6F:0x77:0x6F:0x72:0x6c:0x64:0x00 1.00
```
