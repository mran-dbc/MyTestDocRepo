Web service for validating and committing bibliographic records to the original DB which is part of the Open Search data well complex.

### Versions

| Version | Endpoint                                        | Environment | WSDL                                           |
|---------|-------------------------------------------------|-------------|------------------------------------------------------|
| 2.0     | https://update.addi.dk/UpdateService/2.0        | production  | https://update.addi.dk/UpdateService/2.0?wsdl        |
| 2.0     | https://updatestaging.addi.dk/UpdateService/2.0 | staging     | https://updatestaging.addi.dk/UpdateService/2.0?wsdl |
| 2.0     | http://oss-services.dbc.dk/UpdateService/2.0    | test        | http://oss-services.dbc.dk/UpdateService/2.0?wsdl    |

### Authentication

The service requester must be identified using their netpunkt-login. The purpose of the authentication is to ensure that a record can only be updated on behalf of the library that the record belongs to.

### Template/schema documentation

http://www.danbib.dk/fbs#skabelon (in Danish)

### Service operations

The service has two operations: updateRecord and getSchemas.
updateRecord can be used to commit a record to the repository. The record is validated against the schema specified with the parameter schemaName. A schema represents a specific type of material (eg. book, movie, audio book). Available schema names can be retrieved using the getSchemas-operation.
If the parameter option is set to validate_only, the record is validated but not actually committed to the repository. 

_**updateRecord-parameters:**_

| Parameter           | Required | Repeatable | Sub element of | Description |
|---------------------|----------|------------|----------------|-------------|
| UpdateRecordRequest | yes      | no         | updateRecord   |             |
| authentication|yes|no|UpdateRecordRequest||
| groupIdAut|yes|no|authentication|Identifier of the service requester (most often a library number)|
| passwordAut|yes|no|authentication|Service requester's password|
| userIdAut|yes|no|authentication|Service requester's user name|
| schemaName|yes|no|UpdateRecordRequest|Name of the schema that the record is validated against. Possible values are retrieved using the getSchemas-operation.|
| bibliographicRecord|yes|no|UpdateRecordRequest|The record that is to be validated or updated|
| recordSchema|yes|no|bibliographicRecord|Record format (eg. info:lc/xmlns/marcxchange-v1 for marcXchange records)|
| recordPacking|yes|no|bibliographicRecord|Packing format (eg. XML)|
| recordData|yes|no|bibliographicRecord|The actual record|
| extraRecordData|no|no|bibliographicRecord|Additional data for the record|
| options|no|no|UpdateRecordRequest|Unique ID to track this request|
| option|yes|yes|options|Possible values: validate_only|
| trackingId|no|no|UpdateRecordRequest|Unique ID to track this request|
| doubleRecordKey|no|no|UpdateRecordRequest|Unique id to track handling of forced update of possible double records. Version 2.0 or newer.|

_**updateRecordResponse-parameters (pre version 2.0):**_

| Parameter           | Required | Repeatable | Sub element of | Description |
|---------------------|----------|------------|----------------|-------------|
|updateRecordResult|yes|no|updateRecordResponse|| 
|updateStatus|if error is not present|no|updateRecordResult|Status for the update. Possible values: ok, validate_only, validation_error, failed_invalid_agency, failed_invalid_schema, failed_invalid_option, failed_update_internal_error, failed_internal_error|
|validateInstance|no|no|updateRecordResult|Elaboration if there is a problem with the validation.|
|validateEntry|yes|yes|validateInstance|Entry for each validation problem|
|warningOrError|yes|no|validateEntry|Possible values: warning, error|
|urlForDocumentation|no|no|validateEntry|Reference to description of the relevant field in the cataloging format|
|ordinalPositionOfField|no|no|validateEntry|Reference to position of the field in the given record that causes the problem|
|ordinalPositionOfSubField|no|no|validateEntry|Reference to position of the sub field in the given record that causes the problem|
|message|no|no|validateEntry|Further description of problem|
|error||if updateStatus is not present|no|updateRecordResult|Possible value: authentication_error|

_**updateRecordResponse-parameters (version 2.0 and newer):**_

| Parameter           | Required | Repeatable | Sub element of | Description |
|---------------------|----------|------------|----------------|-------------|
|updateRecordResult|yes|no|updateRecordResponse| |
|updateStatus|if error is not present|no|updateRecordResult|Status for the update. Possible values: ok, validate_only, validation_error, failed_invalid_agency, failed_invalid_schema, failed_invalid_option, failed_update_internal_error, failed_internal_error|
|doubleRecordKey|no|no|updateRecordResult|Unique id to track handling of forced update of possible double records|
|messages|no|no|updateRecordResult| |
|entry|no|yes|messages| |
|type|yes|no|entry| |
|code|no|no|entry| |
|params|no|no|entry| |
|param|no|yes|params| |

**_getSchemas-parameters:_**

| Parameter           | Required | Repeatable | Sub element of | Description |
|---------------------|----------|------------|----------------|-------------|
|getSchemasRequest|yes|no|getSchema| |
|authentication|yes|no|getSchemaRequest| |
|groupIdAut|yes|no|authentication|Identifier of the service requester(most often a library number)|
|passwordAut|yes|no|authentication|Service requester's password|
|userIdAut|yes|no|authentication|Service requester's user name|
|trackingId|no|no|getSchemaRequest|Unique ID to track this request|
 
_**getSchemasResponse-parameters:**_

| Parameter           | Required | Repeatable | Sub element of | Description |
|---------------------|----------|------------|----------------|-------------|
|getSchemasResult|yes|no|getSchemasResponse|Available schemas|
|schema|no|yes|getSchemasResult|Information about each schema|
|schemaName|yes|no|schema|Name of schema|
|schemaInfo|no|no|schema|Additional information about schema|
|schemasStatus|if error is not present|no|getSchemasResult|Possible values: ok, failed_internal_error|
|error|if schemaStatus is not present|no|getSchemasResult|Possible value: authentication_error|


### Request examples
#### getSchema request:

```xml
 <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:cat="http://oss.dbc.dk/ns/catalogingUpdate">
    <soapenv:Header/>
    <soapenv:Body>
       <cat:getSchemas>
          <cat:getSchemasRequest>
             <cat:authentication>
                <cat:groupIdAut>...</cat:groupIdAut>
                <cat:passwordAut>...</cat:passwordAut>
                <cat:userIdAut>...</cat:userIdAut>
             </cat:authentication>
          </cat:getSchemasRequest>
       </cat:getSchemas>
    </soapenv:Body>
 </soapenv:Envelope>
```

#### updateRecord request:

Example of a request that validates a record against the schema "bog".

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:cat="http://oss.dbc.dk/ns/catalogingUpdate">
   <soapenv:Header/>
   <soapenv:Body>
      <cat:updateRecord>
         <cat:updateRecordRequest>
            <cat:authentication>
               <cat:groupIdAut>...</cat:groupIdAut>
               <cat:passwordAut>...</cat:passwordAut>
               <cat:userIdAut>...</cat:userIdAut>
            </cat:authentication>
            <cat:schemaName>bog</cat:schemaName>
            <cat:bibliographicRecord>
               <cat:recordSchema>info:lc/xmlns/marcxchange-v1</cat:recordSchema>
               <cat:recordPacking>xml</cat:recordPacking>
               <cat:recordData>
                  <marcx:collection xmlns:marcx="info:lc/xmlns/marcxchange-v1">
                     <marcx:record format="danMARC2" type="Bibliographic">
                        <marcx:leader>00000n    2200000   4500</marcx:leader>
                        <marcx:datafield ind1="0" ind2="0" tag="001">
                           <marcx:subfield code="a">79038466</marcx:subfield>
                           <marcx:subfield code="b">870970</marcx:subfield>
                           <marcx:subfield code="c">20111209132742</marcx:subfield>
                           <marcx:subfield code="d">20111208</marcx:subfield>
                           <marcx:subfield code="f">a</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="004">
                           <marcx:subfield code="r">n</marcx:subfield>
                           <marcx:subfield code="a">e</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="008">
                           <marcx:subfield code="t">m</marcx:subfield>
                           <marcx:subfield code="u">f</marcx:subfield>
                           <marcx:subfield code="a">2011</marcx:subfield>
                           <marcx:subfield code="b">dk</marcx:subfield>
                           <marcx:subfield code="d">x</marcx:subfield>
                           <marcx:subfield code="j">f</marcx:subfield>
                           <marcx:subfield code="l">dan</marcx:subfield>
                           <marcx:subfield code="v">0</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="009">
                           <marcx:subfield code="a">a</marcx:subfield>
                           <marcx:subfield code="g">xx</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="021">
                           <marcx:subfield code="e">9788771171617</marcx:subfield>
                           <marcx:subfield code="c">hf.</marcx:subfield>
                           <marcx:subfield code="d">kr. 39,95</marcx:subfield>
                           <marcx:subfield code="n">481139</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="032">
                           <marcx:subfield code="x">ACC201149</marcx:subfield>
                           <marcx:subfield code="a">DBF201151</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="041">
                           <marcx:subfield code="a">dan</marcx:subfield>
                           <marcx:subfield code="c">eng</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="100">
                           <marcx:subfield code="a">Marsh</marcx:subfield>
                           <marcx:subfield code="h">Nicola</marcx:subfield>
                           <marcx:subfield code="4">aut</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="241">
                           <marcx:subfield code="a">A ¤trip with the tycoon</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="245">
                           <marcx:subfield code="a">Mit indiske eventyr</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="260">
                           <marcx:subfield code="&amp;">1</marcx:subfield>
                           <marcx:subfield code="a">Kbh.</marcx:subfield>
                           <marcx:subfield code="b">Harlequin</marcx:subfield>
                           <marcx:subfield code="c">2011</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="300">
                           <marcx:subfield code="a">152 sider</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="440">
                           <marcx:subfield code="a">De ¤bedste romaner</marcx:subfield>
                           <marcx:subfield code="v">nr. 754</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="521">
                           <marcx:subfield code="&amp;">REX</marcx:subfield>
                           <marcx:subfield code="b">1. oplag</marcx:subfield>
                           <marcx:subfield code="c">2011</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="652">
                           <marcx:subfield code="n">83</marcx:subfield>
                           <marcx:subfield code="z">26</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="652">
                           <marcx:subfield code="o">sk</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="720">
                           <marcx:subfield code="o">Birgitte Larsen</marcx:subfield>
                           <marcx:subfield code="4">trl</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="945">
                           <marcx:subfield code="a">DBR</marcx:subfield>
                           <marcx:subfield code="z">440(a)</marcx:subfield>
                        </marcx:datafield>
                        <marcx:datafield ind1="0" ind2="0" tag="996">
                           <marcx:subfield code="a">710100</marcx:subfield>
                        </marcx:datafield>
                     </marcx:record>
                  </marcx:collection>
               </cat:recordData>
               <cat:extraRecordData></cat:extraRecordData>
            </cat:bibliographicRecord>
            <cat:options>
               <cat:option>validate_only</cat:option>
            </cat:options>
         </cat:updateRecordRequest>
      </cat:updateRecord>
   </soapenv:Body>
</soapenv:Envelope>
```