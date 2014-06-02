!SLIDE 
## Continuous Integration with Puppet  

!SLIDE transition=fade

![this](cont_integration_dare.jpg) 

(let's go over some terminology first)

!SLIDE transition=fade

## Continuous Integration

***streamlined promotion*** of code through *dev - test - production* 

!SLIDE transition=fade

## Continuous Delievery

CI ***with a human controlled*** realase gate to production.

!SLIDE transition=fade

## Continuous Deployment

CI ***with automated*** release gate to production

!SLIDE transition=fade

# The Theory of Constraints: #
## ***Any improvement*** made anywhere ***besides the bottleneck*** is an ***illusion***

!SLIDE bullets incremental transition=fade

# The Goal: 
* Automate things so it's **like a factory**.
* Then use **factory patterns** to ***solve your problems***.

!SLIDE code transition=fade

<pre class="sh_Puppet">
class carFactory (
	$chasis,
	...
)inherits carFactory::params{	
	include carFactory::robots
	include carFactory::assemblyLine
	include carFactory::qualityAssurance
	...
}
</pre>

