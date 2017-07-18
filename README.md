[![NPM version](https://img.shields.io/npm/v/ntlm-ad-client.svg?style=flat)](https://www.npmjs.com/package/ntlm-ad-client)

# ntlm-ad-client

A simple NTLM Client to negotiate and authenticate against an Active Directory.

*`ntlm-ad-client` is heavily inspired by [PyAuthenNTLM2](https://github.com/Legrandin/PyAuthenNTLM2/). Also a thanks to [Alessandro Mancini](https://github.com/mancioshell) for separating and refactoring the main part of this module from its originating repository [`express-ntlm`](https://github.com/einfallstoll/express-ntlm)*

## install

    $ npm install ntlm-ad-client

## example usage

	const ntlm_client = require('ntlm-ad-client')

	const MESSAGE_NTLM_1 = 'MESSAGE_NTLM_1';
	const MESSAGE_NTLM_3 = 'MESSAGE_NTLM_3'
	const HOSTNAME = 'YOUR_HOSTNAME'
	const PORT = 'YOUR_PORT'
	const DOMAIN = 'YOUR_DOMAIN'

	const client = ntlm_client({
	    hostname: HOSTNAME,
	    port: PORT,
	    domain: DOMAIN,
	    path: null,
	    use_tls: false,
	    tls_options: undefined
	})

	client.negotiate(MESSAGE_NTLM_1, (err, challenge) => {
	    if (err) throw new Error(err);
	    console.log(challenge);

	    client.authenticate(MESSAGE_NTLM_3, (err, result) => {
		if (err) throw new Error(err);
		console.log(result); // // {"DomainName":"MYDOMAIN","UserName":"MYUSER","Workstation":"MYWORKSTATION"}
	    })
	})

## options

| Name | type | description |
|------|------|-------------|
| `hostname` | `string` | Hostname of the Active Directory. |
| `port` | `string` | Port of the Active Directory. |
| `domain` | `string` | Default domain if the DomainName-field cannot be parsed. |
| `path` | `string` | Base DN. *not implemented yet* |
| `use_tls` | `boolean` | Indicates wether to use TLS or not. |
| `tls_options` | `object` | An options object that will be passed to [tls.connect](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback) and [tls.createSecureContext](https://nodejs.org/api/tls.html#tls_tls_createsecurecontext_options). __Only required when using ldaps and the server's certificate is signed by a certificate authority not in Node's default list of CAs.__ (or use [NODE_EXTRA_CA_CERTS](https://nodejs.org/api/cli.html#cli_node_extra_ca_certs_file) environment variable)|
| `tls_options.ca` | `string` /  `array` / `Buffer` | Override the trusted CA certificates provided by Node. Refer to [tls.createSecureContext](https://nodejs.org/api/tls.html#tls_tls_createsecurecontext_options) |

