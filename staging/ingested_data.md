<div style="font-size: 40px">DotAlign data ingestion</div>

<br />

<div style="font-size: 20px">Table of Contents</div>

<!-- TOC -->

- [Introduction](#introduction)
- [Data Types](#data-types)
    - [EmailMessage](#emailmessage)
    - [Participant](#participant)

<!-- /TOC -->

## Introduction
This document contains an overview of the data consumed by DotAlign while indexing
a mailbox. The data is then processed, parsed and stored in the underlying database.  

## Data Types

### EmailMessage 

| Name | Description | Type | Nullable | 
|--|--|--|--|
| Subject | The subject text of the message | string | No |
| Body | The full un-modified text of the message | string | No |
| SubmitTime | The time at which the message was submitted by the sender | DateTime | No |
| ReceivedTime | The time at which the message was received by the relevant recipient | DateTime | No |
| ReplyRequested | Does the message request a reply | boolean | No |
| SourceKey | A unique identifier for the message, assigned by the source | string | No |
| Important | Was this message flagged as important | boolean | No |
| SignatureBlock | An oject representing the signature block found in the message | SigBlock object | Yes |
| PreSignBody | The "active" text of the message, i.e. the text before the signature block | string | No |
| SenderEmail | The email address of the sender of the messaage | string | No |
| SenderName | The name of the sender of the messaage | string | No |
| ReturnPath | The reply-to email address, if any, for the message | string | Yes |
| ConversationKey | The conversation key for the message | string | No |
| TransportHeaders | The transport headers for the message | List of key (string) / value (string) | No |
| InetMsgId | The internet "message id" of the message | string | No |
| FolderSourceKey | A unique identifier for the folder that the message lives in | string | No |
| Participants | A list of message participant objects | List of Participant objects | No |
| Attachments | A list of attachment names (we don't yet process or store attachment content) | List of strings | Yes | Categories | The tags applied to the message at the source | string | Yes |

### Participant

This object represents a participant in an email message or calendar entry.

| Name | Description | Type | Nullable | 
|--|--|--|--|
| EmailAddress | The email address of the participant | string | No |
| Name | The full name of the participant | String | No |
| Role | The role, for example, sender, to, cc of the participant | string | No |

{% include footer.html %}