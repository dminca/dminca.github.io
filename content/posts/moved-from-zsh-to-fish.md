---
title: "Moved from zsh to fish"
date: 2022-07-31T20:20:49+02:00
draft: false
comments: true
toc: true
Cover: 
tags: [linux, shell, zsh, fish]
categories: []
---

I've just moved from `zsh` to `fish` today and guess what, there were some bumps on the road.

First of all, I've been reading some blog posts before migrating which were all
rainbows and unicorns on the idea that `fish` installs quite smooth and without
any issues whatsoever, but those were written 6mo. or even +1 years ago and as
you already know, a lot can change in the Tech/IT/Software domain in that
amount of time and when it does it breaks some shits.

> This is only valid for Apple Silicon <abbr title="Apple MacBooks">Macs</abbr>,
the Intel-based <abbr title="Apple MacBooks">Macs</abbr> will just install the
package in its ordinary place ( `/usr/local/bin/fish` ). The installation was
ran on M1 Mac with MacOS Monterey 12.5.

# Initial install

## `chsh: /etc/shells: non-standard shell`

Like ordinary people, I started with

```sh
brew install fish

echo "/usr/local/bin/fish" | sudo tee -a /etc/shells

chsh -s /usr/local/bin/fish
```

But guess what, you get

```sh
chsh: /etc/shells: non-standard shell
```

Wanna know why?

Because homebrew doesn't install the binary at `/usr/local/bin/fish` but rather
`/opt/homebrew/bin/fish`, if you run this again the right way, it'll work.

```sh
brew install fish
echo "/opt/homebrew/bin/fish" | sudo tee -a /etc/shells
chsh -s /opt/homebrew/bin/fish
```

One problem solved. Now restart terminal and let's move to the next one.

## `fish: Unknown command: fish`

Since it was installed via `homebrew`, those binaries are on a different path
therefore we need to append it to the `${PATH}`.

```sh
# ~/.config/fish/config.fish
if status is-interactive
    # Commands to run in interactive sessions can go here
    export PATH="$PATH:/opt/homebrew/bin:/opt/homebrew/opt/openjdk/bin"
end
```

## Plug-in managers

Now we are able to continue installing [`omf`][6], thanks [Travis for your
blog post][1].

```sh
curl -L https://get.oh-my.fish | fish
```

Additionally, there's [`fisher`][4] which I'm not installing it because I don't want
to bloat my `fish` 😆

```sh
curl -sL https://git.io/fisher | source && fisher install jorgebucaran/fisher
```

# Fine-tuning

After finishing with the initial installation, I need some basic tooling like
`fzf`. Guess what, the [initial `fzf`][2] is not going to do, therefore we need
the [`fzf.fish`][5] from https://github.com/PatrickF1/fzf.fish

I've followed Travis's suggestions to pull it through `fisher`, but Patrick (the
maintainer of `fzf.fish`) [said you don't need `fisher` for that][3], but I'm
lazy meh 🤫

```sh
⋊> ~/Projects fisher install PatrickF1/fzf.fish
fisher install version 4.4.2
Fetching https://api.github.com/repos/patrickf1/fzf.fish/tarball/HEAD
Installing patrickf1/fzf.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_configure_bindings_help.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_extract_var_info.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_preview_changed_file.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_preview_file.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_report_diff_type.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_report_file_type.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_search_directory.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_search_git_log.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_search_git_status.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_search_history.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_search_processes.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_search_variables.fish
           /Users/cyb0rg/.config/fish/functions/_fzf_wrapper.fish
           /Users/cyb0rg/.config/fish/functions/fzf_configure_bindings.fish
           /Users/cyb0rg/.config/fish/conf.d/fzf.fish
           /Users/cyb0rg/.config/fish/completions/fzf_configure_bindings.fish
Installed 1 plugin/s
```

But when you <kbd>⌘</kbd>+<kbd>R</kbd> it just farts

```sh
⋊> ~/Projects fish: Unknown command: fzf
~/.config/fish/functions/_fzf_wrapper.fish (line 19):
    fzf $argv
    ^
in function '_fzf_wrapper' with arguments '--read0 --tiebreak=index --query= --preview=echo\ --\ \{4..\}\ \|\ fish_indent\ --ansi --preview-window=bottom:3:wrap'
	called on line 3 of file ~/.config/fish/functions/_fzf_search_history.fish
in command substitution
	called on line 8 of file ~/.config/fish/functions/_fzf_search_history.fish
in function '_fzf_search_history'
```

Therefore, time for more 🪓

Then we notice in the readme of `fzf.fish` ![you're dumb](/fzf-fish-prereqs.png)

Off we go to do the prereqs

```sh
brew install fd fzf bat
... 5h later **spongebob theme** ...
==> fd
fish completions have been installed to:
  /opt/homebrew/share/fish/vendor_completions.d
==> fzf
To install useful keybindings and fuzzy completion:
  /opt/homebrew/opt/fzf/install

To use fzf in Vim, add the following line to your .vimrc:
  set rtp+=/opt/homebrew/opt/fzf
==> bat
fish completions have been installed to:
  /opt/homebrew/share/fish/vendor_completions.d
```

NOW finally we're ready to go: <kbd>⌘</kbd>+<kbd>R</kbd> and evrika!
![finally](/fzf-fish-completed.png)

## Fonts and shit

Fuck'em. I don't need that crap, defaults are just fine. As long as I can clearly
read whatever's in my term I don't need no bells and whistles.

# Bonus tip: bang bang !! and bang dollar !$ tokens

Drop these functions in your `~/.config/fish/config.fish` or anywhere `fish` 🐟
could pick them up.

```fish
function bind_bang
    switch (commandline -t)[-1]
        case "!"
            commandline -t $history[1]; commandline -f repaint
        case "*"
            commandline -i !
    end
end

function bind_dollar
    switch (commandline -t)[-1]
        case "!"
            commandline -t ""
            commandline -f history-token-search-backward
        case "*"
            commandline -i '$'
    end
end

function fish_user_key_bindings
    bind ! bind_bang
    bind '$' bind_dollar
end
```

And then you can `sudo !!` or whatever.

Kudos:
* https://github.com/fish-shell/fish-shell/wiki/Bash-Style-Command-Substitution-and-Chaining-(!!-!$)
* https://superuser.com/a/944589

# Moving `zsh` history to `fish`

I've done [some ddg.co searches][9] but last that I found were some [dodgy][7] [python][8]
scripts which I currently don't have the time and patience to go through and understand
before actually running random shit from the internet. Never. Do. That. Ever.

If it was a Rust or Go binary then I'd trust it 😺

# What did I gain from this?

1. I'm a prowd [**vscodium**][10] user and I have to say with `fish` I finally
got rid of that shitty pop-up error each time I was closing the shell via
<kbd>⌃</kbd>+<kbd>D</kbd>. This was fucking anoying because when I was to
`git commit` something, write the message and exit via `:x` from `vim`, you'd
normally have to <kbd>ESC</kbd> to drop to *Normal* mode in order to go into
command mode.

2. It's fuckin' fast mate!

3. Typing suggestions - well thank you! It has really good support for these,
not only that but it's also showing typing errors in red


[1]: https://travisbrady.github.io/posts/moving-to-fish-shell/
[2]: https://github.com/junegunn/fzf
[3]: https://github.com/PatrickF1/fzf.fish/discussions/250
[4]: https://github.com/jorgebucaran/fisher#removing-plugins
[5]: https://github.com/PatrickF1/fzf.fish
[6]: https://github.com/oh-my-fish/oh-my-fish
[7]: https://github.com/rsalmei/zsh-history-to-fish
[8]: https://gist.github.com/dvdbng/e08ce5ba50f224b59c9fe2a91fdcea61
[9]: https://duckduckgo.com/?q=migrate+zsh+history+to+fish&t=osx&ia=web
[10]: https://vscodium.com
