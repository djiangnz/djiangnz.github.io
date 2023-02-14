---
layout: post
title: Cron Cheatsheet
description:
summary:
tags: cheatsheet bash
---

# Format

```bash
Min  Hour Day  Mon  Weekday
*    *    *    *    *  command to be executed
┬    ┬    ┬    ┬    ┬
│    │    │    │    └─  Weekday  (0=Sun .. 6=Sat)
│    │    │    └──────  Month    (1..12)
│    │    └───────────  Day      (1..31)
│    └────────────────  Hour     (0..23)
└─────────────────────  Minute   (0..59)
```

# Crontab

```bash
# Adding tasks easily
echo "@reboot echo hi" | crontab

# Open in editor
crontab -e

# List tasks
crontab -l [-u user]
```

# Operators

|     |                            |
| --- | -------------------------- |
| `*` | all values                 |
| `,` | separate individual values |
| `-` | a range of values          |
| `/` | divide a value into steps  |

# Links

- [crontab guru](https://crontab.guru/)
