# Obsidian Auto Tag and Reference Plugin v2.0.0

This is a plugin for equation numbering and reference. It doesn't behave like latex. It's just a compromise. 

In latex, we use `\label` command to mark an equation and `\eqref` command to get its seriel number. In markdown files it doesn't seem to help. (see [the forum](https://forum.obsidian.md/t/automatic-equation-numbering-latex-math/1325), there are several solutions, but none works for me.) 

## Download

Go to [release page](https://github.com/Luessiaw/obsidian-equationAutoTagAndReference/releases/tag/1.0.0), download `main.js`, `manifest.json` and `style.css`, put them in your `vault/.obsidian/plugins/obsidian-formulaAutoTagAndReference`. The turn on the plugin in Obsidian.

## Usage

**Notice** The usage of `2.0.0` version is  different from `1.0.0`. Please check your plugin version.

There is currently only one command. Type `ctrl+P` to active Command Palette, then search for `auto-Tag & Ref: Refresh Tags and References`, click on it or press enter. 

![sample gif](samplegif.gif)

You may first notice that the following two lines are added in front of the file (Of cource they won't be added repeatedly):

```latex
$\newcommand\Ref[2][]{(\mbox{#2})}$
$\newcommand\Label[2][]{\tag{#2}}$
```

These two definations are simple. (Pay attention to the capitalization of letters.) The `\Ref`  and `\Label` command are used to refer to and mark an equation, respectively. Each of them accepts two arguments. 

### To avoid defination statements added in `.md` file

You can use the [Extended MathJax plugin](https://github.com/xldenis/obsidian-latex) to avoid the two statements to be added in your markdown file. The plugin uses an extral file named `preamble.sty` in you vault's root directory to enable self-defined mathjax commands.

You should do the followings steps:
1. Download and activate the [Extended MathJax plugin](https://github.com/xldenis/obsidian-latex).
2. Press `Ctrl+,` to open the Setting Paltte. 
3. Turn on `Use Plugin Extended MathJax` option.
4. Click on the `copy` button to copy the defination statements into your clipboard, and paste them in the `preamble.sty` file.
5. Restart Extended MathJax plugin to reload the preamble file.

## the `\Label` command

The optional argument corresponds to argument of `\Label` in latex. It is the label name of the equation. You can use `\Label` or `\tag` to numbering an equation. If you use the latter, the equation is not named. The plugin treats these two commands as the same by replace `\tag{number}` with `\Label[number]{}`.

The optional argument corresponds to the necessary argument (namely, argument between "{" and "}") of `\tag`. It determines the numbering pattern, and should take one of the following two types:

* Type I: contains numbers or letters. Other characters are regarded as separator. E.g. `1`, `A`, `2b`, `3-c`. 
	* Only the "pattern" is important. Patterns are used to determine level of numbering. 
	* Seperator is not important. `1-a`, `2b`, `3(c)` are considered as of the same pattern. During re-calculating, the seperators are preserved.
* Type II: contains no numbers or letters. The first character will be used for numbering. For example, `*` will generate series like `*,**,***,...` Separator characters are not supported in this case.

As is defined, if the optional argument is omitted, the pattern is regarded as `1,2,3,...`

### Example for `\Label`

```latex
\tag{1}
% equation is not named.
% pattern generates series like 1,2,3,...

\Label[1]{}
% equation is not named, since the optional argument is empty.
% pattern generates series like 1,2,3,...

\Label[*]{eq1}
% equation is named as "eq1".
% pattern generates series like *,**,***,...

\Label[2a]{eq2}
% equation is named as "eq2".
% pattern generates series like 1a,1b,1c...

\Label[1-a]{}
% equation is not named.
% pattern generates series like 1-a,1-b,1-c...
% 1-a is considered as of the same pattern with 1a. 
```

This plugin works by directly modify the file's content. All `\Label` or `\tag` commands will be modified to commands like:

```latex
\Label[eqution number]{equation name}
```

## the `\Ref` command

The meaning of the two arguments of `\Ref` is the same as that of `\Label`. 

The `\Ref` command is modified when the equation it referred is re-numbered. The referred equation is determine by the label name. If the argument is empty, it is determined by the necessary argument. (It's just the number of the equation before modified.)

## Supported platform

This plugin has been tested on windows 11. It cannot work on mobile devices due to `node.js` denpendence.

## Todo list

* Add an interface, like auto-completioin function in vscode.
* Support costumise text in `\Label`. For example, you may want to show equation's name in tag like this,

$$
a^2+b^2=c^2.\tag{1a, the Pythagorean theorem}
$$

* Optimize the code. I'm far from a JavaScript expert.
* Upload this plugin to the community. I've pull the `1.00` release to obsidian team, waiting for review. 