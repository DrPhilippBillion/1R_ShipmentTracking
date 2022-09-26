# Digital-Accompanying-Documents-in-ONE-Record

## Basic Information on this document

### Objective 
The purpose of this document is to provide a Good Practice for using Digital Accompanying documents in an IATA ONE Record-based data eco-system

### Target audience
This document can be used by any party with the interest of using digital accompanying documents in ONE Record. 

### Geographical coverage
As there are no legal or operational restrictions, the solution can be used world wide.

### Creators
This document is the outcome of a ONE Record pilot project with the "Digitales Testfeld Air Cargo" by the German air cargo community. Parties/Persons involved were:

Lufthansa Cargo, Philipp Billion (in lead)

CHI International, Oliver Meschkov

Fraunhofer IML, Oliver Ditz

Lufthansa Industry Solutions, Daniel Döppner

### Continous development and availability

This document is to be used and continously developed, even if the current major stakeholders should move to other topics. Thus a "handover" in Github is planned if responsibilities should shift.

### Use and reference

This Good Practice is free to access and use. 

Please mention the use of this Good practice and provide a link to the Github repository as a source.

### Publication date, version and history

Publication date, version and history should be provided by the Github version control system and not be duplicated here.


## Dependencies

### Standards applied

The ONE Record business ontology version as of APR 13, 2022 was used [Working draft Ontology of 2022APR13](https://github.com/IATA-Cargo/ONE-Record/blob/bbe86e364b04d6a6279f0ab6e9ee47e1905ec9c4/working_draft/ontology/IATA-1R-DM-Ontology.ttl).

The ONE Record API and security specification draft witout a version as of APR 13, 2022 was used (no link available yet).

(no ONE Record server implementation yet)

## Assumptions
One or more stakeholders are able to provide digital accompanying documents, One or more stakeholders are able to consume digital accompanying documents

## Process

### Solution approach

Digital accompanying documents are based on the use of the ONE Record data object 

>ExternalReference

The ExternalReference can be attached to any data object, as a digital accompanying document can relate to any data object. E.g. this can be a ULD certificate linked with a ULD LO, documents releated with the pieces under one contract (= Shipment LO), an external reference can be added to the Shipment LO. Many documents will be bound 

The use of the ExternalReference for Photos, Videos, etc. will be exactly identical to the use for PDFs as described here. Thus in terms of ONE Record, the mechanisms would be completely identical

### Target process

### Solution in current environment

In the legacy messaging environment, digital accompanying documents are not possible. Generic solutions with generic cloud environments are already possible, but result in raised maintenance and implementation costs and are not scalable.

## Data fields

### General data fields

### Specifically used data fields

## API use

### Technical setting

### Basic API-Features used

## Results / Summary

## Additional comments

### Provision of BLOBs on the data provider side

### Procession of BLOBs on the data user side 
