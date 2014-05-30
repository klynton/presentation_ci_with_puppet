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

!SLIDE transition=fade

# Quick CI w/ puppet-lint #

SLIDE transition=fade

# Unit/System/Accceptance Testing #

SLIDE bullets incremental transition=fade

## Unit Testing 

SLIDE bullets incremental transition=fade

## System Testing

SLIDE bullets incremental transition=fade

## Acceptance Testing

SLIDE transition=fade

# Full Circle: Travis CI #

SLIDE transition=fade

# RESTful Deploys #

!SLIDE bullets incremental transition=fade

# r10k #

!SLIDE bullets incremental transition=fade

# A Complete Picture #
