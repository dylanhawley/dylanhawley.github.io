# Dylan's Website

Follow [GitHub Pages documentation](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll) to test site locally with Jekyll.

```sh
$ bundle install
$ bundle exec jekyll serve
```

## Increase [PageSpeed](https://pagespeed.web.dev) Performance by converting images to WebP

How to convert PNG to WebP on Mac:

If you don't have WebP tools installed on your Mac, you can easily install them using Homebrew:

```sh
brew install webp
```

In the Terminal, navigate to the folder containing your PNG files. Use the following command to convert a PNG file to WebP:

```sh
cwebp input.png -o output.webp
```

Replace input.png with the actual name of your PNG file and output.webp with the name you want for the WebP file.

By default, cwebp applies lossy compression. You can adjust the quality of the WebP file by adding the -q option (range 0-100, where 100 is the best quality):

```sh
cwebp -q 50 input.png -o output.webp
```