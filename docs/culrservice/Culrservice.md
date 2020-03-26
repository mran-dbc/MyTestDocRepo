# Description of the CULR-system

CULR (Core User Library Registry) receives local user identifiers from affiliated data providers (eg. libraries) and assigns a global user id (GUID) for each unique user. Access to digital content products for library users depends on the user’s library affiliation. A service (eg. a local or national library service) who wants to give users access to digital content products, can obtain information from CULR, and then offer the users access to relevant content products as the user’s library- or municipality-affiliation warrants.

## Versions

| Version | Endpoint                                           | Environment    | Start of life | End of life | WSDL |
|---------|----------------------------------------------------|----------------|---------------|-------------|------|
| 1.4     | https://culr.addi.dk/1.4/CulrWebService            | production     | 20181126      |             | https://culr.addi.dk/1.4/CulrWebService?wsdl
| 1.4     | https://culrstaging.addi.dk/1.4/CulrWebService     | staging        |               |             | https://culrstaging.addi.dk/1.4/CulrWebService?wsdl
| ~~1.3~~ | ~~https://culr.addi.dk/1.3/CulrWebService~~        | ~~production~~ |               | 20200101    | ~~https://culr.addi.dk/1.3/CulrWebService?wsdl~~
| ~~1.3~~ | ~~https://culrstaging.addi.dk/1.3/CulrWebService~~ | ~~staging~~    |               |             | ~~https://culrstaging.addi.dk/1.3/CulrWebService?wsdl~~|

## System Architecture
CULR consists of three components:

A web service that handles the connection between the affiliated institutions, services and the CULR-system.

A database which stores the submitted data from the affiliated institutions which are data providers

An adminstration system: CULR access for data providers (or brokers acting
on behalf of one or more providers) and services are administered through
DBC’ VIP-base (https://vip.dbc.dk) in section J. CULR profile.

## Definitions
A *Service* uses CULR to obtain user information: Potential services could
for instance be end user access components or common digital library
    content resources. Services have read only-rights and cannot add,
    delete, or modify data.

A *Provider* is a provider of data to CULR, ie. an institution (such as a
library) that have user accounts attached. It may be a public library, an
educational library or a research library. Providers (or brokers acting on
behalf of one or more providers) are set up in CULR administration system
(DBC VIP base) using the 6-digit part of their ISIL number. Providers are
allowed to create, delete, or modify data.

An *Account* contains information about a user's registration with a given
provider. Account information consists of userIdType (CPR, LOCAL, or
UNILOGIN) and userIdValue. The combination of agencyId and userIdValue must
be unique, so that a specific user is clearly identifiable within a given
provider's user registrations. It is possible for a user to have multiple
accounts at given agency.

*Patron* covers all the user identifiers of a person, a company, or another
entity. The patron is used internally by CULR to link accounts across
providers. A patron always has at least one account associated and an
account will always be linked to a patron. When a new account is created,
it will be linked to any existing accounts for the same user, if the user's
CPR is included in the createAccountRequests and it matches an existing
patron. The user's municipality affiliation is a part of the patron
information. A patron can have zero or one municipality affiliations.

## Authentication
Use of the CULR web service requires a DanBib license from DBC (Netpunkt
access). Credentials must be stated in any request for the service.

## Web service operations
There are two categories of operations in the CULR web service.

The first category is intended for services and consists of two operations: getAccountsByGlobalId and getAccountsByLocalId.

The second is intended for providers and consists of the following
operations: createAccount, deleteAccount, updateAccount, mergePatrons, and
getAccountFromProvider.

***getAccountsByGlobalId:***

Based on the account given in getAccountsByGlobadIdRequest, the associated
patron is localised. All accounts aswell as the GUID associated to the
patron is returned in getAccountsByGlobalIdResponse. userIdType in the
getAccountsByGlobalIdRequest must be CPR.

***getAccountsByLocalId:***

This operation looks op a user based on userIdValue and agencyId. This
operation is designed for cases where userIdType is locally defined or
unknown. Output is the same as getAccountsByGlobalId.

***createAccount:***

If the account does not exist, an account aswell as a GUID is created. If
an account is created with a userIdType different from CPR, an additional
globalUID (consisting of uidValue and uidType) should be included if the
information is available for the user. If a patron with the same
useridValue and userIdType (or globalUID if set) already exists, the
account will be linked to the existing patron. Otherwise a new patron and
GUID are created with the values from userIdValue and userIdType (if
userIdType is CPR) or globalUid if any of these are present, and optionally
municipalityNo if set.

***deleteAccount:***

Deletes the account and checks if there are other accounts associated with
the patron. If not the patron is deleted. If there are other accounts
associated with the patron, the patron is not changed. This implies that a
patron's municipality affiliation is not necessarily deleted even if the
user's account at a given agency is deleted from CULR.

***updateAccount:***

Find the requested account and the associated patron and set municipality
number for the patron. If municipalityNo is empty, the patron's
municipality affiliation is deleted.

***mergePatrons:***

Takes two accounts as input: accountToMergeWith and accountToBeMerged.
accountToMergeWith must have userIdType CPR, and accountToBeMerged must
have userIdType LOCAL or UNILOGIN. If merge is a success, accountToBeMerged
will now be linked to the patron which accountToMergeWith is linked to.
This implies that if the patron which accountToBeMerged was linked to
before merge contains a CPRor municipalityNo, the account is no longer
linked to this.

***getAccountFromProvider:***

Retrieve the requested account and GUID, but not the related accounts.

***deleteAllAccountsByProvider:***

Delete all accounts from a specific agency
