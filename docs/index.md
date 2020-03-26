---
permalink: /index.html
---
# Our Development Philosophy
DBC’s IT development is based on open source and service oriented
architecture, thereby following the recommendations of The National
IT and the Danish Agency for Digitisation for public institutions.

We participate in [TING Concept Community](http://www.ting.dk/) with many
Danish public libraries and library system suppliers. TING is an open
source community for user driven development in the Danish library sector.
The vision is to promote cooperation, sharing and digital development in
cultural institutions for the benefit of end users. It is based on a
service oriented architecture, which facilitates openness, synergy and
reuse of the services on many different platforms.

As a partner and member of TING Council, we return our development to TING
in the form of source code, which may be used freely for
further development by all members of TING Community.

We use GNU Affero GPL 3 for Web services. GPL version 3 is used for all other software.

Read more about [our company.](http://www.dbc.dk/english)

## Services
This section contains an overview of DBC web services. For each service
there is a link to a list of available versions, including links to the
WSDL and service endpoint. DBC web services are always to be called with a
specific version number (eg. https://opensearch.addi.dk/b3.5_5.2/).

| Services                                               |
|-------------------------------------------------------| 
| [Updateservice](updateservice/Home.md)                |
| [OpenOrder webservice](OpenOrder-webservice/Home.md)  |
| [iso18626](iso18636/Home.md)                          |
| [CULR](culrservice/Home.md)                           |
| [Order Receipt](orderreceipt/Home.md)                 |

## Web service description
The interfaces of all DBC web services are published in WSDL v1.1. The
parameters that a web service takes for each of its functions, are desribed
in an XSD-file, which is located together with the WSDL. 

## Protocols
Supported protocols are typically SOAP over HTTPS and also URL-requests
over HTTPS aswell as JSON-output. See the individual services for details.

## Output formats
Supported types of output are described within the XSD for each web
service. Most DBC web services provide SOAP output as default and
optionally XML, JSON and PHP-objects. For most services the desired
output-format can be set with parameter outputType. Default output format
for SOAP and URL-requests is SOAP, and XML for XML-requests.

## Source code
You can browse the source code of the web services [on github](http://github.com/DBCDK)


