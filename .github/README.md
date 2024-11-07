## Torogoz Pay
![](./screenshots/TorogozPay-Logo.jpg)

* [Introduction](#intro)
* [Architecture](#arch)
* [Prerequisites](#prereq)
* [Installation](#install)
* [Prep For Execution](#prep)
* [Start The Server](#start)
* [Access The Application](#access)
* [Troubleshooting](#trouble)

### <a name="intro"></a>Introduction
TorogozPay is a payment gateway that allows to process orders, payments and split payments to the value chain stakeholders, using Lightning network. 
Previous to TorogozPay installation, a running and synced Bitcoin node and LND node are required. 

### <a name="arch"></a>Architecture
![](./screenshots/TorogozPay-Arch.jpg)

### <a name="prereq"></a>Prerequisites
* Operation and synchronization of the Bitcoin and LND Lightning node.
* Rust, which can be installed [here](https://www.rust-lang.org/tools/install)
* Java, which can be installed [here](https://www.oracle.com/java/technologies/downloads/)

### <a name="install"></a>Installation
To download a specific version of TorogozPay, follow the instructions on the [release page](https://github.com/torogozpay/releases)

#### First time setup

```
$ git clone https://github.com/torogozpay/api-ecomerce.git
$ cd api-ecomerce
$ cargo build
```

```
$ git clone https://github.com/torogozpay/api-lightning.git
$ cd api-lightning
$ cargo build 
```

```
$ git clone https://github.com/torogozpay/api-business.git
$ cd api-business
$ mvn clean install
```


### <a name="prep"></a>Prep for Execution
TorogozPay requires its own configuration files before starting the API's.

Lightning API
* Locate the complete path of the readable macaroon file (admin.macaroon) and the certificated file(tls.cert) on your node.
* Create the file `Development.toml` located at`./api-lighning/shared/src/config`.
* Modify the `Development.toml` file per the example file below

Example Lightning:
```
[node]
macaroon_file = "admin.macaroon"
cert_file = "tls.cert"
host = "127.0.0.1"
port = 8080
expiry = 10800
cltv_expiry = 144
max_paths = 5  
pathfinding_timeout = 60 
max_fee = 0.01
min_fee = 5
out = 

[server]
database_url = "postgres://USER:PASSWORD@SERVER:PORT/lightningapi?sslmode=disable"
host = "127.0.0.1"
port = "8181"

[log]
level = "info"

[app]
image_url = 

[openapi]
swagger = true

[api]
api_server= "https://DOMAIN"
api_user= 
api_pass= 

[jwt]
jwt_secret= "SECRET"
jwt_secs= 3600 

[job]
seconds= 60
```

Ecommerce API
* Create the file `Development.toml` located at`./api-ecomerce/shared/src/config`.
* Modify the `Development.toml` file per the example file below

Example Ecommerce:
```
[server]
database_url = "postgres://USER:PASSWORD@SERVER:PORT/ecommerceapi?sslmode=disable"
host = "127.0.0.1"
port = "8282"

[log]
level = "debug"

[openapi]
swagger = true

[api]
api_server= "https://DOMAIN"
api_user= 
api_pass= 

[jwt]
jwt_secret= "SECRET"
jwt_secs= 3600 
```

For details on all the configuration options refer to [this page](./docs/Configurations.md).


### <a name="start"></a>Start the API's
Run the API's following command:

`api-lightning> $ cargo build` 
`api-business> $ mvn clean install` 
`api-ecomerce> $ cargo build` 

If the API's started successfully, you should get the below output on the console:

`$ Lightning API is up and running, please open the UI at http://localhost:8181`
`$ Business API is up and running, please open the UI at http://localhost:8383`
`$ Ecommerce API is up and running, please open the UI at http://localhost:8282`

#### Optional: Running TorogozPay as a service (Linux platform users)
In case you are running Linux node, you can configure API's as a service.

* Create the Lightning Service as a systemd unit and with the following content. Save and exit.
```bash
[Unit]
Description=Lightning API Service

[Service]
ExecStart=/home/torogoz/services/api/lightning/target/release/main
Restart=always
WorkingDirectory=/home/torogoz/services/api/lightning

[Install]
WantedBy=multi-user.target
```

* Create the Business Service as a systemd unit and with the following content. Save and exit.
```bash
[Unit]
Description=Lightning Business API
After=network.target

[Service]
ExecStart=java -jar businessapi-0.0.1.jar
Restart=always
WorkingDirectory=/home/torogoz/services/api/business
SuccessExitStatus=143
Restart=on-failure
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

* Create the Ecommerce Service as a systemd unit and with the following content. Save and exit.
```bash
[Unit]
Description=Ecommerce API Service

[Service]
ExecStart=/home/torogoz/services/api/ecommerce/target/release/main
Restart=always
WorkingDirectory=/home/torogoz/services/api/ecommerce

[Install]
WantedBy=multi-user.target
```

* enable and start the Lightning Service
```
$ sudo systemctl enable api-lightning
$ sudo systemctl start api-lightning
```
* monitor the Lightning Service log file in realtime(exit with Ctrl-C)

`$ sudo journalctl -f -u api-lightning`

* enable and start the Business Service
```
$ sudo systemctl enable api-business
$ sudo systemctl start api-business
```
* monitor the Business Service log file in realtime(exit with Ctrl-C)

`$ sudo journalctl -f -u api-business`

* enable and start the Ecommerce Service
```
$ sudo systemctl enable api-ecomerce
$ sudo systemctl start api-ecomerce
```
* monitor the Ecommerce Service log file in realtime(exit with Ctrl-C)

`$ sudo journalctl -f -u api-ecomerce`



### <a name="trouble"></a>Troubleshooting
In case you have any issue with the application or if you have feedback, feel free to open a new issue on our github repo.
You can also find  us in X TorogozDev at [@torogozdev](https://x.com/torogozdev). Thanks for your interest.