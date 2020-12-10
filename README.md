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
  "meta": {
    "tag": [
      {
        "system": "http://terminology.hl7.org/CodeSystem/v3-ObservationValue",
        "code": "SUBSETTED",
        "display": "Resource encoded in summary mode"
      }
    ]
  },
  "type": "collection",
  "entry": [
    {
      "resource": {
        "resourceType": "DiagnosticReport",
        "id": "gn-174580",
        "meta": {
          "tag": [
            {
              "system": "http://terminology.hl7.org/CodeSystem/v3-ObservationValue",
              "code": "SUBSETTED",
              "display": "Resource encoded in summary mode"
            }
          ]
        },
        "extension": [
          {
            "url": "https://bioportal.salud.gov.pr/fhir/r4/StructureDefinition/ordered-by",
            "valueReference": {
              "reference": "Organization/urn:uuid:880bf5c8-1af9-45a3-ac6c-3ba96276355d"
            }
          }
        ],
        "identifier": [
          {
            "value": "gn-174580"
          }
        ],
        "basedOn": [
          {
            "reference": "ServiceRequest/gn-174580"
          }
        ],
        "status": "final",
        "code": {
          "coding": [
            {
              "system": "LN",
              "code": "94500-6"
            }
          ]
        },
        "subject": {
          "reference": "Patient/100741232"
        },
        "issued": "2020-09-07T02:01:14.480-04:00",
        "performer": [
          {
            "reference": "Organization/urn:uuid:880bf5c8-1af9-45a3-ac6c-3ba96276355d",
            "display": "Laboratorio Clinico Toledo"
          }
        ],
        "resultsInterpreter": [
          {
            "reference": "Practitioner/1124085790"
          }
        ],
        "specimen": [
          {
            "reference": "Specimen/580128"
          }
        ],
        "result": [
          {
            "reference": "Observation/urn:uuid:2d6711f6-c5ba-494c-9e4b-968c8f51ce83"
          }
        ]
      },
      "request": {
        "method": "POST",
        "url": "DiagnosticReport"
      }
    },
    {
      "resource": {
        "resourceType": "ServiceRequest",
        "id": "gn-174580",
        "meta": {
          "tag": [
            {
              "system": "http://terminology.hl7.org/CodeSystem/v3-ObservationValue",
              "code": "SUBSETTED",
              "display": "Resource encoded in summary mode"
            }
          ]
        },
        "identifier": [
          {
            "value": "gn-174580"
          }
        ],
        "status": "completed",
        "code": {
          "coding": [
            {
              "system": "http://loinc.org",
              "code": "94500-6"
            }
          ]
        },
        "subject": {
          "reference": "Patient/100741232"
        },
        "requester": {
          "reference": "Practitioner/1124085790"
        },
        "performer": [
          {
            "reference": "Organization/urn:uuid:880bf5c8-1af9-45a3-ac6c-3ba96276355d",
            "display": "Laboratorio Clinico Toledo"
          }
        ],
        "reasonCode": [
          {
            "coding": [
              {
                "system": "ICD-10-CM",
                "code": "U07.2",
                "display": "COVID-19, virus not identified"
              }
            ]
          }
        ]
      },
      "request": {
        "method": "POST",
        "url": "ServiceRequest"
      }
    },
    {
      "fullUrl": "Patient/100741232",
      "resource": {
        "resourceType": "Patient",
        "id": "100741232",
        "meta": {
          "tag": [
            {
              "system": "http://terminology.hl7.org/CodeSystem/v3-ObservationValue",
              "code": "SUBSETTED",
              "display": "Resource encoded in summary mode"
            }
          ]
        },
        "identifier": [
          {
            "system": "urn:mrns",
            "value": "100741232"
          }
        ],
        "name": [
          {
            "family": "Nieves Miranda",
            "given": ["Elvis"]
          }
        ],
        "telecom": [
          {
            "system": "phone",
            "value": "7871234567",
            "use": "home"
          }
        ],
        "gender": "male",
        "birthDate": "1990-01-01",
        "address": [
          {
            "line": ["Calle Test"],
            "city": "San Juan",
            "state": "Puerto Rico",
            "postalCode": "00921"
          }
        ]
      },
      "request": {
        "method": "POST",
        "url": "Patient",
        "ifNoneExist": "identifier=12345"
      }
    },
    {
      "resource": {
        "resourceType": "Observation",
        "id": "urn:uuid:2d6711f6-c5ba-494c-9e4b-968c8f51ce83",
        "meta": {
          "tag": [
            {
              "system": "http://terminology.hl7.org/CodeSystem/v3-ObservationValue",
              "code": "SUBSETTED",
              "display": "Resource encoded in summary mode"
            }
          ]
        },
        "status": "final",
        "category": [
          {
            "coding": [
              {
                "system": "http://terminology.hl7.org/CodeSystem/observation-category",
                "code": "laboratory",
                "display": "laboratory"
              }
            ]
          }
        ],
        "code": {
          "coding": [
            {
              "system": "http://loinc.org",
              "code": "94500-6",
              "display": "SARS-CoV-2 (NAA) by PCR"
            }
          ]
        },
        "subject": {
          "reference": "Patient/100741232"
        },
        "issued": "2020-09-07T02:01:14.451-04:00",
        "performer": [
          {
            "reference": "Organization/urn:uuid:880bf5c8-1af9-45a3-ac6c-3ba96276355d",
            "display": "Laboratorio Clinico Test"
          }
        ],
        "valueCodeableConcept": {
          "coding": [
            {
              "system": "UNDEFINED",
              "code": "NOT_DETECTED",
              "display": "NOT_DETECTED"
            }
          ]
        }
      },
      "request": {
        "method": "POST",
        "url": "Observation"
      }
    },
    {
      "resource": {
        "resourceType": "Specimen",
        "id": "580128",
        "meta": {
          "tag": [
            {
              "system": "http://terminology.hl7.org/CodeSystem/v3-ObservationValue",
              "code": "SUBSETTED",
              "display": "Resource encoded in summary mode"
            }
          ]
        },
        "identifier": [
          {
            "value": "580128"
          }
        ],
        "type": {
          "text": "One swab received for PCR testing."
        },
        "receivedTime": "2020-07-28T17:16:34.77"
      },
      "request": {
        "method": "POST",
        "url": "Specimen"
      }
    },
    {
      "resource": {
        "resourceType": "Practitioner",
        "id": "1124085790",
        "meta": {
          "tag": [
            {
              "system": "http://terminology.hl7.org/CodeSystem/v3-ObservationValue",
              "code": "SUBSETTED",
              "display": "Resource encoded in summary mode"
            }
          ]
        },
        "identifier": [
          {
            "value": "1124085790"
          }
        ],
        "name": [
          {
            "family": "Test, MD.",
            "given": ["Juan"]
          }
        ],
        "telecom": [
          {
            "system": "phone"
          }
        ],
        "address": [
          {
            "line": [
              "Edif. Test",
              "Test Line 2"
            ],
            "state": "Puerto Rico",
            "country": "United States"
          }
        ]
      },
      "request": {
        "method": "POST",
        "url": "Practitioner"
      }
    },
    {
      "resource": {
        "resourceType": "Organization",
        "id": "urn:uuid:880bf5c8-1af9-45a3-ac6c-3ba96276355d",
        "meta": {
          "tag": [
            {
              "system": "http://terminology.hl7.org/CodeSystem/v3-ObservationValue",
              "code": "SUBSETTED",
              "display": "Resource encoded in summary mode"
            }
          ]
        },
        "name": "Laboratorio Clinico Test",
        "address": [
          {
            "line": ["Calle Test"],
            "city": "San Juan",
            "state": "Puerto Rico",
            "postalCode": "00921"
          }
        ]
      },
      "request": {
        "method": "POST",
        "url": "Organization"
      }
    }
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
