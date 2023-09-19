---
description: See events from 3rd party systems in Aspecto
---

# External Events

## How It Works

* Define an event in a 3rd party system.
* Use the event URL to create a webhook between that system and Aspecto.
* When the event happens, it will be reported to Aspecto.
* The external event will be presented in the Trace Search graph.

<div align="center">

<img src="https://app.aspecto.io/a1a1f91d9aa5a99e4c8051700ad6ec4b.svg" alt="">

</div>

## Installation

Use the following URL for the webhook in the system you’re working with (with any HTTP verb):

```
https://notify.aspecto.io/account/fa84bb3f-b49c-47ef-b1ba-4d47fb7906ee/alert
```

After installation, send a test from the 3rd party app, go to [Trace Search](https://app.aspecto.io/04a62f43/search) to see it in Aspecto (traces you send now will appear in Trace Search after a few minutes).
