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

##Table of Contents

[TOC]

##Usage

> **Note:** This guide assumes that you are using authtest.it.usf.edu for development and testing and webauth.usf.edu for production

[CAS](http://www.jasig.org/cas) is a popular single sign-on implementation. It's open source(Apache license) and is easy to get started with but is also highly configurable. In addition it has clients written in Java, .Net, PHP, Perl, and other languages.

There isn't much that you need to do in your application to be a CAS client. Just install this plugin, run the configuration script, and modify any of the default values you want in Config.groovy. These are described in detail in [<i class="icon-share"></i> configuration](#configuration) but in most cases, you won't need to change anything during development.

###Installation

Download the latest version of the plugin zip file from [GitHub](https://github.com/epierce/grails-spring-security-cas-usf/raw/master/spring-security-cas-usf-1.3.0.zip), drop it into the lib directory of your Grails project and update `grails-app/conf/BuildConfig.groovy`:

##<a name="configuration"></a>Configuration