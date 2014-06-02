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

* Run at the local git repository (i.e., on your laptop)
* Scripts that run when particular actions occur (like a *Puppet lint check*)
* Actions like 'commit' or 'push'
* examples in ```your_repo/.git/hooks```

!SLIDE bullets incremental transition=fade

# RESTful Webhooks #

!SLIDE small bullets incremental

## Webhooks ***allow external services to be notified when certain events happen*** on a repo. 

* When the specified events happen a ***POST request is sent to each of the URLs you provide***.

* ... in our case, ***the URL points to a Sinatra server running on our Puppet master***

!SLIDE small transition=fade

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
* ... and ***deploys all the modules in that Puppetfile*** to the ***specified Puppet environment*** via the topic branch
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

## ***For each version of the Puppetfile*** on a given topic branch in the control repo:
* There are **module references to** *git* or *forge* URIs with (optional) refs 


!SLIDE commandline 

## On Branch Development:

<pre class="sh_Ruby">
$ cat Puppetfile

mod "my_module",
  :git => "git://github.com/my_module.git",
  :ref => '2.0.1'
</pre>

!SLIDE transition=fade commandline

## On Branch Staging:

<pre class="sh_Ruby">
$ cat Puppetfile

mod "my_module",
  :git => "git://github.com/my_module.git",
  :ref => '1.5.0'
</pre>

!SLIDE transition=fade commandline

## On Branch Production:

<pre class="sh_Ruby">
$ cat Puppetfile

mod "my_module",
  :git => "git://github.com/my_module.git",
  :ref => '1.0.0'
</pre>

