# Repos Alfred Workflow

[![GitHub Version][shield-version]][gh-releases]
[![GitHub All Releases][shield-downloads]][gh-releases]
[![GitHub][shield-license]][license-mit]

Browse, search and open local Git repositories with [Alfred][alfred].

![][preview]

## Download & Installation

Download the [latest workflow release][gh-latest-release] from GitHub. Open the workflow file to
install in Alfred.

## Usage

This workflow requires some configuration before use. See [Configuration](#configuration) for
details.

- `repos [<query>]` — Show a list of your Git repos filtered by `<query>`
  - `↩` — Open selected repo in `app_default` (see [configuration](#configuration))
  - `⌘ + ↩` — Open selected repo in `app_cmd` (see [configuration](#configuration))
  - `⌥ + ↩` — Open selected repo in `app_alt` (requires [configuration](#configuration))
  - `^ + ↩` — Open selected repo in `app_ctrl` (requires [configuration](#configuration))
  - `⇧ + ↩` — Open selected repo in `app_shift` (requires [configuration](#configuration))
  - `fn + ↩` — Open selected repo in `app_fn` (requires [configuration](#configuration))
  - `⌘⌥ + ↩` — Open selected repo in `app_cmd_alt` (requires [configuration](#configuration))
  - `⌘⌥⇧ + ↩` — Open selected repo in `app_cmd_alt_shift` (requires [configuration](#configuration))
  - etc.
  - etc.
  - `→` — Open Alfred's default File Actions menu
- `reposettings` — Open `settings.json` in default JSON editor
- `reposupdate` — Force workflow to update its cached list of repositories. (By default, the list
  will only be updated—in the background—every 3 hours.)
- `reposhelp` — Open GitHub README in your browser

## Configuration

Before you can use this workflow, you have to configure one or more folders in which the workflow
should search for Git repos. The workflow uses `find` to search for `.git` directories, so you
shouldn't add _huge_ directory trees to it, and use the `depth` option to restrict the search depth.
Typically, a `depth` of `2` will be what you want (i.e. search within subdirectories of specified
directory, but no lower). Add directories to search to the `search_dir` array in `settings.json`
(see below).

The default `settings.json` file looks like this:

```javascript
{
  "app_default": "Finder",               // ↩ to open in this/these app(s)
  "app_cmd": "Terminal",                 // ⌘ + ↩ to open in this/these app(s)
  "app_alt": null,                       // ⌥ + ↩ to open in this/these app(s)
  "app_ctrl": null,                      // ^ + ↩ to open in this/these app(s)
  "app_shift": null,                     // ⇧ + ↩ to open in this/these app(s)
  "app_fn": null,                        // fn + ↩ to open in this/these app(s)
  "global_exclude_patterns": [],         // Exclude from all searches
  "search_dirs": [
    {
      "path": "~/delete/this/example",   // Path to search. ~/ is expanded
      "depth": 2,                        // Search subdirs of `path`
      "name_for_parent": 1,              // Name Alfred entry after parent of `.git`. 2 = grandparent of `.git` etc.
      "excludes": [                      // Excludes specific to this path
        "tmp",                           // Directories named `tmp`
        "bad/smell/*"                    // Subdirs of `bad/smell` directory
      ]
    }
  ]
}
```

This is my `settings.json`:

```javascript
{
  "app_alt": "Fork",
  "app_cmd": "iTerm",
  "app_cmd_alt": [
    "Visual Studio Code",
    "iTerm",
    "Fork",
    "ForkLift"
  ],
  "app_ctrl": "ForkLift",
  "app_default": "Visual Studio Code",
  "app_shift": "Browser",
  "global_exclude_patterns": [],
  "search_dirs": [
    {
      "depth": 2,
      "excludes": [],
      "name_for_parent": 1,
      "path": "~/Development"
    }
  ]
}
```

**Note:** If you specify `Browser`, `Safari`, `Google Chrome`, `Webkit` or `Firefox` as an
application, it will be passed the remote repo URL, not the local filepath. The default remote name
is `origin`. However, you can globally change it to a different remote, such as `upstream`, by using
the `remote_name` setting. `Browser` will open the URL in your default browser.

### Search Directories

Each entry in the `search_dirs` list must be a mapping.

Only `path` is required. `depth` will default to `2` if not specified. `excludes` are globing
patterns, like in `.gitignore`.

`name_for_parent` defaults to `1`, which means the entry in Alfred's results should be named after
the directory containing the `.git` directory. If you want Alfred to show the name of the
grandparent, set `name_for_parent` to `2` etc.

This is useful if your projects are structured, for example, like this and `src` is the actual repo:

```
Code
  Project_1
    src
    other_stuff
  Project_2
    src
    other_stuff
  …
  …
```

Set `name_for_parent` to `2`, and `Project_1`, `Project_2` etc. will be shown in Alfred, not `src`,
`src`, `src`…

By default, the cached list of repositories is updated in the background every 3 hours. You can also
change the default update interval (3h → 180min) in the
[Workflow Environment Variables][alfred-config-sheet] (the `[𝒙]` icon) in Alfred Preferences. Change
the `UPDATE_EVERY_MINS` workflow variable to suit your needs.

### Open in Applications

The applications specified by the `app_XYZ` options are all called using
`open -a AppName path/to/directory`. You can configure any application that can open a directory in
this manner. Some recommendations are Sublime Text, SourceTree, GitHub or iTerm.

The meta app `Browser` will open the repo's `remote/origin` URL in your default browser. Other
recognized browsers are `Safari`, `Google Chrome`, `Firefox` and `WebKit`.

**Note:** As you can see from my `settings.json`, you can also set an `app_XYZ` value to an array of
applications to open the selected repo in more than one app at once:

```javascript
…
  "app_cmd": ["Visual Studio Code", "iTerm", "Fork", "ForkLift"],
…
```

You can also arbitrarily combine modifiers to give yourself many more options:

```javascript
"app_cmd_alt": "Finder",
"app_shift_alt_cmd": "VSCodium",
"app_cmd_fn_alt": "Oni",
etc.
etc.
```

Modifiers may be specified in any order. The only requirements are that the key must start with
`app_` and the modifiers must be separated by `_`.

You can also use `→` on a result to access Alfred's default File Actions menu.

### Change Repository Icons

You can set an icon for all repositories that share the same parent directory.
Simply add an icon file named `.alfred-repos-icon.png` to the parent directory of the repositories. The icon will then appear in Alfred's results.
If no icon is found, the default icon will be displayed.

## Bug Reports and Feature Requests

Please use [GitHub issues][gh-issues] to report bugs or request features.

## Contributors

This Alfred Workflow comes from the [abandoned Workflow][abandoned-workflow] of
[Dean Jackson][deanishe]

## License

Repos Alfred Workflow is licensed under the [MIT License][license-mit]

The workflow uses the following libraries:

- [docopt][docopt] ([MIT License][license-docopt])
- [Alfred-PyWorkflow][alfred-pyworkflow] ([MIT License][license-mit])

The workflow uses the following icons:

- [git-scm.com][git] ([Creative Commons Attribution 3.0 Unported License][license-cc])

[abandoned-workflow]: https://github.com/deanishe/alfred-repos
[alfred-config-sheet]: https://www.alfredapp.com/help/workflows/advanced/variables/#environment
[alfred-pyworkflow]: https://github.com/harrtho/alfred-pyworkflow
[alfred]: https://www.alfredapp.com
[deanishe]: https://github.com/deanishe
[docopt]: https://github.com/docopt/docopt
[gh-issues]: https://github.com/harrtho/alfred-repos/issues
[gh-latest-release]: https://github.com/harrtho/alfred-repos/releases/latest
[gh-releases]: https://github.com/harrtho/alfred-repos/releases
[git]: https://git-scm.com/downloads/logos
[jlong]: https://twitter.com/jasonlong
[license-cc]: https://creativecommons.org/licenses/by/3.0/
[license-docopt]: https://github.com/docopt/docopt/blob/master/LICENSE-MIT
[license-mit]: https://opensource.org/licenses/MIT
[preview]: img/preview.png
[shield-downloads]: https://img.shields.io/github/downloads/harrtho/alfred-repos/total.svg
[shield-license]: https://img.shields.io/github/license/harrtho/alfred-repos.svg
[shield-version]: https://img.shields.io/github/release/harrtho/alfred-repos.svg
