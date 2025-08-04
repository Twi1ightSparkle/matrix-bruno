# Matrix Collection for Bruno

## Setup

1. Clone the GitHub repo

    ```bash
    git clone https://github.com/Twi1ightSparkle/matrix-bruno.git
    ```

1. Install [Bruno](https://www.usebruno.com/)
1. Copy `.env.sample` to `Matrix/.env`
1. Copy `environments.sample` to `Matrix/environments`. Alternatively, if you
    wish to have your environments under source control, symlink your collection
    to `Matrix/environments`
1. For every environment you want to connect to, make a copy of
    `Matrix/environments/_Template.bru` and add your secrets environment
    variables in `Matrix/.env`
1. In Bruno, click `Open collection`
1. Choose the `Matrix` directory in the cloned repo

## MAS Admin Authentication

The collection includes two different methods of authentication to use the
Synapse and MAS Admin APIs for MAS enabled homeservers.

See `What does the different MAS access token prefixes mean?` in the
[Element Documentation](https://docs.element.io/latest/element-support/frequently-asked-questions/#element-general)
for an explanation of the different token prefixes.

If you do not wish to authenticate manually, I have created a simple web-ui to
authenticate against MAS for a MAS+Synapse Admin access token:
<https://twi1ightsparkle.github.io/mas-admin-auth/>.

### Option 1, Non-Interactive Authentication

<https://element-hq.github.io/matrix-authentication-service/topics/admin-api.html#automated-tools>

Create an Admin Client in your MAS configuration, and set the Client ID and
Client Secret in your environment config

`example.com.bru`

```plaintext
  masClientId: {{process.env.matrix-local-masClientId}}
  masClientSecret: {{process.env.matrix-local-masClientSecret}}
```

`.env`

```plaintext
example-com-masClientId=0000000000000000000SYNAPSE
example-com-masClientSecret=secret
```

Authenticate with the `Matrix/MAS/Admin-Client Authenticate.bru` request. The
access token returned from this can only be used with the MAS Admin API. The
authentication request automatically stores the access token in a Bruno runtime
variable for use with subsequent requests, meaning the token is deleted when you
close Bruno. To persist the token, add it to the `masAdminToken` environment
variable. The token is only valid for a short time as configured in your MAS
config.

To use the Synapse Admin API, you need to create an admin compatibility token
using `mas-cli`. This token is valid indefinite, but cannot be used with the MAS
Admin API.

```bash
mas-cli manage issue-compatibility-token \
    --yes-i-want-to-grant-synapse-admin-privileges username_localpart
```

Add the returned access token to the `adminToken` environment variable.

### Option 2, User-Interactive OAuth Authentication

<https://element-hq.github.io/matrix-authentication-service/topics/admin-api.html#user-interactive-tools>

This does not require any configuration changes to your Synapse or MAS, and do
not require access to `mas-cli`. However, as this requires user-interactive
authentication, it may not suitable for fully automated systems. The
authentication request returns two tokens, one sort-lived access token which can
be used with both the MAS Admin API and the Synapse Admin API, and one refresh
token which you can use to renew the access token before it expires.

To authenticate using OAuth, run the first three requests in the directory
`Matrix/MAS/Admin OAuth Authenticate` in order. Number 2 should open your MAS UI
in your browser for you to sign in. (If not, copy the link injected in the
request response body by Bruno). After authenticating you're redirected to
<https://twi1ightsparkle.github.io/matrix-bruno/mas-auth-code.html>. Copy your
code and paste it in the `code` entry on the Body tab of the 3rd request. This
will then give you your access and refresh tokens.

The access token can be renewed with the 4th request before it expires.

The access token and refresh token in persisted in Bruno runtime variables, to
persist when closing Bruno, add the access token to your environment variables
`adminToken` and `masAdminToken`.

### Personal Access Token

This is not yet implemented in MAS
(<https://github.com/element-hq/matrix-authentication-service/issues/4492>).

## Tests/Visualizations

Bruno's support for
[visualizations](https://learning.postman.com/docs/sending-requests/response-data/visualizer/)
is currently only available in Gold Edition
([source](https://github.com/usebruno/bruno/issues/2633#issuecomment-2408801159)),
the visualization code from Postman is commented out for now.
