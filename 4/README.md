```
shortname: PRP-4
name: Standard process to set up a local node for development & testing, using Docker Compose
type: standard
status: raw
editor: juergen@riddleandcode.com
```


## Abstract
In the current Planetmint repository existsa docker-compose.yml file, to set up a local node and run local tests.
Testing is of course not only restricted to the Planetmint repository has to be executed in repositories.

## Motivation
The following challenges became obvious during the transition to Planetmint:
- Several repositories do have different approaches on how to execute tests.
- The basic tests do not reflect all the tests that are executed during the build and CI testing and verification phase. This leads to multiple commits and a non-clear way to verify if a commit will pass CI or not. 

## Specification
Developers have to be able to easily create an environment equal to the CI development and testing environment. This helps them to reproduce and analyse the sitatuions and scenarios much more easily. This implies that there need to be a common understanding on how to setup environments and execute tests in well defined way.

### Proposed Solution
To avoid only implicit documentation by code, some specifics entry patterns for all projects are defined. All of them are executed by a make file. 
From there on, implicit documentation is garanteed:
- help                ## Show this help
- run                 ## Run Planetmint from source (stop it with ctrl+c)
- test                ## Run unit, acceptance tests, and linter tests
- test-unit           ## Run all unit tests
- test-acceptance     ## Run all acceptance tests
- docs                ## Run doc building process/tests
- cov                 ## Check code coverage and open the result in the browser
- lint                ## Run a linter and show all warnings
- clean               ## Clean the build/dist data to enalbe clean rebuilds
- dist                ## Create a distribution package
- release             ## Publish the distribution package
  
In case of a service, the following additional commands should be supported:
- start   ## Run Planetmint from source and daemonize it (stop with `make stop`)
- stop    ## Stop Planetmint daemon
- logs    ## Attach to the logs
- reset   ## Reset the daemons to enable a rebuild of them


### Deployment impact
This will simplify the deployment and integration into the CI and the verification of code on the developersite.
Another deployment impact this change will have is with CI, we will need to update the CI scripts @`planetmint/.ci` to use `docker-compose.yml` instead of `docker-compose.travis.yml`.


### Documentation impact

## Rationale


## Implementation


### Assignee(s)
Primary assignee(s): 


### Targeted Release
Planetmint==0.9.0


### Status
unstable


## Copyright Waiver

<p xmlns:dct="http://purl.org/dc/terms/">
  <a rel="license"
     href="http://creativecommons.org/publicdomain/zero/1.0/">
    <img src="http://i.creativecommons.org/p/zero/1.0/88x31.png" style="border-style: none;" alt="CC0" />
  </a>
  <br />
  To the extent possible under law, all contributors to this PRP
  have waived all copyright and related or neighboring rights to this PRP.
</p>
