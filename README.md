# About
This is just an experimental script to test bash for bulk file/folder renaming. However, the bulk-rename-array script is surprisingly faster than the below snippet in *some* cases, and it replaces for file/folder names that rename can't. It's only slower when there's lots of nested directories within nested directories. Also since it's pure bash, it will work on any system that has just bash >4.0+ installed.

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
