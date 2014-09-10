
# nifty-conventions

> nifty conventions is an opinionated set of conventions surrounding code guidelines, general project requirements, and git workflow for **Rapid MVP's** (namely for use in conjunction with [Eskimo][eskimo]-based projects).


## index

* [code guidelines](#code-guidelines)
* [general project requirements](#general-project-requirements)
* [git workflow](#git-workflow)
* [license](#license)


## code guidelines

* 2 spaces (no tabs)
* semi-colons \*optional\*
  - do use [uglify][uglify] for production deployments 
  - do use [jshint][jshint] for checking missing semicolons and linting code (also use a `.jshintrc` file and share it in the repo with your team; make sure everyone has JSHint plugins on their IDE as well that consume this config file) 
  - **do not mix usage of semi-colons and no semi-colons**
* use apostrophes instead of quotes (when possible)
* peer-review occasionally \*optional for solo projects\*
  - do review other's code when merging required
  - do comment on commits and follow-up if needed
* pull requests
  - do use pull-requests on public open-source repos
  - _do not use pull-requests on private team repos_
* always return early (you don't always need brackets `{}`)

    ```js
    // good   
    if (err) return next(err);
          
    // bad
    if (err) {
      next(err);
    }
    next();

    ```
* **do not run into ["callback hell"][callback-hell]**
* **do not use unnecessary spaces** (think how many times you'll be hitting `SPACE`; think of readability vs. time spent coding)

    ```js
    // good
    if (foo)
      return getBar("good");

    // bad
    if ( foo )
      return getBar( "bad" );
      
    ```
    
* see [Git Workflow](#git-workflow) below


## general project requirements

* use [Eskimo][eskimo] for building the project ("Rapid MVP")
* Readme First Approach ("RFA") &ndash; start a "Readme.md" Markdown file and document the project's needs and scope before you begin.  If you're writing an API, then document the API.  If you're writing a web app, then document the web app's routes and core functionality.
  - spend more time thinking and researching than writing
  - create wireframes/sketches ("mockups") on paper/pen
  - **do not waste time with extremely-detailed mockups**
  - **do not write run-on sentences**
  - **do not worry about punctuation or capitalization, (but be consistent with approach and style)**
  - write succintly and cut out as many words as possible to make a point, save your reader time
  - use analogies when possible and use links everywhere
  - include an "Index" with links to anchors throughout the Markdown file
  - include a "License" and "Contributors" section
  - include a blockquote with a brief description of the project as an opener to the reader
* test-driven-development (and deployment) ("TDD")
  - [Travis-CI][travis-ci]
      * deployment using webhooks
          - use when your server is behind a VPN
      * deployment using ssh
          - **do not use when your server is behind a VPN**
      * **do not allow deployment when builds fail**
  - [Coveralls][coveralls] and [Istanbul][istanbul]
  - [Mocha][mocha]
      * **do not use with gulp, use simply with npm script in `package.json`**
  - [Chai][chai]
      * use with [sinon-chai][sinon-chai]
  - [New Relic][new-relic]
  - [Gulp][gulp]
      * use a watch task
            - with live reloading of CSS/JS
            - with jshint
  - use [zero-downtime deployments][zero-downtime]
* application environments
  - local &ndash; port `3000`
  - server with [node-http-proxy][node-http-proxy] or nginx (with reverse-proxy) running on port `80` (see "server setup" bullet point below)
      * development ("dev") &ndash; accessible behind [BasicAuth][basic-auth] at a URL such as <http://dev.project.com>, port `3040`, use unique DB, same server as production
      * staging ("stag") &ndash; **do not use a staging environment for Rapid MVP's, it's overkill**
      * production ("prod") &ndash; accessible at a URL such as <http://project.com>, port `3080`, use unique DB, same server as development
* server setup
  - Ubuntu 14.04 LTS 64-bit
      * [lock down your server heavily][ubuntu-security]
      * **do not allow root nor password-based log in attempts**
      * **do not require a VPN to connect to the server (should be end-user's responsibility for secure connection)**
      * do create a new user per member on the team (and add their SSH public key to `/home/${USER}/.ssh/authorized_keys`  
* development setup
  - text editor (one of the following)
        * vim (with [zsh][zsh], [oh-my-zsh][oh-my-zsh], [NERDTree][nerdtree] and [other plugins][my-vim-setup]) \*recommended\*
        * Sublime Text (>= 2)
        * Atom
  - operating system
        * one of the following:
            - Linux Mint 17 LTS
            - Ubuntu 14.04 LTS
            - Mac OS X
        * **do not use Vagrant and/or VirtualBox when possible**
        * **do not use Windows as a development environment**
  - package management
        * if Mac OS X, then use [brew][brew] and [LaunchRocket][launch-rocket]
        * if Ubuntu, then use built-in `apt-get`
* server host
  - [Digital Ocean][digital-ocean] &ndash; cheap and great for Rapid MVP's
  - [Linode][linode] &ndash; lots of CPU's (great for Node's [cluster][cluster] module), but somewhat expensive
  - [Amazon EC2][amazon-ec2] &ndash; lots of features, but expensive
* security considerations
  - use VPN when connected to public connections \*or always\*
  - **do not share passwords/keys in repositories when possible**
* use [Jade][jade] for writing short-hand HTML
* use [LESS][less] for CSS and [Bower][bower] for front-end package management
* use [email-templates][email-templates] for emails along with [Postmark][postmark] for transactional email or [Sendgrid][sendgrid] for bulk email (be sure to enable DKIM/SPF signing)
* use [Slate][slate] for API documentation
* use [StatusPage][status-page] for API status page hosting
* use [Compose][compose] for MongoDB hosting
* use [RedisToGo][redis-to-go] for Redis hosting
* use [CDN-hosted assets][cdn-hosted-assets] on Amazon [S3][amazon-s3] and [CloudFront][amazon-cloudfront]
* write log files on server and use [logrotate][logrotate]
* use [upstart][upstart] for node processes on the server
* use [Amazon Route 53][amazon-route-53] for DNS hosting
* use [Namecheap][namecheap] for domain name registration
* use [Google Hangouts][google-hangouts] for team video chat
* use [Slack][slack] for team chat
* use [GeoTrust RapidSSL] (cheap) or [Digicert][digicert] (expensive) for SSL security certificates in production \*regardless if application handles financial transactions\*
* use [Stripe][stripe] for payments


## git workflow

1. read [GIT Conventions][git-conventions]

2. install [git-extras][git-extras] (and set up [git aliases][git-aliases])

3. edit and save changes to a file(s)

4. lint, test, add, commit, pull, and push changes

    ```bash
    # optional
    gulp jshint
    npm test
    
    # required
    git add --all
    git commit -m 'fixed opacity of header. closes #1'
    git pull
    git push
    ```

5. check to make sure builds pass and deploy changes (should be automatic with [Travis-CI][travis-ci])

6. repeat steps 3-5


## license

[MIT](LICENSE)


[git-aliases]: http://tjholowaychuk.tumblr.com/post/26904939933/git-extras-introduction-screencast
[git-conventions]: https://medium.com/code-adventures/git-conventions-a940ee20862d
[git-extras]: https://github.com/visionmedia/git-extras
[callback-hell]: http://callbackhell.com/
[uglify]: https://github.com/terinjokes/gulp-uglify
[jshint]: https://github.com/spenceralger/gulp-jshint
[jade]: http://jade-lang.com
[less]: http://lesscss.org/
[email-templates]: https://github.com/niftylettuce/node-email-templates
[postmark]: https://postmarkapp.com/
[sendgrid]: https://sendgrid.com/
[digital-ocean]: http://digitalocean.com
[linode]: http://linode.com
[amazon-ec2]: https://aws.amazon.com/ec2/
[cluster]: http://nodejs.org/api/cluster.html
[namecheap]: http://namecheap.com
[google-hangouts]: https://plus.google.com
[amazon-s3]: https://aws.amazon.com/s3/
[stripe]: http://stripe.com
[digicert]: http://digicert.com
[slack]: http://slack.com
[amazon-route-53]: https://aws.amazon.com/route53/
[amazon-cloudfront]: https://aws.amazon.com/cloudfront/
[node-http-proxy]: https://github.com/nodejitsu/node-http-proxy
[status-page]: http://statuspage.io
[logrotate]: http://linuxcommand.org/man_pages/logrotate8.html
[cdn-hosted-assets]: https://github.com/niftylettuce/eskimo/tree/master/examples/cdn-hosted-assets
[compose]: http://compose.io
[redis-to-go]: https://redistogo.com
[slate]: https://github.com/tripit/slate
[new-relic]: http://newrelic.com
[zero-downtime]: https://github.com/niftylettuce/eskimo/tree/master/examples/zero-downtime-reloads
[brew]: http://brew.sh
[upstart]: http://upstart.ubuntu.com/
[launch-rocket]: https://github.com/jimbojsb/launchrocket
[nerdtree]: https://github.com/scrooloose/nerdtree
[my-vim-setup]: https://github.com/niftylettuce/.vim
[zsh]: http://www.zsh.org/
[oh-my-zsh]: https://github.com/robbyrussell/oh-my-zsh
[eskimo]: http://eskimo.io
[travis-ci]: http://travis-ci.org
[coveralls]: https://coveralls.io
[istanbul]: https://gotwarlost.github.io/istanbul/
[mocha]: https://visionmedia.github.io/mocha/
[chai]: http://chaijs.com/
[sinon-chai]: https://github.com/domenic/sinon-chai
[ubuntu-security]: https://github.com/niftylettuce/amazon-ec2-node-stack#ubuntu-security-configuration
[basic-auth]: http://todo.com
[gulp]: http://gulpjs.com/