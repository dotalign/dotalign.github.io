---
Layout: default.html
---

# Integrating with DotAlign Data
DotAlign privately analyzes email, calendar, contacts and LinkedIn data, to provide productivity and relationship intelligence, right inside Outlook. It also allows colleagues inside a firm to share the list of People and Company relationships with each other, and allows that information to be exported for integration into other enterprise applications. This document describes the exported data and considerations that must be made while dealing with DotAlign data.

## Data Points

{% include include_mermaid.html %}

<div class="mermaid">
    graph LR
      A --- B
      B-->C[fa:fa-ban forbidden]
      B-->D(fa:fa-spinner);
</div>

### Company
For each company extracted by DotAlign, the following data points  are available.

|Name| Description | Type |  
|--|--|--|
|Anchor Identity | The identity being used as the anchor identity for the company | String |
|Secondary Identifiers | A list of alternate identifiers for the company (apart from the anchor identity) | List of Strings |
|Moniker | The "best" name that DotAlign could infer for the company | String |
| Firm Relationship Score | An aggregated score, representing how well DotAlign users inside the firm as a whole, know this company | Number in the range 1 to 99 |
|Aliases | A list of other names that the company is known by | List of Strings |
|Domains|A list of domain urls that are associated with the company | List of Strings |

#### Json

This is what the json version of the exported data for a company would look like.

````javascript
{
  "identities": [
    "D8A928B2043DB77E340B523547BF16CB4AA483F0645FE0A290ED1F20AAB76257",
    "8ECFD3687812CAD0822A07F3425DD6233ADBD8C4FB2C7CEDCEDF1E967A73B060",
    "65D85C2702625FFE89C5C50D40167F291977D6D97D86BBAD819092B7B137BB33",
    "16F676DF27D827E2ED371698414A0E0FF600C6E63B91685F981F18D8998CFFB2"
  ],
  "name": "Genomic Machines",
  "firm_score": "56",
  "aliases": [
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

|Name| Description | Type
|--|--|--|
|Anchor Identity | The identity being used as the anchor identity for the contact | String |
|Secondary Identifiers | A list of alternate identifiers for the contact (apart from the anchor identity) | List of Strings |
|First Name | Contact's First Name|String|
|Last Name| Contact's Last Name|String|
|Firm Relationship Score | An aggregated score, representing how well DotAlign users inside the firm as a whole, know this contact|Number in the range 1 to 99 |
|Relationships|A list of users who have a relationship with the contact, along with the relationship score| List of Relationship Objects |
|Work Experience | A list of companies and job titles that the contact has been or is associated with | List of Work Experience Objects |  
| Email Addresses | A list of email addresses for the contact | List of Strings|
| Phone Numbers | A list of telephone numbers for this contact | List of Phone Number Objects|
|Last Inbound Touch|The date and time of the last inbound email from the contact to any DotAlign user, along with the list of users | Touch Object |
|Last Outbound Touch|The date and time of the last outbound email from any DotAlign user to the contact| Touch Object|
|Last Meeting|The date and time of the last meeting between the contact and any DotAlign User(s)| Touch Object|

#### Json
This is what the exported data for a contact would look like

````javascript
{
  "identities": [
    "DDD833938ED2D60F44393E00392FF215F1E252EB9D6024BFB4978DEFE9C5507B",
    "F0CD238DE70F38577A0D97F120D08E126E92BA03337829341DEE99FA7FD65F69",
    "273E5D205BE440F22AAF44266C33719541F9695EECAEBDC14CF687D2B6B02501"
  ],
  "first_name": "Sage",
  "last_name": "Syed",
  "firm_score": "67",
  "relationships": [
    {
      "user": "jaspreet@dotalign.com",
      "score": "45"
    },
    {
      "user": "ken@dotalign.com",
      "score": "61"
    },
    {
      "user": "vince@dotalign.com",
      "score": "24"
    }
  ],
  "work_experience": [
    {
      "id": "D8A928B2043DB77E340B523547BF16CB4AA483F0645FE0A290ED1F20AAB76257",
      "job_title": "Chief Technology Office"
    },
    {
      "id": "348E1524291451369A777605925C4ACD93A49494C8F8A4218C9A9F6184C840B2",
      "job_title": "Lead Software Engineer"
    }
  ],
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
  ],
  "last_inbound": {
    "when": "2017-10-28 15:34:45",
    "who": [
      "jaspreet@dotalign.com",
      "ken@dotalign.com"
    ]
  },
  "last_outbound": {
    "when": "2017-10-29 11:12:56",
    "who": [
      "ken@dotalign.com"
    ]
  },
  "last_meeting": {
    "when": "2017-10-11 10:00:00",
    "who": [
      "ken@dotalign.com",
      "vince@dotalign.com"
    ]
  }
}
````

### Relationship
For a given contact, this object defines a relationship with a user

|Name| Description | Type
|--|--|--|
|User|The id (email address) of the DotAlign User|String|
|Relationship Score|The score of the personal relationship between the user and the contact | Number in the range 1 to 99|

### Work Experience

|Name| Description | Type
|--|--|--|
|Company Anchor Identity|The identity being used as the anchor for the company|String|
|Job Title|The job title that was held at the company|String|

### Touch

|Name| Description | Type
|--|--|--|
|When|The date and time at which the touch happened |DateTime|
|Who|A list of DotAlign users involved in that touch |List of Strings|

### Phone Number

|Name| Description | Type
|--|--|--|
|Digits|The telephone number |String|
|Type|One of "Work", "Home", "Mobile", "Fax" or "Other" |String|

## Identity Alignment
This is a concept central to the functioning of DotAlign. DotAlign uses a sophisticated and extensible technique and data model (patent pending) to align people and company identities. This allows DotAlign to dynamically compute identities as there are changes in data, for example the addition or removal of a new data store.

What that practically means is the following:

1. There is no “DotAlign Id” for a person or company. Instead there is a basket of “real-world” identifiers (email address and name in case of a person, and name and domain in case of a company).
1. People and companies are merged automatically when the data justifies it. Also, sometimes DotAlign will get it wrong, and hence the app provides a workflow for users to be able to manually “split” or “merge” people and companies. That will somehow have to be considered while consuming exported data. As an example, an export done last week may have “Sage Syed” and “S. Ahmed Syed” shown as two separate people. A subsequent one may have them merged together because a common email address was found when a new user’s data set came on-line.

### Reconciliation
While integrating with DotAlign data it is important to be able to handle the dynamic nature of People and Company identities, and below is a suggested way to reconcile identities as subsequent exports are consumed.

#### Data Model

We suggest that the following data model is created to track identities on the consumption end.

## Other Important Considerations

### Privacy and Data Management
Privacy is a concern across the board because our product and brand heavily focus on it, and we should definitely keep the conversation going on this topic as we make progress. We’ll look to you to help us figure out the right balance between enabling enterprise integration scenarios and respecting privacy of the individual user. A few things to think about:

1. What happens to exported data when a user decides to stop sharing?
1. What happens to exported data when a user leaves the firm?
1. Should the rules concerning how data is handled be different for work mailbox vs. LinkedIn vs. other mailboxes that the user may have enabled?
1. Should users be required to share out of their work mailbox?

### Auditability
DotAlign gathers data from email, calendar, contacts and LinkedIn. Then, a significant amount of analysis is run to figure out people and company identities from that data. The algorithms and NLP used are constantly evolving as we find issues and improvements, and we’ve found that in an application like ours, where the focus is on automatically gathering and collating information, it is crucial to provide auditing into the “facts” that helped reach a certain conclusion. This is especially true because in certain cases, we get it wrong, and it is important for the user to be able to get a clear understanding of what happened, and take corrective action.

