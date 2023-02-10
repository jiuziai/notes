# Windows 下搭建 Python 开发环境
> ### 使用 ```VSCode``` + ```pyenv``` + ```virtualenv``` 搭建Python开发环境
***

[前往官网下载VSCode](https://code.visualstudio.com/)

***
将下方代码保存为`install.ps1`并运行
```
<#

    1,安装VSCode
    2,运行此脚本

    脚本简介:
        将此脚本保存为*.ps1格式,即PowerShell脚本格式
        运行此脚本,如果访问Github不是很顺畅,则可以选择梯子或者输入 y 循环重试,直至成功(Ctrl+C强制终止)

    脚本功能:
        1,自动安装 Pyenv 版本管理器,并配置环型变量
        2,自动安装 Python 3.11.1, 并设置为全局版本
        3,升级 pip 为最新版本
        4,安装 Virtualenv 虚拟环境管理工具
        5,安装 autopep8 自动格式化代码
        6,设置 pip 只能再虚拟环境下安装包
        7,安装 VSCode 开发Python常用基本插件

    提示:
        如果后续需要全局 pip 安装包, 可以打开 PowerShell 输入 $Env:PIP_REQUIRE_VIRTUALENV = $null
        安装完成后再输入 $Env:PIP_REQUIRE_VIRTUALENV = 'true'
        尽量保持在虚拟环境开发和安装项目需要的包

#>


# 提升至管理员权限
Set-ExecutionPolicy -Scope Process Unrestricted
# 设置字符集,防止中文乱码
$OutputEncoding = [console]::InputEncoding = [console]::OutputEncoding = New-Object System.Text.UTF8Encoding

# 安装VSCode扩展
Function Install-VSCodePlugs() {
    # 扩展列表
    $pluglist = @(
        ('ms-ceintl.vscode-language-pack-zh-hans', 'VSCode中文插件'),
        ('ms-python.python', 'Python扩展'),
        ('ms-python.vscode-pylance', 'Python语言支持'),
        ('visualstudioexptteam.vscodeintellicode', '代码智能提示'),
        ('visualstudioexptteam.intellicode-api-usage-examples', '代码示例'),
        ('ms-toolsai.jupyter', 'jupyter插件'),
        ('ms-vscode-remote.remote-ssh', '远程连接插件'),
        ('ms-vscode-remote.remote-ssh-edit', '远程编辑插件'),
        ('ms-vscode.remote-explorer', '远程浏览插件'),
        ('ms-vscode.remote-repositories', '远程储存库插件'),
        ('github.remotehub', 'Github仓库支持')
    )
    # 遍历扩展列表安装插件
    foreach ($item in $pluglist) {
        $item[1] += ':'
        Write-Host $item[1] -NoNewline
        code --install-extension $item[0]
    }
}

# 下载pyenv并安装
Function Get-PyenvWin() {
    try {
        curl https://codeload.github.com/pyenv-win/pyenv-win/zip/refs/heads/master -o .\pyenv-win-master.zip
        Expand-Archive -Path ".\pyenv-win-master.zip" -DestinationPath "." -Force
        .\pyenv-win-master\pyenv-win\install-pyenv-win.ps1
        return 1
    }
    catch {
        return 0
    }
}

Function Main() {
    Write-Host "如安装错误是否一直重试,直至安装成功?"
    $until_success = Read-Host "( Y 是 / 其他为否 )"
    $install_pyenv = Get-PyenvWin
    if ($until_success -ne "y" -And $install_pyenv) {
        exit
    }
    else {
        try {
            # 清理安装包
            Remove-Item ".\pyenv-win-master.zip"
            Remove-Item ".\pyenv-win-master" -Recurse
            # 不用重启立即生效环境变量
            $PyEnvDir = "${env:USERPROFILE}\.pyenv"
            $PyEnvWinDir = "${PyEnvDir}\pyenv-win"
            $BinPath = "${PyEnvWinDir}\bin"
            $ShimsPath = "${PyEnvWinDir}\shims"
            $Env:PYENV = $PyEnvWinDir
            $Env:PYENV_ROOT = $PyEnvWinDir
            $Env:PYENV_HOME = $PyEnvWinDir
            $PathParts = $Env:PATH -Split ";"
            $NewPathParts = $PathParts.Where{ $_ -ne $BinPath }.Where{ $_ -ne $ShimsPath }
            $NewPathParts = ($BinPath, $ShimsPath) + $NewPathParts
            $NewPath = $NewPathParts -Join ";"
            $Env:PATH = $NewPath
            $Env:PIP_REQUIRE_VIRTUALENV = $null
            # Python 3.11.1
            pyenv install 3.11.1
            # 设定 Python 3.11.1 为全局版本
            pyenv global 3.11.1
            # 升级 pip 为最新版本
            python -m pip install --upgrade pip
            # 安装 virtualenv 虚拟环境
            python -m pip install virtualenv
            # 安装 autopep8 格式化服务
            python -m pip install autopep8
            # 禁止 pip 全局安装
            [System.Environment]::SetEnvironmentVariable('PIP_REQUIRE_VIRTUALENV', 'true', "User")
            $Env:PIP_REQUIRE_VIRTUALENV = 'true'
            
            Install-VSCodePlugs
            Clear-Host
            Write-Host "准备就绪 请尝试:"
            ""
            "print('Hello World')"
            ""
            python
        }
        catch {
            if ($until_success -ne "y") {
                Main
            }
            else {
                exit
            } 
        }
    } 
}
Main
```
