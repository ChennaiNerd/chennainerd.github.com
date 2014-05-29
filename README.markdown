## What is ChennaiNerd?

We are passionate about latest technologies and trends. We love web and always
live on web. We truly believe that learning comes from sharing.
We use this platform to share what we know and like to know what you got.
Finally, We are from Great Place <strong style="color:#d91459">Chennai, INDIA</strong>

### Create and Preview

    rvm use ruby-1.9.3-p484               (Optional. Needed only when you have <=ruby 1.8)
    rake new_post["title of the blog"]
    rake preview

### Generate and Deploy

    rake generate
    rake deploy

    (or)

    rake gen_deploy

Above command will deploy and commit latest changes to source branch.
If you are new user, then run below command before above two commands

    rake setup_github_pages

CAUTION: When you push changes, push it to source branch alone

    git push origin source

## License

(The MIT License)

