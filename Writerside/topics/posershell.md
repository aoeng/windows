# PowerShell 配置

## 安装PowerShell7
-  [官网连接](https://learn.microsoft.com/zh-cn/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.4)

- winget 安装
```bash
winget search Microsoft.PowerShell
winget install --id Microsoft.PowerShell --source winget
```

## 编辑环境变量
```Bash
notepad $profiles

# 不存在如下命令创建
New-Item -ItemType File -Path $PROFILE -Force

#全局环境变量
notepad $profile.AllUsersAllHosts
```

常用配置如下
```psl
# 设置代理
set ALL_PROXY=http://127.0.0.1:7890

function art {
     & php artisan @args
}

function gsh {
    param(
        [string]$Message = "fix"
    )
    
    git add -A
    
    git commit -m $Message
    if ($LASTEXITCODE -ne 0) {
        Write-Error "commit 失败"
        return
    }
    
    git push
}

function gitm {
    param(
        [Parameter(Mandatory=$true)]
        [string]$TargetBranch
    )
    # 获取当前分支名
    $CurrentBranch = git branch --show-current

    if (-not $CurrentBranch) {
        Write-Error "当前不在任何分支上，请确保你已经切换到一个分支。"
        return
    }

    try {
        # 切换到目标分支
        git checkout $TargetBranch
        if ($LASTEXITCODE -ne 0) {
            Write-Error "切换到目标分支 '$TargetBranch' 失败，请检查分支名称是否正确。"
            return
        }

        # 合并当前分支到目标分支
        git merge $CurrentBranch
        if ($LASTEXITCODE -ne 0) {
            Write-Error "合并当前分支 '$CurrentBranch' 到 '$TargetBranch' 失败"
            return
        }
        
        # 提交到远程分支
        git push origin $TargetBranch

        # 切换回原来的分支
        git checkout $CurrentBranch
        if ($LASTEXITCODE -ne 0) {
            Write-Error "切换回原分支 '$CurrentBranch' 失败。"
            return
        }

        Write-Host "分支 '$CurrentBranch' 已成功合并到 '$TargetBranch'，并切换回原分支。" -ForegroundColor Green
    }
    catch {
        Write-Error "发生错误：$_"
    }
}

```

## 安装Scoop
```PowerShell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser 
iwr -useb https://gitee.com/glsnames/scoop-installer/raw/master/bin/install.ps1 | iex

# 国内镜像
scoop config SCOOP_REPO https://gitee.com/glsnames/scoop-installer

# bucket国内镜像
cd $env:SCOOP\buckets\Main
git remote set-url origin https://gitee.com/scoop-bucket/main.git

# 切换回官方镜像
scoop config SCOOP_REPO https://github.com/ScoopInstaller/Scoop
```


## 安装OhMyPosh
```PowerShell
# 安装Posh
winget install JanDeDobbeleer.OhMyPosh -s winget

# 安装字体
oh-my-posh font install
```

- [下载robbyrussell主题](https://github.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/robbyrussell.omp.json)

```PowerShell
# 编辑

```
编辑`notepad $profiles` 加入如下配置,之后`. $profiles`启用配置
```psl
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\robbyrussell.omp.json" | Invoke-Expression
```

## 安装历史命令提示
```PowerShell
Install-Module -Name PowerShellGet -Force
```
编辑 $profiles


```PSL
Import-Module PSReadLine
# 设置预测文本来源为历史记录
Set-PSReadLineOption -PredictionSource History 
# 设置 Tab 为菜单补全和 Intellisense
Set-PSReadLineKeyHandler -Key "Tab" -Function MenuComplete
# 每次回溯输入历史，光标定位于输入内容末尾
Set-PSReadLineOption -HistorySearchCursorMovesToEnd 
# 设置向上键为后向搜索历史记录
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward 
# 设置向下键为前向搜索历史纪录
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward 
```

## 安装vim
```PowerShell
scoop install vim
```

## 移除Logo
启动参数加上 `-nologo`

## 安装Z

```PowerShell
scoop install zoxide
```

编辑 $profiles
```PSL
Invoke-Expression (&{zoxide init pwsh} | Out-String)
```

## 安装服务管理NSSM
```bash
scoop install nssm

nssm install nginx
nssm install php-cgi-84
nssm set php-cgi-82 AppParameters "-b 127.0.0.1:9082"
```
