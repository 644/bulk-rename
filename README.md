# About
This is a replacement script for things like the below snippet. The bulk-rename script is surprisingly faster than the below snippet in *some* cases, and it replaces for file/folder names that rename can't. It's only slower when there's lots of nested directories within nested directories, but not by much. Also since it's pure bash, it will work on any system that has just bash >4.0+ installed. bulk-rename-qsort is entirely experimental, and is much slower than both.

This is a generally better option when performance is important for lots of nested directories, however a pure C implementation would be better still:
```
#!/bin/bash
set -euo pipefail

fnd=' '; rpl='_'; dir=${1-${PWD}}

[[ ! -d ${dir} || ! -O ${dir} ]] && printf 'error with %q\n' "${dir}" >&2 && exit
[[ ${dir:0:1} != '/' ]] && dir=./${dir}

find "${dir}" -depth -type f,d -name '* *' -exec rename "${fnd}" "${rpl}" '{}' \;
```

The reason for making this script was to test my own abilities with bash scripting. In the end I ended up creating a fast, reliable utility with practically no dependencies other than bash. The biggest problem I encountered was trying to order the directories returned from bash's glob matching so that they're ordered by depth rather than alphabetically, because as it's renaming directories alphabetically it will "lose track" of any deeper directories and fail to rename them. There are sorting utilities that can do this, however I wanted to keep it in pure bash, so initially I tried counting the amount of /'s in each directory name (/'s are not allowed in any filenames for any OS, so it is guaranteed to give the correct depth) using a quicksort algorithm, but that was slow. Then I realized since bash allows you to start an array at any given index and skip over indexes, I could create an offset in the array for the directory name using the maximum found depth of all directories minus the slash count, then multiplied by the amount of directories found, plus one incrementation for each directory it's appending, then add it to the array with that offset. This basically sorts the depth quickly and reliably using pure bash, which looks like this:

```
((offset = (maxdepth - ${#slashc}) * foldercount + ++pos))
folsorted[${offset}]=${fol}
```

# Dependencies
Bash >4.0+

# License
MIT
