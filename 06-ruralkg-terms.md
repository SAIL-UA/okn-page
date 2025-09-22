---
layout: default
title: "RuralKG Terms"
nav_order: 6
---

# RuralKG RDF Terms

*by the RuralKG Team • last updated September 22, 2025*

This page lists all classes (types of things) and properties (relationships) used in the **Rural Knowledge Graph (RuralKG)**. The graph integrates cross-domain data covering administrative geography, public health, and justice to study rural resilience in the United States.

Our modeling approach is to reuse established, standard vocabularies where possible and define custom terms only when necessary for our specific domain connections. This page is organized by namespace, with the standard, reused vocabularies listed first, followed by the custom terms created specifically for RuralKG.

---
## Standard Vocabularies (Reused Terms)

These terms are adopted from widely used, external ontologies to ensure our data is interoperable and follows FAIR principles.

### Schema.org (`schema`)
A general-purpose vocabulary for structured data. We use it primarily for geographic and organizational concepts.
* `schema:City`
* `schema:GeoCoordinates`
* `schema:Place`
* `schema:PostalAddress`
* `schema:address`
* `schema:addressLocality`
* `schema:addressRegion`
* `schema:geo`
* `schema:latitude`
* `schema:location`
* `schema:longitude`
* `schema:postalCode`

### Dublin Core (`dcterms`)
Used for metadata describing the RuralKG ontology itself.
* `dcterms:created`
* `dcterms:creator`
* `dcterms:hasVersion`
* `dcterms:title`

### RDF, RDFS, & OWL
Core vocabularies for defining the fundamental structure and semantics of the knowledge graph.
* `rdf:type`
* `rdfs:Class`
* `rdfs:subClassOf`
* `rdfs:label`
* `rdfs:comment`
* `rdfs:isDefinedBy`
* `owl:Ontology`
* `owl:imports`
* `owl:sameAs`

### Friend of a Friend (`foaf`)
Used for social networking concepts, from which we adopt the property for phone numbers.
* `foaf:phone`

---
## RuralKG Vocabularies (Custom Terms)

These terms are defined within the RuralKG namespace (`http://sail.ua.edu/ruralkg/`) to model the specific entities and relationships in our cross-domain data.

### Administrative Area (`adminArea`)
Terms related to geopolitical entities like states, counties, and cities. These form the geographic backbone of the graph.

**Classes**
* `adminArea:AdministrativeArea` - The top-level class for all administrative regions.
* `adminArea:State` - Represents individual states within the U.S.
* `adminArea:County` - Defines counties within a state.
* `adminArea:City` - Represents city entities, linked to their respective counties.

**Properties**
* `adminArea:abbreviation` - The standard two-letter abbreviation for a state (e.g., "AL").
* `adminArea:containsPlace` - An object property linking a larger administrative area (like a State) to a smaller one it contains (like a County).
* `adminArea:fips` - A literal property for the Federal Information Processing Standard (FIPS) code that uniquely identifies states and counties.
* `adminArea:primaryCounty` - An object property linking a `City` instance to its primary `County` instance.

### Settlement Type (`settlement`)
Terms for classifying areas based on demographic data, particularly the Rural-Urban Continuum Codes (RUCC) from the USDA.

**Classes**
* `settlement:SettlementType` - The top-level class for settlement classifications.
* `settlement:RUCC` - Represents a specific Rural-Urban Continuum Code classification.
* `settlement:CountyStatus` - An intermediary class that links a specific county to its demographic status (including RUCC) for a given year.

**Properties**
* `settlement:censusCounty` - Links a `CountyStatus` entity to the `County` it describes.
* `settlement:code` - The specific RUCC code value (e.g., "1", "9").
* `settlement:hasRUCC` - Links a `CountyStatus` entity to its assigned `RUCC` classification instance.
* `settlement:population` - The population of a county for a given year.
* `settlement:year` - The census or reporting year for the demographic data.

### Mental Health Service (`mhs`)
Describes the types of mental health services available, based on the National Directory of Mental Health Treatment Facilities.

**Classes**
* `mhs:MentalHealth` - The top-level class for all mental health concepts.
* `mhs:MentalHealthService` - A specific mental health service offered (e.g., "Cognitive behavioral therapy").
* `mhs:MentalHealthServiceCategory` - A broader category of services (e.g., "Trauma therapy").

**Properties**
* `mhs:code` - The official code for a service or service category.
* `mhs:containsService` - Links a `MentalHealthServiceCategory` to a more specific `MentalHealthService` that falls under it.

### Treatment Provider (`treatment`)
Represents the facilities and organizations that provide mental health services.

**Classes**
* `treatment:TreatmentProvider` - An entity, such as a clinic or hospital, that offers treatment.

**Properties**
* `treatment:providesService` - **Key linking property.** Connects a `TreatmentProvider` to a `MentalHealthService` that it offers.

### Substance Abuse (`substanceabuse`)
Models substances and incident types sourced from the National Survey on Drug Use and Health (NSDUH).

**Classes**
* `substanceabuse:SubstanceAbuse` - The top-level class for all substance abuse concepts.
* `substanceabuse:Substance` - A specific substance (e.g., "Heroin," "Alcohol").
* `substanceabuse:SubstanceRelatedIncident` - A type of event or behavior related to substance use.

**Properties**
* `substanceabuse:sourceDataset` - The source of the data (e.g., "NSDUH").
* `substanceabuse:identifier` - An external identifier, such as a Wikidata ID, used for linking to other knowledge bases.

### Justice (`justice`)
Represents crime data concepts extracted from the National Incident-Based Reporting System (NIBRS). This vocabulary is extensive, structured into segments and their corresponding data elements.

**Classes**
* `justice:Justice` - Top-level class for all justice-related concepts.

**Segments**
* `justice:AdministrativeSegment`
* `justice:ArresteeSegment`
* `justice:OffenderSegment`
* `justice:OffenseSegment`
* `justice:PropertyGroup`
* `justice:VictimSegment`

**Data Elements (by Segment)**
* **Administrative Segment Elements**
    * `justice:ReportDate_Indicator`
    * `justice:ReportingPeriod`
    * `justice:IncidentHour`
    * `justice:CargoTheft`
    * `justice:ExceptionalClearanceDate`
    * `justice:ExceptionalClearance`
* **Arrestee Segment Elements**
    * `justice:ArrestTransactionNumber`
    * `justice:ArrestDate`
    * `justice:TypeOfArrest`
    * `justice:UCRArrestOffenseCode`
    * `justice:ArresteeArmedWith`
    * `justice:AutomaticWeaponIndicator`
    * `justice:ResidentStatus`
    * `justice:DispositionOfArresteeUnder18`
* **Offender Segment Elements**
    * `justice:OffenderSequenceNumber`
    * `justice:AgeOfOffender`
    * `justice:SexOfOffender`
    * `justice:RaceOfOffender`
    * `justice:EthnicityOfOffender`
* **Offense Segment Elements**
    * `justice:UCROffenseCode`
    * `justice:OffenseAttempted_Completed`
    * `justice:OffendersSuspectedOfUsing`
    * `justice:LocationType`
    * `justice:NumberOfPremisesEntered`
    * `justice:MethodOfEntry`
    * `justice:TypeOfWeapon_ForceInvolved`
    * `justice:TypeOfCriminalActivity`
    * `justice:BiasMotivation`
* **Property Group Elements**
    * `justice:TypeOfPropertyLoss`
    * `justice:PropertyDescription`
    * `justice:ValueOfProperty`
    * `justice:DateRecovered`
    * `justice:NumberOfStolenMotorVehicles`
    * `justice:NumberOfRecoveredMotorVehicles`
    * `justice:SuspectedDrugType`
    * `justice:EstimatedDrugQuantity`
    * `justice:TypeDrugMeasurement`
* **Victim Segment Elements**
    * `justice:VictimSequenceNumber`
    * `justice:TypeOfVictim`
    * `justice:AgeOfVictim`
    * `justice:SexOfVictim`
    * `justice:RaceOfVictim`
    * `justice:EthnicityOfVictim`
    * `justice:ResidentStatusOfVictim`
    * `justice:AggravatedAssault_HomicideCircumstances`
    * `justice:AdditionalJustifiableHomicideCircumstances`
    * `justice:TypeOfInjury`
    * `justice:OffenderNumberRelated`
    * `justice:RelationshipOfVictimToOffender`
* **Shared Elements (appear in multiple segments)**
    * `justice:Age`
    * `justice:Sex`
    * `justice:Race`
    * `justice:Ethnicity`