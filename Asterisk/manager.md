# AMI Command Syntax

Management communication consists of tags of the form `"header: value"`, terminated with an empty newline (`\\r\\n`) in the style of SMTP, HTTP, and other headers.

The first tag MUST be one of the following:

*   **`Action`**: An action requested by the CLIENT to the Asterisk SERVER.
*   **`Response`**: A response to an action from the Asterisk SERVER to the CLIENT.
*   **`Event`**: An event reported by the Asterisk SERVER to the CLIENT

> [!NOTE]
>  Only one **`Action`** may be outstanding at any time.
