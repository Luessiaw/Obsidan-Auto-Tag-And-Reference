# Obsidian Auto Tag and Reference Plugin

This is a plugin for equation numbering and reference. It doesn't behave like latex. It's just a compromise. 

In latex, we use \\label command to mark an equation and \\eqref command to get its seriel number. In markdown files it doesn't seem to help. (see [the forum](https://forum.obsidian.md/t/automatic-equation-numbering-latex-math/1325), there are several solutions, but none works for me.) 

## download

Go to [release page](https://github.com/Luessiaw/obsidian-equationAutoTagAndReference/releases/tag/1.0.0), download "main.js", "manifest.json" and "style.css", put them in your vault/.obsidian/plugins/obsidian-formulaAutoTagAndReference. The turn on the plugin in Obsidian.

## usage

There is currently only one command. Type ctrl+P to active Command Palette, then search for "auto-Tag & Ref: Refresh Tags and References", click on it or press enter. (see below video [here](https://youtu.be/f6pe4zMqj2o))

![sample gif](samplegif.gif)

Leaving aside numbers and refrences, you may first notice that the following two lines are added in front of the file (Of cource they won't be added repeatedly):

```latex
$\newcommand\Ref[2][]{(\mbox{#2})}$
$\newcommand\Label[2][]{\tag{#2}}$
```

These two definations are simple. (Pay attention to the capitalization of letters.) The \\Ref  and \\Label command are used to refer to and mark an equation, respectively. Each of them accepts two arguments. 

## the \\Label command

The optional argument corresponds to argument of \\label in latex. It is the label name of the equation. You can use \\Label or \\tag to numbering an equation. If you use the latter, the equation is not named. The plugin treats these two commands as the same.

The necessary argument (namely, argument between "{" and "}") corresponds to that of \\tag. It determines the numbering pattern, and should take one of the following two types:

* Type I: contains numbers or letters. Other characters are regarded as separator. E.g. 1, A, 2b, 3-c. 
	* Only the "pattern" is important. Patterns are used to determine level of numbering. 
	* Seperator is not important. 1-a, 2b, 3(c) are considered as of the same pattern. During re-calculating, the seperators are preserved.
* Type II: contains no numbers or letters. The first character will be used for numbering. For example, \* will generate series like \*, \*\*, \*\*\*, etc. Separator characters are not supported.


### Example for \\Label

```latex
\tag{1}
% equation is not named.
% pattern generates series like 1,2,3,...

\Label[]{1}
% equation is not named, since the optional argument is empty.
% pattern generates series like 1,2,3,...

\Label[eq1]{*}
% equation is named as "eq1".
% pattern generates series like *,**,***,...

\Label[eq2]{2a}
% equation is named as "eq2".
% pattern generates series like 1a,1b,1c...

\Label[]{1-a}
% equation is not named.
% pattern generates series like 1-a,1-b,1-c...
% 1-a is considered as of the same pattern with 1a. 
```

This plugin works by directly modify the file's content. All \\Label or \\tag commands will be modified to commands like:

```latex
\Label[eqution name]{equation number}
```

## the \\Ref command

The meaning of the two arguments of \\Ref is the same as that of \\Label. 

The \\Ref command is modified when the equation it referred is re-numbered. The referred equation is determine according to the optional argument. If this argument is empty, it is determined by the necessary argument. (It's just the number of the equation before modified.)

## Supported platform

This plugin has been tested on windows 11. It cannot work on mobile devices now, according to a file-read function. 

## Todo list

* Add more new commands for convenience. Such as \\LabelN for pattern 1, \\Labela for pattern a, etc.
* Support costumise test in \\Label. For example, you may want to show equation's name in tag like this,

$$
a^2+b^2=c^2.\tag{1a, the Pythagorean theorem}
$$

* Support mobile devices.
* Optimize the code. I'm far from a JavaScript expert.
* Support [Extended MathJax](https://github.com/xldenis/obsidian-latex) plugin so that you can just put the defination statement of \\Label and \\Ref in the preamble file rather than every markdown file.
