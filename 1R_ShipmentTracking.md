# ONE Record-based shipment tracking

## Dependencies

### Standards applied

The ONE Record business ontology version as of APR 13, 2022 was used [Working draft Ontology of 2022APR13](https://github.com/IATA-Cargo/ONE-Record/blob/bbe86e364b04d6a6279f0ab6e9ee47e1905ec9c4/working_draft/ontology/IATA-1R-DM-Ontology.ttl).

The ONE Record API and security specification draft witout a version as of APR 13, 2022 was used (no link available yet).

### ONE Record Server Implementation used

(no ONE Record server implementation yet)

### Other Software products used

## Assumptions

One or more stakeholders of the supply chain are able to report progress in the transportation of shipments along the supply chain.

One or more stakeholders are interested in retrieving this information. The party requesting this information has an HouseAWB Number or a MasterAWB Number as unique ID for the shipment.

Both parties are using ONE Record as means of sharing information.

## Solution approach

This use case is covering the same content that is today provided by generic or cIMP/cXML-based messaging. It is covering the 

Although the data is provided on shipment level, the ONE Record data model is piece-centric as a basic principle. This means, some milestones that are provided in the legacy environment on shipment level, are moved in a ONE Record environment on piece level. This applies, when the milestone has a relation with the piece in the physical world. For example, the milestone FOH reflects the handover of freight from the forwarder to the carrier. In the physical world, this usually happens by handing over pieces at the export acceptance of an airline. As piece-by-piece is handed over in reality, the status of each piece should switch to FOH. Below in #### you´ll find an attribution of milestones to data objects in the ONE Record data model.

## Piece-centricity and 

As most milestones are bound to the piece, this raises the question of how to deal with a piece-centric data model in an environment operating on shipment level, as it is usually the case today. If a party is not working on piece level yet, but on shipment level, it practically means that all pieces are labeled FOH the moment all pieces of a shipment are handed over. And this is exactly the mechanism to be applied if a party is not working on piece level. 

## Solution in current environment

In the legacy messaging environment, end to end CO2 emission tracking on piece level isn´t possible. Generic solutions for transparency on shipment level are in place, but don´t follow a standardized approach for different modes of transportation.

# Data use and target process

Climate relevant emissions are performed by a ***transportMeans*** on a ***transportMovement***, because a ***transportMeans*** does not produce emissions by itself (e.g. a plane that isn´t flying), the primary attribution of CO2-Emissions is linked with the ***transportMovement***. The ***transportMovement*** can be any movement of pieces, like a Truck leg, a flight, or even a forklift-movement. 

While all climate data exchange around ***transportMeans*** and ***transportMovement*** serves for submitting the data basis for climate impact calculation between different stakeholders of the supply chain, the following climate impact data on piece level serves to fulfill the estimation of the actual impact of the transport on the climate.



|   	|Physical object with climate impacting emissions|Climate impacting attributions on piece level|
|---	|---	|---	|
|Focus LOs  	|***transportMeans***, ***TransportMovement***, ***payloadDistance***|***piece***, ***climateEffect***|
|Example|RFS Truck from AMS to CDG caused 12t  
|Purpose|Provide basic data for climate impact calculation on piece level|Provide transparency on climate impact of transport on piece level|
|Provider of data	|Operators (Airline, Trucking Company, etc.)|"Supply chain orchestrators" (can be airlines, forwarders, booking platforms)|
|Target Group / Data consumers |"Supply chain orchestrators" (can be airlines, forwarders, booking platforms)|Shippers, end customers, etc.|

The following diagram shows the relevant data fields in the ONE Record data model:

![DataModel](docs/dm2.svg) 

## transportMovement LO

The TransportMovement directly contains emission-relevant data: The ***distanceMeasured***, the ***distanceCalculated***, the ***fuelType***, the ***fuelAmountMeasured***, the ***fuelAmountCalculated*** and a link towards the correlating CO2-Emissions ***CO2Emissions*** (1:n link).

### Data fields: distanceMeasured and distanceCalculated

If available, the actually measured distance is provided in the ***distanceMeasured*** data field. Only if not available, the ***distanceCalculated*** data field should be populated.

### Data field: fuelType

The ***fuelType*** data field should indicate the fuel that was consumed for this ***transportMovement***. "Kerosene", "SAF", "Renewable electric energy" are examples for possible values ***no standardized list, list by ISO expected; Moritz: standard-liste Referenz***. 
1:n
***Energy Carrier***
***FeedStock***
***FeedStockShare***
Data exchange guidance Table 6 (Mail vom 29.4.2022)

### Data fields: fuelAmountMeasured and fuelAmountCalculated

If available, the actually measured fuel consumption is provided in the ***fuelAmountMeasured*** data field. Only if not available, the ***fuelAmountCalculated*** data field should be populated.

### Data field: totalLoadedWeight

TBD: the total transportation weight is required for climate relevant emission monitoring. Question: How do we deal with this? 

Option 1: Assume shipments are not split, so it is the sum of all totalGrossWeighs of the Shipments. Pro: Realistic and exact, con: doesn´t work if shipments are split

Option 2: Sum the pieces on truck. Pro: easy to calculate; con: is not correct (total gross weight is usually higher with additional loading equipment), Piece info often missing

Option 3: create a new measured value totalLoadedWeight; most accurate, but also available?

## payloadDistance LO

The ***payloadDistance*** LO describes the relevant factor for the climateImpact calculation on a truck.

### Data field: payloadDistanceResult (value)

The payloadDistanceResult is a most relevant parameter for the estimation of the climate impact of this transportMovement. It is usually calculated by multiplying the weight and the distance of the transportMovement. Possible units are kilogram-kilometre ("kgkm"), tonne-kilometre ("tkm"), kilometre-tonne ("kmt") and "ton-mile", which is in the US: 1 ton-mile * ( 0.907185 t / short ton) * ( 1.609344 km / mile ) = 1.460 tkm.

### ISOTransparencyLevel (int)

This parameter shows the level of parameters to be included. 

TBD here, e.g. is there a level including the deadhead legs? Etc.

### FuelConsumptionParameter

This indicator can be either "measured" or "calculated". It describes the calculation basis for the calculation result, and not the data basis, which can be found in the the ***fuelAmountMeasured*** and ***fuelAmountCalculated*** of the transportMovement (TBD: Obsolete due to the ISO Levels bringing clear indicators here?).

### DistanceParameter

This indicator can be either "measured" or "calculated". It describes the calculation basis for the calculation result, and not the data basis, which can be found in the the ***distanceMeasured*** and ***distanceCalculated*** of the transportMovement (TBD: Obsolete due to the ISO Levels bringing clear indicators here?)

### CO2CoefficiencyFactor

**tbd** required?

### Other data fields

Other data fields like ***departureLocation*** and ***arrivalLocation*** could be used to verify the CO2-Emission relevant data sources. Additionally, relevant information could be added as an ***externalReference***, if only available as PDF. This could also be used for an image or a GPS-track of the geo-locational movement to provide an additional layer of information.

The ***movementType*** has a special relevance here, as it indicates wether this is a planned transport movement or an already  performed one.

## transportMeans LO

The ***transportMeans*** describes the means of transportation used to perfom for the linked transportMovement. Classical examples are a truck that performs a road leg for a transportation from the forwarder´s hub to the carrier´s origin airport, or a Boeing 777 freighter to perform a flight from Frankfurt to Rio de Janeiro. 

### Data field: typicalFuelConsumption

The ***typicalFuelConsumption*** describes an average amount of fuel for a defined distance, e.g. 12 l / 100 km. This does not include the type of fuel, as one of the assumptions is that the consumption doesn´t depend on the type of fuel. When using this, the ***unit*** data field is quite extensively used, with a content like "l/100km".

### Data field: typicalCO2Coefficient

The ***typicalCO2Coefficient*** describes ??? required?

### Data field: 

## piece LO

The Piece is the central unit of the ONE Record data model, and thus climate impact should be calculated and published on this level. If no detailed piece information is available, the total gross weight of the shipment is evenly distributed amoungst the pieces of the shipment. The total number of pieces should also be known. If the weights of individual pieces are known, they must be taken into account.


### Data field: grossWeight

The data field ***grossWeight*** within the piece LO describes the weight of the piece, and thus is to be used for the impact calculation

### Data field: skeletonBy

The data field ***skeletonBy*** aims for providing information, if and by whom a piece skeleton was created. Skeleton pieces are placeholders, if the owning party does not provide piece information (usually the Shipper). In that case, the totalGrossWeight of the shipment is evently distributed over the number of pieces, and thus pieces skeletons are created with a generic UPID. If the field is filled with content, piece skeletons were created, if left blank, piece information is available. Piece skeletons can be created by any party. Once piece skeletons are used, they are to be used along the supply chain for any piece-level purpose, instead of creating new piece skeletons by downflow parties.


## ClimateEffect LO

The ***climateEffect*** LO is the Logistics Object documenting the effective climate impact of the transportation of the piece. Each stakeholder should quantify the effect for his own part of the transportation chain, meaning the carrier should provide information for all legs under the MAWB contract, including flight legs, RFS, etc., the forwarder should provide all transportation legs under his control (usually the HAWB), including the carriers legs, etc. To clearly indicate these "embedded" emissions, a climateEffect can contain "embedded" climateEffects.

### Data field: CO2equivalentWTW

**tbd**

### Data field: CO2equivalentTTW

**tbd**

### Data field: MethodName

This field contains the name of the method applied.

### Data field: MethodVersion

This field contains the version of the method calculation, if available.

### Data field: MethodLink

This data field contains a URL to more details on the calculation method applied.

### Data field: Verification

**tbd**

### Data field: Accreditation

**tbd**

### Data field: TransportActivity

### Data field:  includedClimateEffects

This data field contains linked climateEffect calculation of embedded transport activites (see remarks above) 


# API use

### Technical setting

### Basic API-Features used

## Results / Summary

## Additional comments / FAQs

### How do we deal with missing piece information?

Principally, the ONE Record data model is based on the piece. Thus the ***climateImpact*** LO is linked to the piece, never the shipment. Thus we seem to have a problem, if e.g. the weights of each piece are missing, as this is a relevant factor for climate Impact calculation. 

But even if *detailed* piece information are not available, the number of pieces is usually available. In that case, the ***totalGrossWeight*** of the shipment is divided over the number of pieces. Meaning that it is assumed that all pieces have the same weight. This procedure is called the "use of piece skeletons". But this approach is only to be applied, if there´s no piece information available. If piece information are available, they must be taken into account for the climate impact calculation.

If a consumer wants to consume the ***climateImpact*** on shipment level, it is required to sum up the ***climateImpact*** of all pieces within the shipment. Providing the climate impact on shipment level is not possible within ONE Record.
