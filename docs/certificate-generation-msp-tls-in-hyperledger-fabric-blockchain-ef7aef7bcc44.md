# Hyperledger Fabric 区块链中的证书生成(MSP/ TLS)

> 原文：<https://medium.com/geekculture/certificate-generation-msp-tls-in-hyperledger-fabric-blockchain-ef7aef7bcc44?source=collection_archive---------7----------------------->

![](img/1215482039d1019ad41da2d9cc2019e3.png)

Security Key

# 介绍

在本文中，我们将看到为组织(同行、用户、管理员)和订购者生成 MSP/TLS 证书。将看到如何使用**密码**和**认证机构生成证书。**

# 观众

Hyperledger 结构操作员，管理员。这是一篇为 MSP 和 TLS 生成结构网络证书的实践操作文章。对于初学者，我想建议请通过一些基本概念的超帐织物。

> 注意:本文不推荐给 HLF 初学者。

# 先决条件

1.  基本了解 **Hyperledger Fabric 区块链**。
2.  基本了解 PKI 系统，SSL，TLS 证书。
3.  基本的外壳命令、外壳脚本等。

# 系统需求

1.  卷曲。
2.  Openssl。
3.  Docker —版本 17.06.2 或更高版本
4.  Docker Compose —版本 1.28.5 或更高版本

# 设置步骤

*   下载/安装 Hyperledger Fabric 二进制文件和 CA。
*   使用密码工具生成证书
*   结构 CA 服务器的设置。
*   使用证书颁发机构(CA)生成证书
*   清理干净。
*   总结。

# 下载 Fabric 二进制文件。

首先，下载 HLF 二进制文件。按照以下说明下载。更多信息，可以访问 HLF 官方 [doc 这里](https://hyperledger-fabric.readthedocs.io/en/release-2.2/install.html)

**语法**。

```
curl **-**sSL https:**//**bit**.**ly**/**2ysbOFE **|** bash **-**s **--** **<**fabric_version**>** **<**fabric**-**ca_version**>**
```

*安装:*复制以下命令并粘贴到终端中。

```
curl **-**sSL https:**//**bit**.**ly**/**2ysbOFE **|** bash **-**s **--** 2.2.4 1.5**.**2
```

下载后，可以看到面料样品里面的 ***bin*** 目录。这个目录包含 HLF 二进制文件。如下验证您的二进制版本。为了简单起见，我将所有二进制文件复制到 **/usr/local/bin** 目录中，因此我们不需要导出 bin 路径。

![](img/c6b600eb83307682e518ce7ecdb92a67.png)

> 这里，我使用的是 **fabric 2.2.4** 版本和用于 **CA 1.5.2**

# 隐原

*   `**cryptogen**`是生成 Hyperledger 结构证书和密钥的实用工具。不建议在**生产网络中使用。**
*   Cryptoyamlgen 提供了以下命令
*   **帮助:**为密码工具提供帮助。
*   **生成:**命令用于为已有的网络文件生成证书。
*   **showtemplate:显示模板**命令以 YAML 格式显示网络模板。
*   **版本:**显示密码二进制版本。
*   **扩展:**扩展现有的网络配置文件，并向其中添加额外的组织。

让我们使用 cryptogen showtemplate 命令创建一个网络文件，如下所示。

```
cryptogen showtemplate > mynetwork.yaml
```

![](img/0251160e5a733bd598a0e70bd1d2cbd9.png)

在这里，我们可以看到我们的网络文件已经生成，它包含一个单独的订购者和两个组织配置。

> 注意:您可以随意修改 mynetwork.yaml 文件，并查看生成的证书的变化。我不打算解释网络文件的内容。

## 让我们为该网络配置生成证书。

现在我们使用 *cryptogen generate* 命令为组织、订购者生成证书。

```
cryptogen generate --config=./mynetwork.yaml
```

![](img/d38c71edf9eb9d8198895aa36c1eb405.png)

Orderer certificates

在上图中，我们可以看到**订单 MSP** 生成，**订单管理**生成。

**让我们看看订单管理员的 CA 证书。**

```
openssl x509 -in crypto-config/ordererOrganizations/example.com/users/Admin@example.com/msp/cacerts/ca.example.com-cert.pem -text
```

![](img/30ad5a2b0c7890d6f3dfa53f02d57b90.png)

CA-certificate

**让我们看看订购者管理的 TLS 证书。**

```
openssl x509 -in crypto-config/ordererOrganizations/example.com/users/Admin@example.com/tls/ca.crt -text
```

![](img/b97d6d3067feaa3698650286438f4adf.png)

TLS-certificate

密码工具为我们做了很多工作。它创建了 MSP 和 TLS 证书。

同样，它创建了 Org1 和 Org2、MSP 和 TLS 证书。

![](img/e5b543f949b0b78946fa1d5707cb1931.png)

Org1 AND Org2 MSP, TLS

# 认证机构(CA)

在区块链，每个希望与网络互动的演员都需要一个身份。正是 CA 提供了可验证的数字身份。

*   CA-admin/bootstrap identity 创建身份证书请求。

![](img/bf412ecf30d04f0735e389202bcc9351.png)

Image source: Hyperledger Fabric official doc

*   CA 颁发数字证书，其中包含身份的私钥和公钥。(在 cryptogen 材料中，我们可以看到管理员用户有一些私有和公共密钥问题)
*   类似地，我们必须向 CA 注册所有的对等用户、管理员和用户。(在 cryptogen 的情况下，该工具做了一切，这里我们必须手动生成)

![](img/e734b2575f19a4326c076f892824143d.png)

Image Source: Hyperledger Fabric official doc

*   注册后，我们需要让他们注册以获得数字证书，并为我们的区块链网络构建 MSP、TLS 目录。

**托管/部署 CA**

*   这里，我在本地主机上部署 CA。您可以将其部署在生产/测试虚拟机中。
*   克隆下面的存储库。

```
**git clone** [**https://github.com/cmjagtap/HLF-Certification-Generation**](https://github.com/cmjagtap/HLF-Certification-Generation)
```

*   使用以下命令部署并启动 CA。

![](img/549ae32a55d850fe82b6dfce4c556722.png)

CA-deployment

## 让我们为 org1 生成证书。

我们使用 **fabric-ca-client** 二进制文件与 CA-server 进行交互。

首先，我们必须注册 CA 的引导标识。

*   脚本**1 _ register identities . sh**用于注册 CA-bootstrap 身份并注册 org1 的身份。
*   导出 **FABRIC_CA_CLIENT_HOME，**该环境变量充当引导标识的 MSP。

```
mkdir -p crypto-config-ca/peerOrganizations/org1.example.comexport FABRIC_CA_CLIENT_HOME=${PWD}/crypto-config-ca/peerOrganizations/org1.example.comenrollCAAdmin() {fabric-ca-client enroll -u https://admin:adminpw@localhost:7054 --caname ca.org1.example.com --csr.names C=IN,ST=MH,L=Pune,O=org1,OU=peer  --tls.certfiles ${PWD}/fabric-ca/org1/tls-cert.pem
}
```

*   然后我们使用带有 x509 证书的一些额外参数的 *enroll* 命令。

> 你可以在[官网](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/clientcli.html#fabric-ca-client-s-cli)上探索，fabric-ca-client 命令。

*   登记所有的身份，我们需要组织。
*   在这里，我使用的是单一对等体和管理员身份

```
registerIdentities() {fabric-ca-client register --caname ca.org1.example.com --id.name peer0 --id.secret peer0pw --id.type peer --csr.names C=IN,ST=MH,L=Pune,O=$orgname,OU=peer  --tls.certfiles ${PWD}/fabric-ca/org1/tls-cert.pemfabric-ca-client register --caname ca.org1.example.com --id.name org1admin --id.secret org1adminpw --id.type admin --csr.names C=IN,ST=MH,L=Pune,O=$orgname,OU=admin  --tls.certfiles ${PWD}/fabric-ca/org1/tls-cert.pem}
```

执行脚本后，您将看到下面的输出。

![](img/5c91176c0415afcb6067ac92b7d39d24.png)

Register Identities

我们可以看到我们的身份被注册了。

让我们为 peer0，admin 生成 MSP 和 TLS

*   脚本 **2_generateMSP.sh** 用于生成 peer0 的 MSP、admin 和 peer0 的 TLS。
*   MSP 和 TLS 的生成看起来是一样的。但是，在注册 TLS 时，我们必须添加一个额外的标志— **注册。配置文件 tls** 。

```
fabric-ca-client enroll -u https://peer0:peer0pw@localhost:7054 --caname ca.org1.example.com -M ${PWD}/crypto-config-ca/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/msp  --csr.names C=IN,ST=MH,L=Pune,O=org1,OU=peer  --csr.hosts peer0.org1.example.com --tls.certfiles ${PWD}/fabric-ca/org1/tls-cert.pemfabric-ca-client enroll -u https://peer0:peer0pw@localhost:7054 --caname ca.org1.example.com -M ${PWD}/crypto-config-ca/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls --enrollment.profile tls  --csr.names C=IN,ST=MH,L=Pune,O=org1,OU=peer  --csr.hosts peer0.org1.example.com --csr.hosts localhost --tls.certfiles ${PWD}/fabric-ca/org1/tls-cert.pem
```

*   注册组织 1 管理员

```
fabric-ca-client enroll -u https://org1admin:org1adminpw@localhost:7054 --caname ca.org1.example.com -M ${PWD}/crypto-config-ca/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp  --csr.names C=IN,ST=MH,L=Pune,O=org1,OU=admin  --tls.certfiles ${PWD}/fabric-ca/org1/tls-cert.pem
```

![](img/685c62e9427c353cbe193fb3b4369ed9.png)

MSP generation

![](img/3585194e569b33b83991a2f5302360e9.png)

Peer0 signcert

> 注意:我们必须手动生成 MSP 和 TLS 请求。这里，我对 MSP 和 TLS 都使用单个 CA，您可以托管另一个 CA 来生成 TLS。

*   同样，我们必须为所有组织和订购者生成 MSP、TLS 证书。

# 打扫

*   执行以下命令删除 CA-docker 容器及其卷

```
docker-compose -f ca-org1.yaml down -v
```

*   删除生成的加密配置目录。

# 摘要

我们已经看到，cryptogen 工具如何工作，并帮助我们建立一个更快的测试 HLF 网络。此外，我们还研究了 HLF CA，并完成了为组织生成 MSP 和 TLS 的注册流程。