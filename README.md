# auth.sinpapeles

Trustless authentication using domains

## Custom Browser Protocol

This app registers itself to resolve `web+auth:` requests.

### URL

`web+auth:challenge@www.example.com/callback`

| Property  | Type      | Description                                                                                             |
| --------- | --------- | ------------------------------------------------------------------------------------------------------- |
| web+auth: | Protocol  | Should not have `//` in the end                                                                         |
| challenge | String 30 | The challenge to be solved                                                                              |
| callback  | URL       | The URL that will be called with the challenge proof. If no protocol specified, `https://` will be used |

Example: `web+auth:0be771d9f01456abebc48b6f50909bb931b08f5a@www.twiter.com/login/callback?session=c9096806a0`

Note: in this example, `session` is part of the callback URL param.

### Callback

Once the user providers a valid domain (that exposes a public auth key) and the private key, the challenge will be solved and send back to the application that requests the auth.

| Query string | Description                                                                           |
| ------------ | ------------------------------------------------------------------------------------- |
| challenge    | The same challenge sent in the original request.                                      |
| key          | The public key used to sign the challenge.                                            |
| signature    | The signed challenge.                                                                 |
| domain       | The domain used to login. Important: this must to be validated on the requestor side. |

- The authentication process will check what's the public auth key for a give domain.
- The user provides the private key for that.
- The proof is generated by signing the challenge.
- After the callback, the requestor must validate if the domain really exposes the public auth key. Also verify the challenge was correct solved.

Remember: public keys are disposable. You must to identify the user by the domain.
