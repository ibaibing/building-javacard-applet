# Building JavaCard Applet with Shell

>[!Note]
>This guide only tested on Windows.

## Requirements
Please see [README](README.md) for details.

## Build

Use JCSDK directly like this or you can edit  the jcsdk, gpapi and so on, and then run  [build301](build301.bat).

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
