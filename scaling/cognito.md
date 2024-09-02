
# Cognito

## Intro

- User Pools - Sign in and get a Json Web Token
    - user management / sign up / sign in/ MFA / other security features
    - only provides jwt, which can't be used to access lot of aws services
    - can also use third party single sign on (SAML 2.0) - federated identities
    - offers identity pools - provide temporary access
    - Unathenticated identities - guest users
- API gateway is capable of handling JWT
- The JWT can't be directly used to access AWS, identity pool provide temporary creds
- Cognito assumes an IAM role defined in identity pool and returns temporary aws creds

