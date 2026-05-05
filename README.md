# software

[![deploy](https://github.com/GrantBirki/software/actions/workflows/deploy.yml/badge.svg)](https://github.com/GrantBirki/software/actions/workflows/deploy.yml)
[![Unlock On Merge](https://github.com/GrantBirki/software/actions/workflows/unlock-on-merge.yml/badge.svg)](https://github.com/GrantBirki/software/actions/workflows/unlock-on-merge.yml)
[![ci](https://github.com/GrantBirki/software/actions/workflows/ci.yml/badge.svg)](https://github.com/GrantBirki/software/actions/workflows/ci.yml)

## Development

First, run `script/bootstrap` to verify your local environment.

Now, to run the site locally, simply run:

```bash
hugo server -D
```

Now you can visit [`localhost:1313`](http://localhost:1313/) to see the site.

## Theme Updates

To pull in the latest updates from the Dario theme, run:

```bash
script/update
```

### Open Graph Images

Open Graph PNGs are committed in the relevant `content/posts/<post-name>` directories and referenced from front matter.

The text on the open graph images comes from the following front matter fields of a blog post:

```yaml
ogTitle: Grant Birkinbine
ogDescription: "Here is some description"
```

Point the following front matter field to the PNG file:

```yaml
ogImage: /posts/example/og.png
```
