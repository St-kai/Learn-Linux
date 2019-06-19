# pacman
---

用法:  pacman <操作> [...]
操作:
    pacman {-h --help}
    pacman {-V --version}
    pacman {-D --database} <选项> <软件包>  
    pacman {-F --files}    [选项] [软件包]  
    pacman {-Q --query}    [选项] [软件包]  
    pacman {-R --remove}   [选项] <软件包>  
    pacman {-S --sync}     [选项] [软件包]  
    pacman {-T --deptest}  [选项] [软件包]  
    pacman {-U --upgrade}  [选项] <文件>  


用法:  pacman {-R --remove} [选项] <软件包>
选项:
  -b, --dbpath <路径>  指定另外的数据库位置  
  -c, --cascade        删除软件包及所有依赖于此的软件包  
  -d, --nodeps         跳过依赖关系的版本检查 (-dd 跳过所有检查)  
  -n, --nosave         删除配置文件  
  -p, --print          打印目标而不是执行操作  
  -r, --root <路径>    指定另外的安装根目录  
  -s, --recursive      删除不需要的依赖关系  
                       (-ss 包括单独指定安装的依赖关系)  
  -u, --unneeded       删除不需要的软件包  
  -v, --verbose        显示详细信息  
      --arch <架构>    设定另外的架构  
      --assume-installed <package=version>  
                       添加一个虚拟包用以满足依赖要求  
      --cachedir <dir> 指定另外的软件包缓存位置  
      --color <when>   彩色化输出  
      --config <路径>  指定另外的配置文件  
      --confirm        总是询问确认  
      --dbonly         仅修改数据库条目，而非软件包文件  
      --debug          显示调试信息  
      --disable-download-timeout  
                       下载时用宽松的超时  
      --gpgdir <路径>  设定 GnuPG 的其他主目录  
      --hookdir <目录>  指定另外的钩子位置  
      --logfile <路径> 指定另外的日志文件  
      --noconfirm      不询问确认
      --noprogressbar  下载文件时不显示进度条
      --noscriptlet    不执行安装小脚本
      --print-format <字符串>
                       指定如何打印目标
      --sysroot        在一个已挂载的 guest 系统操作（仅 root）


安装软件包  
注意: 软件包通常有很多可选依赖， 它们为软件提供额外功能， 并不强制要求安装它们。 安装软件时, pacman 将会输出它的可选依赖, 但是这个输出不会在 pacman.log中；当你想浏览已安装软件的可选依赖时可以使用pacman -Si ，得到关于可选依赖的简短描述。
警告: 在Arch下安装软件包时，未更新系统前，不要更新软件包数据库（例如，可能出现某软件包不再出现在官方库）。操作时，应使用pacman -Syu package_name, 而不要使用（pacman -Sy package_name），否则可能会有依赖问题。参见 System maintenance (简体中文)#不支持部分升级 和 BBS#89328.
安装指定的包
安装或者升级单个软件包，或者一列软件包（包含依赖包），使用如下命令：

##### pacman -S package_name1 package_name2 ...

用正则表达式安装多个软件包（参见 pacman 小贴士[broken link: invalid section]和这个帖子）：

##### pacman -S $(pacman -Ssq package_regex)
有时候在不同的软件仓库中，一个软件包有多个版本（比如[extra]和[testing]）。可以选择一个来安装：

##### pacman -S extra/package_name
安装多个含有相似名称的软件包，而并非整个包组或全部匹配的软件包； 例如，plasma:

##### pacman -S plasma-{desktop,mediacenter,nm}
当然，可以多层扩展，并不作限制：

# pacman -S plasma-{workspace{,-wallpapers},pa}
安装包组
一些包属于一个可以同时安装的软件包组[broken link: invalid section]。例如，运行下面的命令

##### pacman -S gnome
会提醒用户选择 gnome 内需要安装的包。

有的包组包含大量的软件包，有时用户只需其中几个。除了逐一键入序号外，pacman 还支持选择或排除某个区间内的的软件包：

Enter a selection (default=all): 1-10 15
这将选中序号 1 至 10 和 15 的软件包。而

Enter a selection (default=all): ^5-8 ^2
将会选中除了序号 5 至 8 和 2 之外的所有软件包。

想要查看哪些包属于 gnome 组，运行：

##### pacman -Sg gnome
也可以访问 https://www.archlinux.org/groups/ 查看可用的包组。

注意: 如果列表中的包已经安装在系统中，它会被重新安装，即使它已经是最新的。可以用 --needed 选项覆盖这种行为。
删除软件包
删除单个软件包，保留其全部已经安装的依赖关系

pacman -R package_name
删除指定软件包，及其所有没有被其他已安装软件包使用的依赖关系：

pacman -Rs package_name
要删除软件包和所有依赖这个软件包的程序:

警告: 此操作是递归的，请小心检查，可能会一次删除大量的软件包。
##### pacman -Rsc package_name
要删除软件包，但是不删除依赖这个软件包的其他程序：

##### pacman -Rdd package_name
pacman 删除某些程序时会备份重要配置文件，在其后面加上*.pacsave扩展名。-n 选项可以避免备份这些文件：

pacman -Rn package_name
注意: pacman 不会删除软件自己创建的文件(例如主目录中的 .dot 文件不会被删除。
升级软件包
警告: * 建议所有用户都 经常性的更新系统。
Arch 只支持系统完整升级，详细参见不支持部分升级和#安装软件包。
一个 pacman 命令就可以升级整个系统。花费的时间取决于系统有多老。这个命令会同步非本地(local)软件仓库并升级系统的软件包：

##### pacman -Syu
查询包数据库
pacman 使用 -Q 参数查询本地软件包数据库。参见：

$ pacman -Q --help
使用 -S 参数来查询远程同步的数据库。参见：

$ pacman -S --help
pacman 可以在包数据库中查询软件包，查询位置包含了软件包的名字和描述：

$ pacman -Ss string1 string2 ...
有时，-s的内置正则会匹配很多不需要的结果，所以应当指定仅搜索包名，而非描述或其他子段，下面的命令就会返回很多不必要结果:

$ pacman -Ss '^vim-'
要查询已安装的软件包：

$ pacman -Qs string1 string2 ...
按文件名查找软件库：

$ pacman -Fs string1 string2 ...
显示软件包的详尽的信息：

$ pacman -Si package_name
查询本地安装包的详细信息：

$ pacman -Qi package_name
使用两个 -i 将同时显示备份文件和修改状态：

$ pacman -Qii package_name
要获取已安装软件包所包含文件的列表：

$ pacman -Ql package_name
查询远程库中软件包包含的文件：

$ pacman -Fl package_name
检查软件包安装的文件是否都存在：

$ pacman -Qk package_name
两个参数k将会执行一次更彻底的检查。 查询数据库获取某个文件属于哪个软件包：

$ pacman -Qo /path/to/file_name
查询文件属于远程数据库中的哪个软件包：

$ pacman -Fo /path/to/file_name
要罗列所有不再作为依赖的软件包(孤立orphans)：

$ pacman -Qdt
要罗列所有明确安装而且不被其它包依赖的软件包：

$ pacman -Qet
要显示软件包的依赖树：

$ pactree package_name
检查一个安装的软件包被那些包依赖，可以使用 pkgtoolsAUR中的whoneeds:

$ whoneeds package_name
或者pactree中使用-r:

$ pactree -r package_name
更多信息查看pacman tips。

数据库结构
pacman数据库通常位于 /var/lib/pacman/sync. 对于每一个在/etc/pacman.conf中指定的软件仓库， 这里都有一个一致的数据库。数据库文件夹里每个tar.gz文件都包含着一个仓库的软件包信息。例如which 包:

% tree which-2.20-6 	
which-2.20-6
|-- depends
`-- desc
这个 depends 项列出了该软件的依赖包， 而desc有该包的介绍，例如文件大小和MD5值 。

清理软件包缓存
pacman 将下载的软件包保存在 /var/cache/pacman/pkg/ 并且不会自动移除旧的和未安装版本的软件包，因此需要手动清理，以免该文件夹过于庞大。

使用内建选项即可清除未安装软件包的缓存：

# pacman -Sc
警告:
仅在确定当前安装的软件包足够稳定且不需要降级时才执行清理。pacman -Sc仅会保留软件包的当前有效版本，旧版本的软件包被清理后，只能从其他地方如 Arch Linux Archive (简体中文)中获取了。
pacman -Scc 可以清理所有缓存，但这样 pacman 在重装软件包时就只能重新下载了。除非空间不足，否则不应这么做。
由于以上种种局限，建议使用专门的脚本去处理清理哪些、清理多少缓存：

pacman-contrib 提供的 paccache 命令默认会删除近3个版本前的软件包

##### paccache -r
Tip: 可以使用 pacman hooks 自动执行清理，这里是参考示例。
也可以自己设置保留最近几个版本：

##### paccache -rk 1
清理所有未安装包的缓存文件，再此运行paccache:

##### paccache -ruk0
更多功能参见paccache -h。

paccache，还可以使用 Arch User Repository 中的 pkgcachecleanAUR：

##### pkgcacheclean
，以及pacleanerAUR，这两个是未来的替代工具.

其它命令
升级系统时安装其他软件包：

##### pacman -Syu package_name1 package_name2 ...
下载包而不安装它：

##### pacman -Sw package_name
安装一个本地包(不从源里下载）：

# pacman -U /path/to/package/package_name-version.pkg.tar.xz
要将本地包保存至缓存，可执行：

# pacman -U file://path/to/package/package_name-version.pkg.tar.xz
安装一个远程包（不在 pacman 配置的源里面）：

##### pacman -U http://www.example.com/repo/example.pkg.tar.xz
要禁用 -S, -U 和 -R 动作，可以使用 -p 选项.

pacman 会列出需要安装和删除的软件，并在执行动作前要求需要的权限。

安装原因
pacman数据库按照软件包被安装的原因，将其分为两类：

指定安装包：通过pacman-S或者-U指令安装的软件包。
依赖包：指定安装包所依赖的软件包，尽管命令中未传入，但仍然会被安装。
当安装软件包时，可以把安装原因指定设为依赖:

##### pacman -S --asdeps package_name
但是当重新安装该软件包时，安装原因将会被设为软件包所默认的。 指定安装的软件包列表可用pacman -Qe, 已安装的依赖包可用pacman -Qd获取。 改变某个已安装软件包的安装原因，可以执行：

##### pacman -D --asdeps package_name
使用--asexplicit改为指定安装。

查询一个包含具体文件的包名
同步文件数据库:

##### pacman -Fy
查询包含某个文件的包名，比如:

##### pacman -Fs pacman
core/pacman 5.0.1-4
    usr/bin/pacman
    usr/share/bash-completion/completions/pacman
extra/xscreensaver 5.36-1
    usr/lib/xscreensaver/pacman
提示： 可以设置一个 crontab 或者 systemd timer 来定期同步文件信息数据库。
如果需要高级功能请安装 pkgfile，它使用一个单独的数据库来保存文件和它们所关联的软件包的信息。

配置
pacman 的配置文件位于/etc/pacman.conf。 man pacman.conf 可以查看配置文件的进一步信息。

通用选项
通用选项都在[options]段。阅读 man 手册或者查看默认的 pacman.conf 可以获得有关信息和用法。

升级前对比版本
要查看旧版和新版的有效安装包，请取消/etc/pacman.conf中"VerbosePkgLists"的注释。修改后的pacman -Syu输出如下：

Package (6)             Old Version  New Version  Net Change  Download Size

extra/libmariadbclient  10.1.9-4     10.1.10-1      0.03 MiB       4.35 MiB
extra/libpng            1.6.19-1     1.6.20-1       0.00 MiB       0.23 MiB
extra/mariadb           10.1.9-4     10.1.10-1      0.26 MiB      13.80 MiB
彩色输出
Pacman 具有颜色选项，取消 "Color" 行的注释即可.

不升级软件包
如果由于某种原因，用户不希望升级某个软件包，可以加入内容如下：

IgnorePkg = 软件包名
多软件包可以用空格隔开，也可是用 glob 模式。如果只打算忽略一次升级，可以使用 --ignore 选项。

忽略了的软件包可通过 pacman -S 升级。

不升级软件包组
和软件包一样，也可以不升级某个软件包组：

IgnoreGroup = gnome
跳过软件包文件
Warning: 跳过软件包时请考虑清楚，Arch 官方并不支持 部分更新。
要跳过某些文件夹的安装，可以将它们放到 NoExtract 中，例如不想安装 systemd unit 文件：

NoExtract=usr/lib/systemd/system/*
后面的规则覆盖前面的规则，加上 ! 可以取消跳过效果.

保留多个配置文件
如果你有多个配置文件（比如，主配置和测试testing仓库生效的配置文件），需要共享一些设置：

Include = /path/to/common/settings
/path/to/common/settings文件中是两个配置文件共享的相同配置。
