# About the CargoSoft API

## Authorization
The following chapter will explain the authorization prozess to access the CargoSoft API Endpoints. The use case figure below shows a common example how to retrieve the JWT and how to use it.

![Authorization][useCaseAuthorization]

[useCaseAuthorization]: https://raw.githubusercontent.com/cargosoft-gmbh/cargosoft-api-docs/2021.4/resources/Use_Case_Authentifizierung.svg "Use case Authorization"


### Retrieve a token
First of all each client have to request a bearer Java Web Toke (JWT) from the CargoSoft API. To receive a JWT, username and password are required. Username and password can only create by CargoSoft. 
For more details [see](tag/Authorization#operation/retrieveAuthToken).

> Please note: The JWT is valid for 24 hours. After that you have to request a new one.

### Access API endpoints
To access a CargoSoft API endpoint a JWT is needed. The JWT have to be sent in the HTTP header.

> Attention: Your user may be restricted. The CargoSoft API use user groups and roles to define user access against the CargoSoft API endpoint. If your user is restricted or it's not allowed to use an endpoint you needed please contact our support.

## CRUD
The CargoSoft API works generally with the CRUD operations.

### Lookup
The CargoSoft API extends the CRUD retrieve operations with lookup operations. Each functional group of endpoints should have a lookup method implemented. 
Lookups should be used to search functional objects with given criteria. The criteria may be different to each functional group. Which functional criteria are possible take a look to the functional group.

The search criterias will be sent in the request body up to the API lookup endpoint. A criteria contains the following three information:
1. _match_mode_: The match mode is used to compare the criteria against the object in the database. Possible matche mods are **[EQUALS, CONTAINS, IN, OR, GT, LT, GE, LE]**
2. _key_: The key of the criteria for the functional object to lookup. Which keys are possible please see to the corresponding implementation.
3. _value_: The value of the criteria

**Example**: Lookup request body to search a shipment:
```json
[
    {
        "match_mode": "EQUALS",
        "key": "MBL",   
        "value": "MASTER_12345"
    },
   {
        "match_mode": "EQUALS",
        "key": "MODE_OF_TRANSPORT",
        "value": "SEA"
    }
]
```

> Please note: When using one of any lookup methods please define search criteria as precisely as possible to limit the result of objects.

The look up may contains ordering by the criteria. The ordering will pass as query parameter. 

The result set of a lookup include zero up to n functional objects depending on the search criteria.
Each functional object contains an ID. The ID can be used to retrieve additional information by passing the id as path parameter of the retrieve endpoint.

**Example**: Lookup a shipment and retrieve after that all shipment events:
1. Looking up a shipment with given criteria for instance with the BL number. For more details please [see](tag/Shipment#operation/lookup).
2. Extract the ID of the shipment result object.
3. Call the [Events](tag/Shipment#operation/{id}/events) endpoint with the shipment ID as path parameter.

### Pagination
Many endpoints of the CargoSoft API use pagination mechanics. The moste of them are retrieve or lookup endpoints.

The pagination information will send as request query parameter up to the API endpoint. The pagination contains the following two query parameter:
1. _page_no_: The current number of the page
2. _page_size_: The size of the page

The response contains two additional headers:
1. _X-Page-Count_: The total number of pages.
2. _X-Row-Count_: The total number of found objects.

With this both headers is it possible to retrieve next paged results.

# Use cases

## Receive shipment status
The use case for reading out the status information from CargoSoft can be used in very different ways.
The following graphic shows the process for retrieving status information schematically.

PLATZHALTER SEQUENZDIAGRAMM

CargoSoft has different pools in which status information is stored.
These are described in detail in the next subsections. However, in order to understand the differences
exactly, the following graphic tries to clarify how the status message flow works exactly.

PLATZHALTER STATUSDURCHLAUF VON PROVIDER -> STAUS -> TRANSPORTWEGREITER

All three types of status information can be retrieved via the API, but
the appropriate endpoint must always be called.

### Shipment Provider Status
These status messages come from external status providers
and service providers who supply CargoSoft with status information. 

There cannot be 
entered or changed by CargoSoft employees by hand. A shipment provider status always represents exactly 
one state - planned / estimated or the actual date and time (depending on the name of the status).

The codes of the statuses are defined by the providers and are variable depending on the provider.

### Shipment Status
CargoSoft customers can define this status code themselves. In addition, the shipment provider statuses can be mapped into a shipment status if the CargoSoft customer has stored this. Here, for example, it can be defined that the status ABC of provider XYZ is to be processed as shipment status ETA. In this case ETA would be a freely defined code for the status. There is therefore no list of valid codes.

With this status, there are also two forms of date information per data record. Once a planned and an actual one. This means that a status ETA, although obviously planned, can contain an actual date. However, this is due to the recording of the customer's data.

The CargoSoft customer can add this status information manually, but an existing record can no longer be changed.


### Shipment Events
Shipment events are status events that are defined by CargoSoft. These codes, which are listed in the table below, are fixed and can always be called up - unless they have not been recorded in CargoSoft.

A shipment event can be created automatically from a shipment status if this is set in the CargoSoft master data.

A status always represents exactly one instance - planned or actual and the information can be changed at any time by hand, but also by background processes.

| **Code**          | **Description**                       |
|-------------------|---------------------------------------|
| PICKUP            | The date and time of the pickup event |
| POL_DEPARTURE     | anything                              |
| FINAL_DESTINATION | xya                                   |








## Receive shipment details

# Testing
Server für Testdemo gehostet.
Zugangsdaten am Support anfragen.
Testaccount zugriff auf alle Endpunkte aus der Doku

Demo bestand an Daten wird täglich (Nachts?) resettet. 

