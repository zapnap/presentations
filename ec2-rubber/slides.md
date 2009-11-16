!SLIDE intro full
# EC2 for Rails Deployment
[Nick Plante](http://blog.zerosum.org), NH.rb

November 16, 2009

!SLIDE history full
# Once Upon A Time...
#### Deploying Ruby Web Apps was a *Pain in the Ass*.

!SLIDE modern full
# Modern Conveniences
### Make VPS Deployment much easier:
<br/>
#### Apache / Passenger, [Capistrano](http://capify.org), [GitHub](http://github.com/guides/deploying-with-capistrano), Hoptoad

!SLIDE capistrano split
# Capistrano
### Manage remote servers and automate remote tasks
* Deploy versioned code from source control
* Back up old versions (for roll-back)
* Migrate database (optional)
* Link shared assets, etc. Whatever you want!

!SLIDE capsetup split
# Get Capistrano!
<br/>
* gem install capistrano
* cd your-rails-app
* capify .

<br/>

#### edit config/deploy.rb

!SLIDE capcode split
* cap deploy:setup
* cap deploy:cold
* cap deploy
* cap -T

<br/>

#### * Remember to set up your ssh keys if deploying from GitHub!

!SLIDE palatable full
## This all makes deployment much more palatable.
<br/>
#### And yet...

!SLIDE problems split
### Provisioning servers still takes time.
<br/>
### Configuring servers / services still takes time.
<br/>
### Horizontal scaling takes a lot of time, money, effort.

!SLIDE cloud full
# Amazon Elastic Compute Cloud
#### Enter: The Cloud ([EC2](http://aws.amazon.com/ec2))
<br/>
* Fire up a server with no waiting period.
* Deploy a working systems image (AMI).
* Scale at will, on-demand. Pay for what you use.
* Plug in other AWS pieces ([S3](http://aws.amazon.com/s3), [EBS](http://aws.amazon.com/ebs), [ELB](http://aws.amazon.com/elasticloadbalancing)).

!SLIDE cloudinstances full

!SLIDE cloudissues split
# Complaints?
## It's not "Easy to Use"
<br/>
* Tools like [ElasticFox](http://developer.amazonwebservices.com/connect/entry.jspa?externalID=609) help
* But what we really need is scriptability (!)

!SLIDE scripting full
# Ruby tools (surprise) make this easier
<br/>
* [Rubber](http://github.com/wr0ngway/rubber)
* [PoolParty](http://github.com/auser/poolparty)
* [Moonshine](http://github.com/railsmachine/moonshine)

<br/><br/>

#### Also: see commercial offerings like RightScale, Scalr.

!SLIDE rubber split
# Rubber

* Extends Capistrano
* Support for roles
* Horizontally scale up and down
  * Single instance with everything
  * Or deploy roles to separate instances
* Baked-in redundancy
  * (cuz instances can disappear)

!SLIDE rubbersetup split
# Installing Rubber
### gem install rubber
<br/>
* Sign up for EC2, get keypair
  * Optionally set up rubber-secret.yml
  * Rubber sets up EC2 security groups

!SLIDE vulcanize split
# Vulcanize!
### script/generate vulcanize complete_passenger_mysql
<br/>
* Edit config/rubber/rubber.yml
  * Add keys, account info
  * List system packages, gems to install
  * Dynamic DNS config, etc

!SLIDE staging split
# Create an Instant Staging Instance
<br/>
* cap rubber:create
* cap rubber:bootstrap
* cap deploy
* or...

<br/>

### cap rubber:create_staging

!SLIDE railsenv full
### Use RAILS_ENV to support multiple staging instances
<br/>
* cp config/production.rb config/nick.rb
* RUBBER_ENV=nick cap rubber:create_staging

<br/>
#### Surf to http://nick.mydomain.com

!SLIDE rubberadmin split
# Rubber Admin Server
<br/>
### Web Admin UIs for Monit / HAProxy
### Check server stats (Munin)
<br/>
* ALIAS=tools ROLES=web_tools cap rubber:create
* cap rubber:bootstrap
* cap deploy
* visit http://tools.mydomain.com:8080

!SLIDE scale split
# Time to scale up?
### Add an app server to handle increased load
#### Roles in default templates can scale by default
<br/>
* ALIAS=app02 ROLES=app cap rubber:create
* cap rubber:bootstrap
* cap deploy

!SLIDE rubberconfig full
# Further Customization
### Create custom roles (need background queue / workers?)
### Config files are just ERB templates
<br/>
#### More information at the <a href="http://wiki.github.com/wr0ngway/rubber/configuration">Rubber Wiki</a>

!SLIDE demo full
# Insert Demo Here.

!SLIDE thanks split
# Thanks
<br/>
### Rubber was created by <a href="http://twitter.com/mattconway">Matt Conway</a>
<br/>
### Questions?
#### <a href="http://twitter.com/zapnap">@zapnap</a>
