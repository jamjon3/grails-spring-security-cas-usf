grails-spring-security-cas-usf
==============================

Grails CAS plugin customized for the University of South Florida

##Introduction

The Spring-Security-USF plugin builds on the [spring-security-cas](http://grails.org/plugin/spring-security-cas) plugin. It depends on the [Spring Security Core plugin](http://grails.org/plugin/spring-security-core).

Additional features beyond the standard CAS plugin include

  * Attribute release through [SAML1.1](https://wiki.jasig.org/display/CASUM/SAML+1.1)
  * USF-specific configuration script
  * Spring Security service - UsfCasService
  * <cas:> tag library

Once you have configured a CAS server and have configured your Grails application(s) as clients, you can authenticate to any application that is a client of the CAS server and be automatically authenticated to all other clients.

##Usage

> **Note:** This guide assumes that you are using authtest.it.usf.edu for development and testing and webauth.usf.edu for production

[CAS](http://www.jasig.org/cas) is a popular single sign-on implementation. It's open source(Apache license) and is easy to get started with but is also highly configurable. In addition it has clients written in Java, .Net, PHP, Perl, and other languages.

There isn't much that you need to do in your application to be a CAS client. Just install this plugin, run the configuration script, and modify any of the default values you want in Config.groovy. These are described in detail in [<i class="icon-share"></i> configuration](#configuration) but in most cases, you won't need to change anything during development.

###Installation

Download the latest version of the plugin zip file from [GitHub](https://github.com/epierce/grails-spring-security-cas-usf/raw/master/spring-security-cas-usf-1.3.0.zip), drop it into the lib directory of your Grails project and update `grails-app/conf/BuildConfig.groovy`:

```
  plugins {
        compile ">>
        compile ">>
  }
```

###grails usf-cas-config

After you have installed the plugin, run this command to add the necessary configuration options:

```
grails usf-cas-config
```

The following lines will be added to your Config.groovy:

```
grails.plugins.springsecurity.userLookup.userDomainClassName = 'edu.usf.cims.UsfCasUser'
grails.plugins.springsecurity.cas.active = true
grails.plugins.springsecurity.cas.sendRenew = false
grails.plugins.springsecurity.cas.key = 'a5e3051a58a742948f80a6ff83d51ac' //unique value for each app
grails.plugins.springsecurity.cas.artifactParameter = 'ticket'
grails.plugins.springsecurity.cas.serviceParameter = 'service'
grails.plugins.springsecurity.cas.filterProcessesUrl = '/j_spring_cas_security_check'
grails.plugins.springsecurity.cas.proxyCallbackUrl = 'http://localhost:8080/${appName}/secure/receptor' 
grails.plugins.springsecurity.cas.proxyReceptorUrl = '/secure/receptor'
grails.plugins.springsecurity.cas.useSingleSignout = false
grails.plugins.springsecurity.cas.driftTolerance = 120000
grails.plugins.springsecurity.cas.loginUri = '/login'
grails.plugins.springsecurity.cas.useSamlValidator = true
grails.plugins.springsecurity.cas.authorityAttribute = 'eduPersonEntitlement'
grails.plugins.springsecurity.cas.serverUrlPrefix = 'https://authtest.it.usf.edu'
grails.plugins.springsecurity.cas.serviceUrl = 'http://localhost:8080/${appName}/j_spring_cas_security_check'
```

Moving to a Test or Production server
Once you have tested your app on localhost and are ready to run `grails war`, open a support ticket in [ServiceNow](http://usffl.service-now.com/) and include the following information:

  * Short Description: SSO Project: Application Name
  * Your Name
  * Your Email address
  * Office phone number
  * Application Name
  * Short description of the service
  * Value used for `grails.plugins.springsecurity.cas.serviceUrl`
  * List of attributes that need to be released

##<a name="configuration"></a>Configuration