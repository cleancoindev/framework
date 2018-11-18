# Confirmed API

```ts
import { Context } from '@0xcert/web3-context';

const context = new Context({ makerId? exchangeId?, signMethod?, web3? });
context.platform;
await context.attach();
await context.detach();
await context.sign(data);
```
```ts
import { AssetLedger } from '@0xcert/web3-asset-ledger';

const ledger = new AssetLedger(context, ledgerId);
ledger.platform;
ledger.id; // contract address
ledger.on(event, handler);
ledger.off(event, handler);
ledger.subscribe();
ledger.unsubscribe();
await ledger.getAbilities(accountId);
await ledger.getCapabilities();
await ledger.getInfo();
await ledger.getSupply();
await ledger.getTransferState();
await ledger.assignAbilities(accountId, abilities);
await ledger.revokeAbilities(accountId, abilities);
await ledger.setTransferState(state);
await ledger.transfer(to, assetid);
await ledger.mint(assetId, proof);
```
```ts
import { ValueLedger } from '@0xcert/web3-value-ledger';

const ledger = await new ValueLedger(context, ledgerId);
ledger.platform;
ledger.id; // contract address
ledger.on(event, handler);
ledger.off(event, handler);
ledger.subscribe();
ledger.unsubscribe();
await ledger.getInfo();
await ledger.getSupply();
```
```ts
import { OrderInterface, KittyInterface } from '@0xcert/assets';

const order = {
  $schema: 'http...',
  $evidence: 'http...',
  id: '100',
  folderId: '0x...',
  makerId: '0x...',
  takerId: '0x...',
  actions: [],
  seed: 1234,
  expiration: 5678,
} as OrderInterface;

const asset = {
  $schema: 'http...',
  $evidence: 'http...',
  name: '',
  description: '',
  image: '',
} as KittyInterface;
```
```ts
import { OrderExchange } from '@0xcert/web3-order-exchange';

const exchange = new OrderExchange(context);
exchange.on(event, handler);
exchange.off(event, handler);
exchange.subscribe();
exchange.unsubscribe();
await exchange.claim(order); // signed claim (maker)
await exchange.cancel(order); // (maker)
await exchange.perform(order, signature); // (taker)
```
```ts
import { Cert } from '@0xcert/certification';

const cert = new Cert({ schema, hasher? });
// const paths = contract.getPaths(PropVisibility.EXPOSED); // REQUIRED | ALL
const recipe = cert.notarize(asset);
const evidence = await contract.disclose(paths);
const imprint = await contract.imprint(asset, evidence);

// const notary = new Notary({ algo, schema });
// const recipe = await notary.notarize(asset);
// const evidence = await notary.expose(recipe, paths); // only evidence
// const imprint = await notary.verify(asset, evidence); // imprint should match with the one in the token
```
```ts
import { TransactionQueue } from '@0xcert/web3-transaction-queue';

const queue = new TransactionQueue({ confirmations, removeConfirmed });
queue.on(event, handler);
queue.off(event, handler);
queue.add(transactionId, transactionId, ...);
queue.remove(transactionId, transactionId, ...);
queue.listen();
queue.close();
```

# Structs


Asset JSON object:

```json
{
  "$schema": "",
  "$evidence": "",
  "name": "",
  "description": "",
  "image": ""
}
```

Asset evidence:

```js
const patsh = [
  ['books', 0, 'release', 'date'],
]
const json = {
  name: 'John',
  books: [
    {
      title: 'Start',
      release: {
        date: '',
      },
    },
  ],
};
const recipe = [
  {
    path: [],
    evidence: {
      proofs: [
        { index: 0, hash: '0x', key: 'name' },
        { index: 1, hash: '0x', key: 'book' },
        { index: 2, hash: '0x', key: 'books' },
      ],
      nodes: [
        { index: 0, hash: '0x' },
        { index: 1, hash: '0x' },
      ],
    },
  },
  {
    path: ['book'],
    evidence: {
      proofs: [
        { index: 0, hash: '0x', key: 'title' },
      ],
      nodes: [
        { index: 0, hash: '0x' },
      ],
    },
  },
  {
    path: ['books'],
    evidence: {
      proofs: [
        { index: 0, hash: '0x', key: 0 },
      ],
      nodes: [
        { index: 0, hash: '0x' },
      ],
    },
  },
  {
    path: ['books', 0],
    evidence: {
      proofs: [
        { index: 0, hash: '0x', key: 'title' },
      ],
      nodes: [
        { index: 0, hash: '0x' },
      ],
    },
  },
];
```