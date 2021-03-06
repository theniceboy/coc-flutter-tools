# ![](https://flutter.dev/images/favicon.png) coc-flutter-tools

Flutter support for (Neo)vim

![2019-10-07 23-31-40 2019-10-08 00_04_07](https://user-images.githubusercontent.com/5492542/66328510-58a6c480-e95f-11e9-95ca-0b4ed7c8e83f.gif)

## What is this?
`coc-flutter-tools` is an active fork of [coc-flutter](https://github.com/iamcco/coc-flutter), that fixes bugs and adds new features.

Big thanks to the author of `coc-flutter` [@iamcco](https://github.com/iamcco)

Features of this fork:
- DevTools support
- Better DevLog Notifications (Notification Filtering)
- UI Path Display
- Flutter Outline Panel

![Flutter Outline](https://user-images.githubusercontent.com/8187501/90091803-eca0a400-dd59-11ea-9bee-2401c85ddff9.gif)


## Features

> Make sure that you have the Flutter SDK path in the `PATH` environment variable
- LSP features (power by [analysis_server](https://github.com/dart-lang/sdk/blob/master/pkg/analysis_server/tool/lsp_spec/README.md))
  - Auto-completion
  - Diagnostics
  - Auto-formatting
  - Renaming elements
  - Hovering support
  - Signature help
  - Jumping to definitions/implementations/references
  - Highlighting
  - Widget trees (flutter outline)
  - Document symbols
  - Code actions
  - [More detail](https://github.com/dart-lang/sdk/blob/master/pkg/analysis_server/tool/lsp_spec/README.md)
- Automatic hot reloading on save
- Automatically run `flutter pub get` on `pubspec.yaml` changes
- Flutter Dev Server support
- Snippets (enable with `flutter.provider.enableSnippet`)
- Device/Emulator List
- SDK switching

## Installation

`:CocInstall coc-flutter-tools`

> **NOTE**: The [dart-vim-plugin](https://github.com/dart-lang/dart-vim-plugin) plugin is recommended (for filetype detection and syntax highlighting)

Most likely the extension will find your SDK automatically as long as the `flutter` command maps to an SDK location on your system.

If you are using a version manager like `asdf` that maps the `flutter` command to another binary instead of an SDK location or this extension cannot find your SDK for another reason you'll have to provide the extension with how to find your SDK.
To do this there are a few options:
1. If your version manager supports a `which` command like then you can set the `flutter.sdk.flutter-lookup` config option. Eg. `"flutter.sdk.flutter-lookup": "asdf which flutter"`.
2. You can add the path where to find the SDK to the `flutter.sdk.searchPaths` config option.
   Either specify the exact folder the SDK is installed in or a folder that contains other folders which directly have an SDK in them. *Note that not all of these folders need to have an SDK, if they don't contain one they will simply be ignored*
3. Set the `flutter.sdk.path` config option to the exact path you want to use for your SDK.
   If you have also set the `flutter.sdk.searchPaths` then you can use the `FlutterSdks` list (see below) to see what versions you have installed and set the config option for you. **Note that this means that the `flutter.sdk.path` option will be overriden by this list**


## coc-list sources

- FlutterSdks
  > `:CocList FlutterSdks`
  Shows all the sdks that can be found by using the `searchPaths` config and the `flutter-lookup` config options and allows you to switch between them, either only for your current workspace or globally.
  Besides those two ways to find sdks it also checks if you are using fvm and if so uses those directories to find your sdk.
  *You can disable this using the `flutter.fvm.enabled` config option.*

  You can also use this list to see what your current sdk is since it will have `(current)` behind it clearly.
	
- Flutter Devices: `:CocList FlutterDevices`
- Flutter Emulators: `:CocList FlutterEmulators`

## Settings

- `flutter.enabled` Enables `coc-flutter-tools`, default: `true`
- `flutter.lsp.initialization.onlyAnalyzeProjectsWithOpenFiles`: default: `true`
  > When set to true, analysis will only be performed for projects that have open files rather than the root workspace folder.
- `flutter.lsp.initialization.suggestFromUnimportedLibraries`: default: `true`
  > When set to false, completion will not include synbols that are not already imported into the current file
- [`flutter.lsp.initialization.closingLabels`](#closing-labels): default: `true`
  > When set to true, will display closing labels at end of closing, only neovim support.
- [`flutter.UIPath`](#ui-path): default: `true`
  > When set to true, will display the path of the selected UI component on the status bar
- `flutter.outlineWidth` controls the default width of the flutter outline panel. default: `30`
- `flutter.outlineIconPadding` controls The number of spaces between the icon and the item text in the outline panel. default: `0`
- `flutter.sdk.searchPaths` the paths to search for flutter sdks, either directories where flutter is installed or directories which contain directories where flutter versions have been installed
  eg. `/path/to/flutter` (command at `/path/to/flutter/bin/flutter`) or
  `~/flutter_versions` (command at `~/flutter_versions/version/bin/flutter`).
- `flutter.sdk.dart-command` dart command, leave empty should just work, default: `''`
- `flutter.sdk.dart-lookup` **only use this if you don't have a flutter installation but only dart** command to find dart executable location, used to infer dart-sdk location, default: `''`
- `flutter.sdk.flutter-lookup` command to find flutter executable location, used to infer location of dart-sdk in flutter cache: `''`
- `flutter.provider.hot-reload` Enable hot reload after save, default: `true`
  > only when there are no errors for the save file
- `flutter.provider.enableSnippet` Enable completion item snippet, default: true
  - `import '';` => `import '${1}';${0}`
  - `someName(…)` => `someName(${1})${0}`
  - `setState(() {});` => `setState(() {\n\t${1}\n});${0}`
- `flutter.openDevLogSplitCommand` Vim command to open dev log window, like: `botright 10split`, default: ''
- `flutter.workspaceFolder.ignore` Path start within the list will not treat as workspaceFolder, default: []
  - also flutter sdk will not treat as workspaceFolder, more detail issues [50](https://github.com/iamcco/coc-flutter/issues/50)
- `flutter.runDevToolsAtStartup` Automatically open devtools debugger web page when a project is run, default: false
- `flutter.autoOpenDevLog` Automatically open the dev log after calling flutter run, default: false
- `flutter.autoHideDevLog` Automatically hide the dev log when the app stops running, default: false


**Enable format on save**:

If you have [dart-vim-plugin](https://github.com/dart-lang/dart-vim-plugin) install, put this in your vimrc:
```vim
let g:dart_format_on_save = 1
```

Alternatively, you may use coc-settings.

```jsonc
"coc.preferences.formatOnSaveFiletypes": [
  "dart"
],
```

## Code Actions

`coc.nvim` provides code actions. To enable them, add the following configuration in your vimrc
> this can also be found in `coc.nvim` README

``` vim
xmap <leader>a  <Plug>(coc-codeaction-selected)
nmap <leader>a  <Plug>(coc-codeaction-selected)
```

To show code actions on selected areas:

- `<leader>aap` for current paragraph, `<leader>aw` for the current word

Then you will see a list of code actions:

- Wrap with Widget
- Wrap with Center
- etc

## Commands

Get a list of flutter related commands: `CocList --input=flutter commands`

**Global Commands**:

- `flutter.run` Run flutter dev server
- `flutter.attach` Attach running application
- `flutter.create` Create flutter project using: `flutter create`
- `flutter.doctor` Run: `flutter doctor`
- `flutter.upgrade` Run: `flutter upgrade`
- `flutter.pub.get` Run: `flutter pub get`
- `flutter.devices` open devices list
- `flutter.emulators` open emulators list
- `flutter.outline` opens up an instance of the flutter outline side-panel
- `flutter.toggleOutline` toggles the flutter outline side-panel

**LSP Commands**

- `flutter.gotoSuper` jump to the location of the super definition of the class or method

**Dev Server Commands**:

> These commands will only be available when the Flutter Dev Server is running (i.e after you run `flutter run`)

- `flutter.dev.quit` Quit server
- `flutter.dev.detach` Detach server
- `flutter.dev.hotReload` Hot reload
- `flutter.dev.hotRestart` Hot restart
- `flutter.dev.screenshot` To save a screenshot to flutter.png
- `flutter.dev.openDevLog` Open flutter dev server log
- `flutter.dev.clearDevLog` Clear the flutter dev server log
- `flutter.dev.debugDumpAPP` You can dump the widget hierarchy of the app (debugDumpApp)
- `flutter.dev.elevationChecker` To toggle the elevation checker
- `flutter.dev.debugDumpLayerTree` For layers (debugDumpLayerTree)
- `flutter.dev.debugDumpRenderTree` To dump the rendering tree of the app (debugDumpRenderTree)
- `flutter.dev.openDevToolsProfiler` Open the [DevTools](https://flutter.dev/docs/development/tools/devtools/overview) in the browser
- `flutter.dev.debugPaintSizeEnabled` To toggle the display of construction lines (debugPaintSizeEnabled)
- `flutter.dev.defaultTargetPlatform` To simulate different operating systems, (defaultTargetPlatform)
- `flutter.dev.showPerformanceOverlay` To display the performance overlay (WidgetsApp.showPerformanceOverlay)
- `flutter.dev.debugProfileWidgetBuilds` To enable timeline events for all widget build methods, (debugProfileWidgetBuilds)
- `flutter.dev.showWidgetInspectorOverride` To toggle the widget inspector (WidgetsApp.showWidgetInspectorOverride)
- `flutter.dev.debugDumpSemanticsHitTestOrder` Accessibility (debugDumpSemantics) for inverse hit test order
- `flutter.dev.debugDumpSemanticsTraversalOrder` Accessibility (debugDumpSemantics) for traversal order

### Closing Labels

when `flutter.lsp.initialization.closingLabels` is set to `true`,
the closing labels will be display at end of closing.

> this feature only support neovim since vim do not support virtual text

| disabled                                                                                                                         | enabled                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| <img height="300px" src="https://user-images.githubusercontent.com/5492542/67616073-f0812b00-f806-11e9-8e5c-ac42ab3a293c.png" /> | <img height="300px" src="https://user-images.githubusercontent.com/5492542/67616063-c16ab980-f806-11e9-8522-1c89217096e0.png" /> |


### UI Path
when `flutter.UIPath` is set to `true`, the path for the UI component under cursor will be shown on the status bar.
![Screen Shot 2020-08-09 at 6 25 10 PM](https://user-images.githubusercontent.com/8187501/89730211-575a9280-da6f-11ea-9ad1-73770c1840db.png)

### Flutter Outline
In the Flutter Outline panel, press `enter` to goto the corresponding location in the source file.
