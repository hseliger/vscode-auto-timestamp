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

This version uses the Luxon library instead of Moments, which had been in lpubsppop01's original version. The possible format tokens are listed in the luxon documentation (https://moment.github.io/luxon/#/formatting?id=table-of-tokens).

### Luxon tokens
| Standalone token | Format token | Description                                                    | Example                                                       |
| ---------------- | ------------ | -------------------------------------------------------------- | ------------------------------------------------------------- |
| S                |              | millisecond, no padding                                        | `54`                                                          |
| SSS              |              | millisecond, padded to 3                                       | `054`                                                         |
| u                |              | fractional seconds, functionally identical to SSS              | `054`                                                         |
| uu               |              | fractional seconds, between 0 and 99, padded to 2              | `05`                                                          |
| uuu              |              | fractional seconds, between 0 and 9                            | `0`                                                           |
| s                |              | second, no padding                                             | `4`                                                           |
| ss               |              | second, padded to 2 padding                                    | `04`                                                          |
| m                |              | minute, no padding                                             | `7`                                                           |
| mm               |              | minute, padded to 2                                            | `07`                                                          |
| h                |              | hour in 12-hour time, no padding                               | `1`                                                           |
| hh               |              | hour in 12-hour time, padded to 2                              | `01`                                                          |
| H                |              | hour in 24-hour time, no padding                               | `9`                                                           |
| HH               |              | hour in 24-hour time, padded to 2                              | `13`                                                          |
| Z                |              | narrow offset                                                  | `+5`                                                          |
| ZZ               |              | short offset                                                   | `+05:00`                                                      |
| ZZZ              |              | techie offset                                                  | `+0500`                                                       |
| ZZZZ             |              | abbreviated named offset                                       | `EST`                                                         |
| ZZZZZ            |              | unabbreviated named offset                                     | `Eastern Standard Time`                                       |
| z                |              | IANA zone                                                      | `America/New_York`                                            |
| a                |              | meridiem                                                       | `AM`                                                          |
| d                |              | day of the month, no padding                                   | `6`                                                           |
| dd               |              | day of the month, padded to 2                                  | `06`                                                          |
| c                | E            | day of the week, as number from 1-7 (Monday is 1, Sunday is 7) | `3`                                                           |
| ccc              | EEE          | day of the week, as an abbreviate localized string             | `Wed`                                                         |
| cccc             | EEEE         | day of the week, as an unabbreviated localized string          | `Wednesday`                                                   |
| ccccc            | EEEEE        | day of the week, as a single localized letter                  | `W`                                                           |
| L                | M            | month as an unpadded number                                    | `8`                                                           |
| LL               | MM           | month as a padded number                                       | `08`                                                          |
| LLL              | MMM          | month as an abbreviated localized string                       | `Aug`                                                         |
| LLLL             | MMMM         | month as an unabbreviated localized string                     | `August`                                                      |
| LLLLL            | MMMMM        | month as a single localized letter                             | `A`                                                           |
| y                |              | year, unpadded                                                 | `2014`                                                        |
| yy               |              | two-digit year                                                 | `14`                                                          |
| yyyy             |              | four- to six- digit year, pads to 4                            | `2014`                                                        |
| G                |              | abbreviated localized era                                      | `AD`                                                          |
| GG               |              | unabbreviated localized era                                    | `Anno Domini`                                                 |
| GGGGG            |              | one-letter localized era                                       | `A`                                                           |
| kk               |              | ISO week year, unpadded                                        | `14`                                                          |
| kkkk             |              | ISO week year, padded to 4                                     | `2014`                                                        |
| W                |              | ISO week number, unpadded                                      | `32`                                                          |
| WW               |              | ISO week number, padded to 2                                   | `32`                                                          |
| o                |              | ordinal (day of year), unpadded                                | `218`                                                         |
| ooo              |              | ordinal (day of year), padded to 3                             | `218`                                                         |
| q                |              | quarter, no padding                                            | `3`                                                           |
| qq               |              | quarter, padded to 2                                           | `03`                                                          |
| D                |              | localized numeric date                                         | `9/4/2017`                                                    |
| DD               |              | localized date with abbreviated month                          | `Aug 6, 2014`                                                 |
| DDD              |              | localized date with full month                                 | `August 6, 2014`                                              |
| DDDD             |              | localized date with full month and weekday                     | `Wednesday, August 6, 2014`                                   |
| t                |              | localized time                                                 | `9:07 AM`                                                     |
| tt               |              | localized time with seconds                                    | `1:07:04 PM`                                                  |
| ttt              |              | localized time with seconds and abbreviated offset             | `1:07:04 PM EDT`                                              |
| tttt             |              | localized time with seconds and full offset                    | `1:07:04 PM Eastern Daylight Time`                            |
| T                |              | localized 24-hour time                                         | `13:07`                                                       |
| TT               |              | localized 24-hour time with seconds                            | `13:07:04`                                                    |
| TTT              |              | localized 24-hour time with seconds and abbreviated offset     | `13:07:04 EDT`                                                |
| TTTT             |              | localized 24-hour time with seconds and full offset            | `13:07:04 Eastern Daylight Time`                              |
| f                |              | short localized date and time                                  | `8/6/2014, 1:07 PM`                                           |
| ff               |              | less short localized date and time                             | `Aug 6, 2014, 1:07 PM`                                        |
| fff              |              | verbose localized date and time                                | `August 6, 2014, 1:07 PM EDT`                                 |
| ffff             |              | extra verbose localized date and time                          | `Wednesday, August 6, 2014, 1:07 PM Eastern Daylight Time`    |
| F                |              | short localized date and time with seconds                     | `8/6/2014, 1:07:04 PM`                                        |
| FF               |              | less short localized date and time with seconds                | `Aug 6, 2014, 1:07:04 PM`                                     |
| FFF              |              | verbose localized date and time with seconds                   | `August 6, 2014, 1:07:04 PM EDT`                              |
| FFFF             |              | extra verbose localized date and time with seconds             | `Wednesday, August 6, 2014, 1:07:04 PM Eastern Daylight Time` |
| X                |              | unix timestamp in seconds                                      | `1407287224`                                                  |
| x                |              | unix timestamp in milliseconds                                 | `1407287224054`                                               |

### Further changes
Also, the Japanese localization has been removed, simply because I could not update the text strings for the additional configuration options I added.

## Author
Based on the version by [lpubsppop01](https://github.com/lpubsppop01), changed by [hseliger](https://github.com/hseliger)

## License
[zlib License](https://github.com/lpubsppop01/vscode-auto-timestamp/raw/master/LICENSE.txt) – of course unchanged from lpubsppop01's initial licensing.
