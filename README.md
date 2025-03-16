# Personal blog [![Deploy Hugo site to Pages](https://github.com/dminca/dminca.github.io/actions/workflows/hugo.yml/badge.svg)](https://github.com/dminca/dminca.github.io/actions/workflows/hugo.yml)

## Initial setup
On a first clone, you'd wanna do it this way

```sh
git clone https://github.com/dminca/dminca.github.io.git
```

Then we have to pull the theme which is installed with `hugo`

```sh
hugo mod get -u
```

## Local development
Before pushing on the `hugo` branch which is our 'production' (so-to-speak),
you'd wanna test out your changes quickly locally.

After adding content or modifying anything in the blog, compile it

```sh
hugo --gc --buildDrafts --minify
```

This makes sure that the blog can be rendered and it's also compacting the CSS
and JavaScript in order to have a smaller footprint on the web.

Run the server locally and see how it looks

```sh
hugo serve --buildDrafts -p 8080
```

Now head-off to http://localhost:8080 and check if all okay

## Adding content

1. follow [this guide][3]

```sh
hugo new content content/posts/my-first-post.md
```

### Assets (media and all that)
They're tracked with [`git-lfs`][1] which I've installed via

```sh
brew install git-lfs
```

And then initialised it

```sh
git lfs install
```

#### Tracking files

```sh
git lfs track "*.png"
```

Commit the [`.gitattributes`](.gitattributes) file and done.

#### How does the CI handle that?
That flag's [enabled by default][2] in Woodpecker CI

# Troubleshooting

## template for shortcode not found

```sh
⋊> ~/P/c/d/pages on hugo ◦ hugo -D --gc --minify && hugo serve --gc -D -p 8080
Start building sites … 
hugo v0.101.0+extended darwin/arm64 BuildDate=unknown
Error: Error building site: "/Users/dminca/Projects/codeberg.org/dminca/pages/content/posts/awesome-foss-tooling.md:24:1": failed to extract shortcode: template for shortcode "button" not found
Total in 47 ms
```
Can be resolved by clearing the hugo module cache
```sh
hugo mod clean
```


[1]: https://git-lfs.github.com
[2]: https://woodpecker-ci.org/plugins/plugin-git
[3]: https://gohugo.io/getting-started/quick-start/#add-content
