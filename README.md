# ar-page-things

## profile edit integration

Configurable metadata:

| metadata name  | min length | max length | type |
| ------------- |-------------| ------------- | ------------- |
| bio     | 0     | 150 | string |
| avatar      | 43     | 43 | Arweave TXID |
| nickname      | 1     | 30 | string |
| github | 1 | 38 | string - regex'd as GH username |
| twitter | 1 | 15 | string - regex'd as TWTR username |
| instagram | 1 | 30 | string - regex'd as IG username |
| customUrl | 0 | a valid string only | string not regex'd |

This is how the profile editing function handle the action in-contract. [here](https://github.com/decentldotland/ANS/blob/570065bfcc587d419d7793a6f6e8d10fcacc7f8b/contracts/ans.js#L236)

### Transaction Example:

```js
//async code block
const ANS_CONTRACT = "HrPi8hFc7M5dbrtlELfTKwPr53RRrDBgXGdDkp0h-j4";
const newNickname = "pepe";
const newBio = "Pepe the Frog";
const github = "charmful0x";
const custom_url = "https://example.com";

const interaction = `{"function": "updateProfileMetadata", "nickname": "${newNickname}", "bio": "${newBio}", "github": "${github}", "customUrl": "${custom_url}"}`;

const tx = await arweave.createTransaction({ data: String(Date.now()) });
tx.addTag("App-Name", "SmartWeaveAction");
tx.addTag("App-Version", "0.3.0");
tx.addTag("Contract", ANS_CONTRACT);
tx.addTag("Input", interaction);
tx.addTag("Content-Type", "text/plain");
// to 'ensure' that the  TX will not drop when the network is congested
tx.reward = (+tx.reward * 5).toString();

await arweave.transactions.sign(tx);
await arweave.transactions.post(tx);

```

### Notes
- `avatar` should be passed an Arweave data TXID that has a `image/*` MIME type.
- the contract only handle the passed - defined - metadatas
