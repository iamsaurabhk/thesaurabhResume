---
layout: page
subheadline: "LLVM"
title: "The LLVM Undefined Behavior Sanitizer"
teaser: "This post contains the steps and commands to setup, build and run Undefined Behavior Sanitizer on a small C example. We also do a customary investigation into the LLVM IR generated for different UBSan instrumentations and compare them at a high level."
header:
    image_fullwidth: "pages/technical/llvm/llvm-header.png"
image:
    thumb:  posts/technical/llvm/llvm_logo.png
    homepage: posts/technical/llvm/llvm_logo.png
categories:
    - llvm
    - technical
    - clang
tags:
    - clang
    - ubsan
comments: true
show_meta: false
breadcrumb: true
---

## Environment Setup
Build the following LLVM projects in order to obtain the binaries required to run UBSan:
1. clang
2. clang-tools-extra
3. compiler-rt

### LLVM Build Command
<kbd>
cd llvm-project
mkdir build; cd build;
cmake -G 'Ninja' -DLLVM_ENABLE_PROJECTS='clang;clang-tools-extra;compiler-rt' -DCMAKE_INSTALL_PREFIX=./ -DCMAKE_BUILD_TYPE=Debug -DCMAKE_C_COMPILER=$(which gcc) -DCMAKE_CXX_COMPILER=$(which g++) -DCMAKE_C_FLAGS=-fdump-rtl-expand -DLLVM_ENABLE_DOXYGEN=false -DLLVM_OPTIMIZED_TABLEGEN=True -DLLVM_ENABLE_WERROR=OFF ../llvm
ninja all
</kbd>

### Setup
<kbd>
export PATH=<path_to_llvm_repo_dir>/llvm-project/build/bin/:$PATH        
</kbd> 

## Testcase
The following testcase assigns the address 0 to an integer pointer during declaration. Then it dereferences the pointer to assign the value at that location to another integer variable. This would result in a segmentation fault.
![testcase]({{site.urlimg}}posts\technical\llvm\ubsanIntro\testcase.PNG)

## UBSan Minimal Runtime
### Compilation with Instrumentation
<kbd>
clang -o ubsan_min_exe main.c -fsanitize=undefined -fsanitize-minimal-runtime -O1
</kbd>

#### Execution
<kbd>
./ubsan_min_exe
</kbd>

#### Output
<kbd>
	ubsan: type-mismatch
	4202536
</kbd>

## UBSan Full Runtime
### Compilation with Instrumentation
<kbd>
clang -o ubsan_full_exe main.c -fsanitize=undefined -O1
</kbd>

#### Execution
<kbd>
./ubsan_full_exe
</kbd>

#### Output
<kbd>
	main.c:5:13: runtime error: load of null pointer of type 'int'
	SUMMARY: UndefinedBehaviorSanitizer: undefined-behavior main.c:5:13 in 
	589032128
</kbd>

### Compilation with instrumentation & support to print symbolized stack traces for errors
<kbd>
clang -o ubsan_full_exe main.c -fsanitize=undefined -O1 -g -fno-omit-frame-pointer
</kbd>

#### Execution
<kbd>
./ubsan_full_exe
</kbd>

#### Output
<kbd>
main.c:5:13: runtime error: load of null pointer of type 'int'
#0 0x433c5f in main /repo/emkasuu/test/ubsan/example/main.c:5:13
#1 0x7f437c372554 in __libc_start_main (/lib64/libc.so.6+0x22554)
#2 0x402b6a in _start (/repo/emkasuu/test/ubsan/example/ubsan_full_exe+0x402b6a)
</kbd>

## Examine the LLVM IR
### LLVM IR (without UBSan enabled)
<kbd>
clang -o IR main.c -O1 -S -emit-llvm
</kbd>

#### IR
![IR]({{site.urlimg}}posts\technical\llvm\ubsanIntro\IR.PNG)

### UBSan Minimal Runtime
<kbd>
clang -o minIR main.c -fsanitize=undefined -fsanitize-minimal-runtime -O1 -S -emit-llvm
</kbd>

#### IR (Minimal Runtime)
![Minimal Runtime IR]({{site.urlimg}}posts\technical\llvm\ubsanIntro\minIR.PNG)

### UBSan Full Runtime
<kbd>
clang -o fullIR main.c -fsanitize=undefined -O1 -S -emit-llvm
</kbd>

#### IR (Full Runtime)
![Full Runtime IR]({{site.urlimg}}posts\technical\llvm\ubsanIntro\fullIR.PNG)

## Comparing the IR's
### Normal IR vs UBSan Minimal Runtime IR
![Normal IR vs Minimal Runtime IR]({{site.urlimg}}posts\technical\llvm\ubsanIntro\IRminIRDiff.PNG)

### UBSan Minimal Runtime IR vs Full Runtime IR
![Minimal Runtime IR vs Full Runtime IR]({{site.urlimg}}posts\technical\llvm\ubsanIntro\minIRFullIRDiff.PNG)


{% include next-previous-post-in-category %}
