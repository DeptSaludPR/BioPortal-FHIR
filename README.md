# BioPortal

## Test Results
BioPortal currently collects the following fields for the test results:
| Field Name | Data Type |
|--|--|
| Order ID | string |
| Patient | [Patient](#Patient) |
| Laboratory | [Laboratory](#Laboratory) |
| Ordered By | [Entity](#Entity)|
| Processed By | [Entity](#Entity)|

### Patient
<details>
  <summary>Click to expand!</summary>
  
| Field Name | Data Type |
|--|--|
| Patient ID | string|
| SSN | string |
| First Name | string |
| Middle Name | string |
| Last Name | string |
| Second Last Name | string |
| Birth Date | Date |
| Sex | string |
| Is Pregnant | boolean |
| Pregnant Gestation Weeks | int |
| Pregnant Estimated Birth Date| Date |
| Pregnant Obstetrician | string |
| Is First Responder | boolean |
| Is Homeless | boolean |
| Is In Nursing Home | boolean |
| Address | Address |
| Country | Iso2 Code |
| Phone Number 1 | string |
| Phone Number 2 | string |
| Work Name | string |
| Work Address | Address |
| Marital Status | string |
| Primary Language | string |
| Deceased | boolean |
| Primary Practitioner| [Entity](#Entity)|
</details>

### Laboratory
<details>
  <summary>Click to expand!</summary>
  
| Field Name | Data Type |
|--|--|
| Has Symptoms | boolean |
| Onset Date | Date |
| Symptoms | string[] - Symptoms |
| Sample Collected Date | DateTime UTC|
| Result Reported Date | DateTime UTC|
| Sample Type | string |
| Test LOINC | string |
| Result SNOMED | string |
| Result File LOINC | string |
</details>

### Entity
<details>
  <summary>Click to expand!</summary>

| Field Name | Data Type |
|--|--|
| Entity ID | UUID|
| Entity External ID| string |
| Name | string |
| Address | Address |
| Phone Number | string |
  </details>


### Address
<details>
  <summary>Click to expand!</summary>

| Field Name | Data Type |
|--|--|
| AddressLine1 | string |
| AddressLine2 | string |
| City | string |
| State | string - 2 letter Code |
| Country | Iso2 Code |
</details>




## Send Test Results via HL7 FHIR
BioPortal provides a standarized way to receive test result information utilizing [HL7 FHIR](https://www.hl7.org/fhir/) .

### HL7 FHIR Resources
The resources utilized for sending test results are the following:
+ [Bundle](https://www.hl7.org/fhir/bundle.html)
+ [Diagnostic Report](https://www.hl7.org/fhir/diagnosticreport.html)
+ [Patient](https://www.hl7.org/fhir/patient.html)
+ [Organization](https://www.hl7.org/fhir/organization.html)
+ [Practitioner](https://www.hl7.org/fhir/practitioner.html)
+ [Condition](https://www.hl7.org/fhir/condition.html)
+ [Specimen](https://www.hl7.org/fhir/specimen.html)
+ [Observation](https://www.hl7.org/fhir/observation.html)

### HL7 FHIR Bundle
One laboratory test result will consist of an [HL7 FHIR Bundle](https://www.hl7.org/fhir/bundle.html) with multiple resources. 

Bundle will include:
+ Diagnostic Report
+ Patient
+ Pregnancy Observation
+ Homeless Condition
+ First Responder Condition
+ Nursing Home Condition
+ Symptoms Conditions (0 to X)
+ Specimen
+ Result Observation (0 to X)

Example:

```jsonc
{
  "resourceType": "Bundle",
  "id": "e65fbe84-0675-40c0-bb1c-f6f8a700c948",
  "type": "collection",
  "entry": [
    {
      "fullUrl": "https://example.com/base/DiagnosticReport/e65fbe84-0675-40c0-bb1c-f6f8a700c948",
      "resource": {
        "resourceType": "DiagnosticReport",
        "id": "e65fbe84-0675-40c0-bb1c-f6f8a700c948",
        // etc - follow spec for DiagnosticReport
      }
	},
    {
      "fullUrl": "https://example.com/base/Patient/f9a1c82b-f6ef-45a9-bb71-b7aa2683635b",
      "resource": {
        "resourceType": "Patient",
        "id": "f9a1c82b-f6ef-45a9-bb71-b7aa2683635b",
        // etc - follow spec for Patient
      }
	},
    {
      "fullUrl": "https://example.com/base/Observation/r1",
      "resource": {
        "resourceType": "Observation",
        "id": "r1",
        // etc - follow spec for Observation
      }
	},
	// etc
  ]
}
```

### HL7 FHIR Extensions

#### Diagnostic Report
Ordered By
```json
{
  "url": "https://bioportal.salud.gov.pr/fhir/r4/StructureDefinition/ordered-by",
  "valueReference": "Reference(Organization)"
}
```

#### Patient
Mother’s Maiden Name 
```json
{
  "url": "http://hl7.org/fhir/StructureDefinition/patient-mothersMaidenName",
  "valueString": "Miranda"
}
```

### Send Message

#### Endpoint

| Purpose | VERB | URL |
|--|--|--|
| Send Covid-19 Test Results | POST | https://biolink.salud.gov.pr/fhir/r4|

#### Authentication

Use the following HTTP headers
| Header | Value |
|--|--|
| Authorization | ApiKey **KEY**|
