### Business Need and User Stories
The section identifies the business needs and specific user stories outlining the research data exchange needs.
 
* **User Story #1:** As a research network administrator, there is a need to on-board research data partners to join the research network and contribute data that can be used for research. The data partners extract, transform and load (ETL) the data from data sources such as EHRs, HIEs, or other data respositories. The current processes used to extract, transform, load the data and then contribute the data involve many non-standardized mechanisms (e.g., SFTP, Excel Files, stored procedures), different structures (e.g., formats), and different semantics. As a result of these non-standardized processes, the length of time to on-board a data partner varies from weeks to months. These processes (e.g., ETL) when standardized will expedite on-boarding processes and deliver better quality data at a lower cost. 

* **User Story #2: (future consideration)** Once a data partner joins a research network and contributes data, a researcher should be able to perform queries to filter specific data sets and analyze the data to improve treatments and outcomes. Currently researchers are able to query data marts on a limited basis because of the variations in the data models (e.g., PCORnet CDM, i2b2, OMOP, FHIR Resources) and the technologies (e.g., Microsoft SQL, Postgres SQL, SAS, etc.)  used to host the data mart. Standardizing these data access mechanisms will help overcome the hurdles faced by researchers in accessing data from data partners and EHRs. **NOTE:** User Story #2 is out of scope for this version of the IG, it is being included only as a potential use case that could be expanded on in the future.

### Focus of the MedMorph Research Data Exchange Content IG 

The focus of the MedMorph Research Data Exchange Content IG is User Story #1. This is based on extensive discussions with the PCORnet Front Door team, the MedMorph Reference Architecture (RA) WG, and the CDC MedMorph team. The reasons for choosing User Story #1 is as follows:

* Reduces the number of custom mappings between data sources and the destination research data models (e.g., i2b2, PCORnet, Sentinel and OMOP) to only 4 mappings instead of the large number of mappings (e.g., potentially a mapping used at every site for every data source) currently in use.

The current state is as shown in the Figure below:

{% include img.html img="DataPartnerOnboardingCurrentState.png" caption="Figure 2.1 - DataPartner On-boarding Current State" %}

<br/>

The usage of FHIR during the on-boarding process and expected efficiencies are documented below:


{% include img.html img="DataPartnerOnboardingFuture.png" caption="Figure 2.2 - DataPartner On-boarding Using FHIR" %}


<br/>

### Research Abstract Model for On-boarding a Data Partner to Populate a Data Mart

Figure 2.3 below shows the research abstract model to onboard a data partner who can contribute data from their EHR system to populate a data mart.

{% include img.html img="DataPartnerOnboarding.svg" caption="Figure 2.3 - Populating a Data Mart from an EHR" %}

<br>

As shown in Figure 2.3 above, the Health Data Exchange App (HDEA), MedMorph's backend services app, will extract data from a Data Source for one or more patients and use the Trust Services to perform translation, masking, and populating the data mart. In the above diagram the Data Mart exists within the health care organization. The Data Mart and the HDEA could reside outside the health care organization as shown in Figure 2.4 below. 

{% include img.html img="OnboardingExternalDataPartner.svg" caption="Figure 2.4 - Populating an Externally Hosted Data Mart from a Data Source" %}

<br>


### MedMorph Research Actors and Definitions

This section defines the Actors and Systems that interact within the MedMorph RA to realize the capabilities of the research data exchange use case listed above.

1. __Electronic Health Record (EHR)__:  A system used in care delivery for patients and that captures and stores data about patients and makes the information available instantly and securely to authorized users. While an EHR does contain the medical and treatment histories of patients, an EHR system is built to go beyond standard clinical data collected in a provider’s provision of care location and can be inclusive of a broader view of a patient’s care. EHRs are a vital part of health IT and can:

	* Contain a patient’s medical history, diagnoses, medications, treatment plans, immunization dates, allergies, radiology images, and laboratory and test results
	* Allow access to evidence-based tools that providers can use to make decisions about a patient’s care
	* Automate and streamline provider workflow

	A **FHIR Enabled EHR** exposes FHIR APIs for other systems to interact with the EHR and exchange data. FHIR APIs provide well defined mechanisms to read and write data. The 	FHIR APIs are protected by an Authorization Server which authenticates and authorizes users or systems prior to accessing the data.


2. __Data Mart__: Data Mart represents a system that will hold data that is to be accessed by researchers. Typically researchers do not access the EHR or operational data stores directly as they are used for clinical operations. In order to facilitate access to researchers healthcare organizations populate data stores that can be accessed by researchers. These data stores are called Data Marts in our abstract model. Typically these data stores are present within the healthcare organization, but could also be hosted externally to the healthcare organization. The data models that are used to store the data may depend on the types of research studies. The following are the commonly used data models for research.

	* PCORnet CDM
	* i2b2
	* OMOP
	* Sentinel
	* FHIR is one of the data models that could also be used for research in the future as it gets adopted in the research settings. 

3. __Backend Service App__:  For research use cases, the Backend Service App represents a system that resides within the clinical care setting and interacts with the EHRs, Data Marts and the systems used by the researchers. The system will use specific Knowledge Artifact resources to identify when data has to be extracted, when data has to be queried and how results have to be returned. The term “Backend Service” is used to refer to the fact that the system does not require user intervention to perform reporting. The term “App” is used to indicate that it is similar to SMART on FHIR App which can be distributed to clinical care via EHR vendor-specified processes. The EHR vendor-specified processes enable the Backend Service App to use the EHR's FHIR APIs to access data. The healthcare organization is responsible for choosing/developing and maintaining the Backend Service App within the organization.

4. __Trust Service Provider__:  Trust Service Provider affords capabilities that can be used to translate data between different models, map terminologies between different data models, pseudonymize, anonymize, de-identify, hash, or re-link data that is submitted to public health and/or research organizations. These capabilities are called Trust Services. Trust Services are used, when appropriate, by the Backend Service App.  

5. __Research Organization__: An organization that can access the data from clinical care or data repositories for research purposes with appropriate data use agreements, authorities, and policies.
