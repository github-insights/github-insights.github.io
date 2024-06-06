# Authentication

Authorization is done via a [Github App](https://docs.github.com/en/apps/overview)
which can be installed on a organization. We have created our own Github App
specifically for this purpuse and [you will have to do so too](https://docs.github.com/en/apps/creating-github-apps/registering-a-github-app/registering-a-github-app#registering-a-github-app).
To be able to perform all the nessesary actions the Github App needs to have
the following priviliges.

## Setting up the Github App

### Permissions 

<table width="100%">
    <thead width="100%">
        <tr style="background-color: lightgray;">
            <th>permissions level</th>
            <th>read only permissions to be set</th>
        </tr>
    </thead>
    <tbody width="100%">
        <tr>
            <td rowspan=4>Repository permissions</td>
            <td>Actions</td>
        </tr>
        <tr>
            <td>Administration</td>
        </tr>
        <tr>
            <td>Metadata</td>
        </tr>
        <tr>
            <td>Pull requests</td>
        </tr>
        <tr>
            <td rowspan=3>Organization permissions</td>
            <td>Administrartion</td>
        </tr>
        <tr>
            <td>Projects</td>
        </tr>
        <tr>
            <td>Self-hosted runners</td>
        </tr>
        <tr>
            <td>Account permissions</td>
            <td>none</td>
        </tr>
    </tbody>
</table>

### Access to Repositories

This app then authorizes access to repositories on the organization. The exact
repositories that it will have access to can be defined on installation as well as
changed anytime under `<your-org>` > `Settings` > `Github Apps` > `<configure your-app>`

### Private Key (`PEM`)

To actually authenticate the Service you will need to create a private key from
the settings page of the App you can find the settings page here: `https://github.com/organizations/<your-org>/settings/apps/<your-app-name>`.


Github supplies an RSA private key which needs to be used to sign a JWT with the
details

```json
{
  "iss": "<install_id>",
  "exp": 1714639080,
  "iat": 1714638420
}
```

iss -> the installation id of the application

exp -> The expiration time of the JWT, after which it can't be used to request
an installation token. The time must be no more than 10 minutes into the future.

iat -> The time that the JWT was created. To protect against clock drift, we
recommend that you set this 60 seconds in the past and ensure that your server's
date and time is set accurately (for example, by using the Network Time Protocol). 

this request then returns an app installation token which we can use to
authenticate all future requests.

Click [here](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/generating-a-json-web-token-jwt-for-a-github-app)
for full documentation of this process

When adding the GH App to your organization you can select which repositories it has access to or simply all repositories in the organization. The service does a request to see which repositories it has access to and then only the metrics relative to those repositories are exported.

![image](https://github.com/github-insights/github-metrics/assets/89661092/79e56315-b95b-41d3-a3c6-2a4ca410a224)


## Rate Limiting
