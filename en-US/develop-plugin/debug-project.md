## プロジェクトをデバッグする。

このページでは [プラグイン用のC#プロジェクトを作成する。](https://beditor.net/Document?page=/develop-plugin/create-project) で作成したプロジェクトを Visual Studio Code でデバッグする方法を説明します。

### ワークスペースを作成。

`<プロジェクト名>.code-workspace` ファイルを作成します。
以下をペーストします。

``` Json
{
	"folders": [
		{
			"path": "."
		}
	]
}
```

### tasks.json を作成。

`./.vscode/tasks.json` を作成します。
以下をペーストします。

``` Json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "command": "dotnet",
            "type": "process",
            "args": [
                "build",
                "${workspaceFolder}/<プロジェクト名>.csproj",
                "/property:GenerateFullPaths=true",
                "/consoleloggerparameters:NoSummary"
            ],
            "problemMatcher": "$msCompile"
        },
        {
            "label": "publish",
            "command": "dotnet",
            "type": "process",
            "args": [
                "publish",
                "${workspaceFolder}/<プロジェクト名>.csproj",
                "/property:GenerateFullPaths=true",
                "/consoleloggerparameters:NoSummary"
            ],
            "problemMatcher": "$msCompile"
        },
        {
            "label": "watch",
            "command": "dotnet",
            "type": "process",
            "args": [
                "watch",
                "run",
                "${workspaceFolder}/<プロジェクト名>.csproj",
                "/property:GenerateFullPaths=true",
                "/consoleloggerparameters:NoSummary"
            ],
            "problemMatcher": "$msCompile"
        }
    ]
}
```

### tasks.json を作成。

`./.vscode/launch.json` を作成します。
以下をペーストします。

``` Json
{
    "version": "0.2.0",
    "configurations": [
        {
            // Use IntelliSense to find out which attributes exist for C# debugging
            // Use hover for the description of the existing attributes
            // For further information visit https://github.com/OmniSharp/omnisharp-vscode/blob/master/debugger-launchjson.md
            "name": ".NET Core Launch (console)",
            "type": "coreclr",
            "request": "launch",
            "preLaunchTask": "build",
            // If you have changed target frameworks, make sure to update the program path.
            "program": "<実行ファイルがあるフォルダ>/beditor.dll",
            "args": [],
            "cwd": "<実行ファイルがあるフォルダ>",
            // For more information about the 'console' field, see https://aka.ms/VSCode-CS-LaunchJson-Console
            "console": "internalConsole",
            "stopAtEntry": false
        },
        {
            "name": ".NET Core Attach",
            "type": "coreclr",
            "request": "attach",
            "processId": "${command:pickProcess}"
        }
    ]
}
```

<実行ファイルがあるフォルダ> や <プロジェクト名> は置き換えてください。