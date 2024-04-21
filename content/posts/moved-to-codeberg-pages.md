---
title: "Moving Hugo from GitHub Pages to Codeberg Pages"
date: 2022-08-13T20:10:56+02:00
draft: false
tags: [ howto, tutorial, hugo ]
categories: []
---

# Disable GitHub Pages

## Add deprecation notice

{{< figure
    src="/gh03.png"
    alt="Add deprecation notice in README.md"
    caption="Let your audience know where you moved"
    >}}

Commit and [push it to your repo](https://github.com/dminca/dminca.github.io/commit/3f7244785acc93a47f849999fca78e4d19f522c3)

## Archive repo

{{< alert "lightbulb" >}}
It may be possible that before archiving you'd have to toggle Pages off.
Unfortunately I can't see that option anymore since I've disabled it already. If
that's the case and you find the option in repo Settings be sure to do that
prior to archiving it.
{{< /alert >}}

{{< figure
    src="/gh04.png"
    alt="Archive the repository"
    caption="Archive your repo to disable Pages"
    >}}

# Create Codeberg repo

Head over to [codeberg.org](https://codeberg.org) and create **an empty repo** called `pages`,
you can follow the instructions from https://codeberg.page

{{< alert "triangle-exclamation" >}}
Remember that your `main`/`master` branch is where you'll store the generated
website only. No markdown, no hugo files nothing. Only the result of site build.
{{< /alert >}}

Keep the repository empty for now, we'll come back to it later.

# Move your content

In your old repo where used to be GitHub Pages set a new `git remote`

```sh
git remote set-url origin https://codeberg.org/<user>/pages.git

# rename main/master branch
git branch -m hugo

# copy compiled website up a dir
cp -vr public ../

# move to your main/master Codeberg branch
git checkout -b main

# bring the compiled website on this branch
cp -vr ../public/** .

# commit and push
git add . ; git commit -m 'initial commit'; git push -u origin main
```
{{< alert "circle-info" >}}
Replace `<user>` with your codeberg username. Just set the Codeberg repo as the
main remote.
{{< /alert >}}

The `public/` dir is where your website is dumped after `hugo` build, depending
on your installation some may have it in `docs/` dir. Adjust this accordingly.

When you create a new Codeberg repo it defaults to `main` branch. The name has
no importance as long as the repo settings point to it then your website will
work perfectly fine.

# Automate building your site
Let's automate the above steps because repetitive tasks are not worth to be done
by humans but rather by :robot:
## Request Woodpecker CI access

Head over to [codeberg-ci/request-access](https://codeberg.org/Codeberg-CI/request-access)
and [request Woodpecker CI access](https://codeberg.org/Codeberg-CI/request-access/issues/new).

You could also [donate to Codeberg](https://codeberg.org/Codeberg/org/src/branch/main/Imprint.md#sepa-iban-for-donations) to support them paying for the infra.

## Configure pipeline

{{< alert "lightbulb" >}}
You should have received your CI access to proceed with these steps.
{{< /alert >}}

We basically have to replicate the above steps in your <abbr title="Continous Integration">CI</abbr>
and we do that by creating a file at repository root level `.woodpecker.yml`

You can check out mine as an example [.woodpecker.yml](https://codeberg.org/dminca/pages/src/branch/hugo/.woodpecker.yml)

{{< alert "triangle-exclamation" >}}
Mine is configured with `master` (lines 18,20,26) as the main branch. Adjust this accordingly.
{{< /alert >}}

{{< highlight python "linenos=table,hl_lines=12 18 20 26,linenostart=1" >}}
pipeline:
  build:
    image: klakegg/hugo:0.101.0-ext-alpine-onbuild
    commands:
      - hugo mod get -u
      - hugo --gc --buildDrafts --minify
    when:
      event: [pull_request, push]

  publish:
    image: bitnami/git:2
    secrets: [ cbtoken ]
    commands:
      - apt-get update; apt-get install git-lfs --no-install-recommends -y
      - git config --global --add safe.directory $CI_WORKSPACE/public
      - git config --global user.email "woodpecker-bot@no-reply.eu"
      - git config --global user.name "woodpecker-bot"
      - git config --global init.defaultBranch master
      - git config --global lfs.contenttype 0
      - git clone -b master https://codeberg.org/dminca/pages.git
      - cp -a public/* pages/
      - cd pages/
      - git remote set-url origin https://$CBTOKEN@codeberg.org/dminca/pages.git
      - git add --all
      - git commit -m "deploy $CI_COMMIT_SHA"
      - git push -u origin master
    when:
      event: push
      branch: hugo
{{< / highlight >}}

This pipeline was inspired from [codeberg.org/Codeberg-CI/examples](https://codeberg.org/Codeberg-CI/examples/src/branch/main/Hugo/hugo.yml).

For the <abbr title="Continous Integration">CI</abbr> runner to have push access
to the repo
* create a token in your Codeberg profile settings
* head over to https://ci.codeberg.org/repos and locate your repo
* in repo settings create a Secret `CBTOKEN` and paste the created token (we're refering to it on line 12 in `.woodpecker.yml`)

And that's all! Congratulations, you've successfully migrated from GH Pages to
Codeberg Pages.

You can brag on their matrix channel [#codeberg.org:matrix.org](https://matrix.to/#/#codeberg.org:matrix.org) that you're all set-up.

# Bonus tips
* Store your assets (ie. pictures, pdfs) with [`git-lfs`](https://git-lfs.github.com)
so that your repo doesn't become more sluggish. I've configured mine for pictures
[.gitattributes](https://codeberg.org/dminca/pages/src/branch/hugo/.gitattributes), also
you can check the [README.md](https://codeberg.org/dminca/pages/src/branch/hugo/README.md) for `git lfs` initial instructions
* ignore build directory on your `hugo` branch, [for example](https://codeberg.org/dminca/pages/compare/f6791243ca942e940b7d71bdc3bb6ac031c8620e..3411f60c15a112af70760ba7f64e79fd5900bd3f)

{{< alert "lightbulb" >}}
Ignoring the build dir in your `hugo` branch will help keeping your commit
history sane and in the event of introducing new content you won't have to commit
a whole chunk of HTML, CSS and all other unrelated stuff, just your markdown
file containing your blog post.
{{< /alert >}}
