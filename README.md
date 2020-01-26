# About
This is just an experimental script, to test quicksort recursion in bash for bulk renaming.

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
