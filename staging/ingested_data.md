<div style="font-size: 40px">DotAlign data ingestion</div>

<br />

## Introduction
This document contains an overview of the data consumed by DotAlign while indexing
a mailbox. The data is then processed, parsed and stored in the underlying database,
which is hosted on the client's Azure infrastructure. The data is never sent to DotAlign, Inc.,
and no DotAlign, Inc. employees have access to it.

In addition to being on the client's Azure tenant, the database leverages the following  
security features: 

- SSL Transport Encryption – Azure SQL Server supports the standard SSL encryption protocol to provide 
encryption in motion.

- Transparent Data Encryption (TDE) – TDE is a form of file level encryption provided by Azure SQL Server. It provides encryption at rest. Data files cannot be accessed without the encryption key, which is stored separately in the Azure Key Vault.

- Dynamic Data Masking (DDM) – DotAlign enables dynamic data masking on database columns in Azure SQL Server to hide sensitive data from non-privileged users, to provide an additional layer of security. 

- Firewall – Azure SQL Server can be configured to restrict access to only a specific list of IP addresses. In this case, only infrastructure components related to DotAlign Cloud are on that list.

- Encrypted Connection Strings - App settings and connection strings are stored encrypted and decrypted only before being injected into the application’s process memory when it starts. The encryption keys used, are regularly rotated.


## Data Types

### EmailMessage 

| Name | Description | Type | 
|--|--|--|
| Subject | The subject text of the message | string |
| Body | The full text of the message | string |
| SubmitTime | The time at which the message was submitted by the sender | datetime |
| ReceivedTime | The time at which the message was received by the relevant recipient | datetime |
| SourceKey | A unique identifier for the message, assigned by the source | string |
| Important | Was this message flagged as important | boolean |
| SenderEmail | The email address of the sender of the messaage | string |
| SenderName | The name of the sender of the messaage | string |
| ReturnPath | The reply-to email address, if any, for the message | string |
| ConversationKey | The conversation key for the message | string |
| TransportHeaders | The transport headers for the message | key-value pairs (string-string) |
| InetMsgId | The internet "message id" of the message | string |
| FolderSourceKey | A unique identifier for the folder that the message lives in | string |
| Participants | A list of message participant objects | list of [Participant](#participant) objects |
| Attachments | A list of attachment names (we don't yet process or store attachment content) | list of strings |
| Categories | The tags applied to the message at the source | string |

### Participant

This object represents a participant in an email message or calendar entry.

| Name | Description | Type |
|--|--|--|
| EmailAddress | The email address of the participant | string |
| Name | The full name of the participant | String |
| Role | The role, for example, sender, to, cc of the participant | string |

{% include footer.html %}