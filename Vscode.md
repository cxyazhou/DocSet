# Windows本地使用（不远程连接ubuntu）
## 安装vscode或者trae（字节AI版本）
## 安装高亮插件并配置
1. crtl shift p，输入settings.json，打开用户json配置修改或增加：
```
    "highlightwords.colors": [
        {"dark": "yellow" },
        {"dark": "Cyan" },
        {"dark": "Pink" },
        {"dark": "LightGreen" },
        {"dark": "LightSteelBlue" },
        {"dark": "Plum" }
    ],
    "highlightwords.box": {
        "light": true,
        "dark": false
    },
    "highlightwords.defaultMode": 0,
    "highlightwords.showSidebar": true,
```
2. crtl shift p，输入 Highlight Toggle Current，然后修改对应的快捷键为F8
3. crtl shift p，输入 Highlight Remove All，然后修改对应的快捷键为Shift F8
4. 关闭vscode自带的单词高亮，settings.json修改或增加：
```
    "editor.occurrencesHighlight": "off",
```
5. 修改vscode搜索时的单词高亮，settings.json修改或增加：
```
    "workbench.colorCustomizations": {
        // 当前选中的匹配项（光标所在位置）背景色设为透明
        "editor.findMatchBackground": "#00000000",
        // 其他非当前匹配项的背景色设为透明
        "editor.findMatchHighlightBackground": "#00000000", 
        // 当前选中的匹配项（光标所在位置）边框颜色
        "editor.findMatchBorder": "#FF0000",
        // 其他非当前匹配项的边框颜色
        "editor.findMatchHighlightBorder": "#FF8800"
    },
```
## 搜索快捷键配置
1. crtl shift p，输入查找下一个，修改editorFocus、editorFocus && findInputFocussed为F4
2. crtl shift p，输入查找上一个，修改editorFocus、editorFocus && findInputFocussed为F3
## global配置
1. 将global的bin目录添加到系统环境变量后，重启IDE
2. 安装插件 C/C++ GNU Global
3. 扩展设置：Auto Update：Disabled，Completion：Disabled
4. 全局重建：crtl shift p，输入 rebuild database，选择gtags，然后设置为Alt Shift A。
5. 增量重建设置为Alt Shift S：crtl shift p，输入Tasks: 打开用户任务 → 选 Create tasks.json file from template → Others
```
    "tasks": [
        {
            "label": "GNU Global: Update (global -u)",
            "type": "shell",
            "command": "global",
            "args": ["-u"],
            "options": {
                "cwd": "${workspaceFolder}"   // 强制在项目根目录执行
            },
            "problemMatcher": [],
            "group": "none",
            "presentation": {
                "reveal": "never",
                "focus": false
            }
        }
    ]
```
crtl shift p，输入键盘快捷键，打开keybindings.json
```
    {
        "key": "alt+shift+s",          // 你喜欢的组合键
        "command": "workbench.action.tasks.runTask",
        "args": "GNU Global: Update (global -u)"
    },
```
命令运行失败：管理员PowerShell执行 Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
## 在资源管理器打开
tasks.json增加：
```
        {
			"label": "explorer",
			"type": "shell",
			"command": "explorer",
			"args": ["/select,${file}"],
			"options": {},
			"problemMatcher": [],
			"group": "none",
            "presentation": {
                "reveal": "never",
                "focus": false
            }
		}
```
keybindings.json增加：
```
    {
		"key": "alt+1",
		"command": "workbench.action.tasks.runTask",
		"args": "explorer"
    },
```
## svn操作
tasks.json增加：
```
		{
			"label": "SVN Diff (Current File)",
			"type": "shell",
			"command": "D:\\Program Files\\TortoiseSVN\\bin\\TortoiseProc.exe",
			"args": [
				"/command:diff",
				"/path:${file}"
			],
			"problemMatcher": [],
			"group": "none",
            "presentation": {
                "reveal": "never",
                "focus": false
            }
		},
		{
			"label": "SVN Show Log (Current File)",
			"type": "shell",
			"command": "D:\\Program Files\\TortoiseSVN\\bin\\TortoiseProc.exe",
			"args": [
				"/command:log",
				"/path:${file}"
			],
			"problemMatcher": [],
			"group": "none",
            "presentation": {
                "reveal": "never",
                "focus": false
            }
		},
		{
			"label": "SVN Show Log (Workspace)",
			"type": "shell",
			"command": "D:\\Program Files\\TortoiseSVN\\bin\\TortoiseProc.exe",
			"args": [
				"/command:log",
				"/path:${workspaceFolder}"
			],
			"problemMatcher": [],
			"group": "none",
            "presentation": {
                "reveal": "never",
                "focus": false
            }
		},
		{
			"label": "SVN Commit (Workspace)",
			"type": "shell",
			"command": "D:\\Program Files\\TortoiseSVN\\bin\\TortoiseProc.exe",
			"args": [
				"/command:commit",
				"/path:${workspaceFolder}"
			],
			"problemMatcher": [],
			"group": "none",
            "presentation": {
                "reveal": "never",
                "focus": false
            }
		}
```
keybindings.json增加：
```
    {
		"key": "alt+2",
		"command": "workbench.action.tasks.runTask",
		"args": "SVN Diff (Current File)"
    },
    {
		"key": "alt+3",
		"command": "workbench.action.tasks.runTask",
		"args": "SVN Show Log (Current File)"
    },
    {
		"key": "alt+4",
		"command": "workbench.action.tasks.runTask",
		"args": "SVN Show Log (Workspace)"
    },
    {
		"key": "alt+5",
		"command": "workbench.action.tasks.runTask",
		"args": "SVN Commit (Workspace)"
    },
```
# PC Windows远程虚拟机
## PC安装VSCodeUserSetup-x64-1.110.1.exe，安装后关闭自动更新，然后获取版本信息
版本: 1.110.1 (user setup)
提交: 61b3d0ab13be7dda2389f1d3e60a119c7f660cc3
日期: 2026-03-06T23:03:27.520Z
Electron: 39.6.0
ElectronBuildId: 13330601
Chromium: 142.0.7444.265
Node.js: 22.22.0
V8: 14.2.231.22-electron.0
OS: Windows_NT x64 10.0.19045
## 配置虚拟机vscode服务器
1. 下载服务器，61b3d0ab13be7dda2389f1d3e60a119c7f660cc3是PC vscode的版本哈希
```
https://vscode.download.prss.microsoft.com/dbazure/download/stable/61b3d0ab13be7dda2389f1d3e60a119c7f660cc3/vscode-server-linux-x64.tar.gz
```
2. 放在~/下
3. 创建install_vscode_server.sh
```
code_server=~/vscode-server-linux-x64.tar.gz
[ -d ~/.vscode-server/bin/${@} ] || mkdir -p ~/.vscode-server/bin/${@}
cd ~/.vscode-server/bin/${@} && \
    tar -xvzf $code_server --strip-components 1
```
4. chmod +x install_vscode_server.sh
5. bash install_vscode_server.sh 61b3d0ab13be7dda2389f1d3e60a119c7f660cc3
6. PC vscode免密登陆虚拟机
* 进入电脑用户目录下C:\Users\username，执行：
```
ssh-keygen -t rsa -b 4096
```
* 将生成的公钥id_rsa.pub内容放在虚拟机的~/.ssh/authorized_keys文件里
* vscode ssh config输入以下内容，即可免密登陆
```
Host 虚拟机
  HostName 192.168.11.53
  User root
  IdentityFile ~/.ssh/id_rsa
```
## 代码同步插件
1. 虚拟机安装global
```
apt install libncurses5-dev libncursesw5-dev
wget https://ftp.gnu.org/pub/gnu/global/global-6.6.6.tar.gz
./configure
make
make install
```
2. vscode远程客户机安装插件 GUN global
* 设置Auto Update配置为“Disable”，这是因为我们的工程文件太大，一般的文件大于50MB就要禁用此项功能。
* completion配置为“Disable”，是为了启用VSCode自带的代码自动补全功能，因为不禁用global自动补全功能的话，有些变量输入时无法进行自动补全。
* settings.json指定global路径
```
[hezz@ubu global-6.6 16:48]$which global;which gtags
/usr/local/bin/global
/usr/local/bin/gtags
```
```
    "gnuGlobal.globalExecutable": "/usr/local/bin/global",
    "gnuGlobal.gtagsExecutable": "/usr/local/bin/gtags",
```
3. 代码符号更新
在c++代码工程文件中，该工程目录终端中执行指令gtags，当代码改动时，cmd执行指令global -u即可更新这三个文件。
4. vscode远程客户机安装插件 gtags