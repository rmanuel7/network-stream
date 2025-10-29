# [AMI Command Syntax](https://docs.asterisk.org/Configuration/Interfaces/Asterisk-Manager-Interface-AMI/AMI-Command-Syntax/)

Management communication consists of tags of the form `"header: value"`, terminated with an empty newline (`\\r\\n`) in the style of SMTP, HTTP, and other headers.

The first tag MUST be one of the following:

*   **`Action`**: An action requested by the CLIENT to the Asterisk SERVER.
*   **`Response`**: A response to an action from the Asterisk SERVER to the CLIENT.
*   **`Event`**: An event reported by the Asterisk SERVER to the CLIENT

> [!NOTE]
>  Only one **`Action`** may be outstanding at any time.

<br/>

## [The Asterisk Manager TCP IP API](https://docs.asterisk.org/Configuration/Interfaces/Asterisk-Manager-Interface-AMI/The-Asterisk-Manager-TCP-IP-API/)

The manager is a client/server model over TCP. With the manager interface, you'll be able to control:

*   the PBX,
*   originate calls,
*   check mailbox status,
*   monitor channels and
*   queues as well as
*   execute Asterisk commands.

AMI is the standard management interface into your Asterisk server. You configure AMI in `manager.conf`. By default, AMI is available on TCP port `5038` if you enable it in `manager.conf`.

AMI receive commands, called "actions". These generate a "response" from Asterisk. Asterisk will also send "Events" containing various information messages about changes within Asterisk. Some actions generate an initial response and data in the form list of events. This format is created to make sure that extensive reports do not block the manager interface fully.

Management users are configured in the configuration file `manager.conf` and are given permissions for read and write, where write represents their ability to perform this class of "action", and read represents their ability to receive this class of "event".

If you develop AMI applications, treat the headers in Actions, Events and Responses as local to that particular message. There is no cross-message standardization of headers.

If you develop applications, please try to reuse existing manager headers and their interpretation. If you are unsure, discuss on the asterisk-dev mailing list.

Manager subscribes to extension status reports from all channels, to be able to generate events when an extension or device changes state. The level of details in these events may depend on the channel and device configuration. Please check each channel configuration file for more information. (in `sip.conf`, check the section on subscriptions and call limits)


### [AMI v2 Specification](https://docs.asterisk.org/Configuration/Interfaces/Asterisk-Manager-Interface-AMI/AMI-v2-Specification/)

Historically, AMI has existed in Asterisk as its own core component `manager`. AMI events were raised throughout Asterisk encoded in an AMI specific format, and AMI actions were processed and passed to the functions that implemented the logic. In Asterisk 12, AMI has been refactored to sit on top of Stasis, a generic, protocol independent message bus internal to Asterisk. From the perspective of clients wishing to communicate with Asterisk over AMI very little has changed; internally, the Stasis representation affords a much higher degree of flexibility with how messages move through Asterisk. It also provides a degree of uniformity for information that is propagated to interested parties.

AMI High Level Diagram:

<img width="790" height="439" alt="image" src="https://github.com/user-attachments/assets/463fe17f-62a8-4a29-8fa9-7318a0ad02ec" />


#### Semantics and Syntax

##### Message Sending and Receiving

By default, AMI is an asynchronous protocol that **sends events immediately to clients** when those events are available. Likewise, clients are free to send actions to AMI at any time, which may or may not trigger additional events.

> [!IMPORTANT]
> *   The exception to this is when the connection is over HTTP; in that scenario, events are only transmitted as part of the response to an HTTP POST.
> *   Events will **not be sent** until the client provides authorized credentials.
> *   Keys are case insensitive.
>     ```
>     Action: Originat
>     ACTION: Originate
>     action: Originate
>     ```

##### Message Layout

*   AMI is an **ASCII** protocol that provides **bidirectional communication** with clients.
*   An AMI message – action or event – is composed of fields delineated by the `\r\n` characters.
*   Within a message, each field is a key value pair delineated by a `:`.
*   A **single space** MUST follow the `:` and **precede the value**.
*   Fields with the **same key may be repeated** within an AMI message.
*   An action or event is terminated by an additional '\r\n' character.

```env
Event: Newchannel
Privilege: call,all
Channel: PJSIP/misspiggy-00000001
Uniqueid: 1368479157.3
ChannelState: 3
ChannelStateDesc: Up
CallerIDNum: 657-5309
CallerIDName: Miss Piggy
ConnectedLineName:
ConnectedLineNum:
AccountCode: Pork
Priority: 1
Exten: 31337
Context: inbound
```

This is syntantically equivalent to the following ASCII string:

```
Event: Newchannel\r\nPrivilege: call,all\r\nChannel: PJSIP/misspiggy-00000001\r\nUniqueid: 1368479157.3\r\nChannelState: 3\r\nChannelStateDesc: Up\r\nCallerIDNum: 657-5309\r\nCallerIDName: Miss Piggy\r\nConnectedLineName:\r\nConnectedLineNum:\r\nAccountCode: Pork\r\nPriority:\r\nExten: 31337\r\nContext: inbound\r\n\r\n
```

Actions are specified in a similar manner. Note that depending on the message, some keys can be repeated.

```env
Action: Originate
ActionId: SDY4-12837-123878782
Channel: PJSIP/kermit-00000002
Context: outbound
Exten: s
Priority: 1
CallerID: "Kermit the Frog" <123-4567>
Account: FrogLegs
Variable: MY_VAR=frogs
Variable: HIDE_FROM_CHEF=true
```
