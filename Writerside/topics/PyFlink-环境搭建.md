# PyFlink-环境搭建

记录在 apple silicon 下搭建 apache-flink 1.13.6 的 PyFlink 开发环境的一些命令

## Python 3.7

arm64架构下 conda 试了些channel都没找到 python 3.7 的包

### 使用x64架构 新建 python 3.7 环境
```shell
    CONDA_SUBDIR=osx-64 conda create -n pyflink-env-py37 python=3.7
    conda activate pyflink-env-py37
    conda config --env --set subdir osx-64
```

conda 官方文档说明：
: --subdir, --platform
: Possible choices: emscripten-wasm32, wasi-wasm32, freebsd-64, linux-32, linux-64, linux-aarch64, linux-armv6l, 
linux-armv7l, linux-ppc64, linux-ppc64le, linux-riscv64, linux-s390x, osx-64, osx-arm64, win-32, win-64, win-arm64, 
zos-z
: Use packages built for this platform. The new environment will be configured to remember this choice. 
Should be formatted like 'osx-64', 'linux-32', 'win-64', and so on. Defaults to the current (native) platform.


## PyFlink 1.13.6
PyFlink的安装参考<a href="https://nightlies.apache.org/flink/flink-docs-release-1.13/">flink 1.13.6 官方文档</a>.
```shell
python -m pip install apache-flink==1.13.6
```

## Java JDK 8
java环境用于加载需要的jar文件，用jdk不用jre是因为有些jar不太好找，需要自己构建

参考<a href="https://www.oracle.com/cn/java/technologies/downloads/#java8-mac">官方文档</a>.下载安装JDK8

环境变量配置(此处为.zshrc文件, 末尾追加):
: export JAVA_HOME=/your_path/jdk-1.8.jdk/Contents/Home
: export PATH=$PATH:$JAVA_HOME/bin

判断是否成功:
```shell
    java -version
```
命令行输出java版本信息


## Maven
参考<a href="https://maven.apache.org/install.html">官方文档</a>.下载安装maven

环境变量配置(此处为.zshrc文件, 末尾追加):
: export M2_HOME=/your_path/apache-maven-3.9.6
: export PATH=$PATH:$M2_HOME/bin

判断是否成功:
```shell
    mvn -v
```
命令行输出maven相关信息

maven配置文件(/your_path/apache-maven-3.9.6/conf/setting.xml):
: 可修改生成jar文件目录`<localRepository>/your_path/jars</localRepository>`

构建命令:
```Shell
mvn assembly:assembly
mvn clean install -DskipTests
```
也可以用`mvn clean package`?
