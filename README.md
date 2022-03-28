**This is a fork from the original version by lpubsppop01 for personal use only. No public maintainance intended!**

# Auto Time Stamp
[![Build status](https://ci.appveyor.com/api/projects/status/8jhbugo5d2ejuylh?svg=true)](https://ci.appveyor.com/project/lpubsppop01/vscode-auto-timestamp)

A Visual Studio Code extension that update time stamp in file content when saving document.

## Features
When saving document:
  - Update last modified time field
  - Fill birth time field by file time stamp if it is empty
  - If it is a LaTeX or TeX file, also replace a placeholder by a command to set the date

Each of the fields will be detected with condition of settings.
By default settings, lines like the following will be detected:
```
// Created: 2018/02/04 12:24:41
// Last modified: 2022-03-28T12:08:05.026+02:00
```

Additionally, for TeX, LaTeX or DocTeX (.dtx) files, a configurable placeholder will be found and replaced. E.g., if configured
```
TeX Placeholder: XXX-DATE-WHEN-CREATED-XXX
TeX Format: '\DTMdate{'yyyy-MM-dd'}'
```
a text such as `\Date{XXX-DATE-WHEN-CREATED-XXX}` will get turned into (today is 28 Mar 2022) `\Date{\DTMdate{2022-03-28}}`. LaTeX can use this to properly typeset e.g. the date of a letter. If the date gets manually changed in the document, it will not get updated anymore – which is probably what is intended in this case.


If it does not work, please check the Line Limit setting. This setting is 5 lines from the beginning of the file by default, so it may be too small for your file.

In addition to the above, there is also the setting of the target file name (Filename Pattern), but by default all files are matched.

This version uses the Luxon library instead of Moments, which had been in lpubsppop01's original version. Also, the Japanese localization has been removed, simply because I could not update the text strings for the additional configuration options I added.

## Author
Based on the version by [lpubsppop01](https://github.com/lpubsppop01), changed by [hseliger](https://github.com/hseliger)

## License
[zlib License](https://github.com/lpubsppop01/vscode-auto-timestamp/raw/master/LICENSE.txt) – of course unchanged from lpubsppop01's initial licensing.
