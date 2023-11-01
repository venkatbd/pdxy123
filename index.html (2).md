--- 

title: 

language_tabs:
  - shell: Shell

toc_footers: 
   - <a href='#'>Sign Up for a Developer Key</a> 
   - <a href='https://github.com/pdx'>Documentation Powered by jay/ven</a> 

includes: 


search: true 

--- 
# Overview  

Simple API for exchanging LDExBEM payloads. The API describes transmission of an LDExBEM payload in two parts: 1) Metadata describing the payload (unique ID, sender, receiver, payload type, and spec version) and 2) payload in the LDExBEM format (JSON or XML)

 
# Authentication 


<img src="/images/auth.png" alt="post" style="height: 400px; width:400apx;"/>

# Version 

version 1.0

# Member Management 
![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /bem/...`

<aside class="success">This operation requires authentication</aside>

```shell
# You can also use wget
curl -X POST /bem/..
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'
```

> /bem Schema 
 

``` json
{
    "$schema": "http://json-schema.org/draft/2020-12/schema",
    "title": "LDExBEM",
    "description": "1.3.2022.12.31",    
    "anyOf": [
        {
            "type": "object",
            "properties": {
                "transmission": {"$ref": "#/$defs/Transmission"}
            }
        }
    ],
    "$defs": {
        "Carrier": {
            "type": "object",
            "required": [
                "carrierID",
                "carrierName"
            ],
            "properties": {
                "carrierID": {"type": "string"},
                "carrierName": {"type": "string"}
            }
        },
        "CarrierMasterAgreementNumber": {
            "type": "object",
            "required": [
                "carrierMasterAgreementNumberID",
                "masterAgreementNumber",
                "carrierID"
            ],
            "properties": {
                "carrierMasterAgreementNumberID": {"type": "string"},
                "masterAgreementNumber": {"type": "string"},
                "carrierID": {"type": "string"}
            }
        },
        "PostalAddress": {
            "type": "object",
            "required": [
                "firstLineAddress",
                "cityName",
                "postalCode"
            ],
            "properties": {
                "firstLineAddress": {"type": "string"},
                "secondLineAddress": {"type": "string"},
                "thirdLineAddress": {"type": "string"},
                "cityName": {"type": "string"},
                "stateProvinceCode": {"$ref": "#/$defs/StateProvince"},
                "postalCode": {"type": "string"},
                "countryCode": {"$ref": "#/$defs/Country"}
            }
        },
        "StructuredPersonName": {
            "type": "object",
            "properties": {
                "prefixCode": {"$ref": "#/$defs/Prefix"},
                "firstName": {"type": "string"},
                "middleName": {"type": "string"},
                "lastName": {"type": "string"},
                "suffixCode": {"$ref": "#/$defs/Suffix"}
            }
        },
        "EmploymentIncome": {
            "type": "object",
            "properties": {
                "incomeTypeCode": {"$ref": "#/$defs/IncomeType"},
                "incomeAmount": {"type": "number"},
                "incomeModeCode": {
                    "type": "string",
                    "enum": [
                        "Hourly",
                        "Annual",
                        "Monthly12PerYear",
                        "BiWeekly26PerYear",
                        "SemiMonthly24PerYear",
                        "SemiMonthly21PerYear",
                        "Weekly52PerYear",
                        "Weekly48PerYear",
                        "Quarterly",
                        "SemiAnnual",
                        "9thly",
                        "10thly"
                    ]
                },
                "incomeEffectiveDate": {
                    "type": "string",
                    "format": "date"
                }
            }
        },
        "EmploymentInformationUserDefinedCategory": {
            "type": "object",
            "required": ["categoryName"],
            "properties": {
                "categoryName": {"type": "string"},
                "categoryValueString": {"type": "string"},
                "categoryTypeCode": {"$ref": "#/$defs/CategoryType"}
            }
        },
        "EmploymentContact": {
            "type": "object",
            "required": [
                "contactTypeCode",
                "contactName"
            ],
            "properties": {
                "contactTypeCode": {"$ref": "#/$defs/ContactType"},
                "contactName": {"$ref": "#/$defs/StructuredPersonName"},
                "contactSocialSecurityNumber": {"type": "string"},
                "contactIdentifier": {"type": "string"},
                "contactEmail": {"type": "string"},
                "contactOrganizationName": {"type": "string"},
                "contactPhone": {"type": "string"}
            }
        },
        "WorkSchedule": {
            "type": "object",
            "required": ["workScheduleTypeCode"],
            "properties": {
                "workShiftCode": {"$ref": "#/$defs/WorkShift"},
                "workScheduleTypeCode": {"$ref": "#/$defs/WorkScheduleType"},
                "weeklyWorkedHoursQuantity": {"type": "number"},
                "sundayWorkedHoursQuantity": {"type": "number"},
                "mondayWorkedHoursQuantity": {"type": "number"},
                "tuesdayWorkedHoursQuantity": {"type": "number"},
                "wednesdayWorkedHoursQuantity": {"type": "number"},
                "thursdayWorkedHoursQuantity": {"type": "number"},
                "fridayWorkedHoursQuantity": {"type": "number"},
                "saturdayWorkedHoursQuantity": {"type": "number"}
            }
        },
        "EmploymentSchedule": {
            "type": "object",
            "properties": {
                "scheduledHoursQuantity": {"type": "number"},
                "scheduledHoursFrequencyCode": {"$ref": "#/$defs/WorkHoursFrequency"},
                "last12MonthsWorkHoursQuantity": {"type": "number"},
                "perWeekWorkedHoursQuantity": {"type": "number"},
                "workSchedule": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/WorkSchedule"},
                    "minItems": 0
                }
            }
        },
        "EmploymentInformation": {
            "type": "object",
            "required": [
                "originalHireDate",
                "employmentTypeCode",
                "employmentStatusCode"
            ],
            "properties": {
                "originalHireDate": {
                    "type": "string",
                    "format": "date"
                },
                "mostRecentHireDate": {
                    "type": "string",
                    "format": "date"
                },
                "adjustedServiceDate": {
                    "type": "string",
                    "format": "date"
                },
                "exemptCode": {"$ref": "#/$defs/Exempt"},
                "employmentTypeCode": {"$ref": "#/$defs/EmploymentType"},
                "employmentStatusCode": {"$ref": "#/$defs/EmploymentStatus"},
                "terminationDate": {
                    "type": "string",
                    "format": "date"
                },
                "terminationReasonCode": {"$ref": "#/$defs/TerminationReason"},
                "jobTitleText": {"type": "string"},
                "occupationText": {"type": "string"},
                "workHoursQuantity": {"type": "number"},
                "workHoursFrequencyCode": {"$ref": "#/$defs/WorkHoursFrequency"},
                "workLocationText": {"type": "string"},
                "payrollDeductionFrequencyQuantity": {"type": "integer"},
                "payrollFrequencyQuantity": {"type": "integer"},
                "supervisorFullName": {"type": "string"},
                "supervisorEmailAddress": {"type": "string"},
                "humanResourcePartnerEmailAddress": {"type": "string"},
                "unionIndicator": {"type": "boolean"},
                "unionLocalName": {"type": "string"},
                "unionLocalNumber": {"type": "string"},
                "unionMemberNumber": {"type": "string"},
                "employmentIncome": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/EmploymentIncome"},
                    "minItems": 0
                },
                "employmentInformationUserDefinedCategory": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/EmploymentInformationUserDefinedCategory"},
                    "minItems": 0
                },
                "rule5075Indicator": {"type": "boolean"},
                "workStateCode": {"$ref": "#/$defs/StateProvince"},
                "keyEmployeeIndicator": {"type": "boolean"},
                "employmentContact": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/EmploymentContact"},
                    "minItems": 0
                },
                "employmentSchedule": {"$ref": "#/$defs/EmploymentSchedule"}
            }
        },
        "EnrollmentInformation": {
            "type": "object",
            "required": ["enrollmentInformationID"],
            "properties": {
                "enrollmentInformationID": {"type": "string"},
                "enrollmentMethodCode": {"$ref": "#/$defs/EnrollmentMethod"},
                "employeeEnrollmentLocationAddress": {"$ref": "#/$defs/PostalAddress"},
                "enrollmentCityName": {"type": "string"},
                "enrollmentStateProvinceCode": {"$ref": "#/$defs/StateProvince"}
            }
        },
        "Event": {
            "type": "object",
            "required": [
                "eventID",
                "eventTypeCode",
                "eventTypeReasonCode",
                "eventDate"
            ],
            "properties": {
                "eventID": {"type": "string"},
                "eventTypeCode": {"$ref": "#/$defs/EventType"},
                "eventTypeReasonCode": {"$ref": "#/$defs/EventTypeReason"},
                "eventDate": {
                    "type": "string",
                    "format": "date"
                },
                "transactionDate": {
                    "type": "string",
                    "format": "date"
                },
                "enrollmentInformationID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                }
            }
        },
        "Dependent": {
            "type": "object",
            "required": [
                "dependentPartyID",
                "dependentRelationshipTypeCode",
                "dependentBirthDate"
            ],
            "properties": {
                "dependentPartyID": {"type": "string"},
                "dependentIndividualTaxpayerIdentificationNumber": {"type": "string"},
                "dependentSocialSecurityNumber": {"type": "string"},
                "dependentIdentifier": {"type": "string"},
                "dependentName": {"$ref": "#/$defs/StructuredPersonName"},
                "dependentRelationshipTypeCode": {"$ref": "#/$defs/DependentRelationshipType"},
                "dependentGenderCode": {"$ref": "#/$defs/Gender"},
                "dependentBirthDate": {
                    "type": "string",
                    "format": "date"
                },
                "dependentHomePhone": {"type": "string"},
                "dependentWorkPhone": {"type": "string"},
                "dependentMobilePhone": {"type": "string"},
                "dependentMailingAddress": {"$ref": "#/$defs/PostalAddress"},
                "dependentHomeEmail": {"type": "string"},
                "disabilityIndicator": {"type": "boolean"},
                "studentStatusCode": {"$ref": "#/$defs/StudentStatus"},
                "dependentTobaccoUseCode": {"$ref": "#/$defs/TobaccoUse"},
                "dependentEventID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                }
            }
        },
        "OtherParty": {
            "type": "object",
            "required": [
                "otherPartyID",
                "otherPartyRelationshipTypeCode",
                "otherPartyTypeCode"
            ],
            "properties": {
                "otherPartyID": {"type": "string"},
                "otherPartyIndividualTaxpayerIdentificationNumber": {"type": "string"},
                "otherPartyFederalEmployerIdentificationNumber": {"type": "string"},
                "otherPartySocialSecurityNumber": {"type": "string"},
                "otherPartyOrganizationName": {"type": "string"},
                "otherPartyPersonName": {"$ref": "#/$defs/StructuredPersonName"},
                "otherPartyRelationshipTypeCode": {"$ref": "#/$defs/BeneficiaryRelationshipType"},
                "otherPartyTypeCode": {"$ref": "#/$defs/PartyType"},
                "otherPartyGenderCode": {"$ref": "#/$defs/Gender"},
                "otherPartyBirthDate": {
                    "type": "string",
                    "format": "date"
                },
                "otherPartyHomePhone": {"type": "string"},
                "otherPartyWorkPhone": {"type": "string"},
                "otherPartyMobilePhone": {"type": "string"},
                "otherPartyMailingAddress": {"$ref": "#/$defs/PostalAddress"},
                "otherPartyHomeAddress": {"$ref": "#/$defs/PostalAddress"},
                "otherPartyHomeEmail": {"type": "string"},
                "otherPartyWorkEmail": {"type": "string"},
                "otherPartyEventID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                }
            }
        },
        "Beneficiary": {
            "type": "object",
            "required": [
                "beneficiaryPartyID",
                "beneficiaryPartyTypeCode",
                "beneficiaryPercent",
                "beneficiaryTypeCode"
            ],
            "properties": {
                "beneficiaryPartyID": {"type": "string"},
                "beneficiaryPartyTypeCode": {"$ref": "#/$defs/BeneficiaryPartyType"},
                "beneficiaryPercent": {"type": "number"},
                "beneficiaryTypeCode": {"$ref": "#/$defs/BeneficiaryType"}
            }
        },
        "BeneficiaryGroup": {
            "type": "object",
            "required": [
                "beneficiaryGroupID",
                "beneficiary"
            ],
            "properties": {
                "beneficiaryGroupID": {"type": "string"},
                "beneficiary": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Beneficiary"},
                    "minItems": 1
                }
            }
        },
        "CarrierProducerNumber": {
            "type": "object",
            "required": [
                "carrierProducerNumberID",
                "carrierID",
                "carrierProducerNumber"
            ],
            "properties": {
                "carrierProducerNumberID": {"type": "string"},
                "carrierID": {"type": "string"},
                "carrierProducerNumber": {"type": "string"}
            }
        },
        "ProducerLicense": {
            "type": "object",
            "required": [
                "producerLicenseID",
                "producerLicenseTypeCode",
                "producerLicenseStateProvinceCode",
                "producerLicenseNumber"
            ],
            "properties": {
                "producerLicenseID": {"type": "string"},
                "producerLicenseTypeCode": {"$ref": "#/$defs/ProducerLicenseType"},
                "producerLicenseStateProvinceCode": {"$ref": "#/$defs/StateProvince"},
                "producerLicenseNumber": {"type": "string"}
            }
        },
        "Producer": {
            "type": "object",
            "required": ["producerPartyID"],
            "properties": {
                "producerPartyID": {"type": "string"},
                "producerFirstName": {"type": "string"},
                "producerLastName": {"type": "string"},
                "carrierProducerNumber": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CarrierProducerNumber"},
                    "minItems": 0
                },
                "producerLicense": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/ProducerLicense"},
                    "minItems": 0
                }
            }
        },
        "Provider": {
            "type": "object",
            "properties": {
                "providerIdentificationNumberText": {"type": "string"},
                "providerName": {"type": "string"},
                "existingPatientIndicator": {"type": "boolean"}
            }
        },
        "CoverageInsured": {
            "type": "object",
            "required": [
                "insuredPartyID",
                "primaryInsuredIndicator",
                "insuredCoverageEffectiveDate"
            ],
            "properties": {
                "insuredPartyID": {"type": "string"},
                "primaryInsuredIndicator": {"type": "boolean"},
                "tobaccoUseIndicator": {"type": "boolean"},
                "insuredCoverageEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "insuredCoverageTerminationDate": {
                    "type": "string",
                    "format": "date"
                },
                "provider": {"$ref": "#/$defs/Provider"}
            }
        },
        "CoverageRider": {
            "type": "object",
            "required": [
                "coverageRiderID",
                "riderIdentifier"
            ],
            "properties": {
                "coverageRiderID": {"type": "string"},
                "riderIdentifier": {"type": "string"},
                "riderName": {"type": "string"},
                "coverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "benefitAmount": {"type": "number"},
                "riderUnitQuantity": {"type": "number"},
                "riderOptionIdentifier": {"type": "string"},
                "preTaxCode": {"$ref": "#/$defs/PreTax"},
                "employeePremiumContributionAmount": {"type": "number"},
                "employerPremiumContributionAmount": {"type": "number"},
                "riderEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "riderTerminationDate": {
                    "type": "string",
                    "format": "date"
                }
            }
        },
        "ElectedCoverage": {
            "type": "object",
            "properties": {
                "electedCoverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "electedBenefitCalculationMethodCode": {"$ref": "#/$defs/BenefitCalculationMethod"},
                "electedBenefitAmount": {"type": "number"},
                "electedBenefitFactor": {"type": "number"},
                "electedEmployeeContributionCode": {"$ref": "#/$defs/EmployeeContribution"},
                "electedEmployeePremiumContributionAmount": {"type": "number"},
                "electedEmployerPremiumContributionAmount": {"type": "number"},
                "electedTotalPlanPremiumAmount": {"type": "number"},
                "electedCoverageInsured": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageInsured"},
                    "minItems": 0
                },
                "electedCoverageRider": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageRider"},
                    "minItems": 0
                }
            }
        },
        "CoverageQuestionAnswer": {
            "type": "object",
            "required": [
                "coverageQuestionAnswerID",
                "questionIdentifier",
                "answerPartyID"
            ],
            "properties": {
                "coverageQuestionAnswerID": {"type": "string"},
                "questionIdentifier": {"type": "string"},
                "answerIndicator": {"type": "boolean"},
                "answerText": {"type": "string"},
                "answerPartyID": {"type": "string"}
            }
        },
        "CoverageDeduction": {
            "type": "object",
            "properties": {
                "deductionTypeCode": {
                    "type": "string",
                    "enum": [
                        "Medical",
                        "401(k)",
                        "StateIncomeTax",
                        "PAUnemploymentTax",
                        "LocalityTax",
                        "Dental",
                        "Life",
                        "PensionContribution1",
                        "PensionContribution2",
                        "StateIncomeTax2",
                        "StateIncomeTax3",
                        "FreeForm1",
                        "FreeForm2",
                        "FreeForm3",
                        "FreeForm4",
                        "FreeForm5",
                        "SupplementalLife",
                        "LegalServices",
                        "Health",
                        "Hospitalization",
                        "Disability",
                        "FreeForm",
                        "AccidentalDeath&Dismemberment",
                        "Vision",
                        "RemimbursementHealthcareExpenses",
                        "DependentCareAssistance",
                        "AdoptiveAssistance",
                        "Multiple401K",
                        "PensionContribution",
                        "NJUmeploymentTax",
                        "AKUnemploymentTax",
                        "StateLeaveContribution",
                        "UnemploymentTax"
                    ]
                },
                "deductionAmount": {"type": "number"},
                "deductionFrequencyQuantity": {"type": "integer"},
                "deductionTypeFreeFormValueString": {"type": "string"},
                "deductionTypeStateProvinceCode": {"$ref": "#/$defs/StateProvince"},
                "deductionTypePreTaxCode": {"$ref": "#/$defs/PreTax"}
            }
        },
        "CoverageCompensationSplit": {
            "type": "object",
            "required": [
                "carrierProducerNumberID",
                "compensationSplitPercent"
            ],
            "properties": {
                "carrierProducerNumberID": {"type": "string"},
                "compensationSplitPercent": {"type": "number"}
            }
        },
        "CoverageChange": {
            "type": "object",
            "properties": {
                "changeCoverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "changeBenefitCalculationMethodCode": {"$ref": "#/$defs/BenefitCalculationMethod"},
                "changeBenefitAmount": {"type": "number"},
                "changeBenefitFactor": {"type": "number"},
                "changeEmployeeContributionCode": {"$ref": "#/$defs/EmployeeContribution"},
                "changeEmployeePremiumContributionAmount": {"type": "number"},
                "changeEmployerPremiumContributionAmount": {"type": "number"},
                "changeTotalPlanPremiumAmount": {"type": "number"},
                "priorCoverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "priorBenefitCalculationMethodCode": {"$ref": "#/$defs/BenefitCalculationMethod"},
                "priorBenefitAmount": {"type": "number"},
                "priorBenefitFactor": {"type": "number"},
                "priorEmployeeContributionCode": {"$ref": "#/$defs/EmployeeContribution"},
                "priorEmployeePremiumContributionAmount": {"type": "number"},
                "priorEmployerPremiumContributionAmount": {"type": "number"},
                "priorTotalPlanPremiumAmount": {"type": "number"},
                "priorCoverageInsured": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageInsured"},
                    "minItems": 0
                },
                "priorCoverageRider": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageRider"},
                    "minItems": 0
                }
            }
        },
        "CoverageContribution": {
            "type": "object",
            "properties": {
                "contributionLimitTypeCode": {"$ref": "#/$defs/ContributionLimitType"},
                "planYearEmployeeContributionAmount": {"type": "number"},
                "planYearEmployerContributionAmount": {"type": "number"},
                "toDateEmployeeContributionAmount": {"type": "number"},
                "toDateEmployerContributionAmount": {"type": "number"},
                "contributionsRemainingQuantity": {"type": "integer"}
            }
        },
        "Coverage": {
            "type": "object",
            "required": [
                "coverageID",
                "coverageEffectiveDate",
                "carrierID",
                "coverageInsured"
            ],
            "properties": {
                "coverageID": {"type": "string"},
                "groupPolicyNumber": {"type": "string"},
                "individualPolicyNumber": {"type": "string"},
                "productTypeCode": {"$ref": "#/$defs/ProductType"},
                "benefitPlanIdentifier": {"type": "string"},
                "benefitClassIdentifier": {"type": "string"},
                "benefitSubClassIdentifier": {"type": "string"},
                "billGroupIdentifier": {"type": "string"},
                "billSubGroupIdentifier": {"type": "string"},
                "claimGroupIdentifier": {"type": "string"},
                "claimSubGroupIdentifier": {"type": "string"},
                "originalCoverageEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "coverageEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "coverageTerminationDate": {
                    "type": "string",
                    "format": "date"
                },
                "coverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "benefitCalculationMethodCode": {"$ref": "#/$defs/BenefitCalculationMethod"},
                "benefitAmount": {"type": "number"},
                "benefitFactor": {"type": "number"},
                "employeeContributionCode": {"$ref": "#/$defs/EmployeeContribution"},
                "employeePremiumContributionAmount": {"type": "number"},
                "employerPremiumContributionAmount": {"type": "number"},
                "totalPlanPremiumAmount": {"type": "number"},
                "benefitEarningsAmount": {"type": "number"},
                "benefitEarningsEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "benefitModeQuantity": {"type": "integer"},
                "paymentMethodCode": {"$ref": "#/$defs/PaymentMethod"},
                "premiumModeQuantity": {"type": "integer"},
                "preTaxCode": {"$ref": "#/$defs/PreTax"},
                "issueAgeQuantity": {"type": "integer"},
                "tobaccoUseIndicator": {"type": "boolean"},
                "takeOverCoverageIndicator": {"type": "boolean"},
                "enrollerProducerPartyID": {"type": "string"},
                "beneficiaryGroupID": {"type": "string"},
                "carrierID": {"type": "string"},
                "electedCoverage": {"$ref": "#/$defs/ElectedCoverage"},
                "coverageInsured": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageInsured"},
                    "minItems": 1
                },
                "coverageEventID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                },
                "coverageQuestionAnswer": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageQuestionAnswer"},
                    "minItems": 0
                },
                "coverageDeduction": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageDeduction"},
                    "minItems": 0
                },
                "coverageRider": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageRider"},
                    "minItems": 0
                },
                "coverageCommissionSplit": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageCompensationSplit"},
                    "minItems": 0
                },
                "signatureDate": {
                    "type": "string",
                    "format": "date"
                },
                "coverageTransactionDate": {
                    "type": "string",
                    "format": "date"
                },
                "cobraIndicator": {"type": "boolean"},
                "enrollmentInformationID": {"type": "string"},
                "coverageChange": {"$ref": "#/$defs/CoverageChange"},
                "coverageContribution": {"$ref": "#/$defs/CoverageContribution"},
                "cobraEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "cobraPaidThroughDate": {
                    "type": "string",
                    "format": "date"
                }
            }
        },
        "FormSignature": {
            "type": "object",
            "required": [
                "formSignatureID",
                "formSignatureDate",
                "formSignatureDataTypeCode"
            ],
            "properties": {
                "formSignatureID": {"type": "string"},
                "formSignatureDate": {
                    "type": "string",
                    "format": "date"
                },
                "formSignatureDataTypeCode": {"$ref": "#/$defs/FormSignatureDataType"},
                "formSignatureData": {
                    "type": "array",
                    "items": {"type": "integer"}
                },
                "signedByEmployeePartyID": {"type": "string"},
                "signedByDependentPartyID": {"type": "string"},
                "signedByProducerPartyID": {"type": "string"}
            }
        },
        "Form": {
            "type": "object",
            "required": [
                "formID",
                "formName",
                "formCreationDate",
                "formCompletionDate",
                "formTypeCode",
                "formDataTypeCode",
                "formCoverageID"
            ],
            "properties": {
                "formID": {"type": "string"},
                "formName": {"type": "string"},
                "formNumber": {"type": "string"},
                "formCreationDate": {
                    "type": "string",
                    "format": "date"
                },
                "formCompletionDate": {
                    "type": "string",
                    "format": "date"
                },
                "formTypeCode": {"$ref": "#/$defs/FormType"},
                "formDataTypeCode": {"$ref": "#/$defs/FormDataType"},
                "formData": {
                    "type": "array",
                    "items": {"type": "integer"}
                },
                "formDataGUID": {"type": "string"},
                "formSignature": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/FormSignature"},
                    "minItems": 0
                },
                "formCoverageID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 1
                }
            }
        },
        "Employee": {
            "type": "object",
            "required": [
                "employeePartyID",
                "employeeName",
                "employeeGenderCode",
                "employeeBirthDate",
                "employeeMailingAddress"
            ],
            "properties": {
                "employeePartyID": {"type": "string"},
                "employeeIndividualTaxpayerIdentificationNumber": {"type": "string"},
                "employeeSocialSecurityNumber": {"type": "string"},
                "employeeIdentifier": {"type": "string"},
                "employeeName": {"$ref": "#/$defs/StructuredPersonName"},
                "employeeGenderCode": {"$ref": "#/$defs/Gender"},
                "employeeBirthDate": {
                    "type": "string",
                    "format": "date"
                },
                "maritalStatusCode": {"$ref": "#/$defs/MaritalStatus"},
                "employeeTobaccoUseCode": {"$ref": "#/$defs/TobaccoUse"},
                "employeeHomePhone": {"type": "string"},
                "employeeWorkPhone": {"type": "string"},
                "employeeMobilePhone": {"type": "string"},
                "employeeMailingAddress": {"$ref": "#/$defs/PostalAddress"},
                "employeeHomeAddress": {"$ref": "#/$defs/PostalAddress"},
                "employeeWorkAddress": {"$ref": "#/$defs/PostalAddress"},
                "employeeEmail": {"type": "string"},
                "employeeAlternateEmail": {"type": "string"},
                "employmentInformation": {"$ref": "#/$defs/EmploymentInformation"},
                "enrollmentInformation": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/EnrollmentInformation"},
                    "minItems": 0
                },
                "event": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Event"},
                    "minItems": 0
                },
                "employeeEventID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                },
                "dependent": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Dependent"},
                    "minItems": 0
                },
                "otherParty": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/OtherParty"},
                    "minItems": 0
                },
                "beneficiaryGroup": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/BeneficiaryGroup"},
                    "minItems": 0
                },
                "producer": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Producer"},
                    "minItems": 0
                },
                "coverage": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Coverage"},
                    "minItems": 0
                },
                "employeeForm": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Form"},
                    "minItems": 0
                },
                "disabilityIndicator": {"type": "boolean"}
            }
        },
        "Employer": {
            "type": "object",
            "required": [
                "employerPartyID",
                "carrierMasterAgreementNumber",
                "employerName",
                "employee"
            ],
            "properties": {
                "employerPartyID": {"type": "string"},
                "federalEmployerIdentificationNumber": {"type": "string"},
                "carrierMasterAgreementNumber": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CarrierMasterAgreementNumber"},
                    "minItems": 1
                },
                "employerName": {"type": "string"},
                "employerAddress": {"$ref": "#/$defs/PostalAddress"},
                "employee": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Employee"},
                    "minItems": 1
                }
            }
        },
        "Audit": {
            "type": "object",
            "properties": {
                "auditID": {"type": "string"},
                "carrierRecordQuantity": {"type": "integer"},
                "employerRecordQuantity": {"type": "integer"},
                "employeeRecordQuantity": {"type": "integer"},
                "dependentRecordQuantity": {"type": "integer"},
                "otherPartyRecordQuantity": {"type": "integer"},
                "beneficiaryGroupRecordQuantity": {"type": "integer"},
                "eventRecordQuantity": {"type": "integer"},
                "coverageRecordQuantity": {"type": "integer"},
                "coverageRiderRecordQuantity": {"type": "integer"},
                "employeeFormRecordQuantity": {"type": "integer"}
            }
        },
        "Transmission": {
            "type": "object",
            "required": [
                "transmissionGUID",
                "senderName",
                "senderPlatformName",
                "receiverName",
                "creationDateTime",
                "testProductionCode",
                "transmissionTypeCode",
                "schemaVersionIdentifier",
                "carrier",
                "employer"
            ],
            "properties": {
                "transmissionGUID": {"type": "string"},
                "senderName": {"type": "string"},
                "senderPlatformName": {"type": "string"},
                "receiverName": {"type": "string"},
                "creationDateTime": {
                    "type": "string",
                    "format": "date-time"
                },
                "testProductionCode": {"$ref": "#/$defs/TestProduction"},
                "transmissionTypeCode": {"$ref": "#/$defs/TransmissionType"},
                "schemaVersionIdentifier": {"type": "string"},
                "carrier": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Carrier"},
                    "minItems": 1
                },
                "employer": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Employer"},
                    "minItems": 1
                },
                "audit": {"$ref": "#/$defs/Audit"}
            }
        },
        "TestProduction": {
            "type": "string",
            "enum": [
                "Production",
                "Test"
            ]
        },
        "TransmissionType": {
            "type": "string",
            "enum": [
                "ActivesOnly",
                "ChangesOnly",
                "FullFile"
            ]
        },
        "StateProvince": {
            "type": "string",
            "enum": [
                "AL",
                "AK",
                "AZ",
                "AR",
                "CA",
                "CO",
                "CT",
                "DE",
                "FL",
                "GA",
                "HI",
                "ID",
                "IL",
                "IN",
                "IA",
                "KS",
                "KY",
                "LA",
                "ME",
                "MD",
                "MA",
                "MI",
                "MN",
                "MS",
                "MO",
                "MT",
                "NE",
                "NV",
                "NH",
                "NJ",
                "NM",
                "NY",
                "NC",
                "ND",
                "OH",
                "OK",
                "OR",
                "PA",
                "RI",
                "SC",
                "SD",
                "TN",
                "TX",
                "UT",
                "VT",
                "VA",
                "WA",
                "WV",
                "WI",
                "WY",
                "DC",
                "AS",
                "GU",
                "MP",
                "PR",
                "UM",
                "VI",
                "AB",
                "BC",
                "MB",
                "NB",
                "NL",
                "NS",
                "ON",
                "PE",
                "QC",
                "SK",
                "NT",
                "NU",
                "YT",
                "AG",
                "BS",
                "CM",
                "CS",
                "CH",
                "CL",
                "DF",
                "DG",
                "GT",
                "GR",
                "HG",
                "JA",
                "EM",
                "NA",
                "OA",
                "PU",
                "QT",
                "QR",
                "SL",
                "SI",
                "SO",
                "TB",
                "TM",
                "TL",
                "VE",
                "YU",
                "ZA",
                "AA",
                "AE",
                "AP"
            ]
        },
        "Country": {
            "type": "string",
            "enum": [
                "AF",
                "AL",
                "DZ",
                "AD",
                "AO",
                "AG",
                "AR",
                "AM",
                "AU",
                "AT",
                "AZ",
                "BS",
                "BH",
                "BD",
                "BB",
                "BY",
                "BE",
                "BZ",
                "BJ",
                "BT",
                "BO",
                "BA",
                "BW",
                "BR",
                "BN",
                "BG",
                "BF",
                "BI",
                "CV",
                "KH",
                "CM",
                "CA",
                "CF",
                "TD",
                "CL",
                "CN",
                "CO",
                "KM",
                "CG",
                "CD",
                "CR",
                "CI",
                "HR",
                "CU",
                "CY",
                "CZ",
                "DK",
                "DJ",
                "DM",
                "DO",
                "EC",
                "EG",
                "SV",
                "GQ",
                "ER",
                "EE",
                "SZ",
                "ET",
                "FJ",
                "FI",
                "FR",
                "GA",
                "GM",
                "GE",
                "DE",
                "GH",
                "GR",
                "GD",
                "GT",
                "GN",
                "GW",
                "GY",
                "HT",
                "VA",
                "HN",
                "HU",
                "IS",
                "IN",
                "ID",
                "IR",
                "IQ",
                "IE",
                "IL",
                "IT",
                "JM",
                "JP",
                "JO",
                "KZ",
                "KE",
                "KI",
                "KP",
                "KR",
                "KW",
                "KG",
                "LA",
                "LV",
                "LB",
                "LS",
                "LR",
                "LY",
                "LI",
                "LT",
                "LU",
                "MG",
                "MW",
                "MY",
                "MV",
                "ML",
                "MT",
                "MH",
                "MR",
                "MU",
                "MX",
                "FM",
                "MD",
                "MC",
                "MN",
                "ME",
                "MA",
                "MZ",
                "MM",
                "NA",
                "NR",
                "NP",
                "NL",
                "NZ",
                "NI",
                "NE",
                "NG",
                "MK",
                "NO",
                "OM",
                "PK",
                "PW",
                "PA",
                "PG",
                "PY",
                "PE",
                "PH",
                "PL",
                "PT",
                "QA",
                "RO",
                "RU",
                "RW",
                "KN",
                "LC",
                "VC",
                "WS",
                "SM",
                "ST",
                "SA",
                "SN",
                "RS",
                "SC",
                "SL",
                "SG",
                "SK",
                "SI",
                "SB",
                "SO",
                "ZA",
                "SS",
                "ES",
                "LK",
                "SD",
                "SR",
                "SE",
                "CH",
                "SY",
                "TJ",
                "TZ",
                "TH",
                "TL",
                "TG",
                "TO",
                "TT",
                "TN",
                "TR",
                "TM",
                "TV",
                "UG",
                "UA",
                "AE",
                "GB",
                "US",
                "USA",
                "UY",
                "UZ",
                "VU",
                "VE",
                "VN",
                "YE",
                "ZM",
                "ZW",
                "OTH"
            ]
        },
        "Prefix": {
            "type": "string",
            "enum": [
                "Dr",
                "Mr",
                "Mrs",
                "Ms",
                "Rev"
            ]
        },
        "Suffix": {
            "type": "string",
            "enum": [
                "Jr",
                "Sr",
                "III",
                "II",
                "IV",
                "V",
                "VI",
                "MD",
                "DDS",
                "PHD",
                "Other"
            ]
        },
        "Gender": {
            "type": "string",
            "enum": [
                "Female",
                "Male",
                "NonBinary",
                "NotSpecified"
            ]
        },
        "MaritalStatus": {
            "type": "string",
            "enum": [
                "Divorced",
                "DomesticPartner",
                "LegallySeparated",
                "Married",
                "Single",
                "Widowed",
                "Unknown"
            ]
        },
        "TobaccoUse": {
            "type": "string",
            "enum": [
                "N",
                "Y"
            ]
        },
        "Exempt": {
            "type": "string",
            "enum": [
                "Exempt",
                "Nonexempt"
            ]
        },
        "EmploymentType": {
            "type": "string",
            "enum": [
                "Contract",
                "FullTime",
                "PartTime",
                "PerDiem",
                "Seasonal"
            ]
        },
        "EmploymentStatus": {
            "type": "string",
            "enum": [
                "Active",
                "LeaveOfAbsencePaid",
                "LeaveOfAbsenceUnpaid",
                "MilitaryLeave",
                "OnDI",
                "Retiree",
                "Terminated"
            ]
        },
        "TerminationReason": {
            "type": "string",
            "enum": [
                "ClientTermination-NotCOBRAEligible",
                "ContractEnded-NotCOBRAEligible",
                "Death",
                "DirectHire-NotCOBRAEligible",
                "Layoff",
                "LeaveOfAbsence",
                "LongTermDisability",
                "Medicare/Termination",
                "MilitaryLeave",
                "Retirement",
                "Seasonal-NotCOBRAEligible",
                "Termination",
                "Termination-Declined",
                "Termination-Dissatisfaction",
                "Termination-EligibleForRehire",
                "Terminated-GrossMisconduct",
                "Termination-IneligibleForRehire",
                "Termination-Invol.Separation",
                "Terminated- JobAbandonment",
                "Termination-Mutual",
                "Terminated-NoCall/NoShow",
                "Termination-NotCOBRAEligible",
                "Termination-Other Employment",
                "Termination-Performance",
                "Termination-PositionEliminated",
                "Termination-Relocation",
                "Termination-SeveranceOption",
                "Termination-Voluntary",
                "Other"
            ]
        },
        "WorkHoursFrequency": {
            "type": "string",
            "enum": [
                "Annual",
                "Monthly",
                "Weekly"
            ]
        },
        "IncomeType": {
            "type": "string",
            "enum": [
                "AdditionalCompensation",
                "BenefitSalary",
                "Bonus",
                "Commission",
                "DifferentialPay",
                "FrozenPay",
                "Salary",
                "TotalCompensation"
            ]
        },
        "CategoryType": {
            "type": "string",
            "enum": [
                "GroupStructure",
                "Reporting"
            ]
        },
        "ContactType": {
            "type": "string",
            "enum": [
                "AdditionalReportingContact",
                "BenefitsCoordinator",
                "ReportingManager",
                "UnionContact"
            ]
        },
        "WorkShift": {
            "type": "string",
            "enum": [
                "First",
                "Second",
                "Third",
                "Fixed",
                "On-Call",
                "Rotate",
                "Split"
            ]
        },
        "WorkScheduleType": {
            "type": "string",
            "enum": [
                "Weekly",
                "Holiday",
                "Overtime"
            ]
        },
        "EnrollmentMethod": {
            "type": "string",
            "enum": [
                "CallCenter",
                "HumanResources",
                "InternetOrSelfService",
                "Mobile",
                "OneOnOne",
                "Paper"
            ]
        },
        "EventType": {
            "type": "string",
            "enum": [
                "Coverage",
                "Demographic",
                "Employment"
            ]
        },
        "EventTypeReason": {
            "type": "string",
            "enum": [
                "AdministrativeCorrection",
                "Cancellation",
                "ChildNoLongerEligible",
                "ClientTermination",
                "COBRAActivation",
                "COBRAEndCoverage",
                "InitialEnrollment",
                "LateEntrant",
                "LossOfSpousesBenefitEligibility",
                "New",
                "NewHireEnrollment",
                "NewRetireeEnrollment",
                "NonPaymentOfContributions",
                "OpenEnrollment",
                "QualifyingLifeEventEnrollment",
                "Reenrollment",
                "Reinstatement",
                "RetirementEndCoverage",
                "Rollover",
                "CoverageChange",
                "VoluntaryCancel",
                "BirthAdoption",
                "DependentDeath",
                "DependentRegainsEligibility",
                "Death",
                "DemographicChange",
                "DivorceLegalSeparation",
                "Marriage",
                "TobaccoChange",
                "ZipLocationChange",
                "BenefitClassChange",
                "CommencePaidFMLALeave",
                "CommencePaidNonFMLALeave",
                "LossOfBenefitStatus",
                "Rehire",
                "Retirement",
                "ReturnFromFMLALeave",
                "IncreaseInCoverage",
                "ReductionInCoverage",
                "Termination",
                "LongTermDisability",
                "NewHire",
                "Unknown",
                "ShortTermDisability",
                "CommenceUnpaidFMLALeave",
                "GainOfBenefitStatus",
                "ReturnFromNonFMLALeave",
                "AgeLimitAttained",
                "AgeRelatedReductionInCoverage",
                "BeneficiaryChange"
            ]
        },
        "DependentRelationshipType": {
            "type": "string",
            "enum": [
                "AdultDependent",
                "Child",
                "CollateralDependent",
                "CustodialGrandchild",
                "FormerSpouseOrPartner",
                "Grandchild",
                "Partner",
                "Spouse",
                "Stepchild",
                "Other"
            ]
        },
        "StudentStatus": {
            "type": "string",
            "enum": [
                "FullTime",
                "PartTime",
                "NonStudent"
            ]
        },
        "BeneficiaryRelationshipType": {
            "type": "string",
            "enum": [
                "Child",
                "Spouse",
                "Sibling",
                "Parent",
                "Grandparent",
                "Grandchild",
                "Fiance",
                "DomesticPartner",
                "Friend",
                "Other",
                "Trust",
                "Husband",
                "Wife",
                "Sister",
                "Brother",
                "Son",
                "Daughter",
                "Uncle",
                "Aunt",
                "MotherInLaw",
                "FatherInLaw",
                "CivilUnionPartner",
                "Father",
                "Mother",
                "Estate",
                "Charity",
                "Cousin",
                "OtherRelative",
                "Self",
                "ExSpouse",
                "Employer",
                "Girlfriend",
                "Boyfriend",
                "Stepdaughter",
                "Stepson",
                "Corporation"
            ]
        },
        "PartyType": {
            "type": "string",
            "enum": [
                "Charity",
                "Estate",
                "Person",
                "Trust",
                "Other"
            ]
        },
        "BeneficiaryPartyType": {
            "type": "string",
            "enum": [
                "Dependent",
                "Employee",
                "OtherParty"
            ]
        },
        "BeneficiaryType": {
            "type": "string",
            "enum": [
                "Primary",
                "Secondary"
            ]
        },
        "ProducerLicenseType": {
            "type": "string",
            "enum": [
                "LifeHealth",
                "Health"
            ]
        },
        "ProductType": {
            "type": "string",
            "enum": [
                "Accident",
                "AD&D",
                "Cancer",
                "CriticalIllness",
                "Dental",
                "Hospital",
                "Life",
                "LifestyleBenefits",
                "LTD",
                "Medical",
                "PFL",
                "STD",
                "SavingsSpendingAccounts",
                "StateDI",
                "Vision"
            ]
        },
        "CoverageTier": {
            "type": "string",
            "enum": [
                "ChildOnly",
                "Employee",
                "EmployeeChildren",
                "EmployeeDependent",
                "EmployeeFamily",
                "EmployeeSpouse",
                "Employee2Dependents",
                "SpouseChildren",
                "SpouseDependent",
                "SpouseOnly"
            ]
        },
        "BenefitCalculationMethod": {
            "type": "string",
            "enum": [
                "FlatAmount",
                "MultipleOfSalary",
                "Percentage"
            ]
        },
        "EmployeeContribution": {
            "type": "string",
            "enum": [
                "EmployeePaid",
                "EmployeePartial",
                "EmployerPaid"
            ]
        },
        "PaymentMethod": {
            "type": "string",
            "enum": [
                "DirectBill",
                "PayrollDeduction"
            ]
        },
        "PreTax": {
            "type": "string",
            "enum": [
                "PostTax",
                "PreTax"
            ]
        },
        "ContributionLimitType": {
            "type": "string",
            "enum": [
                "Single",
                "Family"
            ]
        },
        "FormType": {
            "type": "string",
            "enum": [
                "Application",
                "BeneficiaryDesignation",
                "CoverageOutline",
                "DependentStatusDeclaration",
                "Disclosure",
                "EmployeeConsent",
                "EnrollmentConfirmation",
                "LeaveAuthorization",
                "PaidTimeOffAuthorization",
                "PaymentAuthorization",
                "Replacement",
                "SpousalConsent",
                "Other"
            ]
        },
        "FormDataType": {
            "type": "string",
            "enum": [
                "PDF",
                "Image"
            ]
        },
        "FormSignatureDataType": {
            "type": "string",
            "enum": [
                "ClickToSign",
                "Image",
                "Password",
                "Recording",
                "ThirdPartySignature",
                "Topaz",
                "VoicePrint"
            ]
        }
    }
}
```


**Parameters**

| Name                                          | Description                                                                                          |
|:----------------------------------------------|:-----------------------------------------------------------------------------------------------------|
| TransmissionGUID                              | Global Unique identifier created by the originator for this instance of                              |
|                                               |             the electronic transmission.                                                             |
| SenderName                                    | Name of the company sending electronic benefit records on behalf of a                                |
|                                               |             group.                                                                                   |
| SenderPlatformName                            | Identifies the Sender's system that is providing the data for this data                              |
|                                               |             set.                                                                                     |
| ReceiverName                                  | Name of the company receiving electronic benefit records on behalf of a                              |
|                                               |             group.                                                                                   |
| CreationDateTime                              | UTC date and time the transmission was created.                                                      |
| TestProductionCode                            | Indicates the elements contained in this data set are production or                                  |
|                                               |             test data.                                                                               |
| TransmissionTypeCode                          | The type of data contained within the data set.                                                      |
| SchemaVersionIdentifier                       | Identifies the version of the standard that is being adhered to in this                              |
|                                               |             data set.                                                                                |







*Benefits enrollment management*

Use this endpoint to enroll a new employee, update demographics and/or employment information of an existing employee, terminate an employee, reinstate a terminated employee, update an employee coverages by either electing or waiving, add a dependent, terminate a dependent, update dependent demographics, reinstate a terminated dependent.

> Example Response 

> 200 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | Bad request - Payload rejected. |
| 401 | Unauthorized - action requires user authentication. |
| 403 | Forbidden - user does not have permission to upload this payload. |
| 404 | Not Found - resource not found. |
| 405 | Not Allowed - method not allowed on this resource. |
| 406 | Unacceptable - payload content type is not supported. |
| 411 | Length Required - server needs to know the size of the entity body. Specify in Content-Length header. |
| 413 | Payload too large - payload exceeds size limit (too many employees or too many employers). |
| 415 | Unsupported media type - payload type unsupported. |
| 422 | Unprocessable entity - request body failed business validation. |
| 429 | Too many requests - number of requests exceeds limit. |
| 500 | Internal Server Error - fatal error during processing of request. |
| 501 | Not implemented - BEM upload is not supported. |
| 502 | Bad Gateway - the server received an invalid response from an upstream server. |
| 503 | Service Unavailable - unable to process request at this time. Try again later. |

## New Hire Enrollment <img style="float: right;" img src="/images/post.png" alt="post" style="height: 20px; width:20apx;"/>

![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /bem/enrollment ..

<aside class="success">This operation requires authentication</aside>


```shell
# You can also use wget
curl -X POST /bem/..
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'
```

> Request sample

``` json
{
  "bem:Transmission": {
    "@xmlns:bem": "https://ldex.limra.com/xsd/1.0/LDExBEM",
    "@xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
    "@xsi:schemaLocation": "https://ldex.limra.com/xsd/1.0/LDExBEM_1.3.2022.01.01.xsd",
    "TransmissionGUID": "c9fcc147-2b65-48cb-925c-9f898a381815",
    "SenderName": "ABC Tech Partner",
    "SenderPlatformName": "ABC Platform",
    "ReceiverName": "XYZ Carrier",
    "CreationDateTime": "2020-01-17T21:56:36.2619791Z",
    "TestProductionCode": "Production",
    "TransmissionTypeCode": "ChangesOnly",
    "SchemaVersionIdentifier": "1.3.2022.01.01",
    "Carrier": {
      "CarrierID": "1234",
      "CarrierName": "XYZ Carrier"
    },
    "Employer": {
      "EmployerPartyID": "9999",
      "FederalEmployerIdentificationNumber": "99-9999999",
      "CarrierMasterAgreementNumber": {
        "CarrierMasterAgreementNumberID": "AS-M009-87788",
        "MasterAgreementNumber": "M009-87788",
        "CarrierID": "1234"
      },
      "EmployerName": "Sweet Insurance 4 U",
      "EmployerAddress": {
        "FirstLineAddress": "3345 East 45th Street",
        "CityName": "Oklahoma City",
        "StateProvinceCode": "OK",
        "PostalCode": "73112"
      },
      "Employee": {
        "EmployeePartyID": "45",
        "EmployeeSocialSecurityNumber": "998-70-0423",
        "EmployeeIdentifier": "42",
        "EmployeeName": {
          "FirstName": "Sylvia",
          "LastName": "Baldwin"
        },
        "EmployeeGenderCode": "Female",
        "EmployeeBirthDate": "2000-02-08",
        "MaritalStatusCode": "Married",
        "EmployeeTobaccoUseCode": "N",
        "EmployeeHomePhone": "8005257897",
        "EmployeeWorkPhone": "4051234567",
        "EmployeeMobilePhone": "8005257897",
        "EmployeeMailingAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeHomeAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeWorkAddress": {
          "FirstLineAddress": "62 Salt Mine Way",
          "CityName": "Salt Lake City",
          "StateProvinceCode": "UT",
          "PostalCode": "84044"
        },
        "EmployeeEmail": "s.baldwin@saltmine.com",
        "EmployeeAlternateEmail": "s.baldwin@saltmine2.com",
        "EmploymentInformation": {
          "OriginalHireDate": "2018-08-27",
          "MostRecentHireDate": "2018-08-27",
          "ExemptCode": "Exempt",
          "EmploymentTypeCode": "FullTime",
          "EmploymentStatusCode": "Active",
          "JobTitleText": "Head Custodial Engineer",
          "OccupationText": "Head Custodial Engineer",
          "WorkHoursQuantity": "40",
          "WorkHoursFrequencyCode": "Weekly",
          "WorkLocationText": "Corporate",
          "PayrollDeductionFrequencyQuantity": "26",
          "PayrollFrequencyQuantity": "26",
          "UnionIndicator": "false",
          "EmploymentIncome": {
            "IncomeTypeCode": "BenefitSalary",
            "IncomeAmount": "100000",
            "IncomeModeCode": "Annual",
            "IncomeEffectiveDate": "2019-03-20"
          }
        },
        "Event": {
          "EventID": "NHEVT1234",
          "EventTypeCode": "Coverage",
          "EventTypeReasonCode": "NewHireEnrollment",
          "EventDate": "2020-02-10",
          "TransactionDate": "2020-02-05"
        },
        "EmployeeEventID": "NHEVT1234",
        "Dependent": {
          "DependentPartyID": "1522222",
          "DependentSocialSecurityNumber": "997-87-8712",
          "DependentIdentifier": null,
          "DependentName": {
            "FirstName": "Roger",
            "LastName": "Baldwin"
          },
          "DependentRelationshipTypeCode": "Spouse",
          "DependentGenderCode": "Male",
          "DependentBirthDate": "1970-01-01",
          "DependentHomePhone": "2153654587",
          "DependentWorkPhone": "6325698745",
          "DependentMobilePhone": "2014587965",
          "DependentMailingAddress": {
            "FirstLineAddress": "4 Thresa Blvd",
            "SecondLineAddress": "Apt 4",
            "ThirdLineAddress": "Box 3",
            "CityName": "Atlanta",
            "StateProvinceCode": "GA",
            "PostalCode": "30334",
            "CountryCode": "USA"
          },
          "DependentHomeEmail": "someone@flexer.com",
          "DisabilityIndicator": "0",
          "StudentStatusCode": "PartTime",
          "DependentTobaccoUseCode": "N",
          "DependentEventID": "NHEVT1234"
        },
        "OtherParty": [
          {
            "OtherPartyID": "12001",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "256588923",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Ben",
              "LastName": "Hosper"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Male",
            "OtherPartyBirthDate": "1976-01-01",
            "OtherPartyHomePhone": "2055487896",
            "OtherPartyWorkPhone": "2548754587",
            "OtherPartyMobilePhone": "2041549625",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "8 Thresa Blvd",
              "SecondLineAddress": "Apt 3",
              "ThirdLineAddress": "Box 43",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "At.Home@monsure.com",
            "OtherPartyWorkEmail": "At.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          },
          {
            "OtherPartyID": "TT254",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "288956523",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Shelby",
              "LastName": "Cuisine"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Female",
            "OtherPartyBirthDate": "1999-05-15",
            "OtherPartyHomePhone": "2058954876",
            "OtherPartyWorkPhone": "2544588757",
            "OtherPartyMobilePhone": "2049615425",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "568 Climate Dr",
              "SecondLineAddress": null,
              "ThirdLineAddress": null,
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "SCusine@monsure.com",
            "OtherPartyWorkEmail": "ShelbyC.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          }
        ],
        "BeneficiaryGroup": [
          {
            "BeneficiaryGroupID": "UC0014",
            "Beneficiary": {
              "BeneficiaryPartyID": "45",
              "BeneficiaryPartyTypeCode": "Employee",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "2500014",
            "Beneficiary": {
              "BeneficiaryPartyID": "1522222",
              "BeneficiaryPartyTypeCode": "Dependent",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "1773200",
            "Beneficiary": [
              {
                "BeneficiaryPartyID": "1522222",
                "BeneficiaryPartyTypeCode": "Dependent",
                "BeneficiaryPercent": "75.00",
                "BeneficiaryTypeCode": "Primary"
              },
              {
                "BeneficiaryPartyID": "12001",
                "BeneficiaryPartyTypeCode": "OtherParty",
                "BeneficiaryPercent": "25.00",
                "BeneficiaryTypeCode": "Primary"
              }
            ]
          }
        ],
        "Coverage": [
          {
            "CoverageID": "2397070",
            "GroupPolicyNumber": "P123344",
            "ProductTypeCode": "Life",
            "BenefitPlanIdentifier": "W34D",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "1",
            "TakeOverCoverageIndicator": "1",
            "BeneficiaryGroupID": "UC0014",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "45",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397071",
            "GroupPolicyNumber": "P123345",
            "ProductTypeCode": "CriticalIllness",
            "BenefitPlanIdentifier": "BlueCross CI",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "Employee",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": null,
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397072",
            "GroupPolicyNumber": "P983346",
            "ProductTypeCode": "Accident",
            "BenefitPlanIdentifier": "BlueCross Hospital",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "2000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": "1773200",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          }
        ]
      }
    },
    "Audit": {
      "AuditID": "Audit1234",
      "CarrierRecordQuantity": "1",
      "EmployerRecordQuantity": "1",
      "EmployeeRecordQuantity": "1",
      "DependentRecordQuantity": "0",
      "OtherPartyRecordQuantity": "0",
      "BeneficiaryGroupRecordQuantity": "4",
      "EventRecordQuantity": "1",
      "CoverageRecordQuantity": "3",
      "CoverageRiderRecordQuantity": "0"
    }
  }
}
```

*new hire Enrollment*

Use this endpoint to enroll a new employee, update demographics and/or employment information of an existing employee, terminate an employee, reinstate a terminated employee, update an employee coverages by either electing or waiving, add a dependent, terminate a dependent, update dependent demographics, reinstate a terminated dependent.


> Example Response 

> 200 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | Bad request - Payload rejected. |
| 401 | Unauthorized - action requires user authentication. |
| 403 | Forbidden - user does not have permission to upload this payload. |
| 404 | Not Found - resource not found. |
| 405 | Not Allowed - method not allowed on this resource. |
| 406 | Unacceptable - payload content type is not supported. |
| 411 | Length Required - server needs to know the size of the entity body. Specify in Content-Length header. |
| 413 | Payload too large - payload exceeds size limit (too many employees or too many employers). |
| 415 | Unsupported media type - payload type unsupported. |
| 422 | Unprocessable entity - request body failed business validation. |
| 429 | Too many requests - number of requests exceeds limit. |
| 500 | Internal Server Error - fatal error during processing of request. |
| 501 | Not implemented - BEM upload is not supported. |
| 502 | Bad Gateway - the server received an invalid response from an upstream server. |
| 503 | Service Unavailable - unable to process request at this time. Try again later. |

  
## Open Enrollment <img style="float: right;" img src="/images/post.png" alt="post" style="height: 20px; width:20apx;"/>

![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /bem/enrollment ..

<aside class="success">This operation requires authentication</aside>


```shell
# You can also use wget
curl -X POST /bem/..
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'
```

> Request sample

``` json
{
  "bem:Transmission": {
    "@xmlns:bem": "https://ldex.limra.com/xsd/1.0/LDExBEM",
    "@xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
    "@xsi:schemaLocation": "https://ldex.limra.com/xsd/1.0/LDExBEM_1.3.2022.01.01.xsd",
    "TransmissionGUID": "c9fcc147-2b65-48cb-925c-9f898a381815",
    "SenderName": "ABC Tech Partner",
    "SenderPlatformName": "ABC Platform",
    "ReceiverName": "XYZ Carrier",
    "CreationDateTime": "2020-01-17T21:56:36.2619791Z",
    "TestProductionCode": "Production",
    "TransmissionTypeCode": "ChangesOnly",
    "SchemaVersionIdentifier": "1.3.2022.01.01",
    "Carrier": {
      "CarrierID": "1234",
      "CarrierName": "XYZ Carrier"
    },
    "Employer": {
      "EmployerPartyID": "9999",
      "FederalEmployerIdentificationNumber": "99-9999999",
      "CarrierMasterAgreementNumber": {
        "CarrierMasterAgreementNumberID": "AS-M009-87788",
        "MasterAgreementNumber": "M009-87788",
        "CarrierID": "1234"
      },
      "EmployerName": "Sweet Insurance 4 U",
      "EmployerAddress": {
        "FirstLineAddress": "3345 East 45th Street",
        "CityName": "Oklahoma City",
        "StateProvinceCode": "OK",
        "PostalCode": "73112"
      },
      "Employee": {
        "EmployeePartyID": "45",
        "EmployeeSocialSecurityNumber": "998-70-0423",
        "EmployeeIdentifier": "42",
        "EmployeeName": {
          "FirstName": "Sylvia",
          "LastName": "Baldwin"
        },
        "EmployeeGenderCode": "Female",
        "EmployeeBirthDate": "2000-02-08",
        "MaritalStatusCode": "Married",
        "EmployeeTobaccoUseCode": "N",
        "EmployeeHomePhone": "8005257897",
        "EmployeeWorkPhone": "4051234567",
        "EmployeeMobilePhone": "8005257897",
        "EmployeeMailingAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeHomeAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeWorkAddress": {
          "FirstLineAddress": "62 Salt Mine Way",
          "CityName": "Salt Lake City",
          "StateProvinceCode": "UT",
          "PostalCode": "84044"
        },
        "EmployeeEmail": "s.baldwin@saltmine.com",
        "EmployeeAlternateEmail": "s.baldwin@saltmine2.com",
        "EmploymentInformation": {
          "OriginalHireDate": "2018-08-27",
          "MostRecentHireDate": "2018-08-27",
          "ExemptCode": "Exempt",
          "EmploymentTypeCode": "FullTime",
          "EmploymentStatusCode": "Active",
          "JobTitleText": "Head Custodial Engineer",
          "OccupationText": "Head Custodial Engineer",
          "WorkHoursQuantity": "40",
          "WorkHoursFrequencyCode": "Weekly",
          "WorkLocationText": "Corporate",
          "PayrollDeductionFrequencyQuantity": "26",
          "PayrollFrequencyQuantity": "26",
          "UnionIndicator": "false",
          "EmploymentIncome": {
            "IncomeTypeCode": "BenefitSalary",
            "IncomeAmount": "100000",
            "IncomeModeCode": "Annual",
            "IncomeEffectiveDate": "2019-03-20"
          }
        },
        "Event": {
          "EventID": "NHEVT1234",
          "EventTypeCode": "Coverage",
          "EventTypeReasonCode": "NewHireEnrollment",
          "EventDate": "2020-02-10",
          "TransactionDate": "2020-02-05"
        },
        "EmployeeEventID": "NHEVT1234",
        "Dependent": {
          "DependentPartyID": "1522222",
          "DependentSocialSecurityNumber": "997-87-8712",
          "DependentIdentifier": null,
          "DependentName": {
            "FirstName": "Roger",
            "LastName": "Baldwin"
          },
          "DependentRelationshipTypeCode": "Spouse",
          "DependentGenderCode": "Male",
          "DependentBirthDate": "1970-01-01",
          "DependentHomePhone": "2153654587",
          "DependentWorkPhone": "6325698745",
          "DependentMobilePhone": "2014587965",
          "DependentMailingAddress": {
            "FirstLineAddress": "4 Thresa Blvd",
            "SecondLineAddress": "Apt 4",
            "ThirdLineAddress": "Box 3",
            "CityName": "Atlanta",
            "StateProvinceCode": "GA",
            "PostalCode": "30334",
            "CountryCode": "USA"
          },
          "DependentHomeEmail": "someone@flexer.com",
          "DisabilityIndicator": "0",
          "StudentStatusCode": "PartTime",
          "DependentTobaccoUseCode": "N",
          "DependentEventID": "NHEVT1234"
        },
        "OtherParty": [
          {
            "OtherPartyID": "12001",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "256588923",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Ben",
              "LastName": "Hosper"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Male",
            "OtherPartyBirthDate": "1976-01-01",
            "OtherPartyHomePhone": "2055487896",
            "OtherPartyWorkPhone": "2548754587",
            "OtherPartyMobilePhone": "2041549625",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "8 Thresa Blvd",
              "SecondLineAddress": "Apt 3",
              "ThirdLineAddress": "Box 43",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "At.Home@monsure.com",
            "OtherPartyWorkEmail": "At.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          },
          {
            "OtherPartyID": "TT254",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "288956523",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Shelby",
              "LastName": "Cuisine"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Female",
            "OtherPartyBirthDate": "1999-05-15",
            "OtherPartyHomePhone": "2058954876",
            "OtherPartyWorkPhone": "2544588757",
            "OtherPartyMobilePhone": "2049615425",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "568 Climate Dr",
              "SecondLineAddress": null,
              "ThirdLineAddress": null,
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "SCusine@monsure.com",
            "OtherPartyWorkEmail": "ShelbyC.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          }
        ],
        "BeneficiaryGroup": [
          {
            "BeneficiaryGroupID": "UC0014",
            "Beneficiary": {
              "BeneficiaryPartyID": "45",
              "BeneficiaryPartyTypeCode": "Employee",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "2500014",
            "Beneficiary": {
              "BeneficiaryPartyID": "1522222",
              "BeneficiaryPartyTypeCode": "Dependent",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "1773200",
            "Beneficiary": [
              {
                "BeneficiaryPartyID": "1522222",
                "BeneficiaryPartyTypeCode": "Dependent",
                "BeneficiaryPercent": "75.00",
                "BeneficiaryTypeCode": "Primary"
              },
              {
                "BeneficiaryPartyID": "12001",
                "BeneficiaryPartyTypeCode": "OtherParty",
                "BeneficiaryPercent": "25.00",
                "BeneficiaryTypeCode": "Primary"
              }
            ]
          }
        ],
        "Coverage": [
          {
            "CoverageID": "2397070",
            "GroupPolicyNumber": "P123344",
            "ProductTypeCode": "Life",
            "BenefitPlanIdentifier": "W34D",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "1",
            "TakeOverCoverageIndicator": "1",
            "BeneficiaryGroupID": "UC0014",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "45",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397071",
            "GroupPolicyNumber": "P123345",
            "ProductTypeCode": "CriticalIllness",
            "BenefitPlanIdentifier": "BlueCross CI",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "Employee",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": null,
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397072",
            "GroupPolicyNumber": "P983346",
            "ProductTypeCode": "Accident",
            "BenefitPlanIdentifier": "BlueCross Hospital",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "2000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": "1773200",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          }
        ]
      }
    },
    "Audit": {
      "AuditID": "Audit1234",
      "CarrierRecordQuantity": "1",
      "EmployerRecordQuantity": "1",
      "EmployeeRecordQuantity": "1",
      "DependentRecordQuantity": "0",
      "OtherPartyRecordQuantity": "0",
      "BeneficiaryGroupRecordQuantity": "4",
      "EventRecordQuantity": "1",
      "CoverageRecordQuantity": "3",
      "CoverageRiderRecordQuantity": "0"
    }
  }
}
```

*Open Enrollment Request*

Use this endpoint to enroll a new employee, update demographics and/or employment information of an existing employee, terminate an employee, reinstate a terminated employee, update an employee coverages by either electing or waiving, add a dependent, terminate a dependent, update dependent demographics, reinstate a terminated dependent.


> Example Response 

> 200 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | Bad request - Payload rejected. |
| 401 | Unauthorized - action requires user authentication. |
| 403 | Forbidden - user does not have permission to upload this payload. |
| 404 | Not Found - resource not found. |
| 405 | Not Allowed - method not allowed on this resource. |
| 406 | Unacceptable - payload content type is not supported. |
| 411 | Length Required - server needs to know the size of the entity body. Specify in Content-Length header. |
| 413 | Payload too large - payload exceeds size limit (too many employees or too many employers). |
| 415 | Unsupported media type - payload type unsupported. |
| 422 | Unprocessable entity - request body failed business validation. |
| 429 | Too many requests - number of requests exceeds limit. |
| 500 | Internal Server Error - fatal error during processing of request. |
| 501 | Not implemented - BEM upload is not supported. |
| 502 | Bad Gateway - the server received an invalid response from an upstream server. |
| 503 | Service Unavailable - unable to process request at this time. Try again later. |
  
## New Dependent<img style="float: right;" img src="/images/post.png" alt="post" style="height: 20px; width:20apx;"/>

![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /bem/enrollment ..

<aside class="success">This operation requires authentication</aside>


```shell
# You can also use wget
curl -X POST /bem/..
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'
```

> Request sample

``` json
{
  "bem:Transmission": {
    "@xmlns:bem": "https://ldex.limra.com/xsd/1.0/LDExBEM",
    "@xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
    "@xsi:schemaLocation": "https://ldex.limra.com/xsd/1.0/LDExBEM_1.3.2022.01.01.xsd",
    "TransmissionGUID": "c9fcc147-2b65-48cb-925c-9f898a381815",
    "SenderName": "ABC Tech Partner",
    "SenderPlatformName": "ABC Platform",
    "ReceiverName": "XYZ Carrier",
    "CreationDateTime": "2020-01-17T21:56:36.2619791Z",
    "TestProductionCode": "Production",
    "TransmissionTypeCode": "ChangesOnly",
    "SchemaVersionIdentifier": "1.3.2022.01.01",
    "Carrier": {
      "CarrierID": "1234",
      "CarrierName": "XYZ Carrier"
    },
    "Employer": {
      "EmployerPartyID": "9999",
      "FederalEmployerIdentificationNumber": "99-9999999",
      "CarrierMasterAgreementNumber": {
        "CarrierMasterAgreementNumberID": "AS-M009-87788",
        "MasterAgreementNumber": "M009-87788",
        "CarrierID": "1234"
      },
      "EmployerName": "Sweet Insurance 4 U",
      "EmployerAddress": {
        "FirstLineAddress": "3345 East 45th Street",
        "CityName": "Oklahoma City",
        "StateProvinceCode": "OK",
        "PostalCode": "73112"
      },
      "Employee": {
        "EmployeePartyID": "45",
        "EmployeeSocialSecurityNumber": "998-70-0423",
        "EmployeeIdentifier": "42",
        "EmployeeName": {
          "FirstName": "Sylvia",
          "LastName": "Baldwin"
        },
        "EmployeeGenderCode": "Female",
        "EmployeeBirthDate": "2000-02-08",
        "MaritalStatusCode": "Married",
        "EmployeeTobaccoUseCode": "N",
        "EmployeeHomePhone": "8005257897",
        "EmployeeWorkPhone": "4051234567",
        "EmployeeMobilePhone": "8005257897",
        "EmployeeMailingAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeHomeAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeWorkAddress": {
          "FirstLineAddress": "62 Salt Mine Way",
          "CityName": "Salt Lake City",
          "StateProvinceCode": "UT",
          "PostalCode": "84044"
        },
        "EmployeeEmail": "s.baldwin@saltmine.com",
        "EmployeeAlternateEmail": "s.baldwin@saltmine2.com",
        "EmploymentInformation": {
          "OriginalHireDate": "2018-08-27",
          "MostRecentHireDate": "2018-08-27",
          "ExemptCode": "Exempt",
          "EmploymentTypeCode": "FullTime",
          "EmploymentStatusCode": "Active",
          "JobTitleText": "Head Custodial Engineer",
          "OccupationText": "Head Custodial Engineer",
          "WorkHoursQuantity": "40",
          "WorkHoursFrequencyCode": "Weekly",
          "WorkLocationText": "Corporate",
          "PayrollDeductionFrequencyQuantity": "26",
          "PayrollFrequencyQuantity": "26",
          "UnionIndicator": "false",
          "EmploymentIncome": {
            "IncomeTypeCode": "BenefitSalary",
            "IncomeAmount": "100000",
            "IncomeModeCode": "Annual",
            "IncomeEffectiveDate": "2019-03-20"
          }
        },
        "Event": {
          "EventID": "NHEVT1234",
          "EventTypeCode": "Coverage",
          "EventTypeReasonCode": "NewHireEnrollment",
          "EventDate": "2020-02-10",
          "TransactionDate": "2020-02-05"
        },
        "EmployeeEventID": "NHEVT1234",
        "Dependent": {
          "DependentPartyID": "1522222",
          "DependentSocialSecurityNumber": "997-87-8712",
          "DependentIdentifier": null,
          "DependentName": {
            "FirstName": "Roger",
            "LastName": "Baldwin"
          },
          "DependentRelationshipTypeCode": "Spouse",
          "DependentGenderCode": "Male",
          "DependentBirthDate": "1970-01-01",
          "DependentHomePhone": "2153654587",
          "DependentWorkPhone": "6325698745",
          "DependentMobilePhone": "2014587965",
          "DependentMailingAddress": {
            "FirstLineAddress": "4 Thresa Blvd",
            "SecondLineAddress": "Apt 4",
            "ThirdLineAddress": "Box 3",
            "CityName": "Atlanta",
            "StateProvinceCode": "GA",
            "PostalCode": "30334",
            "CountryCode": "USA"
          },
          "DependentHomeEmail": "someone@flexer.com",
          "DisabilityIndicator": "0",
          "StudentStatusCode": "PartTime",
          "DependentTobaccoUseCode": "N",
          "DependentEventID": "NHEVT1234"
        },
        "OtherParty": [
          {
            "OtherPartyID": "12001",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "256588923",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Ben",
              "LastName": "Hosper"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Male",
            "OtherPartyBirthDate": "1976-01-01",
            "OtherPartyHomePhone": "2055487896",
            "OtherPartyWorkPhone": "2548754587",
            "OtherPartyMobilePhone": "2041549625",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "8 Thresa Blvd",
              "SecondLineAddress": "Apt 3",
              "ThirdLineAddress": "Box 43",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "At.Home@monsure.com",
            "OtherPartyWorkEmail": "At.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          },
          {
            "OtherPartyID": "TT254",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "288956523",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Shelby",
              "LastName": "Cuisine"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Female",
            "OtherPartyBirthDate": "1999-05-15",
            "OtherPartyHomePhone": "2058954876",
            "OtherPartyWorkPhone": "2544588757",
            "OtherPartyMobilePhone": "2049615425",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "568 Climate Dr",
              "SecondLineAddress": null,
              "ThirdLineAddress": null,
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "SCusine@monsure.com",
            "OtherPartyWorkEmail": "ShelbyC.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          }
        ],
        "BeneficiaryGroup": [
          {
            "BeneficiaryGroupID": "UC0014",
            "Beneficiary": {
              "BeneficiaryPartyID": "45",
              "BeneficiaryPartyTypeCode": "Employee",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "2500014",
            "Beneficiary": {
              "BeneficiaryPartyID": "1522222",
              "BeneficiaryPartyTypeCode": "Dependent",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "1773200",
            "Beneficiary": [
              {
                "BeneficiaryPartyID": "1522222",
                "BeneficiaryPartyTypeCode": "Dependent",
                "BeneficiaryPercent": "75.00",
                "BeneficiaryTypeCode": "Primary"
              },
              {
                "BeneficiaryPartyID": "12001",
                "BeneficiaryPartyTypeCode": "OtherParty",
                "BeneficiaryPercent": "25.00",
                "BeneficiaryTypeCode": "Primary"
              }
            ]
          }
        ],
        "Coverage": [
          {
            "CoverageID": "2397070",
            "GroupPolicyNumber": "P123344",
            "ProductTypeCode": "Life",
            "BenefitPlanIdentifier": "W34D",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "1",
            "TakeOverCoverageIndicator": "1",
            "BeneficiaryGroupID": "UC0014",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "45",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397071",
            "GroupPolicyNumber": "P123345",
            "ProductTypeCode": "CriticalIllness",
            "BenefitPlanIdentifier": "BlueCross CI",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "Employee",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": null,
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397072",
            "GroupPolicyNumber": "P983346",
            "ProductTypeCode": "Accident",
            "BenefitPlanIdentifier": "BlueCross Hospital",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "2000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": "1773200",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          }
        ]
      }
    },
    "Audit": {
      "AuditID": "Audit1234",
      "CarrierRecordQuantity": "1",
      "EmployerRecordQuantity": "1",
      "EmployeeRecordQuantity": "1",
      "DependentRecordQuantity": "0",
      "OtherPartyRecordQuantity": "0",
      "BeneficiaryGroupRecordQuantity": "4",
      "EventRecordQuantity": "1",
      "CoverageRecordQuantity": "3",
      "CoverageRiderRecordQuantity": "0"
    }
  }
}
```

*New Dependent request*

Use this endpoint to enroll a new employee, update demographics and/or employment information of an existing employee, terminate an employee, reinstate a terminated employee, update an employee coverages by either electing or waiving, add a dependent, terminate a dependent, update dependent demographics, reinstate a terminated dependent.


> Example Response 

> 200 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | Bad request - Payload rejected. |
| 401 | Unauthorized - action requires user authentication. |
| 403 | Forbidden - user does not have permission to upload this payload. |
| 404 | Not Found - resource not found. |
| 405 | Not Allowed - method not allowed on this resource. |
| 406 | Unacceptable - payload content type is not supported. |
| 411 | Length Required - server needs to know the size of the entity body. Specify in Content-Length header. |
| 413 | Payload too large - payload exceeds size limit (too many employees or too many employers). |
| 415 | Unsupported media type - payload type unsupported. |
| 422 | Unprocessable entity - request body failed business validation. |
| 429 | Too many requests - number of requests exceeds limit. |
| 500 | Internal Server Error - fatal error during processing of request. |
| 501 | Not implemented - BEM upload is not supported. |
| 502 | Bad Gateway - the server received an invalid response from an upstream server. |
| 503 | Service Unavailable - unable to process request at this time. Try again later. |

## Demographic Change <img style="float: right;" img src="/images/post.png" alt="post" style="height: 20px; width:20apx;"/>

![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /bem/enrollment ..

<aside class="success">This operation requires authentication</aside>


```shell
# You can also use wget
curl -X POST /bem/..
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'
```

> Request sample

``` json
{
  "bem:Transmission": {
    "@xmlns:bem": "https://ldex.limra.com/xsd/1.0/LDExBEM",
    "@xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
    "@xsi:schemaLocation": "https://ldex.limra.com/xsd/1.0/LDExBEM_1.3.2022.01.01.xsd",
    "TransmissionGUID": "c9fcc147-2b65-48cb-925c-9f898a381815",
    "SenderName": "ABC Tech Partner",
    "SenderPlatformName": "ABC Platform",
    "ReceiverName": "XYZ Carrier",
    "CreationDateTime": "2020-01-17T21:56:36.2619791Z",
    "TestProductionCode": "Production",
    "TransmissionTypeCode": "ChangesOnly",
    "SchemaVersionIdentifier": "1.3.2022.01.01",
    "Carrier": {
      "CarrierID": "1234",
      "CarrierName": "XYZ Carrier"
    },
    "Employer": {
      "EmployerPartyID": "9999",
      "FederalEmployerIdentificationNumber": "99-9999999",
      "CarrierMasterAgreementNumber": {
        "CarrierMasterAgreementNumberID": "AS-M009-87788",
        "MasterAgreementNumber": "M009-87788",
        "CarrierID": "1234"
      },
      "EmployerName": "Sweet Insurance 4 U",
      "EmployerAddress": {
        "FirstLineAddress": "3345 East 45th Street",
        "CityName": "Oklahoma City",
        "StateProvinceCode": "OK",
        "PostalCode": "73112"
      },
      "Employee": {
        "EmployeePartyID": "45",
        "EmployeeSocialSecurityNumber": "998-70-0423",
        "EmployeeIdentifier": "42",
        "EmployeeName": {
          "FirstName": "Sylvia",
          "LastName": "Baldwin"
        },
        "EmployeeGenderCode": "Female",
        "EmployeeBirthDate": "2000-02-08",
        "MaritalStatusCode": "Married",
        "EmployeeTobaccoUseCode": "N",
        "EmployeeHomePhone": "8005257897",
        "EmployeeWorkPhone": "4051234567",
        "EmployeeMobilePhone": "8005257897",
        "EmployeeMailingAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeHomeAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeWorkAddress": {
          "FirstLineAddress": "62 Salt Mine Way",
          "CityName": "Salt Lake City",
          "StateProvinceCode": "UT",
          "PostalCode": "84044"
        },
        "EmployeeEmail": "s.baldwin@saltmine.com",
        "EmployeeAlternateEmail": "s.baldwin@saltmine2.com",
        "EmploymentInformation": {
          "OriginalHireDate": "2018-08-27",
          "MostRecentHireDate": "2018-08-27",
          "ExemptCode": "Exempt",
          "EmploymentTypeCode": "FullTime",
          "EmploymentStatusCode": "Active",
          "JobTitleText": "Head Custodial Engineer",
          "OccupationText": "Head Custodial Engineer",
          "WorkHoursQuantity": "40",
          "WorkHoursFrequencyCode": "Weekly",
          "WorkLocationText": "Corporate",
          "PayrollDeductionFrequencyQuantity": "26",
          "PayrollFrequencyQuantity": "26",
          "UnionIndicator": "false",
          "EmploymentIncome": {
            "IncomeTypeCode": "BenefitSalary",
            "IncomeAmount": "100000",
            "IncomeModeCode": "Annual",
            "IncomeEffectiveDate": "2019-03-20"
          }
        },
        "Event": {
          "EventID": "NHEVT1234",
          "EventTypeCode": "Coverage",
          "EventTypeReasonCode": "NewHireEnrollment",
          "EventDate": "2020-02-10",
          "TransactionDate": "2020-02-05"
        },
        "EmployeeEventID": "NHEVT1234",
        "Dependent": {
          "DependentPartyID": "1522222",
          "DependentSocialSecurityNumber": "997-87-8712",
          "DependentIdentifier": null,
          "DependentName": {
            "FirstName": "Roger",
            "LastName": "Baldwin"
          },
          "DependentRelationshipTypeCode": "Spouse",
          "DependentGenderCode": "Male",
          "DependentBirthDate": "1970-01-01",
          "DependentHomePhone": "2153654587",
          "DependentWorkPhone": "6325698745",
          "DependentMobilePhone": "2014587965",
          "DependentMailingAddress": {
            "FirstLineAddress": "4 Thresa Blvd",
            "SecondLineAddress": "Apt 4",
            "ThirdLineAddress": "Box 3",
            "CityName": "Atlanta",
            "StateProvinceCode": "GA",
            "PostalCode": "30334",
            "CountryCode": "USA"
          },
          "DependentHomeEmail": "someone@flexer.com",
          "DisabilityIndicator": "0",
          "StudentStatusCode": "PartTime",
          "DependentTobaccoUseCode": "N",
          "DependentEventID": "NHEVT1234"
        },
        "OtherParty": [
          {
            "OtherPartyID": "12001",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "256588923",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Ben",
              "LastName": "Hosper"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Male",
            "OtherPartyBirthDate": "1976-01-01",
            "OtherPartyHomePhone": "2055487896",
            "OtherPartyWorkPhone": "2548754587",
            "OtherPartyMobilePhone": "2041549625",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "8 Thresa Blvd",
              "SecondLineAddress": "Apt 3",
              "ThirdLineAddress": "Box 43",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "At.Home@monsure.com",
            "OtherPartyWorkEmail": "At.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          },
          {
            "OtherPartyID": "TT254",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "288956523",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Shelby",
              "LastName": "Cuisine"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Female",
            "OtherPartyBirthDate": "1999-05-15",
            "OtherPartyHomePhone": "2058954876",
            "OtherPartyWorkPhone": "2544588757",
            "OtherPartyMobilePhone": "2049615425",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "568 Climate Dr",
              "SecondLineAddress": null,
              "ThirdLineAddress": null,
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "SCusine@monsure.com",
            "OtherPartyWorkEmail": "ShelbyC.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          }
        ],
        "BeneficiaryGroup": [
          {
            "BeneficiaryGroupID": "UC0014",
            "Beneficiary": {
              "BeneficiaryPartyID": "45",
              "BeneficiaryPartyTypeCode": "Employee",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "2500014",
            "Beneficiary": {
              "BeneficiaryPartyID": "1522222",
              "BeneficiaryPartyTypeCode": "Dependent",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "1773200",
            "Beneficiary": [
              {
                "BeneficiaryPartyID": "1522222",
                "BeneficiaryPartyTypeCode": "Dependent",
                "BeneficiaryPercent": "75.00",
                "BeneficiaryTypeCode": "Primary"
              },
              {
                "BeneficiaryPartyID": "12001",
                "BeneficiaryPartyTypeCode": "OtherParty",
                "BeneficiaryPercent": "25.00",
                "BeneficiaryTypeCode": "Primary"
              }
            ]
          }
        ],
        "Coverage": [
          {
            "CoverageID": "2397070",
            "GroupPolicyNumber": "P123344",
            "ProductTypeCode": "Life",
            "BenefitPlanIdentifier": "W34D",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "1",
            "TakeOverCoverageIndicator": "1",
            "BeneficiaryGroupID": "UC0014",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "45",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397071",
            "GroupPolicyNumber": "P123345",
            "ProductTypeCode": "CriticalIllness",
            "BenefitPlanIdentifier": "BlueCross CI",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "Employee",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": null,
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397072",
            "GroupPolicyNumber": "P983346",
            "ProductTypeCode": "Accident",
            "BenefitPlanIdentifier": "BlueCross Hospital",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "2000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": "1773200",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          }
        ]
      }
    },
    "Audit": {
      "AuditID": "Audit1234",
      "CarrierRecordQuantity": "1",
      "EmployerRecordQuantity": "1",
      "EmployeeRecordQuantity": "1",
      "DependentRecordQuantity": "0",
      "OtherPartyRecordQuantity": "0",
      "BeneficiaryGroupRecordQuantity": "4",
      "EventRecordQuantity": "1",
      "CoverageRecordQuantity": "3",
      "CoverageRiderRecordQuantity": "0"
    }
  }
}
```

*Demographic Change Request*

Use this endpoint to enroll a new employee, update demographics and/or employment information of an existing employee, terminate an employee, reinstate a terminated employee, update an employee coverages by either electing or waiving, add a dependent, terminate a dependent, update dependent demographics, reinstate a terminated dependent.


> Example Response 

> 200 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | Bad request - Payload rejected. |
| 401 | Unauthorized - action requires user authentication. |
| 403 | Forbidden - user does not have permission to upload this payload. |
| 404 | Not Found - resource not found. |
| 405 | Not Allowed - method not allowed on this resource. |
| 406 | Unacceptable - payload content type is not supported. |
| 411 | Length Required - server needs to know the size of the entity body. Specify in Content-Length header. |
| 413 | Payload too large - payload exceeds size limit (too many employees or too many employers). |
| 415 | Unsupported media type - payload type unsupported. |
| 422 | Unprocessable entity - request body failed business validation. |
| 429 | Too many requests - number of requests exceeds limit. |
| 500 | Internal Server Error - fatal error during processing of request. |
| 501 | Not implemented - BEM upload is not supported. |
| 502 | Bad Gateway - the server received an invalid response from an upstream server. |
| 503 | Service Unavailable - unable to process request at this time. Try again later. |
 
## Qualifying Life Event <img style="float: right;" img src="/images/post.png" alt="post" style="height: 20px; width:20apx;"/>

![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /bem/enrollment ..

<aside class="success">This operation requires authentication</aside>


```shell
# You can also use wget
curl -X POST /bem/..
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'
```

> Request sample

``` json
{
  "bem:Transmission": {
    "@xmlns:bem": "https://ldex.limra.com/xsd/1.0/LDExBEM",
    "@xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
    "@xsi:schemaLocation": "https://ldex.limra.com/xsd/1.0/LDExBEM_1.3.2022.01.01.xsd",
    "TransmissionGUID": "c9fcc147-2b65-48cb-925c-9f898a381815",
    "SenderName": "ABC Tech Partner",
    "SenderPlatformName": "ABC Platform",
    "ReceiverName": "XYZ Carrier",
    "CreationDateTime": "2020-01-17T21:56:36.2619791Z",
    "TestProductionCode": "Production",
    "TransmissionTypeCode": "ChangesOnly",
    "SchemaVersionIdentifier": "1.3.2022.01.01",
    "Carrier": {
      "CarrierID": "1234",
      "CarrierName": "XYZ Carrier"
    },
    "Employer": {
      "EmployerPartyID": "9999",
      "FederalEmployerIdentificationNumber": "99-9999999",
      "CarrierMasterAgreementNumber": {
        "CarrierMasterAgreementNumberID": "AS-M009-87788",
        "MasterAgreementNumber": "M009-87788",
        "CarrierID": "1234"
      },
      "EmployerName": "Sweet Insurance 4 U",
      "EmployerAddress": {
        "FirstLineAddress": "3345 East 45th Street",
        "CityName": "Oklahoma City",
        "StateProvinceCode": "OK",
        "PostalCode": "73112"
      },
      "Employee": {
        "EmployeePartyID": "45",
        "EmployeeSocialSecurityNumber": "998-70-0423",
        "EmployeeIdentifier": "42",
        "EmployeeName": {
          "FirstName": "Sylvia",
          "LastName": "Baldwin"
        },
        "EmployeeGenderCode": "Female",
        "EmployeeBirthDate": "2000-02-08",
        "MaritalStatusCode": "Married",
        "EmployeeTobaccoUseCode": "N",
        "EmployeeHomePhone": "8005257897",
        "EmployeeWorkPhone": "4051234567",
        "EmployeeMobilePhone": "8005257897",
        "EmployeeMailingAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeHomeAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeWorkAddress": {
          "FirstLineAddress": "62 Salt Mine Way",
          "CityName": "Salt Lake City",
          "StateProvinceCode": "UT",
          "PostalCode": "84044"
        },
        "EmployeeEmail": "s.baldwin@saltmine.com",
        "EmployeeAlternateEmail": "s.baldwin@saltmine2.com",
        "EmploymentInformation": {
          "OriginalHireDate": "2018-08-27",
          "MostRecentHireDate": "2018-08-27",
          "ExemptCode": "Exempt",
          "EmploymentTypeCode": "FullTime",
          "EmploymentStatusCode": "Active",
          "JobTitleText": "Head Custodial Engineer",
          "OccupationText": "Head Custodial Engineer",
          "WorkHoursQuantity": "40",
          "WorkHoursFrequencyCode": "Weekly",
          "WorkLocationText": "Corporate",
          "PayrollDeductionFrequencyQuantity": "26",
          "PayrollFrequencyQuantity": "26",
          "UnionIndicator": "false",
          "EmploymentIncome": {
            "IncomeTypeCode": "BenefitSalary",
            "IncomeAmount": "100000",
            "IncomeModeCode": "Annual",
            "IncomeEffectiveDate": "2019-03-20"
          }
        },
        "Event": {
          "EventID": "NHEVT1234",
          "EventTypeCode": "Coverage",
          "EventTypeReasonCode": "NewHireEnrollment",
          "EventDate": "2020-02-10",
          "TransactionDate": "2020-02-05"
        },
        "EmployeeEventID": "NHEVT1234",
        "Dependent": {
          "DependentPartyID": "1522222",
          "DependentSocialSecurityNumber": "997-87-8712",
          "DependentIdentifier": null,
          "DependentName": {
            "FirstName": "Roger",
            "LastName": "Baldwin"
          },
          "DependentRelationshipTypeCode": "Spouse",
          "DependentGenderCode": "Male",
          "DependentBirthDate": "1970-01-01",
          "DependentHomePhone": "2153654587",
          "DependentWorkPhone": "6325698745",
          "DependentMobilePhone": "2014587965",
          "DependentMailingAddress": {
            "FirstLineAddress": "4 Thresa Blvd",
            "SecondLineAddress": "Apt 4",
            "ThirdLineAddress": "Box 3",
            "CityName": "Atlanta",
            "StateProvinceCode": "GA",
            "PostalCode": "30334",
            "CountryCode": "USA"
          },
          "DependentHomeEmail": "someone@flexer.com",
          "DisabilityIndicator": "0",
          "StudentStatusCode": "PartTime",
          "DependentTobaccoUseCode": "N",
          "DependentEventID": "NHEVT1234"
        },
        "OtherParty": [
          {
            "OtherPartyID": "12001",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "256588923",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Ben",
              "LastName": "Hosper"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Male",
            "OtherPartyBirthDate": "1976-01-01",
            "OtherPartyHomePhone": "2055487896",
            "OtherPartyWorkPhone": "2548754587",
            "OtherPartyMobilePhone": "2041549625",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "8 Thresa Blvd",
              "SecondLineAddress": "Apt 3",
              "ThirdLineAddress": "Box 43",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "At.Home@monsure.com",
            "OtherPartyWorkEmail": "At.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          },
          {
            "OtherPartyID": "TT254",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "288956523",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Shelby",
              "LastName": "Cuisine"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Female",
            "OtherPartyBirthDate": "1999-05-15",
            "OtherPartyHomePhone": "2058954876",
            "OtherPartyWorkPhone": "2544588757",
            "OtherPartyMobilePhone": "2049615425",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "568 Climate Dr",
              "SecondLineAddress": null,
              "ThirdLineAddress": null,
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "SCusine@monsure.com",
            "OtherPartyWorkEmail": "ShelbyC.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          }
        ],
        "BeneficiaryGroup": [
          {
            "BeneficiaryGroupID": "UC0014",
            "Beneficiary": {
              "BeneficiaryPartyID": "45",
              "BeneficiaryPartyTypeCode": "Employee",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "2500014",
            "Beneficiary": {
              "BeneficiaryPartyID": "1522222",
              "BeneficiaryPartyTypeCode": "Dependent",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "1773200",
            "Beneficiary": [
              {
                "BeneficiaryPartyID": "1522222",
                "BeneficiaryPartyTypeCode": "Dependent",
                "BeneficiaryPercent": "75.00",
                "BeneficiaryTypeCode": "Primary"
              },
              {
                "BeneficiaryPartyID": "12001",
                "BeneficiaryPartyTypeCode": "OtherParty",
                "BeneficiaryPercent": "25.00",
                "BeneficiaryTypeCode": "Primary"
              }
            ]
          }
        ],
        "Coverage": [
          {
            "CoverageID": "2397070",
            "GroupPolicyNumber": "P123344",
            "ProductTypeCode": "Life",
            "BenefitPlanIdentifier": "W34D",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "1",
            "TakeOverCoverageIndicator": "1",
            "BeneficiaryGroupID": "UC0014",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "45",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397071",
            "GroupPolicyNumber": "P123345",
            "ProductTypeCode": "CriticalIllness",
            "BenefitPlanIdentifier": "BlueCross CI",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "Employee",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": null,
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397072",
            "GroupPolicyNumber": "P983346",
            "ProductTypeCode": "Accident",
            "BenefitPlanIdentifier": "BlueCross Hospital",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "2000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": "1773200",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          }
        ]
      }
    },
    "Audit": {
      "AuditID": "Audit1234",
      "CarrierRecordQuantity": "1",
      "EmployerRecordQuantity": "1",
      "EmployeeRecordQuantity": "1",
      "DependentRecordQuantity": "0",
      "OtherPartyRecordQuantity": "0",
      "BeneficiaryGroupRecordQuantity": "4",
      "EventRecordQuantity": "1",
      "CoverageRecordQuantity": "3",
      "CoverageRiderRecordQuantity": "0"
    }
  }
}
```

*Qualifying Life Event Member Request*

Use this endpoint to enroll a new employee, update demographics and/or employment information of an existing employee, terminate an employee, reinstate a terminated employee, update an employee coverages by either electing or waiving, add a dependent, terminate a dependent, update dependent demographics, reinstate a terminated dependent.


> Example Response 

> 200 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | Bad request - Payload rejected. |
| 401 | Unauthorized - action requires user authentication. |
| 403 | Forbidden - user does not have permission to upload this payload. |
| 404 | Not Found - resource not found. |
| 405 | Not Allowed - method not allowed on this resource. |
| 406 | Unacceptable - payload content type is not supported. |
| 411 | Length Required - server needs to know the size of the entity body. Specify in Content-Length header. |
| 413 | Payload too large - payload exceeds size limit (too many employees or too many employers). |
| 415 | Unsupported media type - payload type unsupported. |
| 422 | Unprocessable entity - request body failed business validation. |
| 429 | Too many requests - number of requests exceeds limit. |
| 500 | Internal Server Error - fatal error during processing of request. |
| 501 | Not implemented - BEM upload is not supported. |
| 502 | Bad Gateway - the server received an invalid response from an upstream server. |
| 503 | Service Unavailable - unable to process request at this time. Try again later. |
 
## Termination Request <img style="float: right;" img src="/images/post.png" alt="post" style="height: 20px; width:20apx;"/>

![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /bem/enrollment ..

<aside class="success">This operation requires authentication</aside>


```shell
# You can also use wget
curl -X POST /bem/..
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'
```

> Request sample

``` json
{
  "bem:Transmission": {
    "@xmlns:bem": "https://ldex.limra.com/xsd/1.0/LDExBEM",
    "@xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
    "@xsi:schemaLocation": "https://ldex.limra.com/xsd/1.0/LDExBEM_1.3.2022.01.01.xsd",
    "TransmissionGUID": "c9fcc147-2b65-48cb-925c-9f898a381815",
    "SenderName": "ABC Tech Partner",
    "SenderPlatformName": "ABC Platform",
    "ReceiverName": "XYZ Carrier",
    "CreationDateTime": "2020-01-17T21:56:36.2619791Z",
    "TestProductionCode": "Production",
    "TransmissionTypeCode": "ChangesOnly",
    "SchemaVersionIdentifier": "1.3.2022.01.01",
    "Carrier": {
      "CarrierID": "1234",
      "CarrierName": "XYZ Carrier"
    },
    "Employer": {
      "EmployerPartyID": "9999",
      "FederalEmployerIdentificationNumber": "99-9999999",
      "CarrierMasterAgreementNumber": {
        "CarrierMasterAgreementNumberID": "AS-M009-87788",
        "MasterAgreementNumber": "M009-87788",
        "CarrierID": "1234"
      },
      "EmployerName": "Sweet Insurance 4 U",
      "EmployerAddress": {
        "FirstLineAddress": "3345 East 45th Street",
        "CityName": "Oklahoma City",
        "StateProvinceCode": "OK",
        "PostalCode": "73112"
      },
      "Employee": {
        "EmployeePartyID": "45",
        "EmployeeSocialSecurityNumber": "998-70-0423",
        "EmployeeIdentifier": "42",
        "EmployeeName": {
          "FirstName": "Sylvia",
          "LastName": "Baldwin"
        },
        "EmployeeGenderCode": "Female",
        "EmployeeBirthDate": "2000-02-08",
        "MaritalStatusCode": "Married",
        "EmployeeTobaccoUseCode": "N",
        "EmployeeHomePhone": "8005257897",
        "EmployeeWorkPhone": "4051234567",
        "EmployeeMobilePhone": "8005257897",
        "EmployeeMailingAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeHomeAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeWorkAddress": {
          "FirstLineAddress": "62 Salt Mine Way",
          "CityName": "Salt Lake City",
          "StateProvinceCode": "UT",
          "PostalCode": "84044"
        },
        "EmployeeEmail": "s.baldwin@saltmine.com",
        "EmployeeAlternateEmail": "s.baldwin@saltmine2.com",
        "EmploymentInformation": {
          "OriginalHireDate": "2018-08-27",
          "MostRecentHireDate": "2018-08-27",
          "ExemptCode": "Exempt",
          "EmploymentTypeCode": "FullTime",
          "EmploymentStatusCode": "Active",
          "JobTitleText": "Head Custodial Engineer",
          "OccupationText": "Head Custodial Engineer",
          "WorkHoursQuantity": "40",
          "WorkHoursFrequencyCode": "Weekly",
          "WorkLocationText": "Corporate",
          "PayrollDeductionFrequencyQuantity": "26",
          "PayrollFrequencyQuantity": "26",
          "UnionIndicator": "false",
          "EmploymentIncome": {
            "IncomeTypeCode": "BenefitSalary",
            "IncomeAmount": "100000",
            "IncomeModeCode": "Annual",
            "IncomeEffectiveDate": "2019-03-20"
          }
        },
        "Event": {
          "EventID": "NHEVT1234",
          "EventTypeCode": "Coverage",
          "EventTypeReasonCode": "NewHireEnrollment",
          "EventDate": "2020-02-10",
          "TransactionDate": "2020-02-05"
        },
        "EmployeeEventID": "NHEVT1234",
        "Dependent": {
          "DependentPartyID": "1522222",
          "DependentSocialSecurityNumber": "997-87-8712",
          "DependentIdentifier": null,
          "DependentName": {
            "FirstName": "Roger",
            "LastName": "Baldwin"
          },
          "DependentRelationshipTypeCode": "Spouse",
          "DependentGenderCode": "Male",
          "DependentBirthDate": "1970-01-01",
          "DependentHomePhone": "2153654587",
          "DependentWorkPhone": "6325698745",
          "DependentMobilePhone": "2014587965",
          "DependentMailingAddress": {
            "FirstLineAddress": "4 Thresa Blvd",
            "SecondLineAddress": "Apt 4",
            "ThirdLineAddress": "Box 3",
            "CityName": "Atlanta",
            "StateProvinceCode": "GA",
            "PostalCode": "30334",
            "CountryCode": "USA"
          },
          "DependentHomeEmail": "someone@flexer.com",
          "DisabilityIndicator": "0",
          "StudentStatusCode": "PartTime",
          "DependentTobaccoUseCode": "N",
          "DependentEventID": "NHEVT1234"
        },
        "OtherParty": [
          {
            "OtherPartyID": "12001",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "256588923",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Ben",
              "LastName": "Hosper"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Male",
            "OtherPartyBirthDate": "1976-01-01",
            "OtherPartyHomePhone": "2055487896",
            "OtherPartyWorkPhone": "2548754587",
            "OtherPartyMobilePhone": "2041549625",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "8 Thresa Blvd",
              "SecondLineAddress": "Apt 3",
              "ThirdLineAddress": "Box 43",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "At.Home@monsure.com",
            "OtherPartyWorkEmail": "At.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          },
          {
            "OtherPartyID": "TT254",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "288956523",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Shelby",
              "LastName": "Cuisine"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Female",
            "OtherPartyBirthDate": "1999-05-15",
            "OtherPartyHomePhone": "2058954876",
            "OtherPartyWorkPhone": "2544588757",
            "OtherPartyMobilePhone": "2049615425",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "568 Climate Dr",
              "SecondLineAddress": null,
              "ThirdLineAddress": null,
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "SCusine@monsure.com",
            "OtherPartyWorkEmail": "ShelbyC.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          }
        ],
        "BeneficiaryGroup": [
          {
            "BeneficiaryGroupID": "UC0014",
            "Beneficiary": {
              "BeneficiaryPartyID": "45",
              "BeneficiaryPartyTypeCode": "Employee",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "2500014",
            "Beneficiary": {
              "BeneficiaryPartyID": "1522222",
              "BeneficiaryPartyTypeCode": "Dependent",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "1773200",
            "Beneficiary": [
              {
                "BeneficiaryPartyID": "1522222",
                "BeneficiaryPartyTypeCode": "Dependent",
                "BeneficiaryPercent": "75.00",
                "BeneficiaryTypeCode": "Primary"
              },
              {
                "BeneficiaryPartyID": "12001",
                "BeneficiaryPartyTypeCode": "OtherParty",
                "BeneficiaryPercent": "25.00",
                "BeneficiaryTypeCode": "Primary"
              }
            ]
          }
        ],
        "Coverage": [
          {
            "CoverageID": "2397070",
            "GroupPolicyNumber": "P123344",
            "ProductTypeCode": "Life",
            "BenefitPlanIdentifier": "W34D",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "1",
            "TakeOverCoverageIndicator": "1",
            "BeneficiaryGroupID": "UC0014",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "45",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397071",
            "GroupPolicyNumber": "P123345",
            "ProductTypeCode": "CriticalIllness",
            "BenefitPlanIdentifier": "BlueCross CI",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "Employee",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": null,
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397072",
            "GroupPolicyNumber": "P983346",
            "ProductTypeCode": "Accident",
            "BenefitPlanIdentifier": "BlueCross Hospital",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "2000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": "1773200",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          }
        ]
      }
    },
    "Audit": {
      "AuditID": "Audit1234",
      "CarrierRecordQuantity": "1",
      "EmployerRecordQuantity": "1",
      "EmployeeRecordQuantity": "1",
      "DependentRecordQuantity": "0",
      "OtherPartyRecordQuantity": "0",
      "BeneficiaryGroupRecordQuantity": "4",
      "EventRecordQuantity": "1",
      "CoverageRecordQuantity": "3",
      "CoverageRiderRecordQuantity": "0"
    }
  }
}
```

*Termination Member Request*

Use this endpoint to enroll a new employee, update demographics and/or employment information of an existing employee, terminate an employee, reinstate a terminated employee, update an employee coverages by either electing or waiving, add a dependent, terminate a dependent, update dependent demographics, reinstate a terminated dependent.


> Example Response 

> 200 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | Bad request - Payload rejected. |
| 401 | Unauthorized - action requires user authentication. |
| 403 | Forbidden - user does not have permission to upload this payload. |
| 404 | Not Found - resource not found. |
| 405 | Not Allowed - method not allowed on this resource. |
| 406 | Unacceptable - payload content type is not supported. |
| 411 | Length Required - server needs to know the size of the entity body. Specify in Content-Length header. |
| 413 | Payload too large - payload exceeds size limit (too many employees or too many employers). |
| 415 | Unsupported media type - payload type unsupported. |
| 422 | Unprocessable entity - request body failed business validation. |
| 429 | Too many requests - number of requests exceeds limit. |
| 500 | Internal Server Error - fatal error during processing of request. |
| 501 | Not implemented - BEM upload is not supported. |
| 502 | Bad Gateway - the server received an invalid response from an upstream server. |
| 503 | Service Unavailable - unable to process request at this time. Try again later. |


#Evidence of Insurability

![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /eois/...`

<aside class="success">
This operation requires authentication
</aside>

> EOI schema ..  



``` json
{
    "$schema": "http://json-schema.org/draft/2020-12/schema",
    "title": "LDExBEM",
    "description": "1.3.2022.12.31",    
    "anyOf": [
        {
            "type": "object",
            "properties": {
                "transmission": {"$ref": "#/$defs/Transmission"}
            }
        }
    ],
    "$defs": {
        "Carrier": {
            "type": "object",
            "required": [
                "carrierID",
                "carrierName"
            ],
            "properties": {
                "carrierID": {"type": "string"},
                "carrierName": {"type": "string"}
            }
        },
        "CarrierMasterAgreementNumber": {
            "type": "object",
            "required": [
                "carrierMasterAgreementNumberID",
                "masterAgreementNumber",
                "carrierID"
            ],
            "properties": {
                "carrierMasterAgreementNumberID": {"type": "string"},
                "masterAgreementNumber": {"type": "string"},
                "carrierID": {"type": "string"}
            }
        },
        "PostalAddress": {
            "type": "object",
            "required": [
                "firstLineAddress",
                "cityName",
                "postalCode"
            ],
            "properties": {
                "firstLineAddress": {"type": "string"},
                "secondLineAddress": {"type": "string"},
                "thirdLineAddress": {"type": "string"},
                "cityName": {"type": "string"},
                "stateProvinceCode": {"$ref": "#/$defs/StateProvince"},
                "postalCode": {"type": "string"},
                "countryCode": {"$ref": "#/$defs/Country"}
            }
        },
        "StructuredPersonName": {
            "type": "object",
            "properties": {
                "prefixCode": {"$ref": "#/$defs/Prefix"},
                "firstName": {"type": "string"},
                "middleName": {"type": "string"},
                "lastName": {"type": "string"},
                "suffixCode": {"$ref": "#/$defs/Suffix"}
            }
        },
        "EmploymentIncome": {
            "type": "object",
            "properties": {
                "incomeTypeCode": {"$ref": "#/$defs/IncomeType"},
                "incomeAmount": {"type": "number"},
                "incomeModeCode": {
                    "type": "string",
                    "enum": [
                        "Hourly",
                        "Annual",
                        "Monthly12PerYear",
                        "BiWeekly26PerYear",
                        "SemiMonthly24PerYear",
                        "SemiMonthly21PerYear",
                        "Weekly52PerYear",
                        "Weekly48PerYear",
                        "Quarterly",
                        "SemiAnnual",
                        "9thly",
                        "10thly"
                    ]
                },
                "incomeEffectiveDate": {
                    "type": "string",
                    "format": "date"
                }
            }
        },
        "EmploymentInformationUserDefinedCategory": {
            "type": "object",
            "required": ["categoryName"],
            "properties": {
                "categoryName": {"type": "string"},
                "categoryValueString": {"type": "string"},
                "categoryTypeCode": {"$ref": "#/$defs/CategoryType"}
            }
        },
        "EmploymentContact": {
            "type": "object",
            "required": [
                "contactTypeCode",
                "contactName"
            ],
            "properties": {
                "contactTypeCode": {"$ref": "#/$defs/ContactType"},
                "contactName": {"$ref": "#/$defs/StructuredPersonName"},
                "contactSocialSecurityNumber": {"type": "string"},
                "contactIdentifier": {"type": "string"},
                "contactEmail": {"type": "string"},
                "contactOrganizationName": {"type": "string"},
                "contactPhone": {"type": "string"}
            }
        },
        "WorkSchedule": {
            "type": "object",
            "required": ["workScheduleTypeCode"],
            "properties": {
                "workShiftCode": {"$ref": "#/$defs/WorkShift"},
                "workScheduleTypeCode": {"$ref": "#/$defs/WorkScheduleType"},
                "weeklyWorkedHoursQuantity": {"type": "number"},
                "sundayWorkedHoursQuantity": {"type": "number"},
                "mondayWorkedHoursQuantity": {"type": "number"},
                "tuesdayWorkedHoursQuantity": {"type": "number"},
                "wednesdayWorkedHoursQuantity": {"type": "number"},
                "thursdayWorkedHoursQuantity": {"type": "number"},
                "fridayWorkedHoursQuantity": {"type": "number"},
                "saturdayWorkedHoursQuantity": {"type": "number"}
            }
        },
        "EmploymentSchedule": {
            "type": "object",
            "properties": {
                "scheduledHoursQuantity": {"type": "number"},
                "scheduledHoursFrequencyCode": {"$ref": "#/$defs/WorkHoursFrequency"},
                "last12MonthsWorkHoursQuantity": {"type": "number"},
                "perWeekWorkedHoursQuantity": {"type": "number"},
                "workSchedule": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/WorkSchedule"},
                    "minItems": 0
                }
            }
        },
        "EmploymentInformation": {
            "type": "object",
            "required": [
                "originalHireDate",
                "employmentTypeCode",
                "employmentStatusCode"
            ],
            "properties": {
                "originalHireDate": {
                    "type": "string",
                    "format": "date"
                },
                "mostRecentHireDate": {
                    "type": "string",
                    "format": "date"
                },
                "adjustedServiceDate": {
                    "type": "string",
                    "format": "date"
                },
                "exemptCode": {"$ref": "#/$defs/Exempt"},
                "employmentTypeCode": {"$ref": "#/$defs/EmploymentType"},
                "employmentStatusCode": {"$ref": "#/$defs/EmploymentStatus"},
                "terminationDate": {
                    "type": "string",
                    "format": "date"
                },
                "terminationReasonCode": {"$ref": "#/$defs/TerminationReason"},
                "jobTitleText": {"type": "string"},
                "occupationText": {"type": "string"},
                "workHoursQuantity": {"type": "number"},
                "workHoursFrequencyCode": {"$ref": "#/$defs/WorkHoursFrequency"},
                "workLocationText": {"type": "string"},
                "payrollDeductionFrequencyQuantity": {"type": "integer"},
                "payrollFrequencyQuantity": {"type": "integer"},
                "supervisorFullName": {"type": "string"},
                "supervisorEmailAddress": {"type": "string"},
                "humanResourcePartnerEmailAddress": {"type": "string"},
                "unionIndicator": {"type": "boolean"},
                "unionLocalName": {"type": "string"},
                "unionLocalNumber": {"type": "string"},
                "unionMemberNumber": {"type": "string"},
                "employmentIncome": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/EmploymentIncome"},
                    "minItems": 0
                },
                "employmentInformationUserDefinedCategory": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/EmploymentInformationUserDefinedCategory"},
                    "minItems": 0
                },
                "rule5075Indicator": {"type": "boolean"},
                "workStateCode": {"$ref": "#/$defs/StateProvince"},
                "keyEmployeeIndicator": {"type": "boolean"},
                "employmentContact": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/EmploymentContact"},
                    "minItems": 0
                },
                "employmentSchedule": {"$ref": "#/$defs/EmploymentSchedule"}
            }
        },
        "EnrollmentInformation": {
            "type": "object",
            "required": ["enrollmentInformationID"],
            "properties": {
                "enrollmentInformationID": {"type": "string"},
                "enrollmentMethodCode": {"$ref": "#/$defs/EnrollmentMethod"},
                "employeeEnrollmentLocationAddress": {"$ref": "#/$defs/PostalAddress"},
                "enrollmentCityName": {"type": "string"},
                "enrollmentStateProvinceCode": {"$ref": "#/$defs/StateProvince"}
            }
        },
        "Event": {
            "type": "object",
            "required": [
                "eventID",
                "eventTypeCode",
                "eventTypeReasonCode",
                "eventDate"
            ],
            "properties": {
                "eventID": {"type": "string"},
                "eventTypeCode": {"$ref": "#/$defs/EventType"},
                "eventTypeReasonCode": {"$ref": "#/$defs/EventTypeReason"},
                "eventDate": {
                    "type": "string",
                    "format": "date"
                },
                "transactionDate": {
                    "type": "string",
                    "format": "date"
                },
                "enrollmentInformationID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                }
            }
        },
        "Dependent": {
            "type": "object",
            "required": [
                "dependentPartyID",
                "dependentRelationshipTypeCode",
                "dependentBirthDate"
            ],
            "properties": {
                "dependentPartyID": {"type": "string"},
                "dependentIndividualTaxpayerIdentificationNumber": {"type": "string"},
                "dependentSocialSecurityNumber": {"type": "string"},
                "dependentIdentifier": {"type": "string"},
                "dependentName": {"$ref": "#/$defs/StructuredPersonName"},
                "dependentRelationshipTypeCode": {"$ref": "#/$defs/DependentRelationshipType"},
                "dependentGenderCode": {"$ref": "#/$defs/Gender"},
                "dependentBirthDate": {
                    "type": "string",
                    "format": "date"
                },
                "dependentHomePhone": {"type": "string"},
                "dependentWorkPhone": {"type": "string"},
                "dependentMobilePhone": {"type": "string"},
                "dependentMailingAddress": {"$ref": "#/$defs/PostalAddress"},
                "dependentHomeEmail": {"type": "string"},
                "disabilityIndicator": {"type": "boolean"},
                "studentStatusCode": {"$ref": "#/$defs/StudentStatus"},
                "dependentTobaccoUseCode": {"$ref": "#/$defs/TobaccoUse"},
                "dependentEventID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                }
            }
        },
        "OtherParty": {
            "type": "object",
            "required": [
                "otherPartyID",
                "otherPartyRelationshipTypeCode",
                "otherPartyTypeCode"
            ],
            "properties": {
                "otherPartyID": {"type": "string"},
                "otherPartyIndividualTaxpayerIdentificationNumber": {"type": "string"},
                "otherPartyFederalEmployerIdentificationNumber": {"type": "string"},
                "otherPartySocialSecurityNumber": {"type": "string"},
                "otherPartyOrganizationName": {"type": "string"},
                "otherPartyPersonName": {"$ref": "#/$defs/StructuredPersonName"},
                "otherPartyRelationshipTypeCode": {"$ref": "#/$defs/BeneficiaryRelationshipType"},
                "otherPartyTypeCode": {"$ref": "#/$defs/PartyType"},
                "otherPartyGenderCode": {"$ref": "#/$defs/Gender"},
                "otherPartyBirthDate": {
                    "type": "string",
                    "format": "date"
                },
                "otherPartyHomePhone": {"type": "string"},
                "otherPartyWorkPhone": {"type": "string"},
                "otherPartyMobilePhone": {"type": "string"},
                "otherPartyMailingAddress": {"$ref": "#/$defs/PostalAddress"},
                "otherPartyHomeAddress": {"$ref": "#/$defs/PostalAddress"},
                "otherPartyHomeEmail": {"type": "string"},
                "otherPartyWorkEmail": {"type": "string"},
                "otherPartyEventID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                }
            }
        },
        "Beneficiary": {
            "type": "object",
            "required": [
                "beneficiaryPartyID",
                "beneficiaryPartyTypeCode",
                "beneficiaryPercent",
                "beneficiaryTypeCode"
            ],
            "properties": {
                "beneficiaryPartyID": {"type": "string"},
                "beneficiaryPartyTypeCode": {"$ref": "#/$defs/BeneficiaryPartyType"},
                "beneficiaryPercent": {"type": "number"},
                "beneficiaryTypeCode": {"$ref": "#/$defs/BeneficiaryType"}
            }
        },
        "BeneficiaryGroup": {
            "type": "object",
            "required": [
                "beneficiaryGroupID",
                "beneficiary"
            ],
            "properties": {
                "beneficiaryGroupID": {"type": "string"},
                "beneficiary": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Beneficiary"},
                    "minItems": 1
                }
            }
        },
        "CarrierProducerNumber": {
            "type": "object",
            "required": [
                "carrierProducerNumberID",
                "carrierID",
                "carrierProducerNumber"
            ],
            "properties": {
                "carrierProducerNumberID": {"type": "string"},
                "carrierID": {"type": "string"},
                "carrierProducerNumber": {"type": "string"}
            }
        },
        "ProducerLicense": {
            "type": "object",
            "required": [
                "producerLicenseID",
                "producerLicenseTypeCode",
                "producerLicenseStateProvinceCode",
                "producerLicenseNumber"
            ],
            "properties": {
                "producerLicenseID": {"type": "string"},
                "producerLicenseTypeCode": {"$ref": "#/$defs/ProducerLicenseType"},
                "producerLicenseStateProvinceCode": {"$ref": "#/$defs/StateProvince"},
                "producerLicenseNumber": {"type": "string"}
            }
        },
        "Producer": {
            "type": "object",
            "required": ["producerPartyID"],
            "properties": {
                "producerPartyID": {"type": "string"},
                "producerFirstName": {"type": "string"},
                "producerLastName": {"type": "string"},
                "carrierProducerNumber": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CarrierProducerNumber"},
                    "minItems": 0
                },
                "producerLicense": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/ProducerLicense"},
                    "minItems": 0
                }
            }
        },
        "Provider": {
            "type": "object",
            "properties": {
                "providerIdentificationNumberText": {"type": "string"},
                "providerName": {"type": "string"},
                "existingPatientIndicator": {"type": "boolean"}
            }
        },
        "CoverageInsured": {
            "type": "object",
            "required": [
                "insuredPartyID",
                "primaryInsuredIndicator",
                "insuredCoverageEffectiveDate"
            ],
            "properties": {
                "insuredPartyID": {"type": "string"},
                "primaryInsuredIndicator": {"type": "boolean"},
                "tobaccoUseIndicator": {"type": "boolean"},
                "insuredCoverageEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "insuredCoverageTerminationDate": {
                    "type": "string",
                    "format": "date"
                },
                "provider": {"$ref": "#/$defs/Provider"}
            }
        },
        "CoverageRider": {
            "type": "object",
            "required": [
                "coverageRiderID",
                "riderIdentifier"
            ],
            "properties": {
                "coverageRiderID": {"type": "string"},
                "riderIdentifier": {"type": "string"},
                "riderName": {"type": "string"},
                "coverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "benefitAmount": {"type": "number"},
                "riderUnitQuantity": {"type": "number"},
                "riderOptionIdentifier": {"type": "string"},
                "preTaxCode": {"$ref": "#/$defs/PreTax"},
                "employeePremiumContributionAmount": {"type": "number"},
                "employerPremiumContributionAmount": {"type": "number"},
                "riderEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "riderTerminationDate": {
                    "type": "string",
                    "format": "date"
                }
            }
        },
        "ElectedCoverage": {
            "type": "object",
            "properties": {
                "electedCoverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "electedBenefitCalculationMethodCode": {"$ref": "#/$defs/BenefitCalculationMethod"},
                "electedBenefitAmount": {"type": "number"},
                "electedBenefitFactor": {"type": "number"},
                "electedEmployeeContributionCode": {"$ref": "#/$defs/EmployeeContribution"},
                "electedEmployeePremiumContributionAmount": {"type": "number"},
                "electedEmployerPremiumContributionAmount": {"type": "number"},
                "electedTotalPlanPremiumAmount": {"type": "number"},
                "electedCoverageInsured": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageInsured"},
                    "minItems": 0
                },
                "electedCoverageRider": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageRider"},
                    "minItems": 0
                }
            }
        },
        "CoverageQuestionAnswer": {
            "type": "object",
            "required": [
                "coverageQuestionAnswerID",
                "questionIdentifier",
                "answerPartyID"
            ],
            "properties": {
                "coverageQuestionAnswerID": {"type": "string"},
                "questionIdentifier": {"type": "string"},
                "answerIndicator": {"type": "boolean"},
                "answerText": {"type": "string"},
                "answerPartyID": {"type": "string"}
            }
        },
        "CoverageDeduction": {
            "type": "object",
            "properties": {
                "deductionTypeCode": {
                    "type": "string",
                    "enum": [
                        "Medical",
                        "401(k)",
                        "StateIncomeTax",
                        "PAUnemploymentTax",
                        "LocalityTax",
                        "Dental",
                        "Life",
                        "PensionContribution1",
                        "PensionContribution2",
                        "StateIncomeTax2",
                        "StateIncomeTax3",
                        "FreeForm1",
                        "FreeForm2",
                        "FreeForm3",
                        "FreeForm4",
                        "FreeForm5",
                        "SupplementalLife",
                        "LegalServices",
                        "Health",
                        "Hospitalization",
                        "Disability",
                        "FreeForm",
                        "AccidentalDeath&Dismemberment",
                        "Vision",
                        "RemimbursementHealthcareExpenses",
                        "DependentCareAssistance",
                        "AdoptiveAssistance",
                        "Multiple401K",
                        "PensionContribution",
                        "NJUmeploymentTax",
                        "AKUnemploymentTax",
                        "StateLeaveContribution",
                        "UnemploymentTax"
                    ]
                },
                "deductionAmount": {"type": "number"},
                "deductionFrequencyQuantity": {"type": "integer"},
                "deductionTypeFreeFormValueString": {"type": "string"},
                "deductionTypeStateProvinceCode": {"$ref": "#/$defs/StateProvince"},
                "deductionTypePreTaxCode": {"$ref": "#/$defs/PreTax"}
            }
        },
        "CoverageCompensationSplit": {
            "type": "object",
            "required": [
                "carrierProducerNumberID",
                "compensationSplitPercent"
            ],
            "properties": {
                "carrierProducerNumberID": {"type": "string"},
                "compensationSplitPercent": {"type": "number"}
            }
        },
        "CoverageChange": {
            "type": "object",
            "properties": {
                "changeCoverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "changeBenefitCalculationMethodCode": {"$ref": "#/$defs/BenefitCalculationMethod"},
                "changeBenefitAmount": {"type": "number"},
                "changeBenefitFactor": {"type": "number"},
                "changeEmployeeContributionCode": {"$ref": "#/$defs/EmployeeContribution"},
                "changeEmployeePremiumContributionAmount": {"type": "number"},
                "changeEmployerPremiumContributionAmount": {"type": "number"},
                "changeTotalPlanPremiumAmount": {"type": "number"},
                "priorCoverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "priorBenefitCalculationMethodCode": {"$ref": "#/$defs/BenefitCalculationMethod"},
                "priorBenefitAmount": {"type": "number"},
                "priorBenefitFactor": {"type": "number"},
                "priorEmployeeContributionCode": {"$ref": "#/$defs/EmployeeContribution"},
                "priorEmployeePremiumContributionAmount": {"type": "number"},
                "priorEmployerPremiumContributionAmount": {"type": "number"},
                "priorTotalPlanPremiumAmount": {"type": "number"},
                "priorCoverageInsured": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageInsured"},
                    "minItems": 0
                },
                "priorCoverageRider": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageRider"},
                    "minItems": 0
                }
            }
        },
        "CoverageContribution": {
            "type": "object",
            "properties": {
                "contributionLimitTypeCode": {"$ref": "#/$defs/ContributionLimitType"},
                "planYearEmployeeContributionAmount": {"type": "number"},
                "planYearEmployerContributionAmount": {"type": "number"},
                "toDateEmployeeContributionAmount": {"type": "number"},
                "toDateEmployerContributionAmount": {"type": "number"},
                "contributionsRemainingQuantity": {"type": "integer"}
            }
        },
        "Coverage": {
            "type": "object",
            "required": [
                "coverageID",
                "coverageEffectiveDate",
                "carrierID",
                "coverageInsured"
            ],
            "properties": {
                "coverageID": {"type": "string"},
                "groupPolicyNumber": {"type": "string"},
                "individualPolicyNumber": {"type": "string"},
                "productTypeCode": {"$ref": "#/$defs/ProductType"},
                "benefitPlanIdentifier": {"type": "string"},
                "benefitClassIdentifier": {"type": "string"},
                "benefitSubClassIdentifier": {"type": "string"},
                "billGroupIdentifier": {"type": "string"},
                "billSubGroupIdentifier": {"type": "string"},
                "claimGroupIdentifier": {"type": "string"},
                "claimSubGroupIdentifier": {"type": "string"},
                "originalCoverageEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "coverageEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "coverageTerminationDate": {
                    "type": "string",
                    "format": "date"
                },
                "coverageTierCode": {"$ref": "#/$defs/CoverageTier"},
                "benefitCalculationMethodCode": {"$ref": "#/$defs/BenefitCalculationMethod"},
                "benefitAmount": {"type": "number"},
                "benefitFactor": {"type": "number"},
                "employeeContributionCode": {"$ref": "#/$defs/EmployeeContribution"},
                "employeePremiumContributionAmount": {"type": "number"},
                "employerPremiumContributionAmount": {"type": "number"},
                "totalPlanPremiumAmount": {"type": "number"},
                "benefitEarningsAmount": {"type": "number"},
                "benefitEarningsEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "benefitModeQuantity": {"type": "integer"},
                "paymentMethodCode": {"$ref": "#/$defs/PaymentMethod"},
                "premiumModeQuantity": {"type": "integer"},
                "preTaxCode": {"$ref": "#/$defs/PreTax"},
                "issueAgeQuantity": {"type": "integer"},
                "tobaccoUseIndicator": {"type": "boolean"},
                "takeOverCoverageIndicator": {"type": "boolean"},
                "enrollerProducerPartyID": {"type": "string"},
                "beneficiaryGroupID": {"type": "string"},
                "carrierID": {"type": "string"},
                "electedCoverage": {"$ref": "#/$defs/ElectedCoverage"},
                "coverageInsured": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageInsured"},
                    "minItems": 1
                },
                "coverageEventID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                },
                "coverageQuestionAnswer": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageQuestionAnswer"},
                    "minItems": 0
                },
                "coverageDeduction": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageDeduction"},
                    "minItems": 0
                },
                "coverageRider": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageRider"},
                    "minItems": 0
                },
                "coverageCommissionSplit": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CoverageCompensationSplit"},
                    "minItems": 0
                },
                "signatureDate": {
                    "type": "string",
                    "format": "date"
                },
                "coverageTransactionDate": {
                    "type": "string",
                    "format": "date"
                },
                "cobraIndicator": {"type": "boolean"},
                "enrollmentInformationID": {"type": "string"},
                "coverageChange": {"$ref": "#/$defs/CoverageChange"},
                "coverageContribution": {"$ref": "#/$defs/CoverageContribution"},
                "cobraEffectiveDate": {
                    "type": "string",
                    "format": "date"
                },
                "cobraPaidThroughDate": {
                    "type": "string",
                    "format": "date"
                }
            }
        },
        "FormSignature": {
            "type": "object",
            "required": [
                "formSignatureID",
                "formSignatureDate",
                "formSignatureDataTypeCode"
            ],
            "properties": {
                "formSignatureID": {"type": "string"},
                "formSignatureDate": {
                    "type": "string",
                    "format": "date"
                },
                "formSignatureDataTypeCode": {"$ref": "#/$defs/FormSignatureDataType"},
                "formSignatureData": {
                    "type": "array",
                    "items": {"type": "integer"}
                },
                "signedByEmployeePartyID": {"type": "string"},
                "signedByDependentPartyID": {"type": "string"},
                "signedByProducerPartyID": {"type": "string"}
            }
        },
        "Form": {
            "type": "object",
            "required": [
                "formID",
                "formName",
                "formCreationDate",
                "formCompletionDate",
                "formTypeCode",
                "formDataTypeCode",
                "formCoverageID"
            ],
            "properties": {
                "formID": {"type": "string"},
                "formName": {"type": "string"},
                "formNumber": {"type": "string"},
                "formCreationDate": {
                    "type": "string",
                    "format": "date"
                },
                "formCompletionDate": {
                    "type": "string",
                    "format": "date"
                },
                "formTypeCode": {"$ref": "#/$defs/FormType"},
                "formDataTypeCode": {"$ref": "#/$defs/FormDataType"},
                "formData": {
                    "type": "array",
                    "items": {"type": "integer"}
                },
                "formDataGUID": {"type": "string"},
                "formSignature": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/FormSignature"},
                    "minItems": 0
                },
                "formCoverageID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 1
                }
            }
        },
        "Employee": {
            "type": "object",
            "required": [
                "employeePartyID",
                "employeeName",
                "employeeGenderCode",
                "employeeBirthDate",
                "employeeMailingAddress"
            ],
            "properties": {
                "employeePartyID": {"type": "string"},
                "employeeIndividualTaxpayerIdentificationNumber": {"type": "string"},
                "employeeSocialSecurityNumber": {"type": "string"},
                "employeeIdentifier": {"type": "string"},
                "employeeName": {"$ref": "#/$defs/StructuredPersonName"},
                "employeeGenderCode": {"$ref": "#/$defs/Gender"},
                "employeeBirthDate": {
                    "type": "string",
                    "format": "date"
                },
                "maritalStatusCode": {"$ref": "#/$defs/MaritalStatus"},
                "employeeTobaccoUseCode": {"$ref": "#/$defs/TobaccoUse"},
                "employeeHomePhone": {"type": "string"},
                "employeeWorkPhone": {"type": "string"},
                "employeeMobilePhone": {"type": "string"},
                "employeeMailingAddress": {"$ref": "#/$defs/PostalAddress"},
                "employeeHomeAddress": {"$ref": "#/$defs/PostalAddress"},
                "employeeWorkAddress": {"$ref": "#/$defs/PostalAddress"},
                "employeeEmail": {"type": "string"},
                "employeeAlternateEmail": {"type": "string"},
                "employmentInformation": {"$ref": "#/$defs/EmploymentInformation"},
                "enrollmentInformation": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/EnrollmentInformation"},
                    "minItems": 0
                },
                "event": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Event"},
                    "minItems": 0
                },
                "employeeEventID": {
                    "type": "array",
                    "items": {"type": "string"},
                    "minItems": 0
                },
                "dependent": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Dependent"},
                    "minItems": 0
                },
                "otherParty": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/OtherParty"},
                    "minItems": 0
                },
                "beneficiaryGroup": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/BeneficiaryGroup"},
                    "minItems": 0
                },
                "producer": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Producer"},
                    "minItems": 0
                },
                "coverage": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Coverage"},
                    "minItems": 0
                },
                "employeeForm": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Form"},
                    "minItems": 0
                },
                "disabilityIndicator": {"type": "boolean"}
            }
        },
        "Employer": {
            "type": "object",
            "required": [
                "employerPartyID",
                "carrierMasterAgreementNumber",
                "employerName",
                "employee"
            ],
            "properties": {
                "employerPartyID": {"type": "string"},
                "federalEmployerIdentificationNumber": {"type": "string"},
                "carrierMasterAgreementNumber": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/CarrierMasterAgreementNumber"},
                    "minItems": 1
                },
                "employerName": {"type": "string"},
                "employerAddress": {"$ref": "#/$defs/PostalAddress"},
                "employee": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Employee"},
                    "minItems": 1
                }
            }
        },
        "Audit": {
            "type": "object",
            "properties": {
                "auditID": {"type": "string"},
                "carrierRecordQuantity": {"type": "integer"},
                "employerRecordQuantity": {"type": "integer"},
                "employeeRecordQuantity": {"type": "integer"},
                "dependentRecordQuantity": {"type": "integer"},
                "otherPartyRecordQuantity": {"type": "integer"},
                "beneficiaryGroupRecordQuantity": {"type": "integer"},
                "eventRecordQuantity": {"type": "integer"},
                "coverageRecordQuantity": {"type": "integer"},
                "coverageRiderRecordQuantity": {"type": "integer"},
                "employeeFormRecordQuantity": {"type": "integer"}
            }
        },
        "Transmission": {
            "type": "object",
            "required": [
                "transmissionGUID",
                "senderName",
                "senderPlatformName",
                "receiverName",
                "creationDateTime",
                "testProductionCode",
                "transmissionTypeCode",
                "schemaVersionIdentifier",
                "carrier",
                "employer"
            ],
            "properties": {
                "transmissionGUID": {"type": "string"},
                "senderName": {"type": "string"},
                "senderPlatformName": {"type": "string"},
                "receiverName": {"type": "string"},
                "creationDateTime": {
                    "type": "string",
                    "format": "date-time"
                },
                "testProductionCode": {"$ref": "#/$defs/TestProduction"},
                "transmissionTypeCode": {"$ref": "#/$defs/TransmissionType"},
                "schemaVersionIdentifier": {"type": "string"},
                "carrier": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Carrier"},
                    "minItems": 1
                },
                "employer": {
                    "type": "array",
                    "items": {"$ref": "#/$defs/Employer"},
                    "minItems": 1
                },
                "audit": {"$ref": "#/$defs/Audit"}
            }
        },
        "TestProduction": {
            "type": "string",
            "enum": [
                "Production",
                "Test"
            ]
        },
        "TransmissionType": {
            "type": "string",
            "enum": [
                "ActivesOnly",
                "ChangesOnly",
                "FullFile"
            ]
        },
        "StateProvince": {
            "type": "string",
            "enum": [
                "AL",
                "AK",
                "AZ",
                "AR",
                "CA",
                "CO",
                "CT",
                "DE",
                "FL",
                "GA",
                "HI",
                "ID",
                "IL",
                "IN",
                "IA",
                "KS",
                "KY",
                "LA",
                "ME",
                "MD",
                "MA",
                "MI",
                "MN",
                "MS",
                "MO",
                "MT",
                "NE",
                "NV",
                "NH",
                "NJ",
                "NM",
                "NY",
                "NC",
                "ND",
                "OH",
                "OK",
                "OR",
                "PA",
                "RI",
                "SC",
                "SD",
                "TN",
                "TX",
                "UT",
                "VT",
                "VA",
                "WA",
                "WV",
                "WI",
                "WY",
                "DC",
                "AS",
                "GU",
                "MP",
                "PR",
                "UM",
                "VI",
                "AB",
                "BC",
                "MB",
                "NB",
                "NL",
                "NS",
                "ON",
                "PE",
                "QC",
                "SK",
                "NT",
                "NU",
                "YT",
                "AG",
                "BS",
                "CM",
                "CS",
                "CH",
                "CL",
                "DF",
                "DG",
                "GT",
                "GR",
                "HG",
                "JA",
                "EM",
                "NA",
                "OA",
                "PU",
                "QT",
                "QR",
                "SL",
                "SI",
                "SO",
                "TB",
                "TM",
                "TL",
                "VE",
                "YU",
                "ZA",
                "AA",
                "AE",
                "AP"
            ]
        },
        "Country": {
            "type": "string",
            "enum": [
                "AF",
                "AL",
                "DZ",
                "AD",
                "AO",
                "AG",
                "AR",
                "AM",
                "AU",
                "AT",
                "AZ",
                "BS",
                "BH",
                "BD",
                "BB",
                "BY",
                "BE",
                "BZ",
                "BJ",
                "BT",
                "BO",
                "BA",
                "BW",
                "BR",
                "BN",
                "BG",
                "BF",
                "BI",
                "CV",
                "KH",
                "CM",
                "CA",
                "CF",
                "TD",
                "CL",
                "CN",
                "CO",
                "KM",
                "CG",
                "CD",
                "CR",
                "CI",
                "HR",
                "CU",
                "CY",
                "CZ",
                "DK",
                "DJ",
                "DM",
                "DO",
                "EC",
                "EG",
                "SV",
                "GQ",
                "ER",
                "EE",
                "SZ",
                "ET",
                "FJ",
                "FI",
                "FR",
                "GA",
                "GM",
                "GE",
                "DE",
                "GH",
                "GR",
                "GD",
                "GT",
                "GN",
                "GW",
                "GY",
                "HT",
                "VA",
                "HN",
                "HU",
                "IS",
                "IN",
                "ID",
                "IR",
                "IQ",
                "IE",
                "IL",
                "IT",
                "JM",
                "JP",
                "JO",
                "KZ",
                "KE",
                "KI",
                "KP",
                "KR",
                "KW",
                "KG",
                "LA",
                "LV",
                "LB",
                "LS",
                "LR",
                "LY",
                "LI",
                "LT",
                "LU",
                "MG",
                "MW",
                "MY",
                "MV",
                "ML",
                "MT",
                "MH",
                "MR",
                "MU",
                "MX",
                "FM",
                "MD",
                "MC",
                "MN",
                "ME",
                "MA",
                "MZ",
                "MM",
                "NA",
                "NR",
                "NP",
                "NL",
                "NZ",
                "NI",
                "NE",
                "NG",
                "MK",
                "NO",
                "OM",
                "PK",
                "PW",
                "PA",
                "PG",
                "PY",
                "PE",
                "PH",
                "PL",
                "PT",
                "QA",
                "RO",
                "RU",
                "RW",
                "KN",
                "LC",
                "VC",
                "WS",
                "SM",
                "ST",
                "SA",
                "SN",
                "RS",
                "SC",
                "SL",
                "SG",
                "SK",
                "SI",
                "SB",
                "SO",
                "ZA",
                "SS",
                "ES",
                "LK",
                "SD",
                "SR",
                "SE",
                "CH",
                "SY",
                "TJ",
                "TZ",
                "TH",
                "TL",
                "TG",
                "TO",
                "TT",
                "TN",
                "TR",
                "TM",
                "TV",
                "UG",
                "UA",
                "AE",
                "GB",
                "US",
                "USA",
                "UY",
                "UZ",
                "VU",
                "VE",
                "VN",
                "YE",
                "ZM",
                "ZW",
                "OTH"
            ]
        },
        "Prefix": {
            "type": "string",
            "enum": [
                "Dr",
                "Mr",
                "Mrs",
                "Ms",
                "Rev"
            ]
        },
        "Suffix": {
            "type": "string",
            "enum": [
                "Jr",
                "Sr",
                "III",
                "II",
                "IV",
                "V",
                "VI",
                "MD",
                "DDS",
                "PHD",
                "Other"
            ]
        },
        "Gender": {
            "type": "string",
            "enum": [
                "Female",
                "Male",
                "NonBinary",
                "NotSpecified"
            ]
        },
        "MaritalStatus": {
            "type": "string",
            "enum": [
                "Divorced",
                "DomesticPartner",
                "LegallySeparated",
                "Married",
                "Single",
                "Widowed",
                "Unknown"
            ]
        },
        "TobaccoUse": {
            "type": "string",
            "enum": [
                "N",
                "Y"
            ]
        },
        "Exempt": {
            "type": "string",
            "enum": [
                "Exempt",
                "Nonexempt"
            ]
        },
        "EmploymentType": {
            "type": "string",
            "enum": [
                "Contract",
                "FullTime",
                "PartTime",
                "PerDiem",
                "Seasonal"
            ]
        },
        "EmploymentStatus": {
            "type": "string",
            "enum": [
                "Active",
                "LeaveOfAbsencePaid",
                "LeaveOfAbsenceUnpaid",
                "MilitaryLeave",
                "OnDI",
                "Retiree",
                "Terminated"
            ]
        },
        "TerminationReason": {
            "type": "string",
            "enum": [
                "ClientTermination-NotCOBRAEligible",
                "ContractEnded-NotCOBRAEligible",
                "Death",
                "DirectHire-NotCOBRAEligible",
                "Layoff",
                "LeaveOfAbsence",
                "LongTermDisability",
                "Medicare/Termination",
                "MilitaryLeave",
                "Retirement",
                "Seasonal-NotCOBRAEligible",
                "Termination",
                "Termination-Declined",
                "Termination-Dissatisfaction",
                "Termination-EligibleForRehire",
                "Terminated-GrossMisconduct",
                "Termination-IneligibleForRehire",
                "Termination-Invol.Separation",
                "Terminated- JobAbandonment",
                "Termination-Mutual",
                "Terminated-NoCall/NoShow",
                "Termination-NotCOBRAEligible",
                "Termination-Other Employment",
                "Termination-Performance",
                "Termination-PositionEliminated",
                "Termination-Relocation",
                "Termination-SeveranceOption",
                "Termination-Voluntary",
                "Other"
            ]
        },
        "WorkHoursFrequency": {
            "type": "string",
            "enum": [
                "Annual",
                "Monthly",
                "Weekly"
            ]
        },
        "IncomeType": {
            "type": "string",
            "enum": [
                "AdditionalCompensation",
                "BenefitSalary",
                "Bonus",
                "Commission",
                "DifferentialPay",
                "FrozenPay",
                "Salary",
                "TotalCompensation"
            ]
        },
        "CategoryType": {
            "type": "string",
            "enum": [
                "GroupStructure",
                "Reporting"
            ]
        },
        "ContactType": {
            "type": "string",
            "enum": [
                "AdditionalReportingContact",
                "BenefitsCoordinator",
                "ReportingManager",
                "UnionContact"
            ]
        },
        "WorkShift": {
            "type": "string",
            "enum": [
                "First",
                "Second",
                "Third",
                "Fixed",
                "On-Call",
                "Rotate",
                "Split"
            ]
        },
        "WorkScheduleType": {
            "type": "string",
            "enum": [
                "Weekly",
                "Holiday",
                "Overtime"
            ]
        },
        "EnrollmentMethod": {
            "type": "string",
            "enum": [
                "CallCenter",
                "HumanResources",
                "InternetOrSelfService",
                "Mobile",
                "OneOnOne",
                "Paper"
            ]
        },
        "EventType": {
            "type": "string",
            "enum": [
                "Coverage",
                "Demographic",
                "Employment"
            ]
        },
        "EventTypeReason": {
            "type": "string",
            "enum": [
                "AdministrativeCorrection",
                "Cancellation",
                "ChildNoLongerEligible",
                "ClientTermination",
                "COBRAActivation",
                "COBRAEndCoverage",
                "InitialEnrollment",
                "LateEntrant",
                "LossOfSpousesBenefitEligibility",
                "New",
                "NewHireEnrollment",
                "NewRetireeEnrollment",
                "NonPaymentOfContributions",
                "OpenEnrollment",
                "QualifyingLifeEventEnrollment",
                "Reenrollment",
                "Reinstatement",
                "RetirementEndCoverage",
                "Rollover",
                "CoverageChange",
                "VoluntaryCancel",
                "BirthAdoption",
                "DependentDeath",
                "DependentRegainsEligibility",
                "Death",
                "DemographicChange",
                "DivorceLegalSeparation",
                "Marriage",
                "TobaccoChange",
                "ZipLocationChange",
                "BenefitClassChange",
                "CommencePaidFMLALeave",
                "CommencePaidNonFMLALeave",
                "LossOfBenefitStatus",
                "Rehire",
                "Retirement",
                "ReturnFromFMLALeave",
                "IncreaseInCoverage",
                "ReductionInCoverage",
                "Termination",
                "LongTermDisability",
                "NewHire",
                "Unknown",
                "ShortTermDisability",
                "CommenceUnpaidFMLALeave",
                "GainOfBenefitStatus",
                "ReturnFromNonFMLALeave",
                "AgeLimitAttained",
                "AgeRelatedReductionInCoverage",
                "BeneficiaryChange"
            ]
        },
        "DependentRelationshipType": {
            "type": "string",
            "enum": [
                "AdultDependent",
                "Child",
                "CollateralDependent",
                "CustodialGrandchild",
                "FormerSpouseOrPartner",
                "Grandchild",
                "Partner",
                "Spouse",
                "Stepchild",
                "Other"
            ]
        },
        "StudentStatus": {
            "type": "string",
            "enum": [
                "FullTime",
                "PartTime",
                "NonStudent"
            ]
        },
        "BeneficiaryRelationshipType": {
            "type": "string",
            "enum": [
                "Child",
                "Spouse",
                "Sibling",
                "Parent",
                "Grandparent",
                "Grandchild",
                "Fiance",
                "DomesticPartner",
                "Friend",
                "Other",
                "Trust",
                "Husband",
                "Wife",
                "Sister",
                "Brother",
                "Son",
                "Daughter",
                "Uncle",
                "Aunt",
                "MotherInLaw",
                "FatherInLaw",
                "CivilUnionPartner",
                "Father",
                "Mother",
                "Estate",
                "Charity",
                "Cousin",
                "OtherRelative",
                "Self",
                "ExSpouse",
                "Employer",
                "Girlfriend",
                "Boyfriend",
                "Stepdaughter",
                "Stepson",
                "Corporation"
            ]
        },
        "PartyType": {
            "type": "string",
            "enum": [
                "Charity",
                "Estate",
                "Person",
                "Trust",
                "Other"
            ]
        },
        "BeneficiaryPartyType": {
            "type": "string",
            "enum": [
                "Dependent",
                "Employee",
                "OtherParty"
            ]
        },
        "BeneficiaryType": {
            "type": "string",
            "enum": [
                "Primary",
                "Secondary"
            ]
        },
        "ProducerLicenseType": {
            "type": "string",
            "enum": [
                "LifeHealth",
                "Health"
            ]
        },
        "ProductType": {
            "type": "string",
            "enum": [
                "Accident",
                "AD&D",
                "Cancer",
                "CriticalIllness",
                "Dental",
                "Hospital",
                "Life",
                "LifestyleBenefits",
                "LTD",
                "Medical",
                "PFL",
                "STD",
                "SavingsSpendingAccounts",
                "StateDI",
                "Vision"
            ]
        },
        "CoverageTier": {
            "type": "string",
            "enum": [
                "ChildOnly",
                "Employee",
                "EmployeeChildren",
                "EmployeeDependent",
                "EmployeeFamily",
                "EmployeeSpouse",
                "Employee2Dependents",
                "SpouseChildren",
                "SpouseDependent",
                "SpouseOnly"
            ]
        },
        "BenefitCalculationMethod": {
            "type": "string",
            "enum": [
                "FlatAmount",
                "MultipleOfSalary",
                "Percentage"
            ]
        },
        "EmployeeContribution": {
            "type": "string",
            "enum": [
                "EmployeePaid",
                "EmployeePartial",
                "EmployerPaid"
            ]
        },
        "PaymentMethod": {
            "type": "string",
            "enum": [
                "DirectBill",
                "PayrollDeduction"
            ]
        },
        "PreTax": {
            "type": "string",
            "enum": [
                "PostTax",
                "PreTax"
            ]
        },
        "ContributionLimitType": {
            "type": "string",
            "enum": [
                "Single",
                "Family"
            ]
        },
        "FormType": {
            "type": "string",
            "enum": [
                "Application",
                "BeneficiaryDesignation",
                "CoverageOutline",
                "DependentStatusDeclaration",
                "Disclosure",
                "EmployeeConsent",
                "EnrollmentConfirmation",
                "LeaveAuthorization",
                "PaidTimeOffAuthorization",
                "PaymentAuthorization",
                "Replacement",
                "SpousalConsent",
                "Other"
            ]
        },
        "FormDataType": {
            "type": "string",
            "enum": [
                "PDF",
                "Image"
            ]
        },
        "FormSignatureDataType": {
            "type": "string",
            "enum": [
                "ClickToSign",
                "Image",
                "Password",
                "Recording",
                "ThirdPartySignature",
                "Topaz",
                "VoicePrint"
            ]
        }
    }
}
```

*Evidence of Insurability*

Api endpoint to pass an evidence of insurability decision to a Ben Admin Tech Partner to ensure they have the most current decision for an insured's coverage used to send an EOI Application Status Inquiry request to the Carrier to ensure their that system has the most current Application status for an insured's coverage and carrier sends back EOI Application Status Update to the  Partner


**Parameters**

| Name                                          | Description                                                                                          |
|:----------------------------------------------|:-----------------------------------------------------------------------------------------------------|
| TransmissionGUID                              | Global Unique identifier created by the originator for this instance of                              |
|                                               |             the electronic transmission.                                                             |
| SenderName                                    | Name of the company sending electronic benefit records on behalf of a                                |
|                                               |             group.                                                                                   |
| SenderPlatformName                            | Identifies the Sender's system that is providing the data for this data                              |
|                                               |             set.                                                                                     |
| ReceiverName                                  | Name of the company receiving electronic benefit records on behalf of a                              |
|                                               |             group.                                                                                   |
| CreationDateTime                              | UTC date and time the transmission was created.                                                      |
| TestProductionCode                            | Indicates the elements contained in this data set are production or                                  |
|                                               |             test data.                                                                               |
| TransmissionTypeCode                          | The type of data contained within the data set.                                                      |
| SchemaVersionIdentifier                       | Identifies the version of the standard that is being adhered to in this                              |
|                                               |             data set.                                                                                |



## EOI Request.<img style="float: right;" img src="/images/post.png" alt="post" style="height: 20px; width:20apx;"/>
![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /eois/...`

<aside class="success">
This operation requires authentication
</aside>

> Code samples


*Evidence of Insurability application*

Api endpoint to pass an evidence of insurability decision to a Ben Admin Tech Partner to ensure they have the most current decision for an insured's coverage used to send an EOI Application Status Inquiry request to the Carrier to ensure their that system has the most current Application status for an insured's coverage and carrier sends back EOI Application Status Update to the  Partner

```shell
# You can also use wget
curl -X POST /eois/..
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'

```

``` json
{
  "bem:Transmission": {
    "@xmlns:bem": "https://ldex.limra.com/xsd/1.0/LDExBEM",
    "@xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
    "@xsi:schemaLocation": "https://ldex.limra.com/xsd/1.0/LDExBEM_1.3.2022.01.01.xsd",
    "TransmissionGUID": "c9fcc147-2b65-48cb-925c-9f898a381815",
    "SenderName": "ABC Tech Partner",
    "SenderPlatformName": "ABC Platform",
    "ReceiverName": "XYZ Carrier",
    "CreationDateTime": "2020-01-17T21:56:36.2619791Z",
    "TestProductionCode": "Production",
    "TransmissionTypeCode": "ChangesOnly",
    "SchemaVersionIdentifier": "1.3.2022.01.01",
    "Carrier": {
      "CarrierID": "1234",
      "CarrierName": "XYZ Carrier"
    },
    "Employer": {
      "EmployerPartyID": "9999",
      "FederalEmployerIdentificationNumber": "99-9999999",
      "CarrierMasterAgreementNumber": {
        "CarrierMasterAgreementNumberID": "AS-M009-87788",
        "MasterAgreementNumber": "M009-87788",
        "CarrierID": "1234"
      },
      "EmployerName": "Sweet Insurance 4 U",
      "EmployerAddress": {
        "FirstLineAddress": "3345 East 45th Street",
        "CityName": "Oklahoma City",
        "StateProvinceCode": "OK",
        "PostalCode": "73112"
      },
      "Employee": {
        "EmployeePartyID": "45",
        "EmployeeSocialSecurityNumber": "998-70-0423",
        "EmployeeIdentifier": "42",
        "EmployeeName": {
          "FirstName": "Sylvia",
          "LastName": "Baldwin"
        },
        "EmployeeGenderCode": "Female",
        "EmployeeBirthDate": "2000-02-08",
        "MaritalStatusCode": "Married",
        "EmployeeTobaccoUseCode": "N",
        "EmployeeHomePhone": "8005257897",
        "EmployeeWorkPhone": "4051234567",
        "EmployeeMobilePhone": "8005257897",
        "EmployeeMailingAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeHomeAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeWorkAddress": {
          "FirstLineAddress": "62 Salt Mine Way",
          "CityName": "Salt Lake City",
          "StateProvinceCode": "UT",
          "PostalCode": "84044"
        },
        "EmployeeEmail": "s.baldwin@saltmine.com",
        "EmployeeAlternateEmail": "s.baldwin@saltmine2.com",
        "EmploymentInformation": {
          "OriginalHireDate": "2018-08-27",
          "MostRecentHireDate": "2018-08-27",
          "ExemptCode": "Exempt",
          "EmploymentTypeCode": "FullTime",
          "EmploymentStatusCode": "Active",
          "JobTitleText": "Head Custodial Engineer",
          "OccupationText": "Head Custodial Engineer",
          "WorkHoursQuantity": "40",
          "WorkHoursFrequencyCode": "Weekly",
          "WorkLocationText": "Corporate",
          "PayrollDeductionFrequencyQuantity": "26",
          "PayrollFrequencyQuantity": "26",
          "UnionIndicator": "false",
          "EmploymentIncome": {
            "IncomeTypeCode": "BenefitSalary",
            "IncomeAmount": "100000",
            "IncomeModeCode": "Annual",
            "IncomeEffectiveDate": "2019-03-20"
          }
        },
        "Event": {
          "EventID": "NHEVT1234",
          "EventTypeCode": "Coverage",
          "EventTypeReasonCode": "NewHireEnrollment",
          "EventDate": "2020-02-10",
          "TransactionDate": "2020-02-05"
        },
        "EmployeeEventID": "NHEVT1234",
        "Dependent": {
          "DependentPartyID": "1522222",
          "DependentSocialSecurityNumber": "997-87-8712",
          "DependentIdentifier": null,
          "DependentName": {
            "FirstName": "Roger",
            "LastName": "Baldwin"
          },
          "DependentRelationshipTypeCode": "Spouse",
          "DependentGenderCode": "Male",
          "DependentBirthDate": "1970-01-01",
          "DependentHomePhone": "2153654587",
          "DependentWorkPhone": "6325698745",
          "DependentMobilePhone": "2014587965",
          "DependentMailingAddress": {
            "FirstLineAddress": "4 Thresa Blvd",
            "SecondLineAddress": "Apt 4",
            "ThirdLineAddress": "Box 3",
            "CityName": "Atlanta",
            "StateProvinceCode": "GA",
            "PostalCode": "30334",
            "CountryCode": "USA"
          },
          "DependentHomeEmail": "someone@flexer.com",
          "DisabilityIndicator": "0",
          "StudentStatusCode": "PartTime",
          "DependentTobaccoUseCode": "N",
          "DependentEventID": "NHEVT1234"
        },
        "OtherParty": [
          {
            "OtherPartyID": "12001",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "256588923",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Ben",
              "LastName": "Hosper"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Male",
            "OtherPartyBirthDate": "1976-01-01",
            "OtherPartyHomePhone": "2055487896",
            "OtherPartyWorkPhone": "2548754587",
            "OtherPartyMobilePhone": "2041549625",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "8 Thresa Blvd",
              "SecondLineAddress": "Apt 3",
              "ThirdLineAddress": "Box 43",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "At.Home@monsure.com",
            "OtherPartyWorkEmail": "At.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          },
          {
            "OtherPartyID": "TT254",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "288956523",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Shelby",
              "LastName": "Cuisine"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Female",
            "OtherPartyBirthDate": "1999-05-15",
            "OtherPartyHomePhone": "2058954876",
            "OtherPartyWorkPhone": "2544588757",
            "OtherPartyMobilePhone": "2049615425",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "568 Climate Dr",
              "SecondLineAddress": null,
              "ThirdLineAddress": null,
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "SCusine@monsure.com",
            "OtherPartyWorkEmail": "ShelbyC.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          }
        ],
        "BeneficiaryGroup": [
          {
            "BeneficiaryGroupID": "UC0014",
            "Beneficiary": {
              "BeneficiaryPartyID": "45",
              "BeneficiaryPartyTypeCode": "Employee",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "2500014",
            "Beneficiary": {
              "BeneficiaryPartyID": "1522222",
              "BeneficiaryPartyTypeCode": "Dependent",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "1773200",
            "Beneficiary": [
              {
                "BeneficiaryPartyID": "1522222",
                "BeneficiaryPartyTypeCode": "Dependent",
                "BeneficiaryPercent": "75.00",
                "BeneficiaryTypeCode": "Primary"
              },
              {
                "BeneficiaryPartyID": "12001",
                "BeneficiaryPartyTypeCode": "OtherParty",
                "BeneficiaryPercent": "25.00",
                "BeneficiaryTypeCode": "Primary"
              }
            ]
          }
        ],
        "Coverage": [
          {
            "CoverageID": "2397070",
            "GroupPolicyNumber": "P123344",
            "ProductTypeCode": "Life",
            "BenefitPlanIdentifier": "W34D",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "1",
            "TakeOverCoverageIndicator": "1",
            "BeneficiaryGroupID": "UC0014",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "45",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397071",
            "GroupPolicyNumber": "P123345",
            "ProductTypeCode": "CriticalIllness",
            "BenefitPlanIdentifier": "BlueCross CI",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "Employee",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": null,
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397072",
            "GroupPolicyNumber": "P983346",
            "ProductTypeCode": "Accident",
            "BenefitPlanIdentifier": "BlueCross Hospital",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "2000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": "1773200",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          }
        ]
      }
    },
    "Audit": {
      "AuditID": "Audit1234",
      "CarrierRecordQuantity": "1",
      "EmployerRecordQuantity": "1",
      "EmployeeRecordQuantity": "1",
      "DependentRecordQuantity": "0",
      "OtherPartyRecordQuantity": "0",
      "BeneficiaryGroupRecordQuantity": "4",
      "EventRecordQuantity": "1",
      "CoverageRecordQuantity": "3",
      "CoverageRiderRecordQuantity": "0"
    }
  }
}
```

> Example Response 
> 200 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | Bad request - Payload rejected. |
| 401 | Unauthorized - action requires user authentication. |
| 403 | Forbidden - user does not have permission to upload this payload. |
| 404 | Not Found - resource not found. |
| 405 | Not Allowed - method not allowed on this resource. |
| 406 | Unacceptable - payload content type is not supported. |
| 411 | Length Required - server needs to know the size of the entity body. Specify in Content-Length header. |
| 413 | Payload too large - payload exceeds size limit (too many employees or too many employers). |
| 415 | Unsupported media type - payload type unsupported. |
| 422 | Unprocessable entity - request body failed business validation. |
| 429 | Too many requests - number of requests exceeds limit. |
| 500 | Internal Server Error - fatal error during processing of request. |
| 501 | Not implemented - BEM upload is not supported. |
| 502 | Bad Gateway - the server received an invalid response from an upstream server. |
| 503 | Service Unavailable - unable to process request at this time. Try again later. |




## EOI status.<img style="float: right;" img src="/images/post.png" alt="post" style="height: 20px; width:20apx;"/>
![#1589F0](https://via.placeholder.com/15/1589F0/1589F0.png) `# post /eois/...`

<aside class="success">
This operation requires authentication
</aside>

> Code samples


*Evidence of Insurability application status push by Carrier and Request by Ben Admin*

Api endpoint to pass an evidence of insurability decision to a Ben Admin Tech Partner to ensure they have the most current decision for an insured's coverage used to send an EOI Application Status Inquiry request to the Carrier to ensure their that system has the most current Application status for an insured's coverage and carrier sends back EOI Application Status Update to the  Partner

```shell
# You can also use wget
curl -X POST /eois/..
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'

```

``` json
{
  "bem:Transmission": {
    "@xmlns:bem": "https://ldex.limra.com/xsd/1.0/LDExBEM",
    "@xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
    "@xsi:schemaLocation": "https://ldex.limra.com/xsd/1.0/LDExBEM_1.3.2022.01.01.xsd",
    "TransmissionGUID": "c9fcc147-2b65-48cb-925c-9f898a381815",
    "SenderName": "ABC Tech Partner",
    "SenderPlatformName": "ABC Platform",
    "ReceiverName": "XYZ Carrier",
    "CreationDateTime": "2020-01-17T21:56:36.2619791Z",
    "TestProductionCode": "Production",
    "TransmissionTypeCode": "ChangesOnly",
    "SchemaVersionIdentifier": "1.3.2022.01.01",
    "Carrier": {
      "CarrierID": "1234",
      "CarrierName": "XYZ Carrier"
    },
    "Employer": {
      "EmployerPartyID": "9999",
      "FederalEmployerIdentificationNumber": "99-9999999",
      "CarrierMasterAgreementNumber": {
        "CarrierMasterAgreementNumberID": "AS-M009-87788",
        "MasterAgreementNumber": "M009-87788",
        "CarrierID": "1234"
      },
      "EmployerName": "Sweet Insurance 4 U",
      "EmployerAddress": {
        "FirstLineAddress": "3345 East 45th Street",
        "CityName": "Oklahoma City",
        "StateProvinceCode": "OK",
        "PostalCode": "73112"
      },
      "Employee": {
        "EmployeePartyID": "45",
        "EmployeeSocialSecurityNumber": "998-70-0423",
        "EmployeeIdentifier": "42",
        "EmployeeName": {
          "FirstName": "Sylvia",
          "LastName": "Baldwin"
        },
        "EmployeeGenderCode": "Female",
        "EmployeeBirthDate": "2000-02-08",
        "MaritalStatusCode": "Married",
        "EmployeeTobaccoUseCode": "N",
        "EmployeeHomePhone": "8005257897",
        "EmployeeWorkPhone": "4051234567",
        "EmployeeMobilePhone": "8005257897",
        "EmployeeMailingAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeHomeAddress": {
          "FirstLineAddress": "4 Thresa Blvd",
          "CityName": "Atlanta",
          "StateProvinceCode": "GA",
          "PostalCode": "30334"
        },
        "EmployeeWorkAddress": {
          "FirstLineAddress": "62 Salt Mine Way",
          "CityName": "Salt Lake City",
          "StateProvinceCode": "UT",
          "PostalCode": "84044"
        },
        "EmployeeEmail": "s.baldwin@saltmine.com",
        "EmployeeAlternateEmail": "s.baldwin@saltmine2.com",
        "EmploymentInformation": {
          "OriginalHireDate": "2018-08-27",
          "MostRecentHireDate": "2018-08-27",
          "ExemptCode": "Exempt",
          "EmploymentTypeCode": "FullTime",
          "EmploymentStatusCode": "Active",
          "JobTitleText": "Head Custodial Engineer",
          "OccupationText": "Head Custodial Engineer",
          "WorkHoursQuantity": "40",
          "WorkHoursFrequencyCode": "Weekly",
          "WorkLocationText": "Corporate",
          "PayrollDeductionFrequencyQuantity": "26",
          "PayrollFrequencyQuantity": "26",
          "UnionIndicator": "false",
          "EmploymentIncome": {
            "IncomeTypeCode": "BenefitSalary",
            "IncomeAmount": "100000",
            "IncomeModeCode": "Annual",
            "IncomeEffectiveDate": "2019-03-20"
          }
        },
        "Event": {
          "EventID": "NHEVT1234",
          "EventTypeCode": "Coverage",
          "EventTypeReasonCode": "NewHireEnrollment",
          "EventDate": "2020-02-10",
          "TransactionDate": "2020-02-05"
        },
        "EmployeeEventID": "NHEVT1234",
        "Dependent": {
          "DependentPartyID": "1522222",
          "DependentSocialSecurityNumber": "997-87-8712",
          "DependentIdentifier": null,
          "DependentName": {
            "FirstName": "Roger",
            "LastName": "Baldwin"
          },
          "DependentRelationshipTypeCode": "Spouse",
          "DependentGenderCode": "Male",
          "DependentBirthDate": "1970-01-01",
          "DependentHomePhone": "2153654587",
          "DependentWorkPhone": "6325698745",
          "DependentMobilePhone": "2014587965",
          "DependentMailingAddress": {
            "FirstLineAddress": "4 Thresa Blvd",
            "SecondLineAddress": "Apt 4",
            "ThirdLineAddress": "Box 3",
            "CityName": "Atlanta",
            "StateProvinceCode": "GA",
            "PostalCode": "30334",
            "CountryCode": "USA"
          },
          "DependentHomeEmail": "someone@flexer.com",
          "DisabilityIndicator": "0",
          "StudentStatusCode": "PartTime",
          "DependentTobaccoUseCode": "N",
          "DependentEventID": "NHEVT1234"
        },
        "OtherParty": [
          {
            "OtherPartyID": "12001",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "256588923",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Ben",
              "LastName": "Hosper"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Male",
            "OtherPartyBirthDate": "1976-01-01",
            "OtherPartyHomePhone": "2055487896",
            "OtherPartyWorkPhone": "2548754587",
            "OtherPartyMobilePhone": "2041549625",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "8 Thresa Blvd",
              "SecondLineAddress": "Apt 3",
              "ThirdLineAddress": "Box 43",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "At.Home@monsure.com",
            "OtherPartyWorkEmail": "At.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          },
          {
            "OtherPartyID": "TT254",
            "OtherPartyFederalEmployerIdentificationNumber": "99-9999999",
            "OtherPartySocialSecurityNumber": "288956523",
            "OtherPartyOrganizationName": "Family",
            "OtherPartyPersonName": {
              "FirstName": "Shelby",
              "LastName": "Cuisine"
            },
            "OtherPartyRelationshipTypeCode": "Cousin",
            "OtherPartyTypeCode": "Estate",
            "OtherPartyGenderCode": "Female",
            "OtherPartyBirthDate": "1999-05-15",
            "OtherPartyHomePhone": "2058954876",
            "OtherPartyWorkPhone": "2544588757",
            "OtherPartyMobilePhone": "2049615425",
            "OtherPartyMailingAddress": {
              "FirstLineAddress": "568 Climate Dr",
              "SecondLineAddress": null,
              "ThirdLineAddress": null,
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeAddress": {
              "FirstLineAddress": "1234 Bona Blvd",
              "SecondLineAddress": "Apt3454",
              "ThirdLineAddress": "Box 5",
              "CityName": "Atlanta",
              "StateProvinceCode": "GA",
              "PostalCode": "30334",
              "CountryCode": "USA"
            },
            "OtherPartyHomeEmail": "SCusine@monsure.com",
            "OtherPartyWorkEmail": "ShelbyC.Work@monsure.com",
            "OtherPartyEventID": "NHEVT1234"
          }
        ],
        "BeneficiaryGroup": [
          {
            "BeneficiaryGroupID": "UC0014",
            "Beneficiary": {
              "BeneficiaryPartyID": "45",
              "BeneficiaryPartyTypeCode": "Employee",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "2500014",
            "Beneficiary": {
              "BeneficiaryPartyID": "1522222",
              "BeneficiaryPartyTypeCode": "Dependent",
              "BeneficiaryPercent": "100.00",
              "BeneficiaryTypeCode": "Primary"
            }
          },
          {
            "BeneficiaryGroupID": "1773200",
            "Beneficiary": [
              {
                "BeneficiaryPartyID": "1522222",
                "BeneficiaryPartyTypeCode": "Dependent",
                "BeneficiaryPercent": "75.00",
                "BeneficiaryTypeCode": "Primary"
              },
              {
                "BeneficiaryPartyID": "12001",
                "BeneficiaryPartyTypeCode": "OtherParty",
                "BeneficiaryPercent": "25.00",
                "BeneficiaryTypeCode": "Primary"
              }
            ]
          }
        ],
        "Coverage": [
          {
            "CoverageID": "2397070",
            "GroupPolicyNumber": "P123344",
            "ProductTypeCode": "Life",
            "BenefitPlanIdentifier": "W34D",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "1",
            "TakeOverCoverageIndicator": "1",
            "BeneficiaryGroupID": "UC0014",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "45",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397071",
            "GroupPolicyNumber": "P123345",
            "ProductTypeCode": "CriticalIllness",
            "BenefitPlanIdentifier": "BlueCross CI",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "Employee",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "20000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": null,
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          },
          {
            "CoverageID": "2397072",
            "GroupPolicyNumber": "P983346",
            "ProductTypeCode": "Accident",
            "BenefitPlanIdentifier": "BlueCross Hospital",
            "BenefitClassIdentifier": "01",
            "BenefitSubClassIdentifier": "003",
            "BillGroupIdentifier": "001",
            "OriginalCoverageEffectiveDate": "2019-09-01",
            "CoverageEffectiveDate": "2019-09-01",
            "CoverageTierCode": "SpouseOnly",
            "BenefitCalculationMethodCode": "FlatAmount",
            "BenefitAmount": "2000",
            "EmployeeContributionCode": "EmployeePaid",
            "EmployeePremiumContributionAmount": "3.31",
            "EmployerPremiumContributionAmount": "0",
            "TotalPlanPremiumAmount": "3.31",
            "PaymentMethodCode": "PayrollDeduction",
            "PremiumModeQuantity": "26",
            "PreTaxCode": "PostTax",
            "TobaccoUseIndicator": "0",
            "TakeOverCoverageIndicator": "0",
            "BeneficiaryGroupID": "1773200",
            "CarrierID": "1234",
            "ElectedCoverage": {
              "ElectedBenefitCalculationMethodCode": "FlatAmount",
              "ElectedEmployeeContributionCode": "EmployeePaid",
              "ElectedTotalPlanPremiumAmount": "3.31",
              "ElectedCoverageInsured": {
                "InsuredPartyID": "45",
                "PrimaryInsuredIndicator": "1",
                "InsuredCoverageEffectiveDate": "2019-09-01"
              }
            },
            "CoverageInsured": {
              "InsuredPartyID": "0",
              "PrimaryInsuredIndicator": "1",
              "TobaccoUseIndicator": "0",
              "InsuredCoverageEffectiveDate": "2019-09-01"
            },
            "CoverageEventID": "NHEVT1234"
          }
        ]
      }
    },
    "Audit": {
      "AuditID": "Audit1234",
      "CarrierRecordQuantity": "1",
      "EmployerRecordQuantity": "1",
      "EmployeeRecordQuantity": "1",
      "DependentRecordQuantity": "0",
      "OtherPartyRecordQuantity": "0",
      "BeneficiaryGroupRecordQuantity": "4",
      "EventRecordQuantity": "1",
      "CoverageRecordQuantity": "3",
      "CoverageRiderRecordQuantity": "0"
    }
  }
}
```

> Example Response 
> 200 

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 400 | Bad request - Payload rejected. |
| 401 | Unauthorized - action requires user authentication. |
| 403 | Forbidden - user does not have permission to upload this payload. |
| 404 | Not Found - resource not found. |
| 405 | Not Allowed - method not allowed on this resource. |
| 406 | Unacceptable - payload content type is not supported. |
| 411 | Length Required - server needs to know the size of the entity body. Specify in Content-Length header. |
| 413 | Payload too large - payload exceeds size limit (too many employees or too many employers). |
| 415 | Unsupported media type - payload type unsupported. |
| 422 | Unprocessable entity - request body failed business validation. |
| 429 | Too many requests - number of requests exceeds limit. |
| 500 | Internal Server Error - fatal error during processing of request. |
| 501 | Not implemented - BEM upload is not supported. |
| 502 | Bad Gateway - the server received an invalid response from an upstream server. |
| 503 | Service Unavailable - unable to process request at this time. Try again later. |


# Usecase


**Member Management**

|                    |                                                               |
|------------------------------|-------------------------------------------------------------------|
|BeneficiaryManagement         | Beneficiary Add                                                   |
|                              | Beneficiary Change                                                |
|                              | Beneficiary Allocation Change                                     |
|                              | Beneficiary Type Change                                           |
|                              |                                                                   |
|BenefitsEligibilityManagement | EAP-new hire.re-hire                                              |
|                              | EAP-demographic changes                                           |
|                              | EAP-termination of an existing employment                         |
|                              | FML- new hire.re-hire                                             |
|                              | FML-demographic changes to an existing employee                   |
|                              | FML-termination of an existing employment                         |
|                              | PFL-new hire.re-hire                                              |
|                              | PFL-demographic changes                                           |
|                              | PFL-termination of an existing employment                         |
|                              |                                                                   |
|CoverageChange                | New Hire Coverage Changes                                         |
|                              | Annual Enrollment Coverage Changes                                |
|                              | Qualifying Life Event Coverage Changes                            |
|                              | Account Structure Change                                          |
|                              | Coverage reduction of Employee (benefit based)                    |
|                              | Coverage increase of Employee (benefit based)                     |
|                              | Coverage reduction of Employee (tier-based)                       |
|                              | Coverage increase of Employee  (tier-based)                       |
|                              | Coverage reduction of dependent (benefit based)                   |
|                              | Coverage increase of dependent (benefit based)                    |
|                              | Coverage reduction of dependent (tier based)                      |
|                              | Coverage increase of dependent (tier based)                       |
|                              | Demographic Change Employee                                       |
|                              | Demographic Change Dependent                                      |
|                              | Beneficiary Change Employee                                       |
|                              | Beneficiary Change Dependent                                      |
|                              | Employee cancels.terminated		                   |
|                              | Employee reinstates coverage                                      |
|                              |                                                                   |
|CoverageElection              | New Hire Coverage Elections                                       |
|                              | Open Enrollment Coverage Elections                                |
|                              | Coverage Election due to Qualifying Life Event                    |
|                              |                                                                   |
|CoverageTermination           | Open Enrollment Coverage Terminations                             |
|                              | Qualifying Life Event.Perpetual Enrollment Coverage Terminations  |
|                              |                                                                   |
|FormsManagement               | Embedded Form                                                     |
|                              | Linked Form                                                       |
|                              |                                                                   |
|IssueAge                      | StackedElection                                                   |
|                              | Downgrade                                                         |
|                              | Termination                                                       |
|                              | Portability                                                       |
|                              | Cancelation                                                       |
|                              |                                                                   |
|Non-CoverageChange            | Changes to personal information without coverage changes          |
|                              | Changes to employment information                                 |
|                              |                                                                   |
|SavingsAndSpendingAccounts    | New Election                                                      |
|                              | Change Election                                                   |
|                              | Existing Account Cancelation.Termination                          |
|                              | Monthly Account Change Election                                   |
|                                              | 													                                                                                                                      |



**Evidence of Insurability**


|                    |                                                               |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|EOI Decision (Carrier-initiated)              | EOI full approval .The Insurance carrier approves entire coverage amount requested for underwriting and sends to Ben Admin system to update coverage.                     |
|                                              | EOI partial approval .The Insurance carrier approves partial coverage amount requested for underwriting and sends to Ben Admin system to update coverage.                 |
|                                              | EOI Decline .The Insurance carrier denies coverage amount requested for underwriting and sends to Ben Admin system to update coverage.                                    |
|                                              | EOI Closed .The Insurance carrier closes or expires the request for underwriting.                                                                                         |
|                                              |  													                                                                                                                      |
|EOI Application update to Ben Admin by Carrier| Not Started on file  .Application received but EOI process yet to start                                                                                                   |
|                                              | Not Submitted  .Applicant has not started the application for EOI                                                                                                         |
|                                              | Pending  .EOI process in progress pending with some action                                                                                                                |
|                                              | Completed .EOI process completed and update the status to applicant                                                                                                       |
|                                              | 													                                                                                                                      |
|EOI Application Status Request by Ben Admin   | Not started no record  .The carrier has a record of the enrollment but no EOI application data has been received.                                                         |
|                                              | Not Started on file  .carrier has a record of the enrollment but no EOI application data has been received.                                                               |
|                                              | Not Submitted  .carriers with an online EOI capture tool that can detect that an applicant has begun the process of providing EOI without having submitted the application.|
|                                              | Pending  .carrier has received the EOI application but its Incomplete submission or Waiting on applicant action or Carrier is reviewing.                                  |
|                                              | Completed .carrier has completed its review of the application and reached an EOI Application Decision.                                                    |
 





