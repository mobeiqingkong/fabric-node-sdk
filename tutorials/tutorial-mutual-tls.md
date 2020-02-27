# fabric-client:如何配置双向 TLS(How to configure mutual TLS)

## 说明

如果将 ORDERER_GENERAL_TLS_CLIENTAUTHREQUIRED 环境变量设置为 true，则 Orderer 已启用 TLS 客户端身份验证。

如果将环境变量 CORE_PEER_TLS_CLIENTAUTHREQUIRED 设置为 true，则 Peer 启用 TLS 客户端身份验证。

### 在启用 TLS 客户端身份验证的情况下连接到 Orderer 或 Peer 设备

检索客户端证书和客户端密钥后，将它们分配给 Client 实例。然后，Client 实例会将物料分配给它创建的每个 Orderer 和 Peer。

例如，以下内容演示了如何首先分配给客户端。然后构建一个 Orderer 和 Peer，然后将启用 TLS 客户端身份验证。假设客户端的 PEM 编码 TLS 密钥和证书分别位于 somepath/tls/client.key 和 somepath/tls/client.crt。

```javascript
let serverCert = fs.readFileSync(
  path.join(__dirname, "somepath/msp/tlscacerts/example.com-cert.pem")
);
let clientKey = fs.readFileSync(
  path.join(__dirname, "somepath/tls/client.key")
);
let clientCert = fs.readFileSync(
  path.join(__dirname, "somepath/tls/client.crt")
);

client.setTlsClientCertAndKey(
  Buffer.from(clientCert).toString(),
  Buffer.from(clientKey).toString()
);

let orderer = client.newOrderer("grpcs://localhost:7050", {
  pem: Buffer.from(serverCert).toString()
});

let peer = client.newPeer("grpcs://localhost:7051", {
  pem: Buffer.from(serverCert).toString()
});
```

---
