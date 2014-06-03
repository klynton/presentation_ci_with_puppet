!SLIDE 

# Let's get crazy #

!SLIDE transition=fade

# I don't like to get crazy #

!SLIDE bullets incremental transition=fade

# ... but if I did ... #
* *and I highly recommend you do*

!SLIDE bullets incremental 

# I'd start with ***unit testing***

* Generally speaking **unit testing** enables you to ***test how code behaves*** 
* Literally, ***in the context of Puppet***, they are testing if ***resources declared in your manifests are in the compiled catalog*** 
* ... and have valid parameters. 

!SLIDE small

## ```logrotate/spec/defines/rule_spec.rb```

	@@@ Ruby
	require 'spec_helper'

	describe 'logrotate::rule' do
		let(:title){'nginx'}
		let(:rule){"#{title}"}
		
		it {should contain_class('logrotate::setup')}
		it do
			should contain_file("logrotate_#{rule}").with({
			'ensure' => 'file',
			'path'   => "/etc/logrotate.d/#{rule}",
			})
		end
	end

!SLIDE bullets incremental

## **Make robots run the test for you** 
* ```$module/spec``` is just a subdir of your modules, *so it goes to git when your code does*
* Leverage ***Jenkins*** to run the test when the code is pushed
* Or you can have a tool like ***Travis CI do it for you*** 

!SLIDE 

# THERE'S A LOT MORE #

!SLIDE transition=fade

# BUT THIS IS AN INTRO #

!SLIDE bullets incremental

# *What I did not cover* 
* ***System Testing***
* ***Acceptance Testing***
* ***Leveraging r10k for Hiera data*** 
* ...*and leveraging CI tools to deploy said data*
* ***Multi Master Code Deployment***

!SLIDE transition=fade bullets incremental

# QUESTIONS/DISCUSSION #


