# Windows 下搭建 Python 开发环境
> ### 使用 ```VSCode``` + ```pyenv``` + ```virtualenv``` 搭建Python开发环境
***

[前往官网下载VSCode](https://code.visualstudio.com/)

  
将下方代码保存为`install.ps1`并运行
```ps1
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
    $install_pyenv=Get-PyenvWin
    if ($install_pyenv) {
        exit
    }else {
        try {
            # 清理安装包
            Remove-Item ".\pyenv-win-master.zip"
            Remove-Item ".\pyenv-win-master" -Recurse
            # 不用重启立即生效环境变量
            $PyEnvDir = "${env:USERPROFILE}\.pyenv"
            $PyEnvWinDir = "${PyEnvDir}\pyenv-win"
            $BinPath = "${PyEnvWinDir}\bin"
            $ShimsPath = "${PyEnvWinDir}\shims"
            $Env:PYENV=$PyEnvWinDir
            $Env:PYENV_ROOT=$PyEnvWinDir
            $Env:PYENV_HOME=$PyEnvWinDir
            $PathParts = $Env:PATH -Split ";"
            $NewPathParts = $PathParts.Where{ $_ -ne $BinPath }.Where{ $_ -ne $ShimsPath }
            $NewPathParts = ($BinPath, $ShimsPath) + $NewPathParts
            $NewPath = $NewPathParts -Join ";"
            $Env:PATH = $NewPath
            # Python 3.11.1
            pyenv install 3.11.1
            # 设定 Python 3.11.1 为全局版本
            pyenv global 3.11.1
            # 升级 pip 为最新版本
            python -m pip install --upgrade pip
            # 安装 virtualenv 虚拟环境
            python -m pip install virtualenv
            # 禁止 pip 全局安装
            [System.Environment]::SetEnvironmentVariable('PIP_REQUIRE_VIRTUALENV', 'true', "User")
            $Env:PIP_REQUIRE_VIRTUALENV = 'true'
        }
        catch {
            exit
        }
        Install-VSCodePlugs
        clear
        Write-Host "准备就绪 请尝试:"
        ""
        "print('Hello World')"
        ""
        python
    } 
}
Main

```
