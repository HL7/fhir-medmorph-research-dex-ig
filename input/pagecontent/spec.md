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

#### Profiles and Other IGs used by the specification

This specification makes significant use of [FHIR profiles]({{site.data.fhir.path}}profiling.html), search parameter definitions, and terminology artifacts to describe the content to be shared as part of MedMorph Research Data Exchange Content IG workflows. The implementation guide is based on [FHIR R4]({{site.data.fhir.path}}) and profiles are listed for each interaction.

The full set of profiles defined in this implementation guide can be found by following the links on the MedMorph [FHIR Artifacts](artifacts.html) page.

##### MedMorph Reference Architecture (RA) IG Usage

This IG leverages the [MedMorph RA IG]({{site.data.fhir.ver.medmorphIg}}/index.html) defined by HL7 Public Health WG as the reference architecture for automation and implementing the research data exchange use case.

##### CDMH IG Usage

This IG leverages the [CDMH IG]({{ site.data.fhir.ver.cdmhIg }}/index.html) profiles to export and map data that can be transformed to PCORnet CDM, i2b2, OMOP and Sentinel data models. The CDMH profiles provide a complete set of mapping from FHIR profiles to PCORnet CDM which will be leveraged by the implementers of this IG to transform the FHIR resources to the appropriate CDM hosted by the Data Mart. 

##### US Core Usage

This IG leverages the [US Core]({{ site.data.fhir.ver.uscoreR4 }}) set of profiles defined by HL7 for sharing non-veterinary EMR individual health data in the U.S.  Where US Core profiles exist, this IG either leverages them directly or uses them as a base for any additional constraints needed to support the research use cases.  If no constraints are needed, this IG does not define any profiles.


##### Bulk Data Access IG Usage

This IG leverages the [BulkData Access IG]({{site.data.fhir.ver.buldataIg}}/index.html) defined by HL7 Infrastructure WG for 

	* Performing a Bulk Export of data from an EHR that can be used to populate the Data Mart CDM.
	* Enabling authentication and authorization between various actors involved in the workflows.
	
### Knowledge Artifacts for Research Data Exchange and ETL

This section describes the requirements of the Research Knowledge Artifact to implement the outlined use cases. In order to define the Knowledge Artifact the following workflow is used. 


{% include img.html img="ETL.png" caption="Figure  - Research ETL process steps" %}


<br/>

The Knowledge Artifact should have appropriate actions for successful data extraction from an EHR, transform the data to appropriate CDM and then load the data into the Data Mart CDM. However the MedMorph RA IG currently defines actions only to extract the data from the EHR but does not provide generic actions for transforming and loading the data into the data mart. So the Knowledge Artifact for the Research Data Exchange will contain the actions to extract the data but not the actions for transformation and loading the data mart.


#### Implementation Requirements

#### SMART on FHIR Backend Services Authorization requirements

This section outlines how the SMART on FHIR Backend Services Authorization will be used by the MedMorph Research Data Exchange Content  implementation guide. 

* The system actors namely EHRs and Backend Service App are required to use the SMART on FHIR Backend Services Authorization mechanisms as outlined below for the following interactions 
	
	* Backend Service App accessing data from the EHR

* System actors acting as servers (EHRs) **SHALL** advertise conformance to SMART Backend Services by hosting a Well-Known Uniform Resource Identifiers (URIs) as defined in the [Bulk Data Access IG Authorization Section]({{ site.data.fhir.ver.bulkIg }}/authorization/index.html#advertising-server-conformance-with-smart-backend-services) specification.

* System actors acting as servers **SHALL** include token_endpoint, scopes_supported, token_endpoint_auth_methods_supported and token_endpoint_auth_signing_alg_values_supported as defined in the [Bulk Data Access IG Authorization Section]({{ site.data.fhir.ver.bulkIg }}/authorization/index.html) specification.

* When System actors act as clients (Backend Service App), they **SHALL** share their JSON Web Key Set (JWKS) with the server System actors (EHRs and NCHS data stores) using Uniform Resource Locators (URLs) as defined in the [Bulk Data Access IG Authorization Section]({{ site.data.fhir.ver.bulkIg }}/authorization/index.html#registering-a-smart-backend-service-communicating-public-keys) specification.

* System actors acting as clients **SHALL** obtain the access token as defined in the [Bulk Data Access IG Authorization Section]({{ site.data.fhir.ver.bulkIg }}/authorization/index.html#obtaining-an-access-token) specification.

* For the research data exchange use cases, EHRs **SHALL** support the system/*.read scopes. 

* The healthcare organization's existing processes along with the EHRs authorization server **SHALL** verify consent and other policy requirements before allowing the Backend Service App to access the data to be extracted for research purposes. The processes to be verified by the health care organizations include any consent required from patients, IRB approvals and any other agreements required based on the health care organization policies. 


##### Knowledge Artifact and Knowledge Artifact Repository Requirements 

* Research Organizations **SHALL** create a Knowledge Artifact following the constraints identified by the [MedMorph Provisioning requirements]({{site.data.fhir.ver.medmorphIg}}/provisioning.html#creating-knowledge-artifacts) 


##### EHR Requirements

* The EHR **SHALL** support the requirements as outlined in the [EHR Capability Statement](CapabilityStatement-medmorph-research-dex-ehr.html).

###### Authorization requirements 

* EHRs **SHALL** support the [SMART on FHIR Backend Services Authorization](spec.html#smart-on-fhir-backend-services-authorization-requirements) outlined above as a Server. 
 

###### Group Resource requirements

* EHRs **SHALL** support the creation of [Group resource]({{site.data.fhir.ver.medmorphIg}}/StructureDefinition-research-participant-group.html) to identify the patients who have consented to be participating in a research program.

* EHRs **SHALL** associate Groups with specific clients who can query the Group for data.

* EHRs **SHALL** allow clients to retrieve Group resource by name. When multiple Group resources meet the criteria, the EHR **SHOULD** return each of the Group resources that match the name, unless the EHR can narrow down the Group resource to only one.

* EHRs **SHALL** allow clients to retrieve Group resource by identifier. When multiple Group resources meet the criteria, the EHR **SHOULD** return each of the Group resources that match the name, unless the EHR can narrow down the Group resource to only one.

* EHRs and the Backend Service App **SHOULD** pre-coordinate the group names and/or identifiers that can be used for retrieval of data.

* EHRs **SHALL** support the [Bulk Data Access Group export]({{ site.data.fhir.ver.bulkIg }}/export/index.html#endpoint---group-of-patients) operation to export the data required for the data mart population.

* EHRs **SHALL** support the [Group export operation parameters]({{ site.data.fhir.ver.bulkIg }}/OperationDefinition-group-export.html) for all Group export operations.
 

###### Data API requirements

* EHRs **SHALL** support the [US Core Profiles]({{site.data.fhir.ver.uscoreR4}}/CapabilityStatement-us-core-server.html) as outlined in the [EHR Capability Statement](CapabilityStatement-medmorph-research-dex-ehr.html) for the Backend Service App to access patient data using the Group export operation.

* EHRs **SHOULD** support the [CDMH IG Profiles]({{site.data.fhir.ver.cdmhIg}}/artifacts.html#structures-resource-profiles) for the Backend Service App to access patient data using the Group export operation for populating multiple common data models.


##### BSA Requirements 

###### Authorization requirements

* BSA **SHALL** support the [SMART on FHIR Backend Services Authorization](spec.html#smart-on-fhir-backend-services-authorization-requirements) outlined above as a client. 

###### Knowledge Artifact processing requirements 

* The BSA **SHALL** allow the healthcare organization to activate/deactivate a specific Knowledge Artifact. Activation indicates applying the Knowledge Artifact and deactivation indicates not applying the Knowledge Artifact for events occurring within the healthcare organization.

* The BSA **SHALL** implement FhirPath expression processing to process the MedMoprh Research Data Exchange Knowledge Artifact actions.

* The BSA **SHALL** schedule jobs to perform data extraction from EHRs at scheduled time intervals per the Knowledge Artifact.

###### Data API requirements 

* The BSA acting as a client **SHALL** use the [US Core Server APIs]({{site.data.fhir.ver.uscoreR4}}/CapabilityStatement-us-core-server.html) as outlined in the [EHR Capability Statement](CapabilityStatement-medmorph-research-dex-ehr.html) to access patient data from the EHR

* The BSA acting as a client **SHOULD** use the [CDMH IG Profiles]({{site.data.fhir.ver.cdmhIg}}/artifacts.html#structures-resource-profiles) to extract data required to populate the common data models.


###### MedMorph RA Requirements 

* The BSA **SHALL** implement the MedMorph BSA requirements as outlined in the [MedMorph RA BSA requirements]({{site.data.fhir.ver.medmorphIg}}/CapabilityStatement-medmorph-backend-service-app.html).

##### Trust Service Provider Requirements 

* The Trust Service Provider **SHALL** implement the requirements outlined in the [MedMorph Trust Service Provider Capability Statement]({{site.data.fhir.ver.medmorphIg}}/CapabilityStatement-medmorph-trust-service-provider.html) to provide deidentification, anonymization and psuedonymization services. 

##### Data Mart Requirements 

* The Data Mart **SHALL** implement a transformation service to transform the exported data formatted in `ndjson` to appropriate data models and load the data models following the [CDMH IG Mappings from FHIR to CDMs]({{site.data.fhir.ver.cdmhIg}}/profiles.html).

