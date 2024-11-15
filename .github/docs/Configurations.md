
### To Lightning API<br />

```
[node]
macaroon_file = Path and file name admin.macaroon
cert_file = Path and file name tls.cert
host = Host for LND node
port = Port for LND node, default 8080
expiry = Invoice duration, default 10800
cltv_expiry = Expiration time in blocks, default 144
max_paths = Maximum number of hops that we want the node to attempt to make the payment, default 5  
pathfinding_timeout = Amount of time to try to find a route, default 60 
max_fee = Maximum amount of fee that we are willing to pay for the routing (percentage), default 0.01
min_fee = Minimum amount of fee that we are willing to pay for the routing (satoshis), default 5
out = Channel ID of the peer through which we want to withdraw the payment from our node

[server]
database_url = Database connection URL, for example "postgres://USER:PASSWORD@SERVER:PORT/lightningapi?sslmode=disable"
host = Host for Lightning Api, default 127.0.0.1
port = Port for Lightning APi, default 8181

[log]
level = Logging levels, default info

[app]
image_url = Application image URL, for example "https://"

[openapi]
swagger = Boolean flag to enable the swagger, default false

[api]
api_server= Application URL, for example "https://DOMAIN"
api_user= User for Business Api
api_pass= Password for Business Api

[jwt]
jwt_secret= Secret for create token
jwt_secs= Valid time for token, default 3600 

[job]
seconds= Execution time of scheduled jobs, default 60
```

### To Ecommerce API<br />

```
[server]
database_url = Database connection URL, for example "postgres://USER:PASSWORD@SERVER:PORT/ecommerceapi?sslmode=disable"
host = Host for Ecommerce Api, default 127.0.0.1
port = Port for Ecommerce APi, default 8282

[log]
level = Logging levels, default debug

[openapi]
swagger = Boolean flag to enable the swagger, default false

[api]
api_server= Application URL, for example "https://DOMAIN"
api_user= User for Business Api
api_pass= Password for Business Api

[jwt]
jwt_secret= Secret for create token
jwt_secs= Valid time for token, default 3600 
```

### API Business<br/>
```
@ -0,0 +1,10 @@
server.port=8087
spring.datasource.Url=jdbc:mysql://localhost:3306/lightning
spring.datasource.username=database's username
spring.datasource.password=database's password
#spring.jpa.hibernate.hbm2ddl.auto=update
#spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.springframework.security=DEBUG
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```
