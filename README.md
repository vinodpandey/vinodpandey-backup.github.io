#Documentation
####Installing required dependencies
```sh
gem install jekyll
gem install stringex
```

 See below link if error in installing reby gems.  
 [http://stackoverflow.com/questions/19150017/ssl-error-when-installing-rubygems-unable-to-pull-data-from-https-rubygems-o](http://stackoverflow.com/questions/19150017/ssl-error-when-installing-rubygems-unable-to-pull-data-from-https-rubygems-o)

####checkout code
```sh
git clone git@github.com:vinodpandey/vinodpandey.github.io.git
```

####preview
```sh
jekyll serve --watch
```

####Creating new post
```sh
rake new_post['new post title']
```

####Creating new page
```sh
rake new_page['new page title']
```

####Check-in updates
```sh
git add .
git commit -m "comment"
git push origin master
```

Gihub will automatically generate the website once the updates are checked-in.

#Credits
Below softwares are used/modified for creating this blog.  
* [Octopress](https://github.com/imathis/octopress)  
* [Jekyll](http://jekyllrb.com/)  
* [LightPaper](http://clockworkengine.com/lightpaper-mac/)  
