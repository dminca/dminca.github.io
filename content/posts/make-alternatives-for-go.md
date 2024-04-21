---
title: "GNU-Make Alternatives for Go"
date: 2022-08-01T12:40:39+02:00
draft: false
tags: [ make, go, golang, tools ]
categories: [ Tooling ]
---

C/C++/C# has a way to compile their source code with the help of `Makefiles`,
but how about `Go`? Sure, it's minimalistic, you can just do `go build` and
you're done, but if you want to fine-tune something that one-liner turns into
a Bratwurst 🌭

[I've asked around][2] on the [#go-lang:matrix.org][1] room which alternatives exist
and got 2 tools so far.

# Task

{{< figure
    src="https://taskfile.dev/img/logo.svg"
    alt="Taskfile logo"
    width="20%"
    height="20%"
    caption="Official logo of [Taskfile](https://taskfile.dev)"
    default=true
    >}}

Task is a task runner / build tool that aims to be simpler and easier to use
than, for example, GNU Make.

{{< button href="https://taskfile.dev" target="_blank" >}} Source {{< /button >}}

# Mage

{{< figure
    src="https://magefile.org/images/gary.svg"
    alt="Mage logo"
    width="20%"
    height="20%"
    caption="Official logo of [Mage](https://magefile.org)"
    default=true
    >}}

Mage is a make/rake-like build tool using Go. You write plain-old go functions,
and Mage automatically uses them as Makefile-like runnable targets.

{{< button href="https://magefile.org" target="_blank" >}}Source{{< /button >}}


[1]: https://matrix.to/#/#go-lang:matrix.org
[2]: https://matrix.to/#/!iupBrYlVdqKLEkWtDs:tapenet.org/$ANxnsmi8HzVIYz1vY3ngsnUKocLf4mCxhKalo8KEagQ?via=matrix.org&via=tchncs.de&via=privacytools.io
