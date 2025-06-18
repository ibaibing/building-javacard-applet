# Building JavaCard Applet
>![Note]This guide only tested on Windows.

You can build JavaCard Applets as follows, whichever you choose, you should be prepared for the requirements in this document.

| Name         | Description                                               |
| ------------ | --------------------------------------------------------- |
| ant-javacard | [BUIDING_WITH_ANT_JAVACARD](BUIDING_WITH_ANT_JAVACARD.md) |
| ant          | [BUIDING_WITH_ANT](BUIDING_WITH_ANT.md)                   |
| shell        | [BUIDING_WITH_SHELL](BUIDING_WITH_SHELL.md)               |

## Requirements

### Install JDK

1. Download, OpenJDK, Standard Edition 11 from https://jdk.java.net/java-se-ri/11-MR3

2. Then unpack it into your custom directory, such as: D:\SOFTWARE\OpenJDK\jdk-11.0.0.2

3. Add the environment variable: JAVA_HOME = D:\SOFTWARE\OpenJDK\jdk-11.0.0.2

4. Add %JAVA_HOME%\bin and %JAVA_HOME%\jre\bin to PATH

>![Note]Different JCSDK need different JDK, please see the table in [javacard_sdks](https://github.com/ibaibing/javacard_sdks/blob/master/README.FORK.md)
>In fact, as long as jdk8 and jdk11 are sufficient for our requirements to build your applet with all jcsdk.

### Install JavaCard SDK

1. Download JavaCard SDK from https://github.com/ibaibing/javacard_sdks to **your_project/libs/jcsdks**, or

2. Set https://github.com/ibaibing/javacard_sdks as your project subfolder.
```bat
cd your_project
git submodule add https://github.com/ibaibing/javacard_sdks.git libs/jcsdks
or
git submodule add git@github.com:ibaibing/globalplatform-apis.git libs/jcsdks

```
### Install GlobalPlatform API

1. If you need to use GlobalPlatform API, download GlobalPlatform from to **your_project/libs/gpapis**, or 
1. Add [globalplatform-apis](https://github.com/ibaibing/globalplatform-apis.git) as  submodule in your project.
```bat
cd your_project
git submodule add https://github.com/ibaibing/globalplatform-apis.git libs/gpapis
or
git submodule add git@github.com:ibaibing/globalplatform-apis.git libs/gpapis
```

### Install Custom API

1. If you have your own API, copy the folder to **your_project/libs/custom/** directory.

>![Note]
>- gpapi is not used in this example.
>- Custom apis are not yet available.
>- You need to be careful whether to verify the bytecode when using converter.
