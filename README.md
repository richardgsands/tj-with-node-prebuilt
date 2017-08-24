# Tracy James - Website (Prebuilt)

This repo contains two NodeJS web applications all ready to run:
- main - public website
- admin - private webapp for content management (to be run on same server on different port)

Note that the admin app should be in the same parent folder as the main app, so that relative paths between the two are correct. (The admin server is not necessary if you have another way to provide access to update files in main\client\cms). 

*This repo has been created because building from source was not working - and contains all the necessary dependencies (i.e. from npm and bower) to work.*

[Original repo can be found here](https://github.com/richardgsands/tj-with-node-prebuilt) 

## Installation and running

### Configure new server

Start with a fresh Linux box with NodeJS and git installed.

Install Node Forever

```
npm install -g forever
```

### Install repo files

Clone repository and navigate to newly created folder:
```
git clone https://github.com/richardgsands/tj-with-node-prebuilt.git
cd tj-with-node-prebuilt
```

### Define settings

**Create settings file (admin app won't work without this):**
```
touch admin/server/settings.json
```

Edit the settings file to set username and password. It should have the following format:
```json
{
  "adminUser": "admin",
  "adminPass": "password"
}
```
*If these are changed after starting the NodeJS server, a restart (of the NodeJS server) will be required*

### Start web server!

Start main and admin web servers using forever:
```
cd main
forever start server/app.js

cd ..
cd admin
forever start server/app.js
```

To stop website, run: `forever stopall`

To see forever process (and path to log file), run: `forever list`

Website should now be accessible on port 9000
Admin app shold be accessible on port 9001

### Redirect Port 80 to Port 9000

```
sudo /sbin/iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 9000
```
Now website will be available on standard HTTP port (80)

*Port 9001 will need to be open (or redirected to another port if preferred) as well for the admin app.*