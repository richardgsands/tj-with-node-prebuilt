# Tracy James - Website (Prebuilt)

This repo contains two NodeJS web applications all ready to run:
- main - public website
- admin - private webapp for content management (to be run on same server on different port)

Note that the admin app should be in the same parent folder as the main app, so that relative paths between the two are correct. (The admin server is not necessary if you have another way to provide access to update files in `main\client\cms`). 

*This repo has been created because building from source was not working - and contains all the necessary dependencies (i.e. from npm and bower) to work.*

[Original repo can be found here](https://github.com/richardgsands/tj-with-node) 

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

**Edit settings file `admin/server/settings.json` to set a secure username and password:**

It should have the following format:
```json
{
  "adminUser": "admin",
  "adminPass": "password"
}
```
*If these are changed after starting the NodeJS server, a restart (of the NodeJS server) will be required - this can be done with the command `forever restartall`*

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

## Making changes

### To files directly on server

See [Main App Architecture](#main-app-architecture) for guide on how to make changes to the site.

**Make sure you are changing the files in `main` and not `admin`** (unless you want to makes changes to the admin app)

Once done, navigate to the `main` folder and run:
```
grunt refresh
```

### On a local machine

To make changes on a local dev machine, fork the repository on GitHub (just need a free GitHub account for that). Then set up a server on a local machine (ideally Linux / Mac) with node and git installed. This can be done using instructions in Instructions to set up server above, but:
	- use your fork repository to for git clone command
	- no need to do port redirect
	- no need to run using forever, `grunt serve` will allow you to access server at http://localhost:9000)

Once you've committed and pushed changes to your fork, add it as as remote on the server and run `git pull` to deploy. As all the build files are committed in this version of the repo, there's no need to do anything else.

(Client only changes shouldn't require a server restart, but you can restart it with `forever restartall` if necessary.)

## Main App Architecture

There are two key folders in `main`: server and client.

Server folder contains code running on the web server, and handles sending emails (for contact section)
Client folder contains all content and code which runs in the web browser accessing the website - and this is basically where most of the website is.

In client folder:
`main/client/`

  - index.html
  - app/app.js
  - app/main/main.controller.js
  - app/main/main.jade
  - app/views/
  - app/main/main.scss

`index.html`
The client part of website is written using AngularJS - so index.html is loaded in the browser, and that contains all code needed to load everything else

`app/app.js`
Javascript file to set up AngularJS - this file contains code to handle URL links to different pages on website

`app/main/main.controller.js`
Main javascript file containing code for website - i.e. to send HTTP requests to server for the Contact section, control audio playback, load up gallery images, define URL addresses that appear in navbar and point to particular views (in app/views)

`app/main/main.jade`
Layout for main page - including sidebar, and navbar

`app/views/`
Folder containing layouts for sections of website (in navbar) - i.e. home, biography, education, ensembles, contact, gallery, music, links. Each view file in here should be wired up to a URL in app/main/main.controller.js in navSections array

`app/main/main.scss`
Stylesheet for website - containing CSS classes that are used in Layout (jade) files

** NB: If any of these files are changed, `grunt refresh` must be run on command line to compile everything (Jade and SCSS files into HTML / CSS, etc - see Making changes above**

