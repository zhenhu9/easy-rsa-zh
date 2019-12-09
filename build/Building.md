构建 Easy-RSA 3
===

本文档用作包装参考。

使用构建脚本
---

build/build-dist.sh 从当前源代码树准备可发布的 tarball。 确保干净签出（删除剩余的临时文件），并确认发布更改是否按顺序：

 * 更新了 ChangeLog 的版本，日期和功能更改
 * 在 git 中准备发布标签

准备好后，在压缩包和内部目录中设置匹配版本：

    ./build/build-dist.sh --version=3.2.1

供开发使用，省略 --version 参数默认会创建一个 `git-development` 版本。

Windows Build Extras
---

当更新构建脚本以支持 Windows 时，此部分将更新以匹配。

请注意，Windows 构建需要 unxutils 和 mksh/Win32 项目中的外部二进制文件，这些文件不包含在 Easy-RSA 源代码树中。 从较早的基本构建目录开始，请按照以下步骤操作。

1. 将所有内容从 `distro/windows/` 复制到目标根目录。 确保文本文件遵循 Windows EOL 约定（CR+LF）-- Windows 上的 git  对项目源码的检出通常已经为您做到了。

2. 将 readme/doc 内的 .md 文件转换为 html，以便 Windows 客户端更轻松地查看。 使用 python3 `markdown` 模块的一种选择是：

    find ./ -name '*.md' | while read f
    do
      python3 -m markdown "$f"  "${f/.md/.html}"
      rm "$f"
    done

3. 将 mksh.exe 从 mksh/Win32 项目复制到精确命名为 `bin/sh.exe` 的目标目录中（注意名称不同）。

4. 将以下文件从 unxutils 项目复制到目标 `/bin/` 目录中。 标有 [+] 的文件在非官方版本中是可选的，并且仅用于使 Shell 环境对用户更有用。

   * awk.exe
   * cat.exe
   * cp.exe
   * diff.exe [+]
   * grep.exe [+]
   * ls.exe [+]
   * md5sum.exe [+]
   * mkdir.exe
   * mv.exe [+]
   * printf.exe
   * rm.exe
   * sed.exe [+]
   * which.exe

5. 压缩目标目录以进行发行版分发。
