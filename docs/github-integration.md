# Authorization - Henry
Authorization is done via a Github App which authorizes access to repositories on the organization that it is installed on.

Github supplies an RSA private key which needs to be used to sign a JWT with the details

``{
  "iss": "<install_id>",
  "exp": 1714639080,
  "iat": 1714638420
}``

iss -> the installation id of the application

exp -> The expiration time of the JWT, after which it can't be used to request an installation token. The time must be no more than 10 minutes into the future.

iat -> The time that the JWT was created. To protect against clock drift, we recommend that you set this 60 seconds in the past and ensure that your server's date and time is set accurately (for example, by using the Network Time Protocol). 

this request then returns an app installation token which we can use to authenticate all future requests.

Click [here](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/generating-a-json-web-token-jwt-for-a-github-app) for full documentation of this process

When adding the GH App to your organization you can select which repositories it has access to or simply all repositories in the organization. The service does a request to see which repositories it has access to and then only the metrics relative to those repositories are exported.

![image](https://github.com/github-insights/github-metrics/assets/89661092/79e56315-b95b-41d3-a3c6-2a4ca410a224)


# Rate Limiting - Thomas
