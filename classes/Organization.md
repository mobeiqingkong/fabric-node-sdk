# Organization

## 说明

Organization 类代表目标区块链网络中的组织。

### new Organization(name, mspid)

构造组织对象

- 参数

| 名称  | 类型   | 描述           |
| ----- | ------ | -------------- |
| name  | string | 该组织的名称   |
| mspid | string | 该组织的 mspid |

返回结果

- 组织实例。

  - 类型

    [Organization](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Organization.html)

### Methods

#### addCertificateAuthority(certificateAuthority)

向该组织添加一个证书颁发机构( [CertificateAuthority](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CertificateAuthority.html))

- 参数

| 名称                 | 类型                                                                                                        | 描述                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| certificateAuthority | [CertificateAuthority](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CertificateAuthority.html) | 要添加到此组织的 CertificateAuthorities 列表的 CertificateAuthority 实例 |

#### addPeer(peer)

向该组织添加[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)

- 参数

| 名称 | 类型                                                                        | 描述                                   |
| ---- | --------------------------------------------------------------------------- | -------------------------------------- |
| peer | [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) | 要添加到此组织的对等实体列表的对等实例 |

#### getAdminCert()

获取此组织的 PEM 格式的管理员签名证书。

#### getAdminPrivateKey()

获取此组织的 PEM 格式的管理员私钥。

#### getCertificateAuthorities()

获取此 CertificateAuthorities 的列表[CertificateAuthority](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CertificateAuthority.html)

返回结果

- CertificateAuthority 对象的数组。

  - 类型

    [CertificateAuthority](https://hyperledger.github.io/fabric-sdk-node/release-1.4/CertificateAuthority.html)

#### getMspid()

获取此组织的 MSPID

返回结果

- 该组织的 MSPID。

  - 类型

    string

#### getName()

获取该组织的名称

返回结果

- 该组织的名称。

  - 类型

    string

#### getPeers()

[Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html) 对象数组

返回结果

- 该组织的名称。

  - 类型

    [Peer](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Peer.html)

#### setAdminCert()

为该组织设置 PEM 格式的管理员签名证书。

#### setAdminPrivateKey()

设置此组织的 PEM 格式的管理员私钥。

#### toString()

返回此对象的可打印表示形式

---
