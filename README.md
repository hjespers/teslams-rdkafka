# Tesla Telemetry gateway for Apache Kafka


An implementation in Node.js of the streaming telemetry interface of Tesla Motors with output to Apache Kafka.  

# Disclaimer

Use these programs at your own risk. The authors do not guaranteed the proper functioning of these applications. This code attempts to use the same interfaces used by the official Tesla phone apps. However, it is possible that use of this code may cause unexpected damage for which nobody but you are responsible. 

# Contributors
Hans Jespersen (https://github.com/hjespers)

# Dependencies

Linux dependencies

* openssl
* libssl-dev
* libsasl2-dev
* libsasl2-modules
* C++ toolchain

macOS dependencies

* Apple Xcode command line tools (for the compiler)
* openssl installed via Brew (needed for root certs file in `/usr/local/etc/openssl/cert.pem`)
* Export CPPFLAGS=-I/usr/local/opt/openssl/include and LDFLAGS=-L/usr/local/opt/openssl/lib



# Installation

To use this program you must download and install 'node' from http://nodejs.org

Install from npm on macOS

  brew install openssl
  export CPPFLAGS=-I/usr/local/opt/openssl/include
  export LDFLAGS=-L/usr/local/opt/openssl/lib
  sudo -E npm install -g teslams-rdkafka

Install from npm on Ubuntu

  sudo apt install nodejs-legacy
  sudo apt install openssl libssl-dev libsasl2-dev libsasl2-modules
  sudo npm install -g teslams-rdkafka

Install from npm on CentOS

  sudo yum install epel-release nodejs
  sudo yum install openssl openssl-devel cyrus-sasl-devel
  sudo npm install -g teslams-rdkafka

# Uninstall

  sudo npm uninstall -g ccloud-node-console-client

# Useage

The program requires -u and -p in order to specify the Tesla Motors user name and password to access your Model S.
Or, you can instead create a json file in ~/.teslams/config.json and specify them once, in the following format:

	{
		"username": "Your teslamotors.com username/email",
		"password": "Your teslamotors.com password"
	}
	
In addition to putting your password as a command line option or a file, you can alternatively use the $TSLA_USERNAME and $TSLA_PASSWORD environment variables. These environment variable allow the execution of these apps in Heroku or other Platform-as-a-Service providers.

# teslams-rdkafka - Capture and log real-time telemetry to Apache Kafka for analytics 

An application which uses the TESLA HTTP Long Polling "STREAMING" API to get continuous telemetry from a Tesla Model S or Model X. 
A valid teslamotors.com login and password is required and must be provided on the command line options. 

By default the output is a stream of JSON formatted messages published to a set of Kafka topics called "teslams", "teslams.climate_state", and "teslams.charge_state" with the keys being the `id` if the vehicle. The output topic prefix can be changed by using the --topic command line option.

To execute run:

```
teslams-rdkafka -u <username> -p <password> -U <kafka_username> -P <kafka_password> --kafka localhost:9092 --topic my_kafka_topic 

Usage: teslams-rdkafka -u <username> -p <password> -U <kafka_username> -P <kafka_password> [-sz]
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
  -?, --help           Print usage information     

```                                 

# Troubleshooting

If you see the following error when you run either console producer or consumer, it means you have not installed librdkafka correctly with the required SSL and SASL libraries. See install instructions for installing openssl and setting compiler flags.

  Caught error: Error: Invalid value for configuration property "security.protocol"

If the ccloud-console-producer or ccloud-console-consumer immediately exits to the shell prompt you are likely missing the Root CA Certificates. See the location of these certs (below) and add the correct `-S` flag pointing the the location of the certs on your operating system.
  
The default SSL Certificate location is `/usr/local/etc/openssl/cert.pem`  which works on macOS but every flavor of Linux puts root certificates in different places.

  Ubuntu/Debian/Raspbian: /etc/ssl/certs
  CentOS/RedHat: /etc/pki/tls/cert.pem
  macOS: /usr/local/etc/openssl/cert.pem (from `brew install openssl`)

For Ubuntu add the `-C /etc/ssl/certs` flag to specify your certificate location:

  teslams-rdkafka -u teslaowner@yahoo.com -p MyTeslaPassword -U $CCLOUD_SASL_USERNAME -P $CCLOUD_SASL_PASSWORD -k $CCLOUD_BROKERS -C /etc/ssl/certs
  
For CentOS/RedHat add the `-C /etc/pki/tls/cert.pem` flag to specify your certificate location:

  teslams-rdkafka -u teslaowner@yahoo.com -p MyTeslaPassword -U $CCLOUD_SASL_USERNAME -P $CCLOUD_SASL_PASSWORD -k $CCLOUD_BROKERS -C /etc/pki/tls/cert.pem 

# Feedback and Support

For more information, feedback, or community support see the Tesla Motors Club forum at http://www.teslamotorsclub.com/showthread.php/13410-Model-S-REST-API or email teslams@googlegroups.com

