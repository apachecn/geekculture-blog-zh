# 在 Ansible 中使用机密的三种方式

> 原文：<https://medium.com/geekculture/three-ways-to-use-secrets-in-ansible-922ae18df847?source=collection_archive---------2----------------------->

## 在 Ansible 中使用机密

## GPG，安西布尔金库还是哈希科尔金库？解决同一个问题的三种方案。

![](img/20f6e16cd781604a70b2de4329892701.png)

Photo by [Jan Antonin Kolar](https://unsplash.com/@jankolar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

要用 Ansible 存储秘密、凭证、证书，有几种可能性。你可以使用 Ansible Vault 来加密你所有的敏感信息。Ansible 提供的机制是内置的，可以满足各种用例。此外，您可以使用多个保管库，并将它们与您的库存组织在一起。

这样做有助于您组织和约束与库存相关的秘密。我过去用它来存储我在 GitHub 中的不同剧本和角色。我们可以讨论在 GitHub 上存储加密文件是好是坏，但至少这比在这样的平台上存储带有凭证的明文文件要好得多😅。

你也可以用 ansible var 文件(yaml 文件)构建你自己的解决方案，你可以用 GPG 这样的解决方案“简单地”加密这些文件。这很简单，不太难实现。当你没有成百上千的秘密时，这个解决方案就足够了。这是我们在我的工作中实现的。

最后，您可以找到其他解决方案，比如 Ansible Vault，它们要完整得多，可以处理更大的用例。正如已经解释过的，对于我的实验室，我的目标是探索尽可能多的技术。我想使用 HashiCorp Vault，因为它似乎是一个完整的解决方案，即使是社区版。

# GPG 解决方案

对于那些对这种可能性感兴趣的人，我将试着简要介绍一下 GPG 的解决方案。在 Mac OS X 上，我安装了以下工具:

*   [gpg2](https://gnupg.org/) (管理密钥和加密/解密文件的核心工具)
*   [GPGSuite](https://gpgtools.org/) (用于管理密钥并轻松与 Apple 钥匙链集成的实用程序)

## 装置

为了安装工具，我简单地使用了`homebrew`。

```
# Start the generation of the key. I keep the RSA & RSA default,
# 4096 for the key length and no expiration (update to your needs).
# Then fill a proper name and an email address.**❯ brew cask install gpg-suite-no-mail**
```

## GPG 关键一代

下一步是生成一个 GPG 密钥。GPGSuite 工具会提示您输入密码两次。为了简单起见，我选择不输入密码。拥有一个密码取决于你和你的安全需求。

```
*# Start the generation of the key. I keep the RSA & RSA default,
# 4096 for the key length and no expiration (feel free to update
# to your needs).
# Then, you have to fill a real name and an email address.* **❯ gpg2 --full-generate-key**gpg (GnuPG/MacGPG2) 2.2.20; Copyright (C) 2020 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) yGnuPG needs to construct a user ID to identify your key.Real name: <realName>
Email address: [<emailAddress>](mailto:japan@prevole.ch)
Comment:
You selected this USER-ID:
    "<realName> <[ema](mailto:japan@prevole.ch)ilAddress>"Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit?We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key <ID> marked as ultimately trusted
gpg: revocation certificate stored as '/Users/<username>/.gnupg/openpgp-revocs.d/<anotherId>.rev'
public and secret key created and signed.pub   rsa4096 2020-10-05 [SC]
      <anotherId>
uid   <realName> <emailAddress>
sub   rsa4096 2020-10-05 [E]
```

要检查您的密钥是否已经生成，您可以列出它们。

```
*# You can have other keys, I only kept in the output the one 
# just created*
**❯ gpg2 --list-keys**gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   4  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 4u
/Users/<username>/.gnupg/pubring.kbx
-----------------------------------------
pub   rsa4096 2020-10-05 [SC]
      <anotherId>
uid           [ultimate] <realName> <[emailAddress](mailto:japan@prevole.ch)>
sub   rsa4096 2020-10-05 [E]
```

## 用 GPG 密钥加密

要使用您的密钥加密文件，您可以简单地使用以下命令。

```
# We trust our generated key. We identify the key with the email
# address and we give the file to cipher
**❯ gpg --always-trust --yes --recipient "**[**<emailAddress>**](mailto:japan@prevole.ch)**" --encrypt <pathToTheFileToEncrypt>**
```

如果加密操作成功，控制台中没有特殊输出。该命令产生一个`gpg`文件。

## 用 GPG 密钥解密

要检索加密文件的明文内容，我们只需执行一个相反的命令。

```
# To decrypt, we need to specify the input file, which is the
# encrypted file and the output file. GPG will automatically pick
# the right key to decrypt. The encrypted file contains the info.
**❯ gpg --output <pathToDecryptedFile> --yes --decrypt <pathToCryptedFile>**gpg: encrypted with 4096-bit RSA key, ID <ID>, created 2020-10-05
      "<realName> <[emailAddress](mailto:japan@prevole.ch)>"
```

就是这样。用 GPG 加密和解密的操作非常容易。我们已经准备好了处理秘密使用管理的所有可行任务。

## 安西布尔和 GPG 在一起

是时候把所有的碎片放在一起，写一个剧本了。剧本将包含解密部分和一些最后的清理工作。

Ansible Playbook with Encrypted Var File

**①** 我们解密包含秘密的 var 文件。

**②** 然后，很容易将 var 文件作为普通 var 文件包含在剧本中。

我们运行需要一些秘密(密码、令牌、证书等)的任务

**④** 最后，我们只需删除 clear 文件。

## GPG 摘要

这种方法的主要缺点仍然是，如果剧本由于错误而中断，文件可能不会被清除。您可以使用不同的策略来缓解这个问题，比如通知/处理程序。

在我的工作中，我继承了一个现有的设置，其中 var 文件加密/解密过程是在任何行动手册之外完成的。var 文件被加密并存储在内部 GitHub 企业实例的 git 存储库中。然后，解密后的文件存储在`/etc/ansible/group_vars`中，就像一个用于`all`库存的全局 var 文件。

最后，如果不考虑安全性，我不推荐这些方法。如果你不得不管理的秘密不是那么敏感或者对于家庭实验室来说是非常好的。对于真实的产品，我会增加一点安全性，比如在 GPG 密钥中加入一个密码。此外，我会尽可能避免使用明文。

# 可旋转拱顶

如前所述，Ansible 本身提供了存储敏感数据的解决方案。我在 Ansible 的早期使用过这种机制。这也是一个伟大的解决方案，安全地存储您的敏感数据。

Ansible 文档警告您使用 Ansible Vault 的风险。

> Ansible Vault 加密仅保护“静态数据”。一旦内容被解密(“使用中的数据”)，play 和插件作者有责任避免任何秘密泄露，有关隐藏输出的详细信息，请参见 [no_log](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#keep-secret-data) ，有关与 Ansible Vault 一起使用的编辑器的安全注意事项，请参见[保护编辑器的步骤](https://docs.ansible.com/ansible/latest/user_guide/vault.html#vault-securing-editor)。— [可提供的文件](https://docs.ansible.com/ansible/latest/user_guide/vault.html)

## 使用 Ansible Vault 加密

使用保险库加密数据有两种可能性。一种是直接在行动手册中加密敏感信息。一次只能加密一个变量。第二种可能性是加密整个文件。我加密了一个完整的文件，就像我们之前和 GPG 做的一样。

Ansible Vault 的优点是可以直接加密 var 文件。之后，当我们运行包含或引用 var 文件的剧本时，Ansible 会自动提示您解密。

文件加密就像下面的命令一样简单。

```
*# Encrypt an inventory var file*
**❯ ansible-vault encrypt all.yml**New Vault password: 
Confirm New Vault password: 
Encryption successful
```

要查看加密文件的内容，只需运行以下命令。

```
**❯ ansible-vault view all.yml** Vault password:
```

要编辑加密文件，可以使用类似的命令。

```
**❯ ansible-vault edit all.yml**Vault password:
```

## **用安全金库解密**

vault 文件的解密也很容易。运行以下命令。

```
*# This command will decrypt the var file in place.*
**❯ ansible-vault decrypt all.yml**Vault password: 
Decryption successful
```

## 可转换和不可转换的拱顶在一起

是时候把所有的碎片放在一起了。我们知道如何加密一个变量文件。我们只需要把所有东西放好就可以使用加密文件了。

我用剧本和清单创建了一个简单的项目。我将加密的变量文件放在我的清单的`group_vars`中。

Ansible Vault — Demo Project

**①**var 文件用 Ansible Vault 加密。

Ansible Vault — Var File Encrypted

**①**var 文件只包含加密的内容和像算法一样解密文件的数据。

②明确的内容在那里作为后面的参考。变量`test`被定义。

宿主文件只是一个清单文件，用来链接`group_vars/test`变量。

Ansible Vault — Host File

最后，我们有了行动手册。剧本只包含一个任务，即在 var 文件中打印变量。

Ansible Vault — Demo Playbook

**①** 它将在调试消息任务中打印出加密文件中的变量`test`。

为了运行剧本并查看加密变量的内容，我们运行以下命令。

```
*# We have to specify the inventory file. We also need to specify 
# to be asked for the vault password* **❯ ansible-playbook --ask-vault-password -i inventories/test playbooks/test.yml**Vault password:PLAY [test] *********************************************************************# ①*
TASK [Debug] ********************************************************************
ok: [0.0.0.0] => 
  test: my super secret passwordPLAY RECAP ********************************************************************
0.0.0.0                    : ok=1    changed=0    unreachable=0
                             failed=0    skipped=0    rescued=0
                             ignored=0
```

**①** 调试任务从加密的 var 文件中输出密码。

如果你忘记添加选项`--ask-vault-password`，你会得到以下错误。

```
**❯ ansible-playbook -i inventories/test playbooks/test.yml**PLAY [test] ********************************************************************ERROR! Attempting to decrypt but no vault secrets found
```

## 易变保险库摘要

这种方法非常适合处理秘密和密码。这是一个易于使用的金库。只是有一点麻烦，每次我们调用剧本时都要被迫输入密码。

使用`--vault-password-file`至少有一种可能性，即使用包含明文密码的文件。使用这种技术需要有其他安全机制来避免不允许的人看到密码。

这种机制的主要优点是 Ansible 中的内置特性。你不必安装和设置额外的东西。对我来说，最大的缺点是必须输入密码或者给出一个密码文件作为参数。

还有一种方法可以用 Ansible 设置默认密码或密码文件。事实上，Ansible 文档页面包含了所有的设置信息。

[](https://docs.ansible.com/ansible/latest/user_guide/vault.html#configuring-defaults-for-using-encrypted-content) [## 使用 Ansible Vault 加密内容- Ansible 文档

### Ansible Vault 加密变量和文件，因此您可以保护敏感内容，如密码或密钥，而不是…

docs.ansible.com](https://docs.ansible.com/ansible/latest/user_guide/vault.html#configuring-defaults-for-using-encrypted-content) 

# 哈希公司金库

我一直是像流浪者这样的产品的粉丝。我也在一些任务中使用 Terraform。我听说跳马已经很久了。这些产品来自哈希公司。他们建立了一套产品来管理云/混合基础架构。

HashiCorp Vault 的承诺是管理秘密和更多，但都与安全存储有关。这个解决方案提供了许多可能性，我鼓励你去看看。在本文中，我将只展示在 Vault 中使用键-值对存储的简单秘密管理。

## 装置

我们需要安装哈希公司。在麦克·OS X 的领导下，我们可以用`homebrew`来做到这一点。

```
*# Install the cli tools and the server*
**❯ brew install hashicorp/tap/vault**
```

**‼️** **免责声明:**以下安装和设置步骤是您可以在 HashiCorp Vault 文档中找到的内容的精简版本。我强烈建议遵循他们的文档，而不是我的。在活动日志中查看我的安装过程😃。

安装完成后，您将获得服务器和 cli 工具。HashiCorp Vault 的文档提供了使用 Vault 的教程。这是一个重要的起点，但是服务器将以`dev`模式运行，将秘密存储在内存中。停止服务器意味着丢失你存储的秘密。

[](https://learn.hashicorp.com/tutorials/vault/getting-started-intro?in=vault/getting-started) [## 跳马简介

### Vault 附带各种称为机密引擎的可插拔组件和身份验证方法，允许您…

learn.hashicorp.com](https://learn.hashicorp.com/tutorials/vault/getting-started-intro?in=vault/getting-started) 

完成后，我选择了部署保险库的部分。在同一个教程中，您有一个专门的部分来为“real”部署一个 Vault。

我们需要创建一个文件夹来存储 Vault 数据。

```
*# Create the Vault working directory*
**❯ mkdir -p vault/data**
```

在`vault`目录下，创建下面的配置文件`config.hcl`。

Vault Configuration File

我们准备启动服务器。我们在前台运行服务器，然后命令行提示符被运行的服务器阻塞。

```
*# Start the server with the configuration file specified*
**❯ vault server -config=config.hcl**WARNING! mlock is not supported on this system! An mlockall(2)-like syscall to
prevent memory from being swapped to disk is not supported on this system. For
better security, only run Vault on systems where this call is supported. If
you are running Vault in a Docker container, provide the IPC_LOCK cap to the
container.
==> Vault server configuration:Api Address: http://127.0.0.1:8200
                     Cgo: disabled
         Cluster Address: https://127.0.0.1:8201
              Go Version: go1.14.7
              Listener 1: tcp (addr: "127.0.0.1:8200", cluster address: "127.0.0.1:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
               Log Level: info
                   Mlock: supported: false, enabled: false
           Recovery Mode: false
                 Storage: raft (HA available)
                 Version: Vault v1.5.4
             Version Sha: 1a730771ec70149293efe91e1d283b10d255c6d1==> Vault server started! Log data will stream in below:
```

警告对我的概念验证并不重要。我不会让它运行太久的。麦克·OS X 似乎并不完全支持哈希公司的保险库。它可以使用，但你会失去安全性。文档解释了此警告的原因。

[](https://learn.hashicorp.com/tutorials/vault/getting-started-deploy?in=vault/getting-started#starting-the-server) [## 部署保管库

### 到目前为止，您已经与“dev”服务器进行了交互，它会自动解封 Vault，设置内存存储…

learn.hashicorp.com](https://learn.hashicorp.com/tutorials/vault/getting-started-deploy?in=vault/getting-started#starting-the-server) 

我们需要初始化保险库。要连接到 Vault 服务器，我们需要配置一个环境变量。Vault cli 将使用它来获取目标服务器。

```
*# Run this command or add it to your bash/zsh profile*
**❯ export VAULT_ADDR=**[**http://127.0.0.1:8200**](http://127.0.0.1:8200)
```

然后我们可以初始化保险库。

⚠️ **警告**:在下面的输出中，你会看到我的本地演示解封密钥和根令牌。在我的演示之后，我破坏了我的服务器，密钥和令牌不再有效。**千万不要在网上粘贴敏感信息**。

```
*# The initialisation will create the unseal keys and the root token.*
**❯ vault operator init**Unseal Key 1: 43w3kecmIM62Zlmg3OzEkR3vcFWavPXOSo9R5hSorNMG
Unseal Key 2: sfdRpD2vYSeWbDD+pd4RT+2cuh+g7s4blUzhzm/FKoxZ
Unseal Key 3: 026gP9barUPvsOH8AL7p/UMYqb2XLYT0m562tItcdjmf
Unseal Key 4: XCFT60P4zWq6tFbSSb1RRjsjumt/iZGQ32Ei4Wni9q6m
Unseal Key 5: uM2YN7oAMXDi9AgOqJbBndvamP9SZIHfWE5FEZjK1RlUInitial Root Token: s.KQ6acXT29MpSHzMqTAw4c51OVault initialized with 5 key shares and a key threshold of 3\. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 3 of these keys to unseal it
before it can start servicing requests.Vault does not store the generated master key. Without at least 3 key to
reconstruct the master key, Vault will remain permanently sealed!It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
```

## 验证安装

是时候开启金库了。要打开保险库，我们需要 5 把钥匙中的 3 把。这背后的想法是在 5 个人之间共享密钥。需要 5 个人中的 3 个人才能打开保险库。避免独一无二的人启封金库，这是一个有趣的概念。

```
*# We run three times the command to unseal the Vault*
**❯ vault operator unseal**Unseal Key (will be hidden): <key 1>
Key                Value
---                -----
Seal Type          shamir
Initialized        true
Sealed             true
Total Shares       5
Threshold          3
Unseal Progress    1/3
Unseal Nonce       adabe581-5284-bf2d-7aca-2952338eefd8
Version            1.5.4
HA Enabled         true**❯ vault operator unseal**Unseal Key (will be hidden): <key 2>
Key                Value
---                -----
Seal Type          shamir
Initialized        true
Sealed             true
Total Shares       5
Threshold          3
Unseal Progress    2/3
Unseal Nonce       adabe581-5284-bf2d-7aca-2952338eefd8
Version            1.5.4
HA Enabled         true**❯ vault operator unseal**
Unseal Key (will be hidden): <key 3>
Key                     Value
---                     -----
Seal Type               shamir
Initialized             true
Sealed                  false
Total Shares            5
Threshold               3
Version                 1.5.4
Cluster Name            vault-cluster-23bf4fd7
Cluster ID              d4fd02c6-8935-8f50-4a6d-d445444249a8
HA Enabled              true
HA Cluster              n/a
HA Mode                 standby
Active Node Address     <none>
Raft Committed Index    24
Raft Applied Index      24
```

如果您尝试在存储库密封时登录，将会出现以下错误。

```
*# Login attempt on a sealed Vault*
**❯ vault login <token>**Error authenticating: error looking up token: Error making API request.URL: GET [http://127.0.0.1:8200/v1/auth/token/lookup-self](http://127.0.0.1:8200/v1/auth/token/lookup-self)
Code: 503\. Errors:* Vault is sealed
```

登录以解封保管库

```
*# Login on an unsealed Vault*
**❯ vault login <token>**Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.Key                  Value
---                  -----
token                <token>
token_accessor       <tokenAccessor>
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```

我们准备好创造我们的秘密。我们启用一个秘密引擎来存储凭证。

```
*# Enable key value engine on the path ansible*
**❯ vault secrets enable -path=ansible kv**Success! Enabled the kv secrets engine at: ansible/
```

我们准备好储存我们的第一个秘密。让我们开始吧。

```
*# We write the "password" with identifier "test" into a folder "demo"*
**❯ vault kv put ansible/demo test=password**Success! Data written to: ansible/demo
```

要读取数据，使用`get`命令。

```
*# Read command*
**❯ vault kv get  ansible/demo**==== Data ====
Key     Value
---     -----
test    password
```

## HashiCorp 金库和 Ansible 一起

将 HashiCorp Vault 集成到 Ansible 的一切都已就绪。有两种集成方式:

*   在 Ansible 内部使用 HashiCorp Vault
*   从 Ansible 管理 HashiCorp Vault。

我们需要使用从哈希公司保险库取回的秘密。`community.general`集合中的官方查找插件正是我们消费秘密所需要的。

使用 HashiCorp Vault 的演示项目的结构与我用于 Ansible Vault 的结构大致相同。我刚刚扔掉了`group_vars`文件夹。

HashiCorp Ansible Demo Project Structure

演示 HashiCorp Vault 用法的剧本非常简单。它只包含一个带有查找的调试任务。

**①** 查找任务使用`hashi_vault`插件。我们指定秘密的路径、认证令牌和要连接的 URL。

查找插件的文档显示了检索机密的可用选项。还可以使用环境变量来设置身份验证令牌和 URL。它将避免在每个查找语句中指定。

[](https://docs.ansible.com/ansible/latest/collections/community/general/hashi_vault_lookup.html) [## community.general.hashi_vault -从 HashiCorp 的 vault - Ansible 文档中检索机密

### 注意 env:AWS_SECRET_ACCESS_KEY 对应于访问密钥的 AWS 秘密密钥。别名:aws_secret_access_key…

docs.ansible.com](https://docs.ansible.com/ansible/latest/collections/community/general/hashi_vault_lookup.html) 

然后，我们可以运行剧本，看看集成是如何工作的。

```
*# Run the playbook with the inventory specified*
**❯ ansible-playbook -i inventories/test playbooks/test.yml**PLAY [test] ********************************************************************# **①**
TASK [Debug] ********************************************************************
ok: [0.0.0.0] => 
  msg:
    test: passwordPLAY RECAP ********************************************************************
0.0.0.0                    : ok=1    changed=0    unreachable=0   
                             failed=0    skipped=0    rescued=0   
                             ignored=0
```

**①** 调试任务的输出是结构化数据。这意味着我们取回了一个物体。事实上，存储在 Vault 中的数据可以是多键/值对。

要从对象中获取特定的数据，可以像这样使用查找插件。添加密钥`:test`将只获得我们正在寻找的密码的值。

```
{{ lookup('hashi_vault', 'ansible/demo:test ...) }}
```

输出将会是。

```
TASK [Debug] ********************************************************************
ok: [0.0.0.0] => 
  msg: password
```

## 哈希公司保险库摘要

我对哈希公司的金库没什么经验。对于简单的用例，它似乎很容易使用和管理。与 Ansible 的集成并不比使用 Ansible Vault 更复杂。这两种解决方案配合得很好。

在我看来，HashiCorp Vault 集成了许多管理秘密(短暂秘密)的解决方案。这比那里介绍的其他两种解决方案要先进得多。

# 结论

在本文中，我们探索了 3 种不同的解决方案来存储机密并在 Ansible 中使用它们。

*   基于 GPG 的定制解决方案
*   Ansible Vault 解决方案
*   哈希公司金库解决方案

对我来说，没有最好的解决方案。这始终是一个需要解决的权衡和用例的问题。例如，对于您完全信任运行 Ansible 的计算机的环境，GPG 解决方案是可以接受的。它将允许您使用密钥管理秘密，而不必在每次运行剧本时输入密码。

Ansible Vault 的解决方案也很简单，最重要的是嵌入在 Ansible 中。它不需要任何额外的设置。它可以在任何现成的计算机上运行。您也可以将 Ansible Vault 文件存储在库存的存储库中。

最后，HashiCorp Vault 需要更多的设置(尤其是生产设置)来安装和维护。此外，它还带来了最先进的功能和可能性。它与 Ansible 配合得很好。

最终，由您来选择最符合您需求的解决方案。在我的情况下，我会继续使用 HashiCorp Vault 来发现关于这个工具的更多信息。

# 参考

GPG 和 GPG 套件工具。

[](https://gnupg.org/) [## GNU 隐私卫士

### GnuPG 是由 RFC4880(也称为 PGP)定义的 OpenPGP 标准的完整免费实现。GnuPG…

gnupg.org](https://gnupg.org/) [](https://gpgtools.org/) [## GPG 套房

### GPG 套件包括 30 天的 GPG 邮件试用。要继续使用 GPG 邮件，请购买支持计划使用 GPG…

gpgtools.org](https://gpgtools.org/) 

Ansible Vault 文档

[](https://docs.ansible.com/ansible/latest/user_guide/vault.html) [## 使用 Ansible Vault 加密内容- Ansible 文档

### Ansible Vault 加密变量和文件，因此您可以保护敏感内容，如密码或密钥，而不是…

docs.ansible.com](https://docs.ansible.com/ansible/latest/user_guide/vault.html) 

哈希公司金库

[](https://www.vaultproject.io/) [## 哈希公司跳马

### Vault 保护、存储并严格控制对令牌、密码、证书、API 密钥和其他机密的访问…

www.vaultproject.io](https://www.vaultproject.io/)