# OpenOrder-webservice Build instructions

## Installation
The webservice requires [class_lib](svn://oss.dbc.dk/sync_php/php/OpenLibrary/class_lib) to be installed in ./OLS_class_lib

In the php.ini file:
- make sure that always_populate_raw_post_data = On

Copy openorder.ini_INSTALL to openorder.ini and edit it to reflect your setup

Copy openorder.wsdl_INSTALL to openorder.wsdl and modify
the location of the service (at the end of the file)

Create a symbolic link from index.php to server.php or modify your webserver to default to server.php

Consider copying robots.txt_INSTALL to robots.txt


In the php_exec-catalog the esgaroth_wrapper must be placed. It could look something like:

```
#!/bin/sh
/usr/www-local/esgaroth-orderpolicy/bin/esgaroth-shell --search "builtin: plugin: file:/usr/www-local/esgaroth-orderpolicy/esgaroth-lib file:/usr/www-local/esgaroth-orderpolicy/jslib-lib file:/usr/www-local/esgaroth-orderpolicy/esgaroth-config" /usr/www-local/esgaroth-orderpolicy/esgaroth-script/orderpolicy.js $1 $2 1> /dev/null 2> /dev/null
```

