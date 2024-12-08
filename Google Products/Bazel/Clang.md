# Install 

Let's work with the latest version (clang-19)

```shell
apt update && apt install -y wget lsb-release software-properties-common gnupg
wget https://apt.llvm.org/llvm.sh
chmod u+x llvm.sh
sudo ./llvm.sh 19
```

Let's work with `clang-tidy-19` 

```shell
apt update && apt install -y clang-tidy-19
```
# Specify the compiler (clang) to Bazel

```shell
CC=/usr/lib/llvm-19/bin/clang CXX=/usr/lib/llvm-19/bin/clang++ bazel build <target>
```

Generate the compilation command database

```shell
CC=/usr/lib/llvm-19/bin/clang CXX=/usr/lib/llvm-19/bin/clang++ bazel run @hedron_compile_commands//:refresh_all
```

# Use the linter

```shell
clang-tidy-19 -p path/to/bazel-gnc <files>
```
