---
title: "fish-shell: Tips and Tricks"
date: 2022-08-02T16:56:22+02:00
draft: false
comments: true
toc: false
Cover: 
tags: [linux, shell, fish]
categories: []
---

I'll update this page with anything new 🆕 I find about `fish-shell` 🐟

# Autosuggestions
Bumped into this one here[^1]

Apparently, after you enable some new completions you can run 
`fish_update_completions` and `fish` parses man pages for possible commands
and switches.

# Setting an alias 'the right way' without slowing down your shell
Source[^6]

Aliases created with `alias` will not be available in new shell sessions. If
you want them to persist, use

```fish
alias -s ...
```

which will save it to `~/.config/fish/functions/[alias-name].fish`

Using `alias` inside `~/.config/fish/config.fish` will slow down your shell
start as each alias/function will be eagerly loaded.

To persist aliases across shell sessions, use `alias -s`, which will create a
`function`[^2] and save it to `~/.config/fish/functions`. This takes advantage
of fish function lazy-loading / autoloading[^3] mechanism.

# Fish shell cheatsheet

https://devhints.io/fish-shell

# Fish shell scripting manual

https://developerlife.com/2021/01/19/fish-scripting-manual/

# Running in 'incognito' mode (Private mode)

`fish` calls it *Private mode*, it sorta works like an incognito/private browser
tab by dropping all history. Some contexts when you could use this is decoding
`base64` Kubernetes secrets in the terminal or logging in while passing some
credentials...

```fish
⋊> ~ fish --private
Welcome to fish, the friendly interactive shell
Type help for instructions on how to use fish
fish is running in private mode, history will not be persisted.
⋊> ~
```

You can find out more [on the official docu](https://fishshell.com/docs/current/interactive.html?highlight=private#private-mode)

# Inspect and or hack functions

[`funced`](https://fishshell.com/docs/current/cmds/funced.html?highlight=funced)

```fish
funced fish_greeting
# opens function in $EDITOR

# hack, hack, hack and done; now you wanna save
funcsave fish_greeting
```

# Manipulate history

[history - show and manipulate command history](https://fishshell.com/docs/current/cmds/history.html?highlight=history)

Let's say you fired a command that didn't work out well and don't even want to
persist it in your history

```fish
# look for it
history search --contains "foo"

# delete it
history delete --prefix "foo"

# you can even be more precise if you know what you have to delete
history delete --case-sensitive --exact 'foo'
```

# Check if variable is set

Source[^4]

Test if an environment variable or a local variable from the script was set

```fish
if set -q FOOBAR
    echo its set
else
    echo its not set
end
```

# Check if file exists

Source[^5]

```fish
if test -e /bin/test
    echo its there
else
    echo its not there
end
```

---
{data-content = "footnotes"}

[^1]: https://mvolkmann.github.io/fish-article/#Autosuggestions
[^2]: https://fishshell.com/docs/current/cmds/function.html
[^3]: https://fishshell.com/docs/current/tutorial.html#autoloading-functions
[^4]: https://fishshell.com/docs/current/cmds/set.html?highlight=set
[^5]: https://fishshell.com/docs/current/cmds/test.html?highlight=test
[^6]: https://github.com/jorgebucaran/cookbook.fish#how-do-i-define-an-alias-in-fish
