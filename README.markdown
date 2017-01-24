## What is ChennaiNerd?

We are passionate about latest technologies and trends. We love web and always
live on web. We truly believe that learning comes from sharing.
We use this platform to share what we know and like to know what you got.
Finally, We are from Great Place <strong style="color:#d91459">Chennai, INDIA</strong>

### Setup the pages

[Important] If you clone the repo for the first time, then you should run below command before any other commands

    rake setup_github_pages

CAUTION: When you push changes, push it to source branch alone

    git push origin source

### Create and Preview

    rvm use ruby-2.1.0               (Optional. Needed only when you have <=ruby 1.8)
    rake new_post["title of the blog"]
    rake preview

### Generate and Deploy

Since octopress gen_deploy does not work properly. We are taking shortcuts.
You need to have two cloned repo. One is for source branch. One is for master branch which is used for deployment

    gcl git@github.com:ChennaiNerd/chennainerd.github.com.git
    gcl git@github.com:ChennaiNerd/chennainerd.github.com.git chennainerd.github.com-deploy

    cd chennainerd.github.com && gco source
    cd chennainerd.github.com-deploy && gco master
    cp chennainerd.github.com/build.sh chennainerd.github.com-deploy/

1. Generate files

    cd chennainerd.github.com/
    rake generate

2. Deploy files

    cd chennainerd.github.com-deploy/
    ./build.sh
    git commit
    git push

## License

The MIT License

