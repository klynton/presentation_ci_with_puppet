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

!SLIDE small
# ```$confdir/.git/hooks/pre-commit```
## Let's run the parser to validate first:

	@@@ Ruby
	for file in `git diff --name-only --cached | grep -E '\.(pp)'`
	do
	if [[ -f $file && $file == *.pp ]]
	then
        	puppet parser validate $file
        	if [[ $? -ne 0 ]]
        	then
        		echo "ERROR: puppet parser failed at: $file"
               		syntax_is_bad=1
        	else
           		echo "OK: $file looks valid"
       		fi
     	fi
	done
	echo ""

!SLIDE small
# ```$confdir/.git/hooks/pre-commit```
## Let's run the code through puppet lint for syntax while we're at it:
	@@@ Ruby
	for file in `git diff --name-only --cached | grep -E '\.(pp)'`
	do
		# Only check new/modified files that end in *.pp extension
	if [[ -f $file && $file == *.pp ]]
	then
		puppet-lint \
		--no-80chars-check \
		--no-autoloader_layout-check \
		--no-nested_classes_or_defines-check \
		--with-filename $file
		# Set us up to bail if we receive any syntax errors
		if [[ $? -ne 0 ]]
		then
			syntax_is_bad=1
		else
			echo "$file looks good"
		fi
	fi
	done
	echo ""
!SLIDE small
# ```$confdir/.git/hooks/pre-commit```
## Let's get super fancy and make sure our ERB templates are going to be rad:
	@@@ Ruby
	for file in `git diff --name-only --cached | grep -E '\.(erb)'`
	do
	if [[ -f $file ]]
	then
		erb -P -x -T '-' $file | ruby -c
		if [[ $? -ne 0 ]]
		then
			echo "ERROR: ruby template parser failed at: $file"
			syntax_is_bad=1
		else	
			echo "OK: $file looks like a valid ruby template"
		fi
	fi
	done
	echo ""

!SLIDE small
# ... don't forget our exit code:
	@@@ Ruby
	if [[ $syntax_is_bad -eq 1 ]]
	then
		echo "FATAL: Syntax is bad. See above errors"
		echo "Bailing"
		exit 1
	else
		echo "Everything looks good."
	fi

!SLIDE	

# So far, this has been ***simple*** #

!SLIDE bullets incremental 

## What we've seen:
* Basic CI with puppet code
* Git commit checks code locally for basic syntax, parsability and template structure
* Git push sparks off webhook to run r10k
* r10k pulls down the pushed branch to the Puppet master

!SLIDE bullets incremental 


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
