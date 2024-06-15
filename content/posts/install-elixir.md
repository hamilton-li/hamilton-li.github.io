---
title: CentOS 7.7安装Erlang和Elixir
date: 2019-12-11 22:01:38
cover: 
categories: Elixir
tags:
- Erlang
- Elixir
- CentOS7
---

> Erlang是一种开源编程语言，用于构建对高可用性有要求的大规模可扩展的软实时系统。它通常用于电信，银行，电子商务，计算机电话和即时消息中。Erlang的runtime系统具有对并发，分发和容错的内置支持。它是在爱立信计算机科学实验室设计的。

<!--more-->

安装之前，先看一下它们的简要说明

## Erlang

**Erlang**是一种开源编程语言，用于构建对高可用性有要求的大规模可扩展的软实时系统。它通常用于电信，银行，电子商务，计算机电话和即时消息中。Erlang的runtime系统具有对并发，分发和容错的内置支持。它是在爱立信计算机科学实验室设计的。

## Elixir

**Elixir**是一种动态的功能语言，旨在用于构建可伸缩和可维护的应用程序。Elixir利用了以运行低延迟，分布式和容错系统而著称的Erlang VM，同时也成功地用于Web开发和嵌入式软件领域。

现在开始在CentOS 7.7 64位服务器中安装Erlang和Elixir。

## 安装前的准备

安装Erlang和Elixir之前，需要安装以下依赖。

```sh
$ yum update
$ yum install epel-release
$ yum install gcc gcc-c++ glibc-devel make ncurses-devel openssl-devel autoconf java-1.8.0-openjdk-devel git wget wxBase.x86_64
```

## 安装Erlang

由于官方存储库中的Erlang版本可能比较旧，这里我将下载并安装最新的Erlang版本。

添加Erlang官方存储库以安装最新的Erlang。

首先到 **[Erlang存储库页面](https://packages.erlang-solutions.com/erlang/)**，根据你使用的发行版下载存储库：

由于现在在CentOS 7.7中安装Erlang，因此我将添加以下存储库。

```sh
$ wget http://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
$ rpm -Uvh erlang-solutions-1.0-1.noarch.rpm
```

使用以下命令更新存储库列表：

```sh
$ yum update
```

使用以下命令安装erlang

```sh
$ yum install erlang
```

现在最新版本的Erlang已安装完成。

## 验证Erlang

运行以下命令以验证是否已安装Erlang。

```sh
$ erl
```

**输出示例：**

```sh
Erlang/OTP 22 [erts-10.6] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:1] [hipe]

Eshell V10.6  (abort with ^G)
1> 
```

如果看到如上的Erlang shell，表示安装成功！要关闭shell，只需按两次 **Ctrl-C**。

## 在Erlang中测试示例“ hello_world”程序
创建一个名为“ hello.erl”的新文件。

```sh
$ vi hello.erl
```

添加以下代码：

```
-module(hello).
-export([hello_world/0]).
hello_world() -> io:fwrite("hello, world\n").
```

 ```:wq``` 保存并关闭文件。

使用以下命令打开Erlang shell：

```sh
$ erl
```

运行以下命令，切记在每个命令的末尾添加英文点：

```sh
$ c(hello).
$ hello:hello_world().
```

**输出示例：**

```sh
[root@localhost ~]# erl
Erlang/OTP 22 [erts-10.6] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:1] [hipe]

Eshell V10.6  (abort with ^G)
1> c(hello).
{ok,hello}
2> hello:hello_world().
hello, world
ok
3> 
```

## 安装Elixir

**安装Elixir之前必须要先安装Erlang！**

Elixir在EPEL存储库中可用，但已经过时了。为了安装最新版本，这里使用源码编译安装。

现在将Elixir从GitHub拉取到本地：

```sh
$ git clone -b v1.9.4 https://github.com/elixir-lang/elixir.git
```

上面的命令会将最新版本拉取到当前工作目录中名为 ```elixir```的文件夹中。
注意：最好指定版本，不然的话你安装完查看版本，可能就是如下所示一样，源码和版本号不匹配

```sh
Erlang/OTP 22 [erts-10.6] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:1] [hipe]

Elixir 1.10.0-dev (ee758f9) (compiled with Erlang/OTP 22)
```

接下来进到 ```elixir``` 目录：

```sh
$ cd elixir/
```

运行以下命令开始编译Elixir：

```sh
$ make clean test
```

**输出示例：**

```sh
[root@localhost elixir]# make clean test
rm -rf ebin
rm -rf lib/*/ebin
rm -rf lib/elixir/src/elixir_parser.erl
make[1]: 进入目录“/root/elixir”
rm -rf lib/*/_build/
rm -rf lib/*/tmp/
rm -rf lib/elixir/test/ebin/
rm -rf lib/mix/test/fixtures/deps_on_git_repo/
rm -rf lib/mix/test/fixtures/git_rebar/
rm -rf lib/mix/test/fixtures/git_repo/
rm -rf lib/mix/test/fixtures/git_sparse_repo/
rm -f erl_crash.dump
make[2]: 进入目录“/root/elixir”
rm -f man/elixir.1
rm -f man/elixir.1.bak
rm -f man/iex.1
rm -f man/iex.1.bak
make[2]: 离开目录“/root/elixir”
make[1]: 离开目录“/root/elixir”
Recompile: src/elixir_utils
Recompile: src/elixir_tokenizer
Recompile: src/elixir_sup
Recompile: src/elixir_rewrite
Recompile: src/elixir_quote
Recompile: src/elixir_parser
Recompile: src/elixir_overridable
Recompile: src/elixir_module
Recompile: src/elixir_map
Recompile: src/elixir_locals
Recompile: src/elixir_lexical
Recompile: src/elixir_interpolation
Recompile: src/elixir_import
Recompile: src/elixir_fn
Recompile: src/elixir_expand
Recompile: src/elixir_errors
Recompile: src/elixir_erl_var
Recompile: src/elixir_erl_try
Recompile: src/elixir_erl_pass
Recompile: src/elixir_erl_for
Recompile: src/elixir_erl_compiler
Recompile: src/elixir_erl_clauses
Recompile: src/elixir_erl
Recompile: src/elixir_env
Recompile: src/elixir_dispatch
Recompile: src/elixir_def
Recompile: src/elixir_config
Recompile: src/elixir_compiler
Recompile: src/elixir_code_server
Recompile: src/elixir_clauses
Recompile: src/elixir_bootstrap
Recompile: src/elixir_bitstring
Recompile: src/elixir_aliases
Recompile: src/elixir
Generated elixir app
==> bootstrap (compile)
Compiled lib/elixir/lib/kernel.ex
Compiled lib/elixir/lib/macro/env.ex
Compiled lib/elixir/lib/keyword.ex
Compiled lib/elixir/lib/module.ex
Compiled lib/elixir/lib/list.ex
Compiled lib/elixir/lib/macro.ex
Compiled lib/elixir/lib/kernel/typespec.ex
Compiled lib/elixir/lib/code.ex
Compiled lib/elixir/lib/code/identifier.ex
Compiled lib/elixir/lib/module/checker.ex
Compiled lib/elixir/lib/module/locals_tracker.ex
Compiled lib/elixir/lib/module/parallel_checker.ex
Compiled lib/elixir/lib/module/types/helpers.ex
Compiled lib/elixir/lib/module/types/infer.ex
Compiled lib/elixir/lib/module/types/expr.ex
Compiled lib/elixir/lib/module/types/pattern.ex
Compiled lib/elixir/lib/module/types.ex
Compiled lib/elixir/lib/kernel/utils.ex
Compiled lib/elixir/lib/exception.ex
Compiled lib/elixir/lib/protocol.ex
Compiled lib/elixir/lib/stream/reducers.ex
Compiled lib/elixir/lib/enum.ex
Compiled lib/elixir/lib/map.ex
Compiled lib/elixir/lib/inspect/algebra.ex
Compiled lib/elixir/lib/inspect.ex
Compiled lib/elixir/lib/access.ex
Compiled lib/elixir/lib/range.ex
Compiled lib/elixir/lib/regex.ex
Compiled lib/elixir/lib/string.ex
Compiled lib/elixir/lib/string/chars.ex
Compiled lib/elixir/lib/io.ex
Compiled lib/elixir/lib/path.ex
Compiled lib/elixir/lib/file.ex
Compiled lib/elixir/lib/system.ex
Compiled lib/elixir/lib/kernel/cli.ex
Compiled lib/elixir/lib/kernel/error_handler.ex
Compiled lib/elixir/lib/kernel/parallel_compiler.ex
Compiled lib/elixir/lib/kernel/lexical_tracker.ex
make[1]: 进入目录“/root/elixir”
==> unicode (compile)
Compiling /root/elixir/lib/elixir/unicode/unicode.ex (it's taking more than 15s)
make[1]: 离开目录“/root/elixir”
==> elixir (compile)
Compiling /root/elixir/lib/elixir/lib/base.ex (it's taking more than 15s)
make[1]: 进入目录“/root/elixir”
Generated elixir app
make[1]: 离开目录“/root/elixir”
==> eex (compile)
==> mix (compile)
Generated mix app
==> ex_unit (compile)
Generated ex_unit app
==> logger (compile)
Generated logger app
Generated eex app
==> iex (compile)
Generated iex app
==> elixir (eunit)
  All 190 tests passed.

==> elixir (ex_unit)
........................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................

Finished in 81.6 seconds (47.2s on load, 34.4s on tests)
1569 doctests, 3374 tests, 0 failures, 7 excluded

Randomized with seed 847643
==> ex_unit (ex_unit)
...............................................................................................................................................................................................................................................................................................................................................................

Finished in 6.2 seconds (3.9s on load, 2.3s on tests)
42 doctests, 309 tests, 0 failures

Randomized with seed 541463
==> logger (ex_unit)
............................................................................................................................

Finished in 2.4 seconds (1.9s on load, 0.4s on tests)
3 doctests, 121 tests, 0 failures

Randomized with seed 211604
==> mix (ex_unit)
Excluding tags: [windows: true]

............................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................

Finished in 158.5 seconds (12.1s on load, 146.4s on tests)
9 doctests, 611 tests, 0 failures

Randomized with seed 334499
==> eex (ex_unit)
...........................................................................................

Finished in 0.5 seconds (0.4s on load, 0.06s on tests)
5 doctests, 86 tests, 0 failures

Randomized with seed 599118
==> iex (ex_unit)
.......................................................................................................................................................................................................................................

Finished in 8.5 seconds (2.1s on load, 6.3s on tests)
231 tests, 0 failures

Randomized with seed 207545
[root@localhost elixir]# 
```

如果测试通过，则表示编译安装完成。

接下来将Elixir的bin路径添加到PATH环境变量中，否则Elixir将无法正常工作。

方法一（暂时生效）
现在运行以下命令：

```sh
$ export PATH="\$PATH:/root/elixir/bin"
```

在这里，我把Elixir安装在了 ```/root/elixir/```  。你必须将此路径替换为你实际的Elixir安装路径。


方法二（只对当前登陆用户生效，永久生效）

```sh
$ vi \~/.bash_profile
```

默认如下：

```
PATH=$PATH:$HOME/bin
```

添加后：

```
PATH=$PATH:$HOME/bin:$PATH:/root/elixir/bin
```

接下来执行 ```source ~/.bash_profile``` 使其立即生效或者 ```reboot``` 重启生效


方法三（对所有系统用户生效，永久生效）

```sh
$ vi /etc/profile
```

在文件最后添加以下两行：

```
PATH="$PATH:/root/elixir/bin"
export PATH
```

接下来执行 ```source /etc/profile``` 使其立即生效或者 ```reboot``` 重启生效



## 验证Elixir

要验证是否已安装Elixir，请运行：

```
$ iex
```

**输出示例：**

```
Erlang/OTP 22 [erts-10.6] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:1] [hipe]

Interactive Elixir (1.9.4) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> 
```

如果你看到Elixir shell，则表示安装成功！

同样，要关闭Elixir shell，只需按两次**Ctrl-C**。

查看Elixir版本：

```
$ elixir --version
```

**输出示例：**

```sh
Erlang/OTP 22 [erts-10.6] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:1] [hipe]

Elixir 1.10.0-dev (ee758f9) (compiled with Erlang/OTP 22)

```

现在CentOS 7.7服务器中已成功安装并设置可工作的Erlang和Elixir开发环境，所有安装已完成！


## 简便方法

直接使用`yum`或者`apt`来安装，详情查看[Erlang Solutions 软件仓库镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/erlang-solutions/)


**参考链接：**

*   **[Unixmen](https://www.unixmen.com)**
*   **[Erlang网站](https://www.erlang.org)**
*   **[Erlang文档](https://www.erlang.org/docs)**
*   **[Elixir网站](https://elixir-lang.org)**
*   **[Elixir文档](https://elixir-lang.org/docs.html)**
*   **[安装Erlang](https://docs.riak.com/riak/kv/2.2.3/setup/installing/source/erlang/)**


