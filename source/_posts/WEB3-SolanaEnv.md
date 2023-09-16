---
title: Solana环境部署
cover: ''
date: 2023-09-14 16:51:28
updated: 2023-09-14 16:51:28
tags: Web3
categories: Web3
---

## 安装

### Cargo安装

### 二进制安装




## 问题处理

1. Linux出现不支持AVX2指令的解决办法

    ````
    [2023-09-14T08:41:31.273811400Z INFO  solana_perf] AVX detected
    [2023-09-14T08:41:31.273825030Z ERROR solana_perf] Incompatible CPU detected: missing AVX2 support. Please build from source on the target
    Aborted (core dumped)
    ```
    - Check Your CPU: First, determine if your CPU has support for AVX2. This can be done using:
    For Linux:
    ```
    grep avx2 /proc/cpuinfo
    ```
    If you get output, your CPU supports AVX2. If you don't get any output, it doesn't.

    For macOS:
    ```
    sysctl -a | grep machdep.cpu.features
    ```
    Look for AVX2 in the output.

    - Building From Source
    If your CPU does not support AVX2, the recommendation, as given by the error message, is to build Solana from source on the target machine. This way, the build process will optimize for the specific features that your CPU supports.
    Here's a basic guide to building Solana from source:
    Clone the Solana repository:
    ```
    git clone https://github.com/solana-labs/solana.git
    cd solana
    ```

    Build Solana:

    ```
    cargo build --release
    ```
    Once built, the Solana binaries will be available under the target/release directory.

    Upgrade or Change Your Hardware: If you're running on a cloud provider, consider changing your instance type to one that has AVX2 support. If you're on physical hardware and you require AVX2 support frequently, consider upgrading your CPU to a more recent one that supports AVX2.

    - Use Docker: If Solana provides a Docker image, you might be able to run Solana in a container. Docker containers can abstract away some of the hardware dependencies, although performance might be impacted.

    Remember that not having AVX2 support will not prevent Solana from running when built from source; it just means you won't be able to utilize some of the optimizations that the pre-built binaries might have.


2. 


