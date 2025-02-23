---
id: ory-proxy
title: ory proxy
description: ory proxy Run your app and Ory on the same domain using a reverse proxy
---

<!--
This file is auto-generated.

To improve this file please make your change against the appropriate "./cmd/*.go" file.
-->
## ory proxy

Run your app and Ory on the same domain using a reverse proxy

### Synopsis

This command starts a reverse proxy which must be deployed in front of your application.
This proxy works both in development and in production, for example when deploying a
React, NodeJS, Java, PHP, ... app to a server / the cloud or when developing it locally
on your machine.

Before you start, you need to have a running Ory Cloud project. You can create one with the following command:

	$ ory create project --name "Command Line Project"

Pass the project's slug as a flag to the tunnel command:

	$ ory proxy --project <your-project-slug> ...
	$ ORY_PROJECT_SLUG=<your-project-slug> ory proxy ...

The first argument `app-url` points to the location of your application. If you are
running the proxy and your app on the same host, this could be localhost.

The second argument `[publish-url]` is optional. It refers to the public URL of your
application (e.g. https://www.example.org).

If `[publish-url]` is not set, it will default to the default
host and port this proxy listens on:

	http://localhost:4000

You must set the `[publish-url]` if you are not using the Ory Proxy in locally or in
development:

	$ ory proxy --project <your-project-slug> \
		http://localhost:3000 \
		https://example.org

Please note that you can not set a path in the `[publish-url]`!

Per default, the proxy listens on port 4000. If you want to listen on another port, use the
port flag:

	$ ory proxy --port 8080  --project <your-project-slug> \
		http://localhost:3000 \
		https://example.org

If your public URL is available on a non-standard HTTP/HTTPS port, you can set that port in the `[publish-url]`:

	$ ory proxy --project <your-project-slug> \
		http://localhost:3000 \
		https://example.org:1234

If this proxy runs on a subdomain, and you want Ory's cookies (e.g. the session cookie) to
be available on all of your domain, you can use the following CLI flag to customize the cookie
domain:

	$ ory proxy --project <your-project-slug> \
		--cookie-domain example.org \
		http://127.0.0.1:3000 \
		https://ory.example.org

Per default all default redirects will go to to `[publish-url]`. You can change this behavior using
the `--default-redirect-url` flag:

    $ ory --project <your-project-slug> \
		--default-redirect-url /welcome \
		http://127.0.0.1:3000 \
		https://ory.example.org

Now, all redirects happening e.g. after login will point to `/welcome` instead of `/` unless you
have specified custom redirects in your Ory configuration or in the flow's `?return_to=` query parameter.

If the request is not authenticated, the HTTP Authorization Header will be empty:

	GET / HTTP/1.1
	Host: localhost:3000

If the request was authenticated, a JSON Web Token will be sent in the HTTP Authorization Header containing the
Ory Session:

	GET / HTTP/1.1
	Host: localhost:3000
	Authorization: Bearer the-json-web-token

The JSON Web Token claims contain:

* The "sub" field which is set to the Ory Identity ID.
* The "session" field which contains the full Ory Session.

The JSON Web Token is signed using the ES256 algorithm. The public key can be found by fetching the /.ory/jwks.json path
when calling the proxy - for example: `http://127.0.0.1:4000/.ory/jwks.json`

An example payload of the JSON Web Token is:

	{
	  "id": "821f5a53-a0b3-41fa-9c62-764560fa4406",
	  "active": true,
	  "expires_at": "2021-02-25T09:25:37.929792Z",
	  "authenticated_at": "2021-02-24T09:25:37.931774Z",
	  "issued_at": "2021-02-24T09:25:37.929813Z",
	  "identity": {
		"id": "18aafd3e-b00c-4b19-81c8-351e38705126",
		"schema_id": "default",
		"schema_url": "https://example.projects.oryapis.com/api/kratos/public/schemas/default",
		"traits": {
		  "email": "foo@bar",
		  // ... your other identity traits
		}
	  }
	}


```
ory proxy app-url [publish-url] [flags]
```

### Options

```
      --cookie-domain string          Set a dedicated cookie domain.
      --default-redirect-url string   Set the URL to redirect to per default after e.g. login or account creation.
  -h, --help                          help for proxy
      --no-jwt                        Do not create a JWT from the Ory Kratos Session. Useful if you need fast start up times of the Ory Proxy.
      --open                          Open the browser when the proxy starts.
      --port int                      The port the proxy should listen on. (default 4000)
      --project string                The slug of your Ory Cloud Project.
```

### SEE ALSO

* [ory](ory)	 - The ORY CLI

