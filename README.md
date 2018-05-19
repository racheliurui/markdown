title: Hey, all StackOverflow
date: 2017-05-05 20:10:33
tags:
- Hexo  
---


# updates

* 12 May 2018
  * update Hexo , Next Theme ; add DISQUS  


# How to update the Hexo & Theme

1) update Hexo

update the package.json

2) clone the Hexo Branch back to local

```
git clone -b hexo git@github.com:racheliurui/racheliurui.github.io.git

# for example we want next v5.1.2
mkdir themes/next512
curl -L https://api.github.com/repos/iissnan/hexo-theme-next/tarball/v5.1.2 | tar -zxv -C themes/next512 --strip-components=1

# update racheliurui.github.io/_config.yml pointing to new theme (folder name)
# backup & update new theme's _config.yml file (referring to old theme config file)
# Refer to Theme's github website to do any extra config
# git add -A; git commit "udpates"
git push  origin hexo
```

## other themes

https://github.com/ptsteadman/hexo-theme-corporate-example

https://www.duyidong.com/2017/03/07/Deploy-Hexo-to-S3/

https://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html
