# tojson

[![License: BSD 3 Clause](https://img.shields.io/badge/License-BSD_3--Clause-yellow.svg)](https://opensource.org/licenses/BSD-3-Clause)

Convert text to JSON via regular expression.

#### Usage:
```
tojson REGEX
```

Take input text from STDIN and convert it to JSON via the given Python regular
expression REGEX. Each line of the input is searched for the first match of the
regular expression and converted to a JSON object using names of the capturing
groups as keys, and their corresponding captures as values. Unnamed capturing
groups and groups that did not match are ignored. Each input line is stripped
of any trailing whitespace before applying REGEX. Output always goes to STDOUT.

By default, in each output object all values are JSON strings, unless the
capturing group name has "__N" suffix, in which case the program makes an attempt
to convert the value to a JSON number, or "__B" suffix to convert the value to
JSON "true" or "false". In either case, the suffix is removed from the key, and
failed conversions leave values as strings. The boolean conversion treats strings
like "true" or "yes" as JSON "true", and strings like "false" and "no" as JSON
"false", case-insensitive.

Typical use cases include parsing plain text log files or Unix commands output
to extract useful information and convert it to JSON for further processing.

#### Example
```sh
▶ ps -u root -o pid,%cpu,%mem,args \
| tojson '^\s*(?P<pid__N>\d+)\s+(?P<cpu__N>\d+\.\d+)\s+(?P<mem__N>\d+\.\d+)\s+(?P<cmd>[^[].+)$'
```

Output:

```JSON
[ {"cmd": "/sbin/init splash", "pid": 1, "cpu": 0.0, "mem": 0.1}
, {"cmd": "/lib/systemd/systemd-journald", "pid": 517, "cpu": 0.0, "mem": 0.2}
, {"cmd": "bpfilter_umh", "pid": 543, "cpu": 0.0, "mem": 0.0}
, {"cmd": "/lib/systemd/systemd-udevd", "pid": 554, "cpu": 0.0, "mem": 0.0}
, {"cmd": "/usr/lib/accountsservice/accounts-daemon", "pid": 809, "cpu": 0.0, "mem": 0.1}
, {"cmd": "/usr/sbin/acpid", "pid": 811, "cpu": 0.0, "mem": 0.0}
...
]
```

#### Installation
Just copy `tojson` file to a directory included in the `$PATH` environment variable.

#### Status
Tested on Linux Mint 20.1 with Python 3.8.5.
