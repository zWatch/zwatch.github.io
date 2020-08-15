# Godot Tools

A complete set of tools to code games with [Godot Engine](http://www.godotengine.org/) in Visual Studio Code.

**IMPORTANT NOTE:** Versions 1.0.0 and later of this plugin only support Godot 3.2 or later.

## Features

The extension comes with a wealth of features to make your Godot programming experience as comfortable as possible:

该扩展程序具有许多功能，可以使您的Godot编程体验尽可能舒适：

- Syntax highlighting for the GDScript (`.gd`) language

  GDScript（.gd）语言的语法突出显示

- Syntax highlighting for the `.tscn` and `.tres` scene formats

  .tscn和.tres场景格式的语法高亮显示

- Full typed GDScript support

  全类型GDScript支持

- Optional "Smart Mode" to improve productivity with dynamically typed scripts

  可选的“智能模式”，可通过动态键入脚本提高生产力

- Function definitions and documentation display on hover (see image below)

  功能定义和文档显示在悬停上（请参见下图）

- Rich autocompletion

  丰富的自动补全

- Display script warnings and errors

  显示脚本警告和错误

- Ctrl + click on a variable or method call to jump to its definition

  Ctrl +单击变量或方法调用以跳转至其定义

- Full documentation of the Godot Engine's API supported

  支持Godot引擎API的完整文档

- Run a Godot project from VS Code

  从VS Code运行Godot项目

- Debug your Godot project from VS Code with breakpoints, step-in/out/over, variable watch, call stack, and active scene tree

  使用断点，移入/移出/移出，变量监视，调用堆栈和活动场景树从VS Code调试您的Godot项目

![Showing the documentation on hover feature](https://github.com/godotengine/godot-vscode-plugin/raw/master/img/godot-tools.png)

## Available commands

The extension adds a few entries to the VS Code Command Palette under "Godot Tools":

该扩展在“ Godot工具”下的VS代码命令面板中添加了一些条目：

- Open workspace with Godot editor

  使用Godot编辑器打开工作区

- Run the workspace as a Godot project

  将工作空间作为Godot项目运行

- List Godot's native classes

  列出Godot的native classes

## Settings

### Godot

If you like this extension, you can set VS Code as your default script editor for Godot by following these steps:

如果您喜欢此扩展程序，则可以按照以下步骤将VS Code设置为Godot的默认脚本编辑器：

1. Open the **Editor Settings**
2. Select **Text Editor > External**
3. Make sure the **Use External Editor** box is checked
4. Fill **Exec Path** with the path to your VS Code executable
5. Fill **Exec Flags** with `{project} --goto {file}:{line}:{col}`

### VS Code

You can use the following settings to configure Godot Tools:

您可以使用以下设置来配置Godot工具：

- `editor_path` - The absolute path to the Godot editor executable.

  Godot编辑器可执行文件的绝对路径。

- `gdscript_lsp_server_port` - The WebSocket server port of the GDScript language server.

  GDScript语言服务器的WebSocket服务器端口。

- `check_status` - Check the GDScript language server connection status.

  检查GDScript语言服务器的连接状态。

#### Debugger

To configure the debugger:

要配置调试器：

1. Open the command palette:打开命令面板：
2. `>Debug: Open launch.json`
3. Select the Debug Godot configuration.选择“调试Godot”配置。
4. Change any relevant settings.更改任何相关设置。
5. Press F5 to launch.按F5键启动。

*Configurations*

*Required*

- "project": Absolute path to a directory with a project.godot file. Defaults to the currently open VSCode workspace with `${workspaceFolder}`.

  “ project”：包含project.godot文件的目录的绝对路径。 默认为当前打开的VSCode工作区，带有$ {workspaceFolder}`。

- "port": Number that represents the port the Godot remote debugger will connect with. Defaults to `6007`.

  “ port”：代表Godot远程调试器将连接的端口的数字。 默认为6007

- "address": String that represents the IP address that the Godot remote debugger will connect to. Defaults to `127.0.0.1`.

  “ address”：代表Godot远程调试器将连接到的IP地址的字符串。 默认为127.0.0.1

*Optional*

- "launch_game_instance": true/false. If true, an instance of Godot will be launched. Will use the path provided in `editor_path`. Defaults to `true`.

  “ launch_game_instance”：是/否。 如果为true，将启动Godot实例。 将使用`editor_path`中提供的路径。 默认为true。

- "launch_scene": true/false. If true, and launch_game_instance is true, will launch an instance of Godot to a currently active opened TSCN file. Defaults to `false`.

  “ launch_scene”：是/否。 如果为true，且launch_game_instance为true，则将Godot实例启动到当前活动的已打开TSCN文件。 默认为`false`。

- "scene_file": Path *relative to the project.godot file* to a TSCN file. If launch_game_instance and launch_scene are both true, will use this file instead of looking for the currently active opened TSCN file.

  “ scene_file”：相对于project.godot文件*到TSCN文件的路径。 如果launch_game_instance和launch_scene都为true，则将使用此文件而不是查找当前活动的已打开TSCN文件。

*Usage*

- Stacktrace and variable dumps are the same as any regular debugger

  Stacktrace和variable dumps 与任何常规调试器相同

- The active scene tree can be refreshed with the Refresh icon in the top right.

  可以使用右上角的“刷新”图标来刷新活动场景树。

- Nodes can be brought to the fore in the Inspector by clicking the Eye icon next to nodes in the active scene tree, or Objects in the inspector.

  通过单击活动场景树中的节点或检查器中的“对象”旁边的“眼睛”图标，可以使节点在检查器中脱颖而出。

- You can edit integers, floats, strings, and booleans within the inspector by clicking the pencil icon next to each.

  通过单击检查器旁边的铅笔图标，可以在检查器中编辑它们的整数，浮点数，字符串和布尔值。

![Showing the debugger in action](https://github.com/godotengine/godot-vscode-plugin/raw/master/img/godot-debug.png)

## Issues and contributions

The [Godot Tools](https://github.com/godotengine/godot-vscode-plugin) extension is an open source project from the Godot organization. Feel free to open issues and create pull requests anytime.

[Godot工具]（https://github.com/godotengine/godot-vscode-plugin）扩展是Godot组织的一个开源项目。 随时打开问题并随时创建拉取请求。

See the [full changelog](https://github.com/GodotExplorer/godot-tools/blob/master/CHANGELOG.md) for the latest changes.

### Building from source

#### Requirements

- [npm](https://www.npmjs.com/get-npm)
- Typescript to compile, installed using npm with `npm install -g typescript`
- VSCE to create a VSIX file, installed using npm with `npm install -g vsce`

#### Process

1. Open a command prompt/terminal and browse to the location of this repository on your local filesystem.
2. Download dependencies by using the command `npm install`
3. When done, package a VSIX file by using the command `vsce package`.
4. Install it by opening Visual Studio Code, opening the Extensions tab, clicking on the More actions (**...**) button in the top right, and choose **Install from VSIX...** and find the compiled VSIX file.

When developing for the extension, you can open this project in Visual Studio Code and debug the extension by using the **Run Extension** launch configuration instead of going through steps 3 and 4. It will launch a new instance of Visual Studio Code that has the extension running. You can then open a Godot project folder and debug the extension or GDScript debugger.

## FAQ

### Why does it fail to connect to the language server?

- Godot 3.2 or later is required.

- Make sure to open the project in the Godot editor first. If you opened the editor after opening VS Code, you can click the **Retry** button in the bottom-right corner in VS Code.

  确保首先在Godot编辑器中打开项目。 如果在打开VS Code后打开了编辑器，则可以单击VS Code右下角的** Retry **按钮。

### Why isn't IntelliSense displaying script members?

- GDScript is a dynamically typed script language. The language server can't infer all variable types.

  GDScript是一种动态类型的脚本语言。 语言服务器无法推断所有变量类型。

- To increase the number of results displayed, open the **Editor Settings**, go to the **Language Server** section then check **Enable Smart Resolve**.

  要增加显示的结果数量，请打开“编辑器设置”，转到“语言服务器”部分，然后选中“启用智能解析”。