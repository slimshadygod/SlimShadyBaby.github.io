---
title: vscode配置
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-13 19:56:30
password:
summary: vscode基本配置
tags:
- vscode
categories:
- vscode
---

## settings

``` json
    {
    "editor.suggestSelection": "first",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    // "python.autoComplete.extraPaths": ["C:\\Python36\\Lib\\site-packages","C:\\Python36\\Scripts", "C:\\Python27\\Lib\\site-packages"],
    "python.autoComplete.extraPaths": [
        "C:\\Users\\BBD\\anaconda3\\Lib\\site-packages",
        "C:\\Users\\BBD\\anaconda3\\Scripts"
    ],
    "python.autoComplete.addBrackets": true,
    "python.pythonPath": "C:\\Users\\BBD\\anaconda3\\python",
    "[python]": {
        "editor.defaultFormatter": "ms-python.python"
    },
    "python.languageServer": "Pylance",
    "code-runner.runInTerminal": true,
    "code-runner.cwd": "$pythonPath $fullFileName",
    // "code-runner.fileDirectoryAsCwd": true, //使用要执行的文件目录
    "code-runner.executorMap": {
        // "python": "python3 -u",
        "python": "$pythonPath $fullFileName",
    },
    "code-runner.executorMapByGlob": {
        // 预发布
        // "*.txt": "-v URL:http://dataapiv2.gray.bbdops.com -v dbHost:mysql.read.bbdops.com -v dbPort:53606 -v dbUserName:dp_app_reader -v dbPassword:bEisnDlBMrUWdnvp -v newDbHost:10.28.121.11 -v newDbPort:60114 -v newDbUserName:bbd_dp_read -v newDbPassword:v9PkZlUZ6CvRL1iYj6gs -d results $fullFileName",
        // 正式
        "*.txt": "robot -v URL:http://dataapi.bbdservice.com -v dbHost:mysql.read.bbdops.com -v dbPort:53606 -v dbUserName:dp_app_reader -v dbPassword:bEisnDlBMrUWdnvp -v newDbHost:10.28.121.11 -v newDbPort:60114 -v newDbUserName:bbd_dp_read -v newDbPassword:v9PkZlUZ6CvRL1iYj6gs -d results $fullFileName",
        // 测试
        // "*.txt": "-v URL:http://10.28.200.214:18080 -v dbHost:10.28.100.51 -v dbPort:3306 -v dbUserName:root -v dbPassword:Dataom123!@## -v newDbHost:10.28.100.51 -v newDbPort:3306 -v newDbUserName:root -v newDbPassword:Dataom123!@## -d results  $fullFileName",
    },
    "python.linting.enabled": true,
    "python.linting.pylintEnabled": false,
    "python.linting.pycodestyleEnabled": true,
    "python.linting.pycodestyleArgs": [
        "--max-line-length=79",
        "--ignore=E501"
    ],
    "python.linting.flake8Enabled": true,
    "python.linting.flake8Args": [
        "--max-line-length=79",
        "--ignore=E501"
    ],
    "python.formatting.provider": "yapf",
    "files.autoSave": "onWindowChange",
    // "files.autoSaveDelay": 1000,
    "editor.fontFamily": "Consolas, 'Courier New', Dengxian",
    "editor.fontSize": 15,
    "editor.fontLigatures": true,
    "editor.tokenColorCustomizations": {
        "textMateRules": [
        {
            "name": "Comment",
            "scope": [
            "comment",
            "comment.block",
            "comment.block.documentation",
            "comment.line",
            "comment.line.double-slash",
            "punctuation.definition.comment",
            ],
            "settings": {
            "fontStyle": "italic",
            //斜体 "fontStyle": "italic",
            //斜体下划线 "fontStyle": "italic underline",
            //斜体粗体下划线 "fontStyle": "italic bold underline",
            }
        },
        ]
    },
    "search.exclude": {
        "**/.history": true
    },
    "files.associations": {
        "*.txt": "robot"
    },
    "rfLanguageServer.includePaths": [
        "**/*.robot",
        "**/*.py",
        "**/*.txt"
    ],
    "rfLanguageServer.libraries": [
        "BuiltIn-3.0.4",
        "DateTime-3.0.4",
        "Collections-3.0.4",
        "OperatingSystem-3.0.4"
    ],
    "rfLanguageServer.pythonKeywords": true,
    "python.autoUpdateLanguageServer": false,
    "vsintellicode.features.python.deepLearning": "enabled",
    "files.trimTrailingWhitespace": true,
    "java.semanticHighlighting.enabled": true,
    "files.exclude": {
        "**/.classpath": true,
        "**/.project": true,
        "**/.settings": true,
        "**/.factorypath": true
    },
    "sync.gist": "",
    "maven.executable.path": "E:\\apache-maven-3.5.0\\bin\\mvn.cmd",
    "maven.terminal.useJavaHome": true,
    "maven.terminal.customEnv": [
        {
        "environmentVariable": "JAVA_HOME",
        "value": "C:\\Program Files\\Java\\jdk1.8.0_231"
        }
    ],
    "java.home": "C:\\Program Files\\Java\\jdk-11.0.8",
    "java.configuration.runtimes": [
        {
        "name": "JavaSE-1.8",
        "path": "C:\\Program Files\\Java\\jdk1.8.0_231",
        "default": true
        },
        {
        "name": "JavaSE-11",
        "path": "C:\\Program Files\\Java\\jdk-11.0.8",
        }
    ],
    "java.test.config": [
        {
        "name": "myConfiguration",
        "workingDirectory": "${workspaceFolder}",
        }
    ],
    "java.saveActions.organizeImports": true,
    "java.debug.settings.console": "internalConsole",
    "quarkus.tools.alwaysShowWelcomePage": false,
    "python.analysis.extraPaths": [
        "C:\\Users\\BBD\\anaconda3\\Lib\\site-packages",
        "C:\\Users\\BBD\\anaconda3\\Scripts"
    ],
    "python.analysis.completeFunctionParens": true,
    "kite.showWelcomeNotificationOnStartup": false,
    }
```

## python文件头

1. 依次点击"File" -> "Preference" -> "User Snippets"

2. 在输入框输入"python",会自动生成一个python.json文件

3. 把如下配置拷贝到python.json文件中

4. 在写python的时候,输入prefix中的值"header",即可快速添加头部信息

    ![设置](settings.gif)

    ``` json
        {
            "HEADER": {
                "prefix": "header",
                "body": [
                    "#!/usr/bin/env python",
                    "## -*- encoding: utf-8 -*-",
                    "'''",
                    "@Author: Hulin",
                    "@Date: ${CURRENT_YEAR}-${CURRENT_MONTH}-${CURRENT_DATE} ${CURRENT_HOUR}:${CURRENT_MINUTE}:${CURRENT_SECOND}",
                    "@Description: $0",
                    "'''",
                ],
            }
        }
    ```
