# SAOU

Service endpoint: https://saou.addi.dk/2.0/SaouWebService

Example client: https://saou.addi.dk/2.0/

WSDL: https://saou.addi.dk/2.0/SaouWebService?wsdl

## Description of SAOU

SAOU (Service for Authentication and Objects) returns information about a library users's access to electronic resources based on the access terms and subscriptions of all the libraries that the user is affiliated with, including a link to the given resource, rewritten match the access conditions granted by the user's librart. SAOU can be used by local and national library services to check if an e-resource is available to a specific user.

An electronic resource can be identified by either a PID, a URL, or a combination of both.

A user is identified by a userId, which is related to the user's library, identified by an agencyId. The user's municipality affiliation (which may grant extended access terms for the user) can be set with the element municipalityAgency.
