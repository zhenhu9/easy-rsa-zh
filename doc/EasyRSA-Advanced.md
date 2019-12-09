Easy-RSA 高级参考
=============================

这是熟悉 PKI 流程的高级用户的技术参考。 如果您需要更详细的说明，请参阅 `EasyRSA-Readme` 或 `Intro-To-PKI` 文档。

配置参考
-----------------------

#### 配置源

  有 3 种可能的方式来执行 Easy-RSA 的外部配置，按照以下顺序选择，以第一个定义的结果为准：

  1. 命令行选项
  2. 环境变量
  3. “vars” 文件（如果存在）（请参见下面的 `vars 自动检测`）
  4. 内置默认

  请注意，尽管可以将所有 env-var 添加到 “vars” 文件中，即使默认情况下未显示，但并非所有可能的配置选项都可以在任何地方设置。

#### vars 自动检测

  “vars” 文件是简单命名为 `vars`（无扩展名）的文件，Easy-RSA 将会 source 此配置文件。 该文件专门设计为 *并非*用来替换已使用更高优先级方法（例如 CLI opts 或 env-vars）设置的变量。

  按照以下顺序检查以下位置的 vars 文件。 仅使用找到的第一个：

  1. --vars CLI 选项引用的文件。
  2. 名为 `EASYRSA_VARS_FILE` 的环境变量所引用的文件。
  3. `EASYRSA_PKI` 环境变量引用的目录。
  4. 位于 $PWD/pki 的默认的 PKI 目录。
  4. `EASYRSA` env-var 引用的目录。
  5. 包含 easyrsa 程序的目录。

  在所有情况下，定义环境变量 `EASYRSA_NO_VARS` 将覆盖 vars 文件的来源，包括随后将其定义为全局选项。
  
#### OpenSSL 配置

  Easy-RSA 与 OpenSSL 配置文件（.cnf）紧密耦合，以提供脚本提供的灵活性。 要求此文件可用，但是可以为特定的 PKI 使用不同的 OpenSSL 配置文件，甚至可以为特定的调用对其进行更改。

  按照以下顺序搜索 OpenSSL 配置文件：

  1. 环境变量 `EASYRSA_SSL_CONF`。
  2. 'vars' 文件 (请参阅上面的 `vars 自动检测`)。
  3. 具有文件名为 `openssl-easyrsa.cnf` 的 `EASYRSA_PKI` 目录。
  4. 具有文件名为 `openssl-easyrsa.cnf` 的 `EASYRSA` 目录。

高级扩展处理
---------------------------

通常，证书扩展名是在签名期间通过 CLI 上给出的证书类型选择的； 这将导致处理 x509-types 子目录中的匹配文件以添加 OpenSSL 扩展。 可以通过在另一个 `EASYRSA_PKI` 目录中放置另一个 x509 类型的目录来覆盖特定的 PKI。

x509-types 目录中名为 `COMMON` 的文件将附加到每种证书类型； 这是专为 CDP 使用而设计的，但可用于应适用于每个签名证书的任何扩展。

另外，环境变量 `EASYRSA_EXTRA_EXTS` 的内容附加了其原始文本，并添加到了 OpenSSL 扩展中。 内容按原样附加到 cert 扩展名； 无效的 OpenSSL 配置通常会导致失败。

环境变量参考
---------------------------------

环境变量列表，任何匹配设置/覆盖它的全局选项（CLI）以及可能的简洁描述如下所示：

 *  `EASYRSA` - 应该指向 easyrsa 脚本所在的 Easy-RSA 顶级目录。
 *  `EASYRSA_OPENSSL` - 调用 openssl 的命令。
 *  `EASYRSA_SSL_CONF` - 使用的 openssl 配置文件。
 *  `EASYRSA_PKI` (CLI: `--pki-dir`) - 用于保存所有 PKI 特定文件的 dir，默认为 $PWD/pki。
 *  `EASYRSA_DN` (CLI: `--dn-mode`) - 设置为字符串 `cn_only` 或 `org` 以更改要包含在请求 DN 中的字段。
 *  `EASYRSA_REQ_COUNTRY` (CLI: `--req-c`) - 使用 org 模式设置 DN 国家
 *  `EASYRSA_REQ_PROVINCE` (CLI: `--req-st`) - 使用 org 模式设置 DN 国家/省
 *  `EASYRSA_REQ_CITY` (CLI: `--req-city`) - 使用 org 模式设置 DN 城市/位置
 *  `EASYRSA_REQ_ORG` (CLI: `--req-org`) - 使用 org 模式设置 DN 组织
 *  `EASYRSA_REQ_EMAIL` (CLI: `--req-email`) - 使用 org 模式设置 DN 邮箱
 *  `EASYRSA_REQ_OU` (CLI: `--req-ou`) - 使用 org 模式设置 DN 组织部门
 *  `EASYRSA_KEY_SIZE` (CLI: `--key-size`) - 以位为单位设置密钥大小
 *  `EASYRSA_ALGO` (CLI: `--use-algo`) - 设置使用的加密算法：rsa 或 ec
 *  `EASYRSA_CURVE` (CLI: `--curve`) - 定义要使用的命名 EC 曲线
 *  `EASYRSA_EC_DIR` - 存储生成的 ecparams 的目录
 *  `EASYRSA_CA_EXPIRE` (CLI: `--days`) - 设置 CA 有效期（天）
 *  `EASYRSA_CERT_EXPIRE` (CLI: `--days`) - 设置已颁发证书的有效期（天）
 *  `EASYRSA_CRL_DAYS` (CLI: `--days`) - 以天为单位设置 CRL “下一次发布”时间
 *  `EASYRSA_NS_SUPPORT` (CLI: `--ns-cert`) - 字符串“yes”或“no”字段以包含不建议使用的 Netscape 扩展
 *  `EASYRSA_NS_COMMENT` (CLI: `--ns-comment`) - 使用不推荐使用的 Netscape 扩展时要包括的字符串注释
 *  `EASYRSA_TEMP_FILE` - 动态创建 req/cert 扩展名时使用的临时文件
 *  `EASYRSA_REQ_CN` (CLI: `--req-cn`) - 默认 CN，必须在 BATCH 模式下设置
 *  `EASYRSA_DIGEST` (CLI: `--digest`) - 设置哈希摘要以用于请求/证书签名
 *  `EASYRSA_BATCH` (CLI: `--batch`) - 启用批处理（无提示）模式； 将 env-var 设置为非零字符串以启用（CLI 不带任何选项）
