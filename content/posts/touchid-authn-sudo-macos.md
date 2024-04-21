---
title: "TouchID for sudo AuthN on MacOS"
date: 2022-08-26T23:15:44+02:00
draft: false
description: "Authenticate with TouchID for sudo on MacOS"
tags:
- macos
- tips
- authn
---

The file we have to edit is `/etc/pam.d/sudo`.

We have to add

```bash
auth sufficient pam_tid.so
```

below the `pam_smartcard.so` line

```bash
auth       sufficient     pam_smartcard.so
auth       sufficient     pam_tid.so
auth       required       pam_opendirectory.so
account    required       pam_permit.so
password   required       pam_deny.so
session    required       pam_permit.so
```

Credits to [digitaino](https://it.digitaino.com/use-touchid-to-authenticate-sudo-on-macos/)
