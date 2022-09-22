### Business Need and User Stories
The section identifies the business needs and specific user stories outlining the research data exchange needs.

#### Business Need
The purpose of the Making Electronic Data More Available for Research and Public Health (MedMorph) Research Data Exchange Implementation Guide (IG) is to streamline and expedite the process of onboarding research data partners and contributing data to research networks. The current processes used to extract, transform, and load (ETL) the data and then contribute the data involve many non-standardized mechanisms (e.g., Secure File Transfer Protocol (SFTP), Excel Files, stored procedures), different structures (e.g., formats), and different semantics. As a result of these non-standardized processes, the length of time to onboard a data partner varies from weeks to months. This research data exchange use case along with leveraging the MedMorph Reference Architecture (RA) IG with other existing Health Level 7 (HL7<sup>®</sup>) Fast Healthcare Interoperability Resources (FHIR<sup>®</sup>) IGs will help reduce the length of time it takes to onboard new data exchange partners. 

#### Goals of the Use Case
The goals of the Research Data Exchange use case include:
* Improve efficiency in onboarding new data exchange partners
* Improve data latency 
* Increase the volume, quality, completeness, and timeliness of data submitted to research organization at a lower cost
* Provide standardize way to collect and exchange data for research
* Leverage the MedMorph RA IG
* Streamline data element mapping from the data mart 

#### Scope of the Use Case

**In Scope**
* Bulk data exchange
* Identify the data elements to be retrieved from the Data Source for research needs

**Out of Scope**
* State and local policies around data exchange for research 
* Assessment of the data quality of the content extracted from the Data Source.
* Data captured outside the Data Source and communicated directly to data marts.
* Data exchange/Data use agreements between data source and research organization
* Utilization of unstructured data (e.g., images or text blobs)
 
#### User Story #1: Onboarding Research Data Partners
As a research network administrator, there is a need to onboard research data partners to join the research network and contribute data that can be used for research. The data partners extract, transform, and load (ETL) the data from data sources such as Electronic Health Records (EHRs), Health Information Exchanges (HIEs), or other data repositories (e.g., clinical data warehouses). These processes (e.g., ETL) when standardized will expedite onboarding processes and deliver better quality data at a lower cost.

#### User Story #2: Accessing Additional Data for a Specific Research Question (future consideration)
Once a data partner joins a research network and contributes data, a researcher should be able to perform queries to filter specific data sets and analyze the data. Currently researchers can query data marts on a limited basis because of the variations in the data models (e.g., National Patient-Centered Clinical Research Network (PCORnet) Clinical Data Management (CDM), Informatics for Integrating Biology & the Bedside (i2b2), Observational Medical Outcomes Partnership (OMOP), FHIR Resources) and the technologies (e.g., Microsoft Structured Query Language (SQL), Postgres SQL, Statistical Analysis System (SAS)) used to host the data mart. Standardizing these data access mechanisms will help overcome the hurdles faced by researchers in accessing data from data partners and EHRs. 
**NOTE:** User Story #2 is out of scope for this version of the IG, it is being included only as a potential use case that could be expanded on in the future.

### Focus of the MedMorph Research Data Exchange IG 

The focus of the MedMorph Research Data Exchange IG is User Story #1. This is based on extensive discussions with the PCORnet Front Door team, the MedMorph Reference Architecture (RA) WG, and the CDC MedMorph team. The reasons for choosing User Story #1 is as follows:

* Reduces the number of custom mappings between data sources and the destination research data models (e.g., i2b2, PCORnet, Sentinel and OMOP) to only 4 mappings instead of the large number of mappings (e.g., potentially a mapping used at every site for every data source) currently in use.

The current state is as shown in the Figure below:

{% include img.html img="DataPartnerOnboardingCurrentState.png" caption="Figure 2.1 - Data Partner Onboarding Current State" %}

<br/>

The usage of FHIR during the onboarding process and expected efficiencies are documented below in Figure 2.2.


{% include img.html img="DataPartnerOnboardingFuture.png" caption="Figure 2.2 - Data Partner Onboarding Using FHIR" %}


<br/>

### MedMorph Research Data Exchange Actors and Definitions

The following actors from the [MedMorph RA IG]({{site.data.fhir.ver.medmorphIg}}/usecases.html#medmorph-actors-and-definitions) are used by the Research Data Exchange use case:

* Data Source (e.g., EHR, HIE, data repository)
* Data Mart acting as a Data Receiver with FHIR capabilities per the MedMorph RA IG
* HDEA
* Trust Service Provider
* Knowledge Artifact Repository

### Research Abstract Model for Onboarding a Data Partner to Populate a Data Mart

Figure 2.3 below shows the research abstract model to onboard a data partner who can contribute data from their data source (e.g., and EHR system) to populate a data mart.

{% include img.html img="DataPartnerOnboardingv2.png" caption="Figure 2.3 - Populating a Data Mart from an EHR" width="100px" %}

The description of the interactions as illustrated in Figure 2.3 include:
* S1: The HDEA initiates the extraction process to get data from a Data Source for one or more patients.
* S2: The HDEA uses the Trust Service Provider to transform the data if needed from one data model to another data model. This step may also involve de-identification, anonymization, or pseudonymization.
* S3: The data is populated into the Data Mart.

<br>

As an alternative to Figure 2.3, the Data Mart and the HDEA could reside outside the health care organization as shown in Figure 2.4 below. 

{% include img.html img="OnboardingExternalDataPartnerv2.png" caption="Figure 2.4 - Populating an Externally Hosted Data Mart from a Data Source" width="100px" %}

The description of the interactions as illustrated in Figure 2.4 include:
* S1: The HDEA initiates the extraction process to get data from a Data Source for one or more patients.
* S2: The HDEA uses the Trust Service Provider to transform the data if needed from one data model to another data model. This step may also involve de-identification, anonymization, or pseudonymization.
* S3: The data is populated into the Data Mart.

<br>

