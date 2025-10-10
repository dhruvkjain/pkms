[[index]]



**OAuth response:**
```shell
{
  user: {
    id: 'a_number',
    name: 'name',
    email: 'email.com',
    image: undefined
  },
  account: {
    provider: 'google',
    type: 'oauth',
    providerAccountId: 'a_number',
    access_token: 'string',
    expires_at: 1758458065,
    scope: 'https://www.googleapis.com/auth/userinfo.email https://www.googleapis.com/auth/userinfo.profile openid',
    token_type: 'Bearer',
    id_token: 'very_long_string'
  },
  profile: {
    id: 'a_number',
    name: 'name',
    email: 'email.com',
    image: undefined
  },
  isNewUser: undefined
}
```

