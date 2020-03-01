---
layout: page
title: 使用VSCode进行GDB调试
image: https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Visual_Studio_Code_1.35_icon.svg/256px-Visual_Studio_Code_1.35_icon.svg.png
hero_image: https://www.bing.com/th?id=OHR.OtterCreekVT_ZH-CN0564511657_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp
toc: true
tags: VSCode GDB
published: true
---

<!--more-->

### launch.json配置实例
```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "<put your executable file path here>",
            "args": [],
            //"additionalSOLibSearchPath": <i try to use this option , but it's not work>,
            "stopAtEntry": false, //stopped at main function
            "cwd": "${workspaceFolder}",
            "environment": [
                { "name": "LD_LIBRARY_PATH", "value": "<put library search path here, seperated by comma>" }
            ],
            "externalConsole": false,
            "MIMode": "gdb",
            "windows": {
                "program": "${fileDirname}/${fileBasenameNoExtension}",
                "miDebuggerPath": "/usr/bin/gdb"
            },
            "logging": { "engineLogging": true },
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                },
                /* // this gdb command will be executed when setup
                   // when fork follow child/parent(default)
                {
                    "description":"Follow the child process",
                    "text": "set follow-fork-mode child",
                    "ignoreFailures": true
                }
                */
            ],
            //"preLaunchTask": "g++"
        },
        {
            "name": "C++ Attach",
            "type": "cppdbg",
            "request": "attach",
            "program": "<put your executable file here>",
            "processId": "${command:pickProcess}", // 要Attach的进程ID
            "linux": {
                "MIMode": "gdb",
                "setupCommands": [{
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }]
            },
            "osx": {
                "MIMode": "lldb"
            },
            "windows": {
                "MIMode": "gdb",
                "setupCommands": [{
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }]
            }
        }
    ]
}
```

### 快捷使用
> launch类型

1. 默认Debug已经绑定了F5快捷键

> attach类型

1. <ctrl-p>进入panel
2. ?
3. 选择C++ Attach 
4. 在源文件中打断点
