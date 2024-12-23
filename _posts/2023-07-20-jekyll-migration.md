---
layout: post
title: "I♥Tech: Site migrated to Github Pages and from Hugo to Jekyll"
categories: I♥Tech
---

Since Hugo was somehow limited and I was sick taking care of it, I moved my site to GitHub Pages. These are already public documents more or less, so it doesn't matter where the files are located. For everyone interested, this is a pretty straight forward process.


## Theme
I have choosen "Chirpy" as theme for Jekyll.
The features are exactly what I want. Search, black/white theme, archive. Perfect.

## Roadmap

1. Login to GitHub
1. Use template   
See [Chirpy Option 1](https://chirpy.cotes.page/posts/getting-started/#option-1-using-the-chirpy-starter) and use the template to create your repository
1. Update your _config.yml
1. Put your images in _assets
1. Put your posts in _posts  
Post filenames: yyyy-mm-dd-titel-of-the-post.md   
All post need to start with a yaml front matter:  
```yaml
---
layout: post
title: "I♥Tech: Site migration"
---
```
1. DNS Update  
You need to update your DNS to point your chooses subdomain to github.io.  
CNAME mysubdomain.domain.tld -> myusername.github.io
1. Wait until its propagated
1. GitHub -> Repository -> Settings -> Pages  
Custom DNS and add your mysubdomain.domain.tld
1. Update _config.yml  
url: "https://mysubdomain.domain.tld/"