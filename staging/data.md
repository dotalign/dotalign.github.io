<div style="font-size: 40px">Consuming DotAlign Data</div>

<br />

<div style="font-size: 20px">Table of Contents</div>

<!-- TOC -->

- [Introduction](#introduction)
- [Data Points](#data-points)
    - [Company](#company)
        - [Json](#json)
    - [Contact](#contact)
        - [Json](#json-1)
    - [Relationship](#relationship)
    - [Work Experience](#work-experience)
    - [Touch](#touch)
    - [Phone Number](#phone-number)
- [Identity Alignment](#identity-alignment)
    - [Identifiers](#identifiers)
    - [Reconciliation](#reconciliation)
        - [Suggested Data Model](#suggested-data-model)
            - [company table](#company-table)
            - [company_identities table](#company_identities-table)
        - [Suggested Algorithm](#suggested-algorithm)
- [Other Important Considerations](#other-important-considerations)
    - [Privacy and Data Management](#privacy-and-data-management)
    - [Auditability](#auditability)

<!-- /TOC -->

## Introduction
DotAlign privately analyzes email, calendar, contacts, and LinkedIn data, to provide productivity and relationship intelligence, right inside Outlook. It also allows colleagues inside a firm to share the list of People and Company relationships with each other, and allows that information to be exported for integration into other enterprise applications. This document describes the exported data and considerations that must be made while dealing with DotAlign data

## Data Points

### Company
For each company extracted by DotAlign, the following data points  are available.

|Name| Description | Type | Nullable| 
|--|--|--|--|
|Identities | A list of strings that are considered identifiers for the company | List of Strings | No|
|Name | The "best" name that DotAlign could infer for the company | String | No|
| Firm Score | An aggregated relationship score, representing how well DotAlign users inside the firm as a whole, know this company | Number in the range 1 to 99 | No|
|Company Aliases | A list of other names that the company is known by | List of Strings | Yes|
|Domains|A list of domain urls that are associated with the company | List of Strings | Yes|

#### Json

This is what the json version of the exported data for a company would look like.

````json
{
  "identities": [
    "D8A928B2043DB77E340B523547BF16CB4AA483F0645FE0A290ED1F20AAB76257",
    "8ECFD3687812CAD0822A07F3425DD6233ADBD8C4FB2C7CEDCEDF1E967A73B060",
    "65D85C2702625FFE89C5C50D40167F291977D6D97D86BBAD819092B7B137BB33",
    "16F676DF27D827E2ED371698414A0E0FF600C6E63B91685F981F18D8998CFFB2"
  ],
  "company_information": {
    "company_name": "Genomic Machines",
    "firm_score": 56
  },
  "company_aliases": [
    "Genomic Macs",
    "Genomic Machines Inc."
  ],
  "domains": [
    "genmac.com",
    "genomicmachines.com",
    "genetics.com"
  ]
}
````
### Contact
For each contact extracted by DotAlign, the following data points  are available.

|Name| Description | Type| Nullable|
|--|--|--|--|
|Identities | A list of strings that are considered as identities for the contact | List of Strings | No |
|Is Collaborator| Indicates whether this contact is a collaborator | Boolean | No|
|Collaborator Primary Email| Collaborator's registered email with DotAlign| String | Yes |
|First Name | Contact's First Name|String| No |
|Last Name| Contact's Last Name|String| No |
|Firm Score | An aggregated relationship score, representing how well DotAlign users inside the firm as a whole, know this contact|Number in the range 1 to 99 | No |
| Email Addresses | A list of email addresses for the contact | List of Strings|Yes|
| Phone Numbers | A list of telephone numbers for this contact | List of [Phone Number](#phone-number) Objects|Yes|
|Work Experience | A list of companies and job titles that the contact has been or is associated with, as well as whether those associations seem to be former employment experiences or not | List of [Work Experience](#work-experience) Objects | Yes| 
|Last Inbound Email|The date and time of the last inbound email from the contact to any DotAlign user, along with the list of users | [Touch](#touch) Object |Yes|
|Last Outbound Email|The date and time of the last outbound email from any DotAlign user to the contact| [Touch](#touch) Object|Yes|
|Last Meeting|The date and time of the last meeting between the contact and any DotAlign User(s)| [Touch](#touch) Object|Yes|
|Relationships|A list of users who have a relationship with the contact, along with the relationship score| List of [Relationship](#relationship) Objects |No |

#### Json
This is what the exported data for a contact would look like

````javascript
{
  "identities": [
    "DDD833938ED2D60F44393E00392FF215F1E252EB9D6024BFB4978DEFE9C5507B",
    "F0CD238DE70F38577A0D97F120D08E126E92BA03337829341DEE99FA7FD65F69",
    "273E5D205BE440F22AAF44266C33719541F9695EECAEBDC14CF687D2B6B02501"
  ],
  "is_collaborator": false,
  "collaborator_primary_email": null,
  "personal_data": {
    "first_name": "Sage",
    "last_name": "Syed",
    "firm_score": 63
  },
  "contact_information": {
    "email_addresses": [
      "sage@secureagents.com",
      "sage.syed@gmail.com"
    ],
    "phone_numbers": [
      {
        "digits": "6379203729",
        "type": "mobile"
      },
      {
        "digits": "7283724920",
        "type": "work"
      }
    ]
  },
  "work_experience": [
    {
      "company_identity": "D8A928B2043DB77E340B523547BF16CB4AA483F0645FE0A290ED1F20AAB76257",
      "company_name": "Genomic Machines",
      "job_title": "Chief Technology Officer",
      "is_former": false
    },
    {
      "company_identity": "348E1524291451369A777605925C4ACD93A49494C8F8A4218C9A9F6184C840B2",
      "company_name": "Oracle Engines",
      "job_title": "Lead Software Engineer",
      "is_former": true
    }
  ],
  "interaction_data": {
    "last_inbound_email": {
      "when": "2018-02-20T20:19:21Z",
      "people": [
        {
          "first_name": "Vince",
          "last_name": "Scafaria",
          "collaborator_primary_email": "vince@dotalign.com"
        },
        {
          "first_name": "Minh",
          "last_name": "Truong",
          "collaborator_primary_email": "minh@dotalign.com"
        }
      ]
    },
    "last_outbound_email": {
      "when": "2018-02-16T20:28:03.1731318Z",
      "people": [
        {
          "first_name": "Ken",
          "last_name": "Dreyer",
          "collaborator_primary_email": "ken@dotalign.com"
        }
      ]
    },
    "last_meeting": {
      "when": "2018-02-15T20:00:00Z",
      "people": [
        {
          "first_name": "Ken",
          "last_name": "Dreyer",
          "collaborator_primary_email": "ken@dotalign.com"
        }
      ]
    }
  },
  "relationships": [
    {
      "email_address": "jaspreet@dotalign.com",
      "relationship_score": "45"
    },
    {
      "email_address": "ken@dotalign.com",
      "relationship_score": "61"
    },
    {
      "email_address": "vince@dotalign.com",
      "relationship_score": "24"
    }
     {
      "email_address": "minh@dotalign.com",
      "relationship_score": "15"
    },
  ]
}
````

### Relationship
For a given contact, this object defines a relationship with a user

|Name| Description | Type|Nullable|
|--|--|--|--|
|Email Address|The id (email address) of the DotAlign User|String|No |
|Relationship Score|The score of the personal relationship between the user and the contact | Number in the range 1 to 99|No |

### Work Experience

|Name| Description | Type|Nullable|
|--|--|--|--|
|Company Identity|One of the valid identities for the company|String|No|
|Company Name|One of the names that DotAlign could infer for the company|String|No|
|Job Title|The job title that was held at the company|String|Yes|
|Is Former|Indicator reflecting the likely status of the association between the person and this work experience|Boolean|No|

### Touch

|Name| Description | Type|Nullable|
|--|--|--|--|
|When|The date and time at which the touch happened |DateTime|No|
|Who|A list of DotAlign user ids (i.e. email addresses) involved in that touch |List of Strings|No|

### Phone Number

|Name| Description | Type|Nullable|
|--|--|--|--|
|Digits|The digits (and possibly other characters) that make up the telephone number |String|No|
|Type|One of "Work", "Home", "Mobile", "Fax" or "Other" |String|Yes|

## Identity Alignment
This is a concept central to the functioning of DotAlign. DotAlign uses a sophisticated and extensible technique and data model (patent pending) to align people and company identities. This allows DotAlign to dynamically compute identities as there are changes in data, for example the addition or removal of a new data store.

What that practically means is the following:

1. There is no “DotAlign Id” for a person or company. Instead, there is a basket of “real-world” identifiers (email address and name in case of a person, and name and domain in case of a company).
1. People and companies are merged automatically when the data justifies it. Also, in the event of DotAlign getting it wrong, the app provides a workflow for users to be able to manually “split” or “merge” people and companies. This aspect must be considered while consuming exported data. As an example, an export done last week may have “Sage Syed” and “S. Ahmed Syed” shown as two separate people. A subsequent one may have them merged together because a common email address was found when a new user’s data set came on-line.

### Identifiers
A note about identifiers. While conceptually, DotAlign uses "real-world" identifiers like email address, domains, and name, in practice, what is used is a [SHA256 transformation](https://en.wikipedia.org/wiki/SHA-2) of the actual identifier text. This is done because in certain cases, the actual identifier itself may be a private piece of information, not meant to be shared. There can also be cases where a certain name or email address are not considered valid identitiers by DotAlign because of certain business rules, and so separately specifiying the identifiers in a different format (SHA256 in this case) adds a level of clarity.

> If one of your goals is to map existing people and company data to data exported by DotAlign data, please let us know, we can help to map the identifiers in that data to the DotAlign SHA265 identifiers. 

### Reconciliation
While importing  DotAlign data it is important to be able to handle the dynamic nature of People and Company identities. Essentially, while consuming exported data, the following can be true:

1. New entities show up in the export because new data sources (new users or additional mailboxes from existing users) come on-line. 
1. Entities which existed before may not be present because their data source is no longer being shared out.
1. Entities may have been updated and essential information may have changed.
1. Entities which existed before may not be present because they were merged into other entities.
1. New entities show up in the export because they were "split" out from existing entities. This usually happens because a user points out that a certain email address does not belong to a contact. 

Here are some files that illustrate how the different scenarios would look like in terms of actual exported data:

|Scenario |
|--|
|[Baseline](https://github.com/dotalign/dotalign.github.io/blob/master/data_samples/Baseline.json)  |
|[Simple Update](https://github.com/dotalign/dotalign.github.io/blob/master/data_samples/SimpleUpdate.json) |
|[Merge](https://github.com/dotalign/dotalign.github.io/blob/master/data_samples/Merge.json) |
|[Split](https://github.com/dotalign/dotalign.github.io/blob/master/data_samples/Split.json) |
|[Split and Merge](https://github.com/dotalign/dotalign.github.io/blob/master/data_samples/Splitandmerge.json) |

To account for these possibilities, we suggest performing "reconciliation" as a part of the process of importing DotAlign data. One way to do that is shown below. 

#### Suggested Data Model

Two tables would be required, one with a row for each company, and the other with a mapping between the company and its identifiers. The same data model would also apply to contacts.

##### company table

|id| name | superseded_by |
|--|--|--|
|1|Genomic Machines| null |
|2|Secure Agents, Inc.| null |
|3|Delta Analytics| null |
|4|Zander Corporation| null |
|5|AdelProc, Inc.| null |

> **id**             - The identifier that can be used to represent the company <br />
> **name**           - The name of the company as specified by DotAlign <br />
> **superseded_by**  - If the company has been merged into another, then this column should contain the id of the other company. Based on the information available to DotAlign, it may decide that two companies are indeed the same, and merge them. That is when one of the companies is said to be superseded by the other. 

##### company_identities table

|company_id| identifier |  
|--|--|
|1| D8A928B2043DB77E340B523547BF16CB4AA483F0645FE0A290ED1F20AAB76257|
|1| 8ECFD3687812CAD0822A07F3425DD6233ADBD8C4FB2C7CEDCEDF1E967A73B060|
|1| 65D85C2702625FFE89C5C50D40167F291977D6D97D86BBAD819092B7B137BB33|
|1| 16F676DF27D827E2ED371698414A0E0FF600C6E63B91685F981F18D8998CFFB2|
|2| 6FB68D19F18D30076C247201D9D4A5EEEC2B4594A5CE526C4A36D0BFB387A0EE|
|2| F4D319980F50500CC741268D754D2D90BA06C1E39ADCADA97CFE5D040D05DF53|
|2| 8E35C2CD3BF6641BDB0E2050B76932CBB2E6034A0DDACC1D9BEA82A6BA57F7CF|
|3| 6E4D3C7BA1CC49518FAC193F0A8348F89689BAAC58E2B0D55A4B210F59091DD5|
|3| 859B83D445E97B9A7701DC4CAC10824E87A582372BBD54C4B0B42E2497D8490E|
|4| 72FF33DD10A07B8F9260975E499104F818412DA1268DF8E7B7B08E3A5B82AD52|
|4| E294AFFA6863E16FBECDFF7CFFBAF1237A8F2EE2AB2805BB3CACC7FC16D079B2|
|4| 7807EBC369F474BA6BC1FA51B0BA1CC8D6F79FC8321C1CC8BABCEECB44084EB8|
|5| D876D59095F13054C120F77202C5378AA25D7787D4ADF70980DBB3F2A7125AC1|
|5| 4C94485E0C21AE6C41CE1DFE7B6BFACEEA5AB68E40A2476F50208E526F506080|
|5| 8E35C2CD3BF6641BDB0E2050B76932CBB2E6034A0DDACC1D9BEA82A6BA57F7CF|

> **company_id**     - The id from the company table <br />
> **identifier**     - A DotAlign identifier <br />

Firstly, this allows the consumer to have a reasonably stable id that they can use to point other external data to. And secondly, it allows for the lookup needed to generate events to inform external systems about the latest state of an entity. 

#### Suggested Algorithm

The algorithm can be described using the following flow chart.

![Data Import Flowchart](/images/AlgorithmFlowChart.png)

Or using the following pseudo code.

```` c#

// newly created entities from new data to import 
var newEntitySet();
// outdated entities due to entity splits and merges
var deleteSet();

foreach (entity in dataToImport)
{
  var matchingEntities = GetMatchingEntitiesFromPreviousImport(entity);

  if (matchingEntities.Count == 0)
  {
    var newEntity = MakeNewEntryInEntityTable(entity);  
    newEntitySet.Add(newEntity);
  }
  else if (matchingEntities.Count >= 1)
  {
    var modificationsMade = GetModifications(matchingEntities);
    var newEntity = MakeNewEntryInEntityTable(entity);  
    newEntity.ApplyModifications(modificationsMade);
    newEntitySet.Add(newEntity);
    
    deleteSet.Add(matchingEntities);
  }
}

if(RevokeData == true)
{
    // In order to implement data revocation, all older entities not found in new import must be deleted.
    // the deleteSet is not necessary in this case.
    entityData = newEntitySet;
}
else
{
    // No data revocation
    entityData.Delete(deleteSet);
    entityData.Add(newEntitySet);
}

````
Considerations regarding privacy and data revocation are discussed below.

## Other Important Considerations

### Privacy and Data Management
Privacy is a concern across the board, because of the natural dual ownership of relationship data between the individual and the enterprise. DotAlign's brand is about respecting the human sensibilities that go with sharing relationships and enabling individuals to be able to control access to their data. We think that when users feel comfortable and trust the platform, they in fact share more.

A few things to think about w.r.t privacy:

1. What happens to exported data when a user decides to stop sharing?
1. What happens to exported data when a user leaves the firm?
1. Should the rules concerning how data is handled be different for work mailbox vs. LinkedIn vs. other mailboxes that the user may have enabled?
1. Should users be required to share out of their work mailbox?

Our pseudo code in the section above contains an example of how to address these issues and revoke data that is no longer being shared.

### Auditability
DotAlign gathers data from email, calendar, contacts and LinkedIn. Then, a significant amount of analysis is run to extract people and company identities from that data. The algorithms and NLP used are constantly evolving as we find issues and improvements, and we’ve found that in an application like ours, where the focus is on automatically gathering and collating information, it is crucial to provide auditing into the “facts” that helped reach a certain conclusion. This is especially true because in certain cases, the platform gets it wrong, and it is important for the user to be able to get a clear understanding of what happened, and take corrective action.

{% include footer.html %}
