# 通过 PKCS#11 接口测试硬件安全模块 (Testing for Hardware Security Module via PKCS#11 interface)

## 通过 PKCS#11 接口测试硬件安全模块(Testing for Hardware Security Module via PKCS#11 interface)

本教程说明了从 1.4 版本开始，通过用于 Node.js 的 Hyperledger Fabric SDK 通过 PKCS#11 接口安装，配置和测试硬件安全模块 SoftHSM 的不同方法。

有关更多信息，请参阅[SoftHSM](https://www.opendnssec.org/softhsm/)。

## 概述

该 SDK 支持 PKCS#11 接口，以便允许应用程序使用 HSM 设备进行密钥管理。

## 安装

为了运行测试，请安装 PKCS#11 接口的软件仿真器。

### 使用软件包管理器为您的主机系统安装

- Ubuntu: apt-get install softhsm2
- macOS: brew install softhsm
- Windows: 不支持.

### 从源码安装

1. install openssl 1.0.0+ or botan 1.10.0+
2. 从 [https://dist.opendnssec.org/source/softhsm-2.2.0.tar.gz](https://hyperledger.github.io/fabric-sdk-node/release-1.4///dist.opendnssec.org/source/softhsm-2.2.0.tar.gz) 下载源码
3. tar -xvf softhsm-2.2.0.tar.gz
4. cd softhsm-2.2.0
5. ./configure --disable-gost (将需要其他库，请将其关闭，除非您需要俄罗斯市场的 Gost 算法支持)
6. make
7. sudo make install

### 设置环境变量 "SOFTHSM2_CONF" to "./test/fixtures/softhsm2.conf"

```shell
export SOFTHSM2_CONF="./test/fixtures/softhsm2.conf"
```

### 创建令牌以将密钥存储在插槽 0 中

```shell
softhsm2-util --init-token --slot 0 --label "My token 1"
```

然后，系统将提示您两个 PIN:可用于重新初始化令牌的 SO(安全员)PIN，以及应用程序用于访问令牌以生成和检索密钥的用户 PIN。

## 测试

单元测试已经在 SoftHSM2 上进行了尝试，并假定插槽为'0'，并且用户 PIN 为 98765432。如果您的配置不同，请使用以下环境变量来传递值:

- PKCS11_LIB - SoftHSM2 库的路径，如果未指定，则测试用例将搜索一系列常用的安装位置
- PKCS11_PIN
- PKCS11_SLOT

要关闭这些测试，应指定 npm 脚本 testNoHSM。

---
