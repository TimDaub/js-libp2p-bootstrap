# @libp2p/bootstrap <!-- omit in toc -->

[![libp2p.io](https://img.shields.io/badge/project-libp2p-yellow.svg?style=flat-square)](http://libp2p.io/)
[![IRC](https://img.shields.io/badge/freenode-%23libp2p-yellow.svg?style=flat-square)](http://webchat.freenode.net/?channels=%23libp2p)
[![Discuss](https://img.shields.io/discourse/https/discuss.libp2p.io/posts.svg?style=flat-square)](https://discuss.libp2p.io)
[![codecov](https://img.shields.io/codecov/c/github/libp2p/js-libp2p-bootstrap.svg?style=flat-square)](https://codecov.io/gh/libp2p/js-libp2p-bootstrap)
[![CI](https://img.shields.io/github/workflow/status/libp2p/js-libp2p-interfaces/test%20&%20maybe%20release/master?style=flat-square)](https://github.com/libp2p/js-libp2p-bootstrap/actions/workflows/js-test-and-release.yml)

> Node.js IPFS Implementation of the railing process of a Node through a bootstrap peer list

## Table of contents <!-- omit in toc -->

- [Install](#install)
- [Usage](#usage)
- [Contribute](#contribute)
- [License](#license)
- [Contribute](#contribute-1)

## Install

```console
$ npm i @libp2p/bootstrap
```

## Usage

The configured bootstrap peers will be discovered after the configured timeout. This will ensure
there are some peers in the peer store for the node to use to discover other peers.

They will be tagged with a tag with the name `'bootstrap'` tag, the value `50` and it will expire
after two minutes which means the nodes connections may be closed if the maximum number of
connections is reached.

Clients that need constant connections to bootstrap nodes (e.g. browsers) can set the TTL to `Infinity`.

```JavaScript
import { createLibp2p } from 'libp2p'
import { Bootstrap } from '@libp2p/bootstrap'
import { TCP } from 'libp2p/tcp'
import { Noise } from '@libp2p/noise'
import { Mplex } from '@libp2p/mplex'

let options = {
  transports: [
    new TCP()
  ],
  streamMuxers: [
    new Mplex()
  ],
  connectionEncryption: [
    new Noise()
  ],
  peerDiscovery: [
    new Bootstrap({
      list: [ // a list of bootstrap peer multiaddrs to connect to on node startup
        "/ip4/104.131.131.82/tcp/4001/ipfs/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ",
        "/dnsaddr/bootstrap.libp2p.io/ipfs/QmNnooDu7bfjPFoTZYxMNLWUQJyrVwtbZg5gBMjTezGAJN",
        "/dnsaddr/bootstrap.libp2p.io/ipfs/QmQCU2EcMqAqQPR2i9bChDtGNJchTbq5TbXJJ16u19uLTa"
      ],
      timeout: 1000, // in ms,
      tagName: 'bootstrap',
      tagValue: 50,
      tagTTL: 120000 // in ms
    })
  ]
}

async function start () {
  let libp2p = await createLibp2p(options)

  libp2p.on('peer:discovery', function (peerId) {
    console.log('found peer: ', peerId.toB58String())
  })

  await libp2p.start()

}

start()
```

## Contribute

The libp2p implementation in JavaScript is a work in progress. As such, there are a few things you can do right now to help out:

- Go through the modules and **check out existing issues**. This is especially useful for modules in active development. Some knowledge of IPFS/libp2p may be required, as well as the infrastructure behind it - for instance, you may need to read up on p2p and more complex operations like muxing to be able to help technically.
- **Perform code reviews**. More eyes will help a) speed the project along b) ensure quality and c) reduce possible future bugs.

## License

Licensed under either of

- Apache 2.0, ([LICENSE-APACHE](LICENSE-APACHE) / <http://www.apache.org/licenses/LICENSE-2.0>)
- MIT ([LICENSE-MIT](LICENSE-MIT) / <http://opensource.org/licenses/MIT>)

## Contribute

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.
