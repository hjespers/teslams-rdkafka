#Tesla Telemetry gateway for Apache Kafka


An implementation in Node.js of the streaming telemetry interface of Tesla Motors with output to Apache Kafka.  

#Disclaimer

Use these programs at your own risk. The authors do not guaranteed the proper functioning of these applications. This code attempts to use the same interfaces used by the official Tesla phone apps. However, it is possible that use of this code may cause unexpected damage for which nobody but you are responsible. 

#Contributors
Hans Jespersen (https://github.com/hjespers)

#Installation

To use these programs you must download and install 'node' from http://nodejs.org
. Once node is installed, use the included 'npm' utility to download and install the teslams tools and all it's dependent modules

	npm install -g teslams-rdkafka
	
or if you are not logged in as the root (administrator) use:
	
	sudo npm install -g teslams-rdkafka

Alternatively, to run from github sources, clone teslams, go to the main folder and run

	npm install

All example programs normally require -u and -p in order to specify the Tesla Motors user name and password to access your Model S.
Or, you can instead create a json file in ~/.teslams/config.json and specify them once, in the following format:

	{
		"username": "Your teslamotors.com username/email",
		"password": "Your teslamotors.com password"
	}
	
In addition to putting your password as a command line option or a file, you can alternatively use the $TSLA_USERNAME and $TSLA_PASSWORD environment variables. These environment variable allow the execution of these apps in Heroku or other Platform-as-a-Service providers.

#rdkstreaming.js - Capture and log real-time telemetry to Apache Kafka for analytics 

<img src=http://farm9.staticflickr.com/8241/8526534730_75643b3247_c.jpg>

An application which uses the TESLA HTTP Long Polling "STREAMING" API to get continuous telemetry from the Tesla Model S. 
A valid teslamotors.com login and password is required and must be provided on the command line options. 

By default the output is a stream of AVRO formatted messages published to a Kafka topic called "teslams". The output topic can be changed by adding the --topic command line option.

To execute run:

	rdkstreaming -u <username> -p <password> 

For help run :

	rdkstreaming --help

	Usage: kstreaming -u <username> -p <password> [--topic <topic_name>] [--silent]

	Options:
		  -u, --username  Teslamotors.com login                  [required]
		  -p, --password  Teslamotors.com password               [required]
		  -s, --silent    Silent mode: no output to console      [boolean]
		  -t, --topic     Kafka publish topic
		  -?, --help      Print usage information                                            

#Feedback and Support

For more information, feedback, or community support see the Tesla Motors Club forum at http://www.teslamotorsclub.com/showthread.php/13410-Model-S-REST-API or email teslams@googlegroups.com

