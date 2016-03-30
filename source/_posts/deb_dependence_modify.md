title: 如何修改deb安装包里的依赖关系/冲突
tags:
  - deb
  - dependence
  - linux
date: 2011-03-12 20:15:18
---

今天装code::blocks时， 总是提示  
<pre>
错误： 破坏已有软件包 'codeblocks-common' 对 codeblocks (< 10.05-1) 的冲突关系
</pre>

直接打开deb包修改里面的control文件也没用，关闭后又恢复原样了。在 [ubuntuforms](http://ubuntuforums.org/showthread.php?t=636724) 搜到一段shell脚本倒是可以修改control文件后重新生成一个deb包， 脚本如下：

    #!/bin/bash

    if [[ -z "$1" ]]; then
        echo "Syntax: $0 debfile"
        exit 1
    fi
    DEBFILE="$1"
    TMPDIR=mktemp -d /tmp/deb.XXXXXXXXXX || exit 1
    OUTPUT=basename "$DEBFILE" .deb.modfied.deb
    if [[ -e "$OUTPUT" ]]; then
        echo "$OUTPUT exists."
        rm -r "$TMPDIR"
        exit 1
    fi
    dpkg-deb -x "$DEBFILE" "$TMPDIR"
    dpkg-deb --control "$DEBFILE" "$TMPDIR"/DEBIAN
    if [[ ! -e "$TMPDIR"/DEBIAN/control ]]; then
        echo DEBIAN/control not found.
        rm -r "$TMPDIR"
        exit 1
    fi
    CONTROL="$TMPDIR"/DEBIAN/control
    MOD=stat -c "%y" "$CONTROL"
    vi "$CONTROL"
    if [[ "$MOD" == stat -c "%y" "$CONTROL" ]]; then
        echo Not modfied.
    else
        echo Building new deb...
        dpkg -b "$TMPDIR" "$OUTPUT"
    fi
    rm -r "$TMPDIR"
