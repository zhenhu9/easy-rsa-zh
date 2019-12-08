Easy-RSA 3 快速入门自述文件
============================

Easy-RSA 版本 3 使用快速入门指南。可以通过运行 ./easyrsa -h 找到有关用法和特定命令的详细帮助。 其它文档可以在 doc/ 目录中找到。

如果要从 Easy-RSA 2.x 系列进行升级，则在 doc/ 路径下也有可用的升级说明。

设置并签署第一个请求
-----------------------------------

以下是以启动新的 PKI 并签署您的第一个实体证书的快速入门：

1. 选择一个系统作为您的 CA，并创建一个新的 PKI 和 CA：

        ./easyrsa init-pki
        ./easyrsa build-ca

2. 在请求证书的系统上，初始化其相应的 PKI，并生成密钥对/请求。 请注意，当_只_在单独的系统（或至少一个单独的 PKI 目录）上执行时，才使用 init-pki。这是推荐的步骤。 如果您不使用此推荐的步骤，请跳过下一个 import-req 步骤。

        ./easyrsa init-pki
        ./easyrsa gen-req EntityName

3. 将请求（.req 文件）传输到 CA 系统并导入。 此处给出的名称是任意的，仅用于命名请求文件。

        ./easyrsa import-req /tmp/path/to/import.req EntityName

4. 使用正确的类型为请求签名。 本示例使用 client 类型：

        ./easyrsa sign-req client EntityName

5. 将新签署的证书传输到请求实体。 除非该实体具有先前证书的副本，否则可能还需要 CA 证书（ca.crt）。

6. 该实体现在具有自己的密钥对，已签名的证书和 CA。

签名后续请求
---------------------------

请按照上面的步骤 2-6 生成后续密钥对，并让 CA 返回已签名的证书。

废除证书并创建 CRL
--------------------------------

这是特定于 CA 的工作。

要永久废除已颁发的证书，可使用导入期间采用的简称：

        ./easyrsa revoke EntityName

要创建一个更新的 CRL，其中包含到那时为止所有已废除的证书：

        ./easyrsa gen-crl

生成后，要将 CRL 发送到引用它的系统。

生成 Diffie-Hellman（DH）参数
-------------------------------------

初始化 PKI 后，任何实体都可以创建其所需的 DH 参数。 通常仅由 TLS 服务器使用。 虽然 CA PKI 可以生成此文件，但在其所在的服务器上执行此操作更有意义，以避免在生成文件后将文件发送到另一个系统。

DH 参数可以通过以下方式生成：

        ./easyrsa gen-dh

显示请求或证书的详细信息
------------------------------------

要通过引用实体名简称 EntityName 来显示请求或证书的详细信息，请使用以下命令之一。 在没有匹配文件的情况下调用它们会产生错误。

        ./easyrsa show-req EntityName
        ./easyrsa show-cert EntityName

更改私钥密码短语
--------------------------------

RSA 和 EC 私钥可以重新加密，因此可以根据密钥类型使用以下命令之一提供新的密码短语：

        ./easyrsa set-rsa-pass EntityName
        ./easyrsa set-ec-pass EntityName

（可选）可以使用“nopass”选项将密码短语完全删除。
请参阅命令帮助以获取详细信息。
