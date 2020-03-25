# Updateservice build instructions
Web service for retrieving a template and an unique identification number
(FAUST) for a bibliographic record, based on the type of material and the
record format. Used for cataloging purposes.

## Versions

| Endpoint                                        | Environment | WSDL                                           |
|-------------------------------------------------|-------------|------------------------------------------------------|
| https://build.addi.dk/CatalogingBuildServices/OpenBuild | Production | https://build.addi.dk/CatalogingBuildServices/OpenBuild?wsdl |
| https://buildstaging.addi.dk/CatalogingBuildServices/OpenBuild | Staging | https://buildstaging.addi.dk/CatalogingBuildServices/OpenBuild?wsdl |
| https://oss-services.dbc.dk/CatalogingBuildServices/OpenBuild | Test | https://oss-services.dbc.dk/CatalogingBuildServices/OpenBuild?wsdl |

## Authentication

The service requester must be identified using their netpunkt-login. The purpose of the authentication is to ensure that a record can only be updated on behalf of the library that the record belongs to.

## Template/schema documentation

http://www.danbib.dk/fbs#skabelon (in Danish)

## Service operations:

The service has one operation: build

The build-operation can either take an existing bibliographic record and
give it a new identifier or the recordData-element can be empty. If empty,
a template matching the given schema name, record schema and record packing
is returned.

The only record schema currently supported is marcXchange v1.0, while xml
is the only supported record packing.  

Available schema names can be retrieved using the getSchemas-operation of
the update-web service. Each schema represents a different type of material
(eg. a book, recorded music, film etc.)


_**build-parameters:**_


| Parameter           | Required | Repeatable | Sub element of | Description|
|---------------------|----------|------------|----------------|------------|
| buildRequest |yes|no|build||
| schemaName|yes|no|buildRequest|Name of build schema based on material type (eg. bog, musik, film etc.). Available schema names can be retrieved using the getSchemas-operation of the update-web service|
| bibliographicRecord|yes|no|buildRequest|Information about the bibliographic record or template|
| recordSchema|yes|no|bibliographicRecord|Bibliographic format (eg. info:lc/xmlns/marcxchange-v1 for marcXchange records)|
| recordPacking|yes|no|bibliographicRecord|Packing format (eg. xml)|
| recordData|yes|no|bibliographicRecord|The actual bibliographic record. May be empty.
| extraRecordData|no|no|bibliographicRecord|Additional data for the bibliographic record|
| trackingId|no|no|buildRequest|Unique ID to track this request|


_**buildResponse-parameters:**_
| Parameter           | Required | Repeatable | Sub element of | Description |
|---------------------|----------|------------|----------------|-------------|
| buildResult|yes|no|buildResponse||	
| buildStatus|yes|no|buildResult|Status for the build. Possible values: ok, failed_invalid_schema, failed_invalid_record_schema, failed_invalid_record_packing, failed_update_internal_error, failed_internal_error|
| bibliographicRecord|no|no|buildRequest|Information about the bibliographic record or template returned by the service, including the actual record or template|
| recordSchema|yes|no|bibliographicRecord|Bibliographic format (eg. info:lc/xmlns/marcxchange-v1 for marcXchange records)|
| recordPacking|yes|no|bibliographicRecord|Packing format (eg. xml)|
| recordData|yes|no|bibliographicRecord|The bibliographic record, or the template for the bibliographic record, as created by the service, including the unique identifier (FAUST-number)|
| extraRecordData|no|no|bibliographicRecord|Additional data for the bibliographic record|
 
## Request examples

Retrieves an identification number and marcXchange template for a
bibliographic record describing a book.
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:cat="http://oss.dbc.dk/ns/catalogingBuild">
   <soapenv:Header/>
   <soapenv:Body>
      <cat:build>
         <cat:buildRequest>
            <cat:schemaName>bog</cat:schemaName>
            <cat:bibliographicRecord>
               <cat:recordSchema>info:lc/xmlns/marcxchange-v1</cat:recordSchema>
               <cat:recordPacking>xml</cat:recordPacking>
               <cat:recordData/>
            </cat:bibliographicRecord>
         </cat:buildRequest>
      </cat:build>
   </soapenv:Body>
</soapenv:Envelope>
```
