# How to create an OAuth 2.0 Provider

This is an example of OAuth 2.0 server in [Authlib](https://authlib.org/).
If you are looking for old Flask-OAuthlib implementation, check the
`flask-oauthlib` branch.

- Documentation: <https://docs.authlib.org/en/latest/flask/2/>
- Authlib Repo: <https://github.com/lepture/authlib>

## Take a quick look

```bash
$ docker-compose build
$ docker-compose up
```

Now, you can open your browser with `http://127.0.0.1:5000/`, login with any
name you want.

Before testing, we need to create a client:

![create a client](https://user-images.githubusercontent.com/290496/38811988-081814d4-41c6-11e8-88e1-cb6c25a6f82e.png)

### Password flow example

Get your `client_id` and `client_secret` for testing. In this example, we
have enabled `password` grant types, let's try:

```
$ curl -u ${client_id}:${client_secret} -XPOST http://127.0.0.1:5000/oauth/token -F grant_type=password -F username=${username} -F password=valid -F scope=profile
```

Because this is an example, every user's password is `valid`. Now you can access `/api/me`:

```bash
$ curl -H "Authorization: Bearer ${access_token}" http://127.0.0.1:5000/api/me
```

### Authorization code flow example

To test the authorization code flow, you can just open this URL in your browser.
```bash
$ http://127.0.0.1:5000/oauth/authorize?response_type=code&client_id=${client_id}&scope=profile
```

After granting the authorization, you should be redirected to `${redirect_uri}/?code=${code}`

Then your app can send the code to the authorization server to get an access token:

```bash
$ curl -u ${client_id}:${client_secret} -XPOST http://127.0.0.1:5000/oauth/token -F grant_type=authorization_code -F scope=profile -F code=${code}
```

Now you can access `/api/me`:

```bash
$ curl -H "Authorization: Bearer ${access_token}" http://127.0.0.1:5000/api/me
```

## Other Routes

But that is not enough. In this demo, you will need to have some web pages to
create and manage your OAuth clients. Check that `/create_client` route.

And we have an API route for testing. Check the code of `/api/me`.


## Finish

Here you go. You've got an OAuth 2.0 server.

Read more information on <https://docs.authlib.org/>.

## License

Same license with [Authlib](https://authlib.org/plans).
