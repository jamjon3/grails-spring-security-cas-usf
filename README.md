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

There are a few configuration options for the CAS plugin.

> All of these property overrides must be specified in `grails-app/conf/Config.groovy` using the `grails.plugins.springsecurity` prefix, for example:
> 
> ```
grails.plugins.springsecurity.cas.serverUrlPrefix =
     'https://authtest.it.usf.edu'
``` 
> 

| Name	                        | Default	                 | Meaning                                                                                                                                                                                               |
| ----------------------------- | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|<sub>userLookup.userDomainClassName</sub> | <sub>`edu.usf.cims.UsfCasUser`</sub>	 | <sub>SpringSecurity User Class </sub>                                                                                                                                                                            |
|<sub>cas.active</sub>	                | <sub>`true`</sub>                         | <sub>whether the plugin is enabled or not (e.g. to disable per-environment)     </sub>                                                                                                                           |
|<sub>cas.serverUrlPrefix</sub>	        | <sub>`https://authtest.it.usf.edu`</sub>  | <sub>the 'root' of all CAS server URLs    </sub>                                                                                                                                                                 |
|<sub>cas.loginUri</sub>	                | <sub>`/login`</sub>	                 | <sub>the login URI, relative to `cas.serverUrlPrefix`, e.g. `/login` </sub>                                                                                                                                      |
|<sub>cas.sendRenew</sub>	                | <sub>`false`</sub>	                 | <sub>if true, ticket validation will only succeed if it was issued from a login form, but will fail if it was issued from a single sign-on session. Analagous to `IS_AUTHENTICATED_FULLY` in Spring Security</sub> |
|<sub>cas.serviceUrl</sub>	                | <sub>`http://localhost:8080/${appName}/j_spring_cas_security_check`</sub> |	<sub>the local application login URL</sub> |
|<sub>cas.key</sub>	| <sub>random value</sub>	| <sub>used by `CasAuthenticationProvider` to identify tokens it previously authenticated. Generated automatically by `grails usf-cas-config`</sub> |
|<sub>cas.artifactParameter</sub>	| <sub>`ticket`</sub>	| <sub>the ticket login url parameter</sub> |
|<sub>cas.serviceParameter</sub>	| <sub>`service`</sub>	| <sub>the service login url parameter</sub> |
|<sub>cas.filterProcessesUrl</sub>	| <sub>`/j_spring_cas_security_check`</sub>	| <sub>the URL that the filter intercepts for login</sub> |
|<sub>cas.proxyCallbackUrl</sub>	| <sub>`http://localhost:8080/${appName}/secure/receptor`</sub>	| <sub>proxy callback url</sub> |
|<sub>cas.proxyReceptorUrl</sub>	| <sub>`/secure/receptor`</sub>	| <sub>proxy receptor url</sub> |
|<sub>cas.useSingleSignout</sub>	| <sub>`false`</sub>	| <sub>if `true` a `org.jasig.cas.client.session.SingleSignOutFilter` is registered in web.xml</sub> |
|<sub>cas.useSamlValidator</sub>	| <sub>`true`</sub>	| <sub>Use SAML 1.1 for attribute release</sub> |
|<sub>cas.driftTolerance</sub>	| <sub>`12000`</sub>	| <sub>SAML tokens are very time sensitive. Handle 'time drift' between client and server. (ms)</sub> |
|<sub>cas.authorityAttribute</sub>	| <sub>`eduPersonEntitlement`</sub> 	| <sub>Read attribute for SpringSecurity Roles</sub> |

grails.plugins.springsecurity.userLookup.userDomainClassName = 'edu.usf.cims.UsfCasUser'

## Helper Classes

Use the plugin helper classes in your application to avoid dealing with some lower-level details of Spring Security.

###UsfCasService

**getUsername()**

Retrieves the username passed during authentication (NetID)

Example:

```
class SomeController {
   def usfCasService
   def someAction = {
      def user = usfCasService.username
      …
   }
}
```

**getEppa()**

Retrieves the EduPersonPrimaryAffiliation for the currently logged in user

Example:

```
class SomeController {
   def usfCasService
   def someAction = {
      def primaryAffil = usfCasService.eppa
      …
   }
}
```

**getAttributes()**

Retrieves a map of the attributes passed by CAS for the currently logged in user

Example:

```
class SomeController {
   def usfCasService
   def someAction = {
      def attrMap = usfCasService.attributes

      def commonName = attributes.CommonName
      …
   }
}
```

> `edu.usf.cims.UsfCasService` inherits from `grails.plugins.springsecurity.SpringSecurityService`, so the rest of this document was copied from spring-security-core's docs

`edu.usf.cims.UsfCasService` provides security utility functions. It is a regular Grails service, so you use dependency injection to inject it into a controller, service, taglib, and so on:

```
def usfCasService
```

**getCurrentUser()**

Retrieves a domain class instance for the currently authenticated user. During authentication a user/person domain class instance is loaded to get the user's password, roles, etc. and the id of the instance is saved. This method uses the id and the domain class to re-load the instance.

Example:

```
class SomeController {
   def usfCasService

   def someAction = {
      def user = usfCasService.currentUser
      …
   }
}
```

**isLoggedIn()**

Checks whether there is a currently logged-in user.

Example:

```
class SomeController {
   def usfCasService

   def someAction = {
      if (usfCasService.isLoggedIn()) {
         …
      }
      else {
         …
      }
   }
}
```

**getAuthentication()**

Retrieves the current user's [Authentication](http://static.springsource.org/spring-security/site/docs/3.0.x/apidocs/org/springframework/security/core/Authentication.html). If authenticated in, this will typically be a [UsernamePasswordAuthenticationToken](http://static.springsource.org/spring-security/site/docs/3.0.x/apidocs/org/springframework/security/authentication/UsernamePasswordAuthenticationToken.html).

If not authenticated and the [AnonymousAuthenticationFilter](http://static.springsource.org/spring-security/site/docs/3.0.x/apidocs/org/springframework/security/web/authentication/AnonymousAuthenticationFilter.html) is active (true by default) then the anonymous user's authentication will be returned ([AnonymousAuthenticationToken](http://static.springsource.org/spring-security/site/docs/3.0.x/apidocs/org/springframework/security/authentication/AnonymousAuthenticationToken.html) with username 'anonymousUser' unless overridden).

Example:

```
class SomeController {
   def usfCasService

   def someAction = {
      def auth = usfCasService.authentication
      String username = auth.username
      def authorities = auth.authorities // a Collection of GrantedAuthority
      boolean authenticated = auth.authenticated
      …
   }
}
```
