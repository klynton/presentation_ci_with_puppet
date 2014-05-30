!SLIDE transition=fade

## How we do things at Puppet Labs

!SLIDE bullets incremental transition=fade

## ...we start with some tools 

!SLIDE bullets incremental transition=fade

# Git #

* Most widely used & most widely misunderstood VCS in existance
* Doesn't fit nicely with our existing compute paradigms

~~~SECTION:notes~~~

ex subversion: 
* just files and folders; commits are numbered sequentially. 
* even branching and tagging is simple — it’s just like taking a backup of a folder.

Also mention:
* **blobs** = the fundamental git *data type*
* **tree objects** = kinda like *directories*, but also kinda like sym links: can contain pointers to other blobs & trees
* **commit object** = *pointer to tree object & metadata*
* **tag object** = *pointer to commit object & metadata*
* **references** = *pointer to tag or commit object*

~~~ENDSECTION~~~

!SLIDE bullets incremental transition=fade

# Git Hooks #

* Scripts that run when particular actions occur
* (like 'commit' or 'push') 
* examples in ```your_repo/.git/hooks```

!SLIDE bullets incremental transition=fade

# RESTful Webhooks #

!SLIDE transition=fade

## On the puppet master: 

<pre class="sh_Ruby">
class Server Sinatra::Base
...
	post '/payload' do
		data = JSON.parse request.body.read
		branch = data['ref'].split("/").last
		"ref branch: #{branch}/n"
		deploy(branch)
	end
...
end

</pre>

!SLIDE bullets incremental transition=fade

# r10k  

* ***r10que?*** 

!SLIDE bullets incremental transition=fade

* ***maps git topic branches*** on a remote which contains a Puppetfile ***to local puppet environmenets*** 
* ... and deploys all the modules in that Puppetfile to the specified Puppet environment via the topic branch
* simple. 

!SLIDE bullets incremental  transition=fade

# Where do I start? #

* The ***Control Repo***

!SLIDE bullets incremental

## this **monolithic repo** contains: 

* ***Puppetfile***
* ***Hieradata***
* ...sometimes the ***hiera.yaml*** 

!SLIDE bullets incremental 

## where do the files live on the Puppet master? 

* For PE: **/etc/puppetlabs/puppet**
* For FOSS: **/etc/puppet/**

!SLIDE bullets incremental 

<pre class="sh_Bash">
$ pwd
/etc/puppetlabs/puppet/
$ git branch
  development
</pre>

* ***DO NOT*** ```git add .``` in this dir!

!SLIDE

# The ***Puppetfile*** #

!SLIDE bullets incremental

* For each version of the Puppetfile on a given topic branch in the control repo:
* Contains module references to URI's with specific (optional) refs

!SLIDE code

## On Branch Development:

<pre class="sh_Shell">
$ cat Puppetfile

mod "my_module",
  :git => "git://github.com/my_module.git",
  :ref => '2.0.1'
</pre>

!SLIDE code transition=fade

## On Branch Staging:

<pre class="sh_Shell">
$ cat Puppetfile

mod "my_module",
  :git => "git://github.com/my_module.git",
  :ref => '1.5.0'
</pre>

!SLIDE code transition=fade

## On Branch Production:

<pre class="sh_Shell">
$ cat Puppetfile

mod "my_module",
  :git => "git://github.com/my_module.git",
  :ref => '1.0.0'
</pre>

!SLIDE
 
# Deploy Branches to Puppet Environments #

!SLIDE code

 <pre class="sh_Bash">
</pre>
# Unit/System/Accceptance Testing #

SLIDE bullets incremental transition=fade

## Unit Testing 

SLIDE bullets incremental transition=fade

## System Testing

SLIDE bullets incremental transition=fade

## Acceptance Testing

SLIDE transition=fade


SLIDE transition=fade


!SLIDE bullets incremental transition=fade


!SLIDE bullets incremental transition=fade

