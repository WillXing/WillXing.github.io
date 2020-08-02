---
layout: post
title:  "Create vscode extension"
author: "Will Xing"
categories:
  - javascript
  - blog
date: 2020-07-31T00:00:00Z
---

When I was writing some article with [vscode](https://en.wikipedia.org/wiki/VSCode), usually I will need to refer to some external link from [wikipedia](https://en.wikipedia.org/wiki/Wikipedia) which can help to get more extended reading from my article.

But I don't want to open browser and search in wiki, then copy past link in the markdown link format, so I created this extension called [Wiki Link](https://marketplace.visualstudio.com/items?itemName=willxing.wiki-link).

Here just to share about how to create a vscode extension.

## Create hello world project in 5 mins

Assumption here is you alread have [Node.js](https://en.wikipedia.org/wiki/Node.js) & [npm](https://en.wikipedia.org/wiki/Npm_(software)) installed in your local.

Firstly, install Yeoman and vscode extension generator

```bash
npm install -g yo generator-code
```

Then we can use it to create the project by simple use Yeoman

```bash
yo code
```

And follow the prompt to create the project. Let's say the project name is `vsext`

Now under `vsext` it's the extension project!

## Develop

When developing vscode extension, mostly using `vscode` which imported by `import * as vscode from 'vscode';`.

The Api docs are here: https://code.visualstudio.com/api/references/vscode-api

Here just give a simple example of `showInputBox`, and show information box of the input.

```js
let disposable = vscode.commands.registerCommand('your-command.command', async () => {
  const options: vscode.InputBoxOptions = {
    placeHolder: "Input something"
  }
  const inputText = await vscode.window.showInputBox(options);
  vscode.window.showInformationMessage(inputText);
}
```

Explain this line by line.

Firstly, `vscode.commands.registerCommand(commandKey, callback)`, this is to register the command in vscode. 2 paramerters here:

1. commandKey: This is the key you registered in `package.json` like following
```json
"contributes": {
    "commands": [
      {
        "command": "command.key",
        "title": "Human readable command title"
      }
    ]
  },
```
2. callback: When selected the command match with commandKey from the command prompt, this callback will get called.

You can have some other way to invoke callback, like `keybidings`.

Inside call back, we got our function living there, first we create an `InputBoxOptions`, this is a pre configuration for `showInputBox` method, detail can read in the API doc. There are many more options for different type of interaction, for example like `showOpenDialog` will use `OpenDialogOptions`.

After that, there's the place we really open the inputBox to customer. `const inputText = await vscode.window.showInputBox(options);`, the `showInputBox` function will actually return a object which type called `Thenable` in the API doc, I believe it's wrap by promise so here we can just `await` to get the real value from it.

Then here is another window function called `showInformationBox` which is a box at the right bottom of vscode we usually see, and in the box we can have our text living there.

## Run extension locally

Use vscode to open the project, then hit `F5` or just click in menu `Run -> Start Debugging` will start another vscode window which running the extension in.

Here's how our code running like:

![extension demo](/assets/images/extension-demo.gif)

## Publish extension

### Get Azure Devops token

Firsly, login to Azure Devops site: https://dev.azure.com/, and create a personal access token by click the menu item on right top, find `Personal access tokens`.

New a token there, and for the scope of the token, give all access to the `MarketPlace`.

### Create publisher with vsce

Install vsce by `npm i -g vsce`. Then can run `vsce create-publisher [your publisher name]`. After that, put the publisher information in `package.json`

```json
{
  //...
  "publisher": "willxing"
}
```

### Use vsce to pack and publish

First command is `vsce package`, this can pack your code, and put them in `out` folder.

Then run command `vsce publish`, this will publish your extension to marketplace.

That's it! Your extension should show up in marketplace after couple minutes.

## Some animation gif to show how to use the plugin

Firsly, can turn on the screencast mode in vscode, by use command tool to find the toggle and just select it.

Then can use some screen recording tool, like [LICEcap](https://www.cockos.com/licecap/) to generate GIF of the screen.
