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

An application which uses the TESLA HTTP Long Polling "STREAMING" API to get continuous telemetry from a Tesla Model S or Model X. 
A valid teslamotors.com login and password is required and must be provided on the command line options. 

By default the output is a stream of JSON formatted messages published to a set of Kafka topics called "teslams", "teslams.climate_state", and "teslams.charge_state" with the keys being the `id` if the vehicle. The output topic prefix can be changed by using the --topic command line option.

To execute run:

```
rdkstreaming -u <username> -p <password> -U <kafka_username> -P <kafka_password> --kafka localhost:9092 --topic my_kafka_topic 

Usage: rdkstreaming -u <username> -p <password> -U <kafka_username> -P <kafka_password> [-sz]
        [--kafka localhost:9092] [--topic my_kafka_topic] [--ca_cert_loc /usr/local/etc/openssl/cert.pem]
        [--values <value list>] [--maxrpm <#num>] [--vehicle offset] [--naptime <#num_mins>]

Options:
  -u, --username       Teslamotors.com login
  -p, --password       Teslamotors.com password
  -U, --sasl_username  Confluent Cloud login                                                        [required]
  -P, --sasl_password  Confluent Cloud password                                                     [required]
  -C, --ca_cert_loc    location of SSL Certs                                                        [default: "/usr/local/etc/openssl/cert.pem"]
  -s, --silent         Silent mode: no output to console                                            [boolean]
  -z, --zzz            enable sleep mode checking                                                   [boolean]
  -r, --maxrpm         Maximum number of requests per minute                                        [default: 6]
  -k, --kafka          Kafka bootstrap servers                                                      [required]
  -t, --topic          Kafka publish topic                                                          [default: "teslams"]
  -n, --naptime        Number of minutes to nap                                                     [default: 30]
  -N, --napcheck       Number of minutes between nap checks                                         [default: 1]
  -O, --vehicle        Select the vehicle offset (i.e. 0 or 1) for accounts with multiple vehicles  [default: 0]
  -S, --sleepcheck     Number of minutes between sleep checks                                       [default: 1]
  -v, --values         List of values to collect                                                    [default: "speed,odometer,soc,elevation,est_heading,est_lat,est_lng,power,shift_state,range,est_range,heading"]
  -?, --help           Print usage information     ```                                 

#Feedback and Support

For more information, feedback, or community support see the Tesla Motors Club forum at http://www.teslamotorsclub.com/showthread.php/13410-Model-S-REST-API or email teslams@googlegroups.com

