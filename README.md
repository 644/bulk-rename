# About
This is just an experimental script to test quicksort recursion in bash for bulk renaming. However, the bulk-rename-array script is surprisingly *only a constant* 2x slower than the below snippet; 10,000 folders takes rename ~7s and bulk-rename-array ~14s; 30,000 folders takes rename ~20s and bulk-rename-array ~40s, and it replaces for file/folder names that rename can't.

Do not use this if you care about performance. Instead use the below snippet.

```
#!/bin/bash
set -euo pipefail

fnd=' '; rpl='_'; dir=${1-${PWD}}

[[ ! -d ${dir} || ! -O ${dir} ]] && printf 'error with %q\n' "${dir}" >&2 && exit
[[ ${dir:0:1} != '/' ]] && dir=./${dir}

find "${dir}" -depth -type f,d -name '* *' -exec rename "${fnd}" "${rpl}" '{}' \;
```
# Dependencies
Bash > 4.0+

# License
MIT
