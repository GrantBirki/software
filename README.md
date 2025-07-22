# software

[![deploy](https://github.com/GrantBirki/software/actions/workflows/deploy.yml/badge.svg)](https://github.com/GrantBirki/software/actions/workflows/deploy.yml)
[![Unlock On Merge](https://github.com/GrantBirki/software/actions/workflows/unlock-on-merge.yml/badge.svg)](https://github.com/GrantBirki/software/actions/workflows/unlock-on-merge.yml)
[![ci](https://github.com/GrantBirki/software/actions/workflows/ci.yml/badge.svg)](https://github.com/GrantBirki/software/actions/workflows/ci.yml)

## Development

First, run `script/bootstrap` to get your environment set up for development. This will install the necessary dependencies.

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

If you wish to render open graph images, you can use the following commands:

```bash
script/images
```

Then copy the resulting PNGs that got generated from the front matter of your blog posts into their corresponding `content/posts/<post-name>` directories.

The text on the open graph images comes from the following front matter fields of a blog post:

```yaml
ogTitle: Grant Birkinbine
ogDescription: "Here is some description"
```

After you drop the resulting PNGs into your `content/posts/<post-name>` directories, you can point the following front matter field to the PNG file:

```yaml
ogImage: /posts/example/og.png
```
