# Blog

## Getting Started

First need to install ruby (following [this guide](https://jekyllrb.com/docs/installation/macos/)):

```bash
brew install ruby@3

# Add to ~/.zshrc

echo 'export PATH="/opt/homebrew/lib/ruby/gems/3.2.0/bin:$PATH"' >> ~/.zshrc

# restart console
gem install jekyll
```

Next run the local server:

```bash
bundle exec jekyll serve
```

## Structure

TODO

## Future Features

- [x] Integrate Disqus comments
- [x] Add tags
- [ ] Custom layout
- [ ] Add search
- [ ] Find better way for citation system?
- [ ] Port to Flutter Blog?
