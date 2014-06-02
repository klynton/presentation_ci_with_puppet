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
