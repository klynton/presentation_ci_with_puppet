!SLIDE

# Deploy Branches to Puppet Environments #

!SLIDE transition=fade smbullets incremental

## *A quick review*

- We have a **Pupeptfile** in a **control repo** on **git**
- For **each topic branch** we have **different modules** declared in the **Puppetfile**
- We have a **RESTful** webhook on the Puppet master, and our **git repo** is configured to **POST** on ***'git push'***  
- ... now the fun part

!SLIDE transition=fade smbullets incremental

# deploy() #

!SLIDE smaller 
## First, the ***JSON sent in the POST*** to our Puppet master ***is parsed for the branch ref***:
	@@@ Ruby
	...
	post '/payload' do
		request.body.rewind  # in case someone already read it
		data = JSON.parse request.body.read
		branch = data['ref'].split("/").last
		"ref branch: #{branch}" # ref branch: development
		deploy(branch)
	end

##  Second, ***an RPC mCollective agent runs r10k*** which pulls down the updated branch from the repo:
	@@@ Ruby
	...
	def deploy(branch)
   		options =  MCollective::Util.default_options
   		options[:config] = '/var/lib/peadmin/.mcollective'
   		client = rpcclient('r10k', :exit_on_failure => false,:options => options)
  		client.discovery_timeout = 10
   		client.timeout = 120
   		result = client.send('deploy_only', branch)
   		{:status => :success, :message => "r10k started"}.to_json
	end

!SLIDE

# Wait, what?! #
 
!SLIDE

# So... #

!SLIDE small bullets incremental 

## On my dev machine, ***I added some modules to the Puppetfile for branch development*** 
* **'git push'** 
* The ***git repo*** received the push and sent a ***POST*** to our Puppet master URI
* On receiving the ***POST*** our webhook running on the Puppet master ***parsed the JSON sent with the POST*** and pulled down the branch with r10k.
* *Automagically*

!SLIDE bullets incremental 
 
## This is a *somewhat* typical CI setup for Puppet code

* But it's **missing some critical parts**
* ***No QA*** checking of the code 
* You could ***easily push bad code to branch*** **'production'** and break your kingdom

!SLIDE

# ***Basic*** *code checks using local* **git hooks** #

!SLIDE bullets incremental

## Run ```$ puppet parser validate``` on every **'git commit'**

* It should ***only run on*** '.pp' files
* It should ***cancel commit*** for exit code < 0 

!SLIDE

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
