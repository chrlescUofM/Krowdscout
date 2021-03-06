# Krowdscout
Every wondered how busy a place was before you get there? Maybe your favorite restaurant, bar, or even the park?  Krowdscout is a platform that provides real time information about how busy your favorite places are, at the touch of a button.

This is a on-going project of mine that I have been working on in my free time for the last several months, as such it is rather ad-hoc with what interests me at the time and may seem haphazard to the on-looker.  Further, the technologies used are constantly in flux as I expand and, hopefully, improve my knowledge.

Current technologies in use:
* Spark Core - The current hardware.  The spark is an Arduino like embedded system that has built in WIFI capabilities and cloud support.
	* Infrared Sensor -  Used to detect motion.
	* Ultrasonic Sensor - Used to detect motion.
	* dB Sensor - Used to detection noise level, which may be correlated to how busy a place is.
* Heroku Server - Currently the web app is hosted on Heroku as they have an excellent free tier with basic database support
* Flask Microframework - Handles the backend including the creation of an accessible API for future app development.


##Repository Structure
Note: Some files omitted for brevity.
* Krowdscout/ - The top level directory.
  * [config.py](/config.py) - Contains the flask configuration variables.
  * [Procfile](/Procfile) - Used to declare what commands are run by the dynos on the Heroku platform.
  * [README.md](/README.md) - This file.
  * [requirements.txt](/requirements.txt) - Used to tell Heroku what packages need to be installed on a dyno.
  * [web_hook.json](/web_hook.json) - Used to create a webhook via the Particle CLI.
  * [app/](/app/) - Contains the Flask web-app.
    * [forms.py](/app/forms.py) - Contains the form models for submissions in the flask framework.
    * [models.py](/app/models.py) - Contains the database object models.
    * [views.py](/app/views.py) - Contains the valid web-app routes and behaviors.
    * [static/](/app/static/) - Directory that contains static content such as images and css.
    * [templates/](/app/templates/) - Directory that contains the web-page content templates.
      * [add_location.html](/app/templates/add_location.html) - A management page to add a new device.
      * [base.html](/app/templates/base.html) - The base page, from which all others inherit.
      * [index.html](/app/templates/index.html) - The home page where devices are displayed.
  * [spark/](/spark/) - Contains the firmware to load onto the Spark core.
    * [occ_count.ino](/spark/occ_count.ino) - The spark core firmware.

##Running the web-app locally
NOTE:  The heroku version of the web app uses a webhook, this hook will not
communicate with your localhost.  The new spark code added with this commit
no longer works with localhost either.

1.  Setting up the virtual environment:

	If you are using Mac OS X, type the following:
	```
	$ sudo easy_install virtualenv
	```
	If you are on linux, such as Ubuntu, type the following:
	```
	$ sudo apt-get install python-virtualenv
	```
	Now we need to construct our virtualenv.  Navigate to the top level 
	directory of the repository and type the following:
	```
	$ virtualenv venv
	```
	To activate our new virtual environment type:
	```
	$ source venv/bin/activate
	```
	Now we need to install our application dependencies in our virtual
	environment, this can be accomplished with the following command:
	```
	$ pip install Flask gunicorn flask-login flask-sqlalchemy sqlalchemy-migrate
	```
2.  Modifying configuration variables:
	
	It is currently assumed that all Spark cores will have the same access token
	and so the ACCESS_TOKEN is stored in the file config.py.  You can find your
	access token by going to the Spark online IDE and clicking on the Settings
	wheel in the bottom left corner.  Update this variable in your config file.

3.  Creating a local database to use:

	You must also create a local database file to use.  Do this by typing the
	following from the top level directory:
	```
	$ ./db_create.py
	```
4.  Running the web-app:
	
	In order to run the web-app we type:
	```
	$ foreman start
	```
	Currently, we most also run the worker script to update spark variables.
	Make sure that you execute this command from within your virtual env.
	```
	$ python worker.py
	```
	If you open your browser and navigate to "localhost:5000" you should now see
	the webpage.

5.  Adding your location/core:
	
	At this point you can add your core to the database by going to the
	"Add Location" link in the top bar of the webpage.  You can find the id of 
	the core that you want to add by going to the Spark online IDE and clicking
	on the device target in the left panel and then selecting your core in the
	pop-up.
