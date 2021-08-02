This section defines the specific requirements for systems wishing to conform to actors specified in this MedMorph Research Content IG.  The specification focuses on using the Backend Service App to perform the ETL process used for data partner on boarding within research networks.

### Context

#### Pre-reading
Before reading this formal specification, implementers should first be familiar with these sections of the specification:

* The [Use Cases](usecases.html) page provides the business context and general process flow enabled by the formal specification.


#### Conventions
This implementation guide uses specific terminology to flag statements that have relevance for the evaluation of conformance with the guide:

* **SHALL** indicates requirements that must be met to be conformant with the specification.

* **SHOULD** indicates behaviors that are strongly recommended (and which may result in interoperability issues or sub-optimal behavior if not adhered to), but which do not, for this version of the specification, affect the determination of specification conformance.

* **MAY** describes optional behaviors that are free to consider but where the is no recommendation for or against adoption.


#### Claiming Conformance 

Actors and Systems asserting conformance to this implementation guide have to implement the requirements outlined in the corresponding capability statements. The following definition of MUST SUPPORT is to be used in the implementation of the requirements.

##### MUST SUPPORT Definition

* Systems SHALL be capable of populating data elements as specified by the profiles and data elements are returned using the specified APIs in the capability statement.
* Systems SHALL be capable of processing resource instances containing the MUST SUPPORT data elements without generating an error or causing the application to fail. In other words, Systems SHOULD be capable of displaying the data elements for human use or storing it for other purposes.
* In situations where information on a particular data element is not present and the reason for absence is unknown, Systems SHALL NOT include the data elements in the resource instance returned from executing the API requests.
* When accessing MedMorph Research Content IG data, Systems SHALL interpret missing data elements within resource instances returned from API requests as data not present.


#### Profiles
This specification makes significant use of [FHIR profiles]({{site.data.fhir.path}}profiling.html), search parameter definitions, and terminology artifacts to describe the content to be shared as part of MedMorph Research Content IG workflows. The implementation guide is based on [FHIR R4]({{site.data.fhir.path}}) and profiles are listed for each interaction.

The full set of profiles defined in this implementation guide can be found by following the links on the MedMorph [FHIR Artifacts](artifacts.html) page.


#### US Core

This IG also leverages the [US Core](http://hl7.org/fhir/us/core) set of profiles defined by HL7 for sharing non-veterinary EMR individual health data in the U.S.  Where US Core profiles exist, this IG either leverages them directly or uses them as a base for any additional constraints needed to support the research use cases.  If no constraints are needed, this IG does not define any profiles.

Where US Core profiles do not yet exist (e.g., for PlanDefinition, Bundle), MedMorph Reference Architecture IG profiles will be used.


#### SMART on FHIR Backend Services Authorization
This section outlines how the SMART on FHIR Backend Services Authorization will be used by the MedMorph Research Content  implementation guide. 

* The following actors comprise the System Actors: EHRs, Knowledge Artifact Repository, Backend Service App, Trust Service Provider and Research Organizations.

* System Actors (EHRs, Knowledge Artifact Repository, Backend Service App, Trust Service Provider and Research Organization) SHALL advertise conformance to SMART Backend Services by hosting a Well-Known Uniform Resource Identifiers (URIs) as defined in the [Bulk Data Access IG](http://hl7.org/fhir/uv/bulkdata/export/index.html) specification.

* System Actors SHALL include token_endpoint, scopes_supported, token_endpoint_auth_methods_supported and token_endpoint_auth_signing_alg_values_supported as defined in the [Bulk Data Access IG](http://hl7.org/fhir/uv/bulkdata/export/index.html) specification.

* When System Actors act as clients, they SHALL share their JSON Web Key Set (JWKS) with the server System Actors using Uniform Resource Locators (URLs) as defined in the [Bulk Data Access IG](http://hl7.org/fhir/uv/bulkdata/export/index.html) specification.

* Client System Actors SHALL obtain the access token as defined in the [Bulk Data Access IG](http://hl7.org/fhir/uv/bulkdata/export/index.html) specification.

* For the research use cases, EHRs SHALL support the system/*.read scopes. The Healthcare organization processes along with the EHRs authorization server SHALL verify consent and other policy requirements before allowing the Backend Service App to access the data.


### Usage of Common Data Model Harmonization (CDMH) artifacts 

The CDMH IG contains a mappings that can be used to transform from FHIR to other data models namely PCORnet CDM, i2b2, Sentinel and OMOP. The MedMorph Research Content IG leverages these mappings directly instead of re-creating them as part of this IG. 

### Knowledge Artifact for Research ETL

This section describes the requirements of the Research Knowledge Artifact to implement the outlined use cases. In order to define the Knowledge Artifact the following workflow is used. 


{% include img.html img="ETL.png" caption="Figure  - Research ETL process steps" %}


<br/>

The following are the requirements that will be addressed by the Research PlanDefinition

* The PlanDefition SHALL contain a time based trigger event.
* The PlanDefinition SHALL contain an extract-data action.
* The PlanDefinition SHALL specify the identifier to be used for querying the Group relevant for data extraction.
* The PlanDefinition SHALL specify a transform-data action.
* The PlanDefinition SHALL specify a load-data action. 


