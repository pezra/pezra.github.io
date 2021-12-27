---
id: 536
title: Saml SP announce
date: 2010-11-08T05:30:15-07:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=536
permalink: /blog/2010/11/saml-sp-announce/
aktt_notify_twitter:
  - 'yes'
aktt_tweeted:
  - "1"
categories:
  - Software Development
---
Saml-sp provides support for being a SAML 2.0 service provider in an HTTP artifact binding SSO conversation.

## Synopsis {#synopsis}

This library provides parsing of SAML 2.0 artifacts. For example.

    artifact = Saml2::Type4Artifact.new_from_string(params[&#39;SAMLart&#39;])
    # => #<Saml2::Type4Artifact ...>
    artifact.source_id    # => &#39;a314Xc8KaSd4fEJAd8R&#39;
    artifact.type_code    # => 4

Once you have an artifact you can resolve it into it&#8217;s associated assertion:

    assertion = artifact.resolve     # => #<Saml2::Assertion>

With the assertion you can identify the user and retrieve attributes:

    assertion.subject_name_id        # => &#39;1234&#39;
    assertion[&#39;mail&#39;]                # => &#39;john.doe@idp.example&#39;

### Configuration {#configuration}

If you are using Rails the SamlSp will automatically load configuration info from `config/saml_sp.conf`.

For non-Rails apps the saml-sp configuration file can be place in the application configuration directory and loaded using the following code during application startup.

    SamlSp::Config.load_file(APP_ROOT + "/config/saml_sp.conf")

#### Logging {#logging}

If you are using saml-sp in a rails app it will automatically log to the Rails default logger. For non-Rails apps you can specify a Logger object to be used in the config file.

    logger MY_APP_LOGGER

#### Artifact Resolution Service {#artifact_resolution_service}

For artifact resolution to take place you need to configure an artifact resolution service for the artifacts source. This is done by adding block similar to the following to your saml-sp config file.

    artifact_resolution_service {
      source_id         &#39;opaque-id-of-the-idp&#39;
      uri               &#39;https://samlar.idp.example/resolve-artifact&#39;
      identity_provider &#39;http://idp.example/&#39;
      service_provider  &#39;http://your-domain.example/&#39;
      http_basic_auth {
        realm    &#39;the-idp-realm&#39;
        user_id  &#39;my-user-id&#39;
        password &#39;my-password&#39;
      }
    }

The configuration details are:

  * source_id: The id of the source that this resolution service can resolve. This is a 20 octet binary string.

  * uri: The endpoint to which artifact resolve requests should be sent.

  * identity_provider: The URI identifying the identity provider that issues assertions using the source id specified.

  * service_provider: The URI identifying the your software (the service provider) to the identity provider.

  * http\_basic\_auth: (Optional) The credentials needed to authenticate with the IdP using HTTP basic authentication.

#### Promiscuous Auth {#promiscuous_auth}

If the IdP does not provide proper HTTP challenge responses you can specify the HTTP auth in promiscuous mode. For example,

    http_basic_auth {
      promiscuous
      user_id  &#39;my-user-id&#39;
      password &#39;my-password&#39;
    }

In promiscuous mode the credentials are sent with every request to this resolutions service regardless of it&#8217;s realm.

## Requirements {#requirements}

  * Nokogiri
  * Resourcful
  * uuidtools

## Install {#install}

  * sudo gem install saml-sp