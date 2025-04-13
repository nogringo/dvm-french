# DVM French

This repository contain my Nostr DVM that show the latest notes in french. It also contain a simple template for anyone who want to create a DVM.

## Use the template

### Generate a secret key

```js
import { generateSecretKey } from 'nostr-tools/pure';
import { bytesToHex } from '@noble/hashes/utils';

const sk = generateSecretKey();
const skHex = bytesToHex(sk);

console.log(skHex);
```

### Generate an id

```js
const dvmId = [...Array(16)]
  .map(() => Math.floor(Math.random() * 16).toString(16))
  .join('');

console.log(dvmId);
```

### Create the discovery event

```js
import { getPublicKey, finalizeEvent } from 'nostr-tools/pure';
import { hexToBytes } from '@noble/hashes/utils';

const skHex = process.env.skHex;

const sk = hexToBytes(skHex);
const pk = getPublicKey(sk);

const content = {
  "name": "your_dvm_name", // Replace with your DVM name
  "picture": "url_of_your_dvm_picture", // Replace with your url to your DVM picture
  "about": "your_dvm_description", // Replace with your DVM description
  "lud16": "your_lightning address", // Replace with your lightning address
  "supportsEncryption": true,
  "acceptsNutZaps": false,
  "personalized": false,
  "amount": "free",
  "nip90Params": {
    "max_results": {
      "required": false,
      "values": [],
      "description": "The number of maximum results to return (default currently 200)"
    }
  }
}

const discoveryEvent = {
  "pubkey": pk,
  "created_at": Math.floor(Date.now() / 1000),
  "kind": 31990,
  "tags": [
    [
      "k",
      "5300"
    ],
    [
      "d",
      "your_dvm_id" // Replace with your DVM id
    ]
  ],
  "content": JSON.stringify(content),
}

const event = finalizeEvent(discoveryEvent, sk);

console.log(event);
```

### Publish your discovery event

```js
import { SimplePool, useWebSocketImplementation } from 'nostr-tools/pool';
import WebSocket from 'ws';

useWebSocketImplementation(WebSocket);

const relays = [
  "wss://offchain.pub",
  "wss://nostr.mom/",
  "wss://relay.nostr.bg/",
  "wss://relay.nostr.band",
  "wss://relay.current.fyi",
  "wss://relay.primal.net",
  "wss://nos.lol",
  "wss://relay.snort.social",
  "wss://nostr.fmt.wiz.biz/",
  "wss://nostr.oxtr.dev/",
  "wss://relay.damus.io/",
  "wss://eden.nostr.land",
  "wss://brb.io",
  "wss://nostr-pub.wellorder.net/",
  "wss://nostr.bitcoiner.social/",
];

const discoveryEvent = {} // Replace with your event

const pool = new SimplePool();

pool.publish(relays, discoveryEvent);
```

### Edit the file dvm_template.html

If you have any question ask me on Nostr npub1kg4sdvz3l4fr99n2jdz2vdxe2mpacva87hkdetv76ywacsfq5leqquw5te
