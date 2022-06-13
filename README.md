auto import
eslint
prettier
vetur
auto close tag
auto rename tag
bracket-pair-colorizer-2
{
  "javascript.preferences.quoteStyle": "single",
  "workbench.colorTheme": "Default Light+",
  "files.autoSave": "onFocusChange",
  "eslint.format.enable": true,
  "eslint.nodeEnv": "",
  // 重新设定tabsize
  "editor.tabSize": 2,
  "editor.fontSize": 16,
  "debug.console.fontSize": 14,
  "terminal.integrated.fontSize": 14,
  "explorer.confirmDragAndDrop": false,
  "bracket-pair-colorizer-2.colors": [
    "Orchid",
    "YellowGreen",
    "Orange"
  ],
  "[typescript]": {
    "editor.defaultFormatter": "vscode.typescript-language-features"
  },
  "[json]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  // vscode默认启用了根据文件类型自动设置tabsize的选项
  "editor.detectIndentation": false,
  // #每次保存的时候自动格式化 
  "editor.formatOnSave": true,
  //  #代码结尾的分号 
  "prettier.semi": true,
  //  #使用带引号替代双引号 
  "prettier.singleQuote": true,
  //  #让函数(名)和后面的括号之间加个空格
  "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
  // #这个按用户自身习惯选择 
  "vetur.format.defaultFormatter.html": "js-beautify-html",
  // #让vue中的js按编辑器自带的ts格式进行格式化 
  "vetur.format.defaultFormatter.js": "vscode-typescript",
  "vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
      "wrap_attributes": "auto", // #vue组件中html代码格式化样式 force-aligned
      "prettier": {
        "semi": true,
        "singleQuote": true
      }
    }
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }, // 两个选择器中是否换行
  "vetur.experimental.templateInterpolationService": false, // vue template代码中，有ts的提示
  "workbench.iconTheme": "vscode-icons",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "[less]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  //导入文件时是否携带文件的拓展名
  "path-autocomplete.extensionOnImport": true,
  //配置@的路径提示
  "path-autocomplete.pathMappings": {
    "@": "${folder}/src/render"
  },
}
