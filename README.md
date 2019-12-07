# 概述

easy-rsa 是用于构建和管理 PKI CA 的 CLI 实用程序。 简单来说，意思是要创建根证书颁发机构，并请求和签名证书，包括子 CA 和证书吊销列表（CRL）。

# 下载

如果您正在寻找发行版本下载，请参阅 GitHub 上的发行版本部分。 发行版本也可以使用命名标签作为源签出。

# 文档

有关 3.x 项目文档和用法，请参阅 [README.quickstart.md](README.quickstart.md) 文件或 doc/ 目录下的更加详细的文档。 .md 文件为 Markdown 格式，可以根据需要将其转换为 html 文件，以作为发行版软件包的使用，也可以以文本原样阅读。

# 获得 Easy-RSA 使用帮助

目前，尽管 Easy-RSA 开发是个单独的项目，但它是与 OpenVPN 共存的。 在撰写本文时，以下资源是寻求 Easy-RSA 使用帮助的好地方：

[openvpn-users mailing list](https://lists.sourceforge.net/lists/listinfo/openvpn-users)  邮件列表是发布用法或请求帮助问题的好地方。

您也可以尝试在 IRC 的 Freenode/#openvpn 上获得常规支持，或者在 Freenode/#easyrsa 进行开发讨论。

# 分支结构

easy-rsa master 分支当前正在跟踪 3.x 发行版周期的开发。 请注意，在任何时候，master 分支可能会有问题。 随时可以针对 master 分支提出问题，但是在使用 master 分支时要有耐心。 建议使用发行版，并且优先考虑最新发行版中发现的错误。

先前的 2.x 和 1.x 版本可用作发行分支，用于跟踪和修补相关修订。 一下为分支布局为：

    master         <- 3.x, at present
    v3.x.x            pre-release branches, used for staging branches
    release/2.x
    release/1.x

3.x 的许可信息位于 [COPYING.md](COPYING.md) 文件中。

# Code style, standards

我们正在尝试遵守 POSIX 标准，该标准可在此处找到：

http://pubs.opengroup.org/onlinepubs/9699919799/
