# tojson

[![License: BSD 3 Clause](https://img.shields.io/badge/License-BSD_3--Clause-yellow.svg)](https://opensource.org/licenses/BSD-3-Clause)

Convert text to JSON via regular expression.

#### Usage:
```
▶ tojson
Usage: tojson REGEX
  Convert input text from STDIN to JSON via the given Python regular expression
  REGEX. Each line of the input is searched for the first match of the regular
  expression and converted to a JSON object using names of the capturing groups
  as keys, and their corresponding captures as values. Unnamed capturing groups
  and groups that did not match are ignored. Each input line is stripped of any
  trailing whitespace before applying REGEX. Output always goes to STDOUT.
```

#### Installation
Just copy `tojson` file to a directory included in the `$PATH` environment variable.

#### Example
```sh
▶ ps -u root -o pid,%cpu,%mem,args \
| tojson '^\s*(?P<pid>\d+)\s+(?P<cpu>\d+\.\d+)\s+(?P<mem>\d+\.\d+)\s+(?P<cmd>[^[].+)$'
```

Output:

```JSON
[ {"pid": 1, "cpu": 0.0, "mem": 0.1, "cmd": "/sbin/init splash"}
, {"pid": 519, "cpu": 0.0, "mem": 0.5, "cmd": "/lib/systemd/systemd-journald"}
, {"pid": 546, "cpu": 0.0, "mem": 0.0, "cmd": "bpfilter_umh"}
, {"pid": 555, "cpu": 0.0, "mem": 0.0, "cmd": "/lib/systemd/systemd-udevd"}
, {"pid": 833, "cpu": 0.0, "mem": 0.1, "cmd": "/usr/lib/accountsservice/accounts-daemon"}
, {"pid": 834, "cpu": 0.0, "mem": 0.0, "cmd": "/usr/sbin/acpid"}
, {"pid": 837, "cpu": 0.0, "mem": 0.0, "cmd": "/usr/sbin/cron -f"}
, {"pid": 840, "cpu": 0.0, "mem": 0.2, "cmd": "/usr/sbin/NetworkManager --no-daemon"}
, {"pid": 847, "cpu": 0.0, "mem": 0.0, "cmd": "/usr/sbin/irqbalance --foreground"}
...
]
```

#### Status
Tested on Linux Mint 20.1 with Python 3.8.5.
