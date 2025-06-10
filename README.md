## My Mac Setup

This repo contains info on apps / tools / settings I use on my Mac.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [OS Settings](#os-settings)
  - [Desktop](#desktop)
  - [Finder](#finder)
  - [Dock](#dock)
  - [Useful Shortcuts](#useful-shortcuts)
- [Quick Launching](#quick-launching)
- [Homebrew](#homebrew)
  - [Homebrew](#homebrew-1)
  - [RayCast Homebrew Plugin](#raycast-homebrew-plugin)
- [App Switching](#app-switching)
- [Chrome](#chrome)
- [Terminal](#terminal)
  - [Shell](#shell)
- [Git](#git)
  - [Global Settings](#global-settings)
  - [Github SSH Setup](#github-ssh-setup)
  - [Multiple SSH Keys for Git Platforms](#multiple-ssh-keys-for-git-platforms)
- [Node.js](#nodejs)
  - [Global Modules](#global-modules)
- [VS Code](#vs-code)
  - [Extensions](#extensions)
  - [Settings](#settings)
  - [Useful Shortcuts](#useful-shortcuts-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## OS Settings

These are my preferred settings for `Desktop`, `Finder` and the `Dock`.

### Desktop

I don't like the new Desktop, Stage Manager or Widget features in Sonoma, so I disable them.

* System Preferences
  * Desktop & Dock
    * Desktop & Stage Manager
      * Show Items
        * On Desktop -> uncheck
        * In Stage Manager -> uncheck
      * Click wallpaper to reveal desktop -> Only in Stage Manager
      * Stage Manager -> uncheck
      * Widgets
        * On Desktop -> uncheck
        * In Stage Manager -> uncheck

### Finder

* Finder -> Preferences
  * General -> Show these on the desktop -> Select None
      * I try to keep my desktop completely clean.
  * General -> New Finder windows show -> Home Folder
      * I prefer to see my home folder in each new finder window instead of recent documents
  * Advanced -> Show all filename extensions -> Yes
  * Advanced -> Show warning before changing an extension -> No
  * Advanced -> When performing a search -> Search the current folder
* View
  * Show Status Bar
  * Show Path Bar
  * Show Tab Bar

### Dock

I don't use the Dock at all. It takes up screen space, and I can use RayCast to launch apps and AltTab to switch between apps. I make the dock as small as possible and auto hide it.

* System Preferences
  * Desktop & Dock
    * Size -> Small as possible
    * Position on screen -> Left
    * Automatically hide and show the Dock -> Yes
    * Animate opening applications -> No
    * Show suggested and recent apps in the Dock -> No

### Useful Shortcuts

* Find [here](/shortcuts.md) useful Mac shortcuts.

## Quick Launching

I replaced the built in Spotlight search with [RayCast](https://www.raycast.com/). 

```sh
brew install raycast
```

## Homebrew

### Homebrew

[Homebrew](https://brew.sh/) allows us to install tools and apps from the command line.

To install it, open up the built in `Terminal` app and run this command:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

This will also install the xcode build tools which is needed by many other developer tools.

After Homebrew is done installing, we will use it (via RayCast) to install everything else we need.


### RayCast Homebrew Plugin

Install the [RayCast Homebrew Plugin](https://www.raycast.com/nhojb/brew) so we can easily install formulae and casks directly from RayCast.

## App Switching

The built in App switcher only shows application icons, and only shows 1 icon per app regardless of how many windows you have open in that app.

I use an app switcher called [AltTab](https://alt-tab-macos.netlify.app/). It shows full window previews, and has an option to show a preview for every open window in all applications (even minimized ones).

I replace the built-in `CMD+TAB` shortcut with AltTab.

Search for `alt-tab` in RayCast `brew search` or:

```sh
brew install alt-tab
```

## Terminal

I prefer [iTerm2](https://iterm2.com/) because:
* Lots of customization options
* Clickable links
* Native OS notifications

There are a lot of options for a terminal replacement, but I've been using iTerm2 for years and it works great for my needs.

Checkout their documentation for more info on what iTerm2 can do: [https://iterm2.com/documentation.html](https://iterm2.com/documentation.html)


```
brew install iterm2
```

Once installed, launch it and customize the settings / preferences to your liking. These are my preferred settings:

* Appearance
  * Theme
    * Minimal
* Profiles
  * Default
      * General -> Working Directory -> Reuse previous session's directory
      * Colors -> Basic Colors -> Foreground -> Lime Green
      * Text -> Font -> Anonymous Pro
          * You can download this font [here](https://www.marksimonson.com/fonts/view/anonymous-pro).
          * I use this font in VS Code as well
      * Text -> Font Size -> 36
          * I use my Macbook to present / teach, so a big font size is important so everyone can see the commands I'm typing
      * Keys -> Key Mappings -> Presets -> Natural Text Editing
          * This allows me to use the [keyboard shortcuts](https://gist.github.com/w3cj/022081eda22081b82c52) I know and love inside of iTerm2

### Shell

Mac now comes with `zsh` as the default [shell](https://en.wikipedia.org/wiki/Comparison_of_command_shells). I've switched to using this with [Oh My Zsh](https://ohmyz.sh/).

Config for **Oh My Zsh** in `~/.zshrc`:

```bash
export ZSH="$HOME/.oh-my-zsh"

ZSH_THEME="robbyrussell"
CASE_SENSITIVE="true"
ENABLE_CORRECTION="true"

plugins=(git)

source $ZSH/oh-my-zsh.sh

export EDITOR='nano'
```

[Powerlevel10k](https://github.com/romkatv/powerlevel10k) is a popular theme.

## Git

### Global Settings

To see all your global Git configurations, run:

```bash
git config --global --list
```

This command will display all the settings stored in your global Git configuration file, typically located at `~/.gitconfig`. This is my config:

```bash
[user]
    name = Your Name
    email = your.email@example.com
[core]
    editor = nano
    excludesfile = /Users/marcel/.gitignore_global
[init]
    defaultBranch = main
[pager]
    branch = false
```

Config in `~/.gitignore_global`:

```bash
.DS_Store
```

### Github SSH Setup

* Follow [this guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to setup an ssh key for github
* Follow [this guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) to add the ssh key to your github account

### Multiple SSH Keys for Git Platforms
* Follow [this guide](/multiple-ssh-config.md) to set up distinct SSH keys on your Mac for different Git platforms (like GitHub and GitLab)

## Node.js

I use nvm to manage the installed versions of Node.js on my machine. This allows me to easily switch between Node.js versions depending on the project I'm working in.

See installation instructions [here](https://github.com/nvm-sh/nvm#installing-and-updating).

OR run this command (make sure v0.39.7 is still the latest)

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

Now that nvm is installed, you can install a specific version of node.js and use it:

```sh
nvm install 20
nvm use 20
node --version
```

### Global Modules

There are a few global node modules I use a lot:

* lite-server
  * Auto refreshing static file server. Great for working on static apps with no build tools.
* http-server
  * Simple static file server.
* license
  * Auto generate open source license files
* gitignore
  * Auto generate `.gitignore` files base on the current project type

```
npm install -g lite-server http-server license gitignore
```

## VS Code

VS Code is my preferred code editor, it includes a command to add itself to the PATH (be sure it's located in the Applications directory):

1.  Open **VS Code**.
2.  Press `Cmd + Shift + P` to open the **Command Palette**.
3.  Type `Shell Command` in the search bar.
4.  Select the command **"Shell Command: Install 'code' command in PATH"**.
5.  After it runs, you may need to **restart your terminal**.

Once you have completed these steps, you can open any folder or the current directory in VS Code directly from your command line by typing:

```bash
code .
```

### Extensions

* Theme / Editor Experience
  * [vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons)
    * Nice / colorful icons for many different file types
  * [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
    * Integrates ESLint JS
  * [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
    * Automatically format javascript, JSON, CSS, Sass
  * [PostCSS Intellisense and Highlighting](https://marketplace.visualstudio.com/items?itemName=vunguyentuan.vscode-postcss)
    * Works better than the other more popular one of a similar name.
  * [Pretty TypeScript Errors](https://marketplace.visualstudio.com/items?itemName=yoavbls.pretty-ts-errors)
    * Makes TS errors more human readable.
* Languages and Libraries
  * [Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)
    * Intelligent Tailwind CSS tooling for VS Code.
  * [ES7+ React/Redux/React-Native snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)
    * Extensions for React, React-Native and Redux in JS/TS with ES7+ syntax. Customizable. Built-in integration with prettier.
  * [Svelte for VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode)
    * Svelte language support for VS Code.
  * [Prisma](https://marketplace.visualstudio.com/items?itemName=Prisma.prisma)
    * Adds syntax highlighting, formatting, auto-completion, jump-to-definition and linting for .prisma files.

### Settings

To directly edit your user settings in the `settings.json` file, follow these steps:

1.  Open Visual Studio Code.

2.  Open the Command Palette using the keyboard shortcut:
    ```
    Cmd + Shift + P
    ```

3.  In the search bar that appears, type `settings json`.

4.  Select the **Preferences: Open User Settings (JSON)** option from the dropdown list and press `Enter`.

5.  The `settings.json` file will open in a new editor tab. You can now add or modify any setting using key-value pairs.

```json
{
    "editor.multiCursorModifier": "ctrlCmd",
    "diffEditor.ignoreTrimWhitespace": false,
    "editor.detectIndentation": true,
    "editor.fontSize": 13,
    "editor.formatOnPaste": false,
    "editor.inlineSuggest.enabled": true,
    "editor.snippetSuggestions": "top",
    "editor.suggestSelection": "first",
    "editor.lineHeight": 0,
    "editor.minimap.enabled": false,
    "editor.linkedEditing": true,
    "editor.tabSize": 2,
    "editor.unicodeHighlight.invisibleCharacters": false,
    "emmet.showAbbreviationSuggestions": false,
    "terminal.integrated.fontSize": 13,
    "workbench.startupEditor": "newUntitledFile",
    "workbench.iconTheme": "vscode-icons",
    "vsicons.dontShowNewVersionMessage": true,
    "workbench.editor.labelFormat": "medium",
    "workbench.editor.showTabs": "none",
    "eslint.enable": true,
    "eslint.validate": [
        "vue",
        "react",
        "typescript",
        "html",
        "javascript"
    ],
    "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[handlebars]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[html]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascriptreact]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[json]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[jsonc]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[markdown]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[scss]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[svelte]": {
        "editor.defaultFormatter": "svelte.svelte-vscode"
    },
    "[typescript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[typescriptreact]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
}
```

### Useful Shortcuts

* Find [here](/shortcuts.md) most used VS code shortcuts.

## Chrome
