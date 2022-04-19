=================
unWallet provider
=================

**unWallet provider** is an `EIP1193`_-compliant provider that provides access to unWallet.


Quick Start
===========

Installation
------------

.. code-block:: sh

    $ npm install unwallet-provider

Setup
-----

.. code-block:: js

    import { UnWalletProvider } from "unwallet-provider";

    const provider = new UnWalletProvider();

`EIP1102`_-compliant account exposure
-------------------------------------

.. code-block:: js

    const accounts = await provider.request<string[]>({
      method: "eth_requestAccounts",
    });

Sending RPC request
-------------------

.. code-block:: js

    const txHash = await provider.request<string>({
      method: "eth_sendTransaction",
      params: [
        {
          from: "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
          to: "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
          gas: "0x76c0",
          gasPrice: "0x9184e72a000",
          value: "0x9184e72a",
          data: "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675",
        },
      ],
    });

Disconnect from wallet
----------------------

.. code-block:: js

    await provider.disable();


Wrapping with other libraries
=============================

ethers.js
---------

.. code-block:: js

    import { ethers } from "ethers";

    const web3Provider = new ethers.providers.Web3Provider(provider);


.. _supported-rpc-methods:

Supported RPC methods
=====================

.. note::

    RPC methods other than the following are available by setting arbitrary RPC endpoints to the provider. See :ref:`configuration` for details.


eth_requestAccounts
-------------------

Parameters
^^^^^^^^^^

none

Returns
^^^^^^^

``Array`` of ``DATA (20 Bytes)`` - addresses that the user approved to access

Example
^^^^^^^

.. code-block:: js

    // Request
    const accounts = await provider.request<string[]>({
      method: "eth_requestAccounts",
    });

    // Result
    ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]

eth_accounts
------------

Parameters
^^^^^^^^^^

none

Returns
^^^^^^^

``Array`` of ``DATA (20 Bytes)`` - addresses that the user approved to access

Example
^^^^^^^

.. code-block:: js

    // Request
    const accounts = await provider.request<string[]>({
      method: "eth_accounts",
    });

    // Result
    ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]

eth_chainId
-----------

Parameters
^^^^^^^^^^

none

Returns
^^^^^^^

``Number`` - integer of the chain ID currently connected

Example
^^^^^^^

.. code-block:: js

    // Request
    const chainId = await provider.request<number>({
      method: "eth_chainId",
    });

    // Result
    1

personal_sign
-------------

Parameters
^^^^^^^^^^

#. ``DATA`` - message to be signed
#. ``DATA (20 Bytes)`` - address of the account that will sign the message

Returns
^^^^^^^

``DATA`` - signature

Example
^^^^^^^

.. code-block:: js

    // Request
    const sig = await provider.request<string>({
      method: "personal_sign",
      params: [
        "0xdeadbeaf",
        "0x9b2055d370f73ec7d8a03e965129118dc8f5bf83",
      ],
    });

    // Result
    "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"

eth_sign
--------

Parameters
^^^^^^^^^^

#. ``DATA (20 Bytes)`` - address of the account that will sign the message
#. ``DATA`` - message to be signed

Returns
^^^^^^^

``DATA`` - signature

Example
^^^^^^^

.. code-block:: js

    // Request
    const sig = await provider.request<string>({
      method: "eth_sign",
      params: [
        "0x9b2055d370f73ec7d8a03e965129118dc8f5bf83",
        "0xdeadbeaf",
      ],
    });

    // Result
    "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"

eth_signTypedData
-----------------

Parameters
^^^^^^^^^^

#. ``DATA (20 Bytes)`` - address of the account that will sign the messages
#. ``Object`` - `EIP712`_-compliant typed structured data to be signed

Returns
^^^^^^^

``DATA`` - signature

Example
^^^^^^^

.. code-block:: js

    // Request
    const sig = await provider.request<string>({
      method: "eth_signTypedData",
      params: [
        "0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826",
        {
          types: {
            EIP712Domain: [
              {
                name: "name",
                type: "string",
              },
              {
                name: "version",
                type: "string",
              },
              {
                name: "chainId",
                type: "uint256",
              },
              {
                name: "verifyingContract",
                type: "address",
              },
            ],
            Person: [
              {
                name: "name",
                type: "string",
              },
              {
                name: "wallet",
                type: "address",
              },
            ],
            Mail: [
              {
                name: "from",
                type: "Person",
              },
              {
                name: "to",
                type: "Person",
              },
              {
                name: "contents",
                type: "string",
              },
            ],
          },
          primaryType: "Mail",
          domain: {
            name: "Ether Mail",
            version: "1",
            chainId: 1,
            verifyingContract: "0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC",
          },
          message: {
            from: {
              name: "Cow",
              wallet: "0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826",
            },
            to: {
              name: "Bob",
              wallet: "0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB",
            },
            contents: "Hello, Bob!",
          },
        },
      ],
    });

    // Returns
    "0x4355c47d63924e8a72e509b65029052eb6c299d53a04e167c5775fd466751c9d07299936d304c153f6443dfa05f40ff007d72911b6f72307f996231605b915621c"

eth_signTypedData_v4
--------------------

.. note::

    This method is provided for compatibility with `MetaMask`_.

Parameters
^^^^^^^^^^

#. ``DATA (20 Bytes)`` - address of the account that will sign the messages
#. ``String`` - JSON encoded `EIP712`_-compliant typed structured data to be signed

Returns
^^^^^^^

``DATA`` - signature

Example
^^^^^^^

.. code-block:: js

    // Request
    const sig = await provider.request<string>({
      method: "eth_signTypedData_v4",
      params: [
        "0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826",
        `{"types":{"EIP712Domain":[{"name":"name","type":"string"},{"name":"version","type":"string"},{"name":"chainId","type":"uint256"},{"name":"verifyingContract","type":"address"}],"Person":[{"name":"name","type":"string"},{"name":"wallet","type":"address"}],"Mail":[{"name":"from","type":"Person"},{"name":"to","type":"Person"},{"name":"contents","type":"string"}]},"primaryType":"Mail","domain":{"name":"Ether Mail","version":"1","chainId":1,"verifyingContract":"0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC"},"message":{"from":{"name":"Cow","wallet":"0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826"},"to":{"name":"Bob","wallet":"0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB"},"contents":"Hello, Bob!"}}`,
      ],
    });

    // Returns
    "0x4355c47d63924e8a72e509b65029052eb6c299d53a04e167c5775fd466751c9d07299936d304c153f6443dfa05f40ff007d72911b6f72307f996231605b915621c"

eth_sendTransaction
-------------------

Parameters
^^^^^^^^^^

#. ``Object`` - transaction object

  - ``from``: ``DATA (20 Bytes)`` - (optional) address that the transaction is send from
  - ``to``: ``DATA (20 Bytes)`` - address that the transaction is directed to
  - ``gas``: ``QUANTITY`` - (optional) integer of the gas provided for the transaction execution
  - ``gasPrice``: ``QUANTITY`` - (optional) integer of the gas price used for each paid gas
  - ``value``: ``QUANTITY`` - (optional) integer of the value sent with the transaction
  - ``data``: ``DATA`` - (optional) hash of the invoked method signature and encoded parameters

Returns
^^^^^^^

``DATA (32 Bytes)`` - transaction hash

Example
^^^^^^^

.. code-block:: js

    const txHash = await provider.request<string>({
      method: "eth_sendTransaction",
      params: [
        {
          from: "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
          to: "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
          gas: "0x76c0",
          gasPrice: "0x9184e72a000",
          value: "0x9184e72a",
          data: "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675",
        },
      ],
    });

wallet_switchEthereumChain
--------------------------

.. note::

    See also `EIP3326`_.

#. ``Object``

  - ``chainId``: integer ID of the chain as a hexadecimal string

Returns
^^^^^^^

``null``

Example
^^^^^^^

.. code-block:: js

    await provider.request<null>({
      method: "wallet_switchEthereumChain",
      params: [
        {
          chainId: "0x1",
        },
      ],
    });

.. _configuration:

Configuration
=============

rpc
---

You can execute RPC methods other than :ref:`supported-rpc-methods` by setting arbitrary RPC endpoints to the provider as follows.

.. code-block:: js

    const provider = new UnWalletProvider({
      rpc: {
        // <CHAIN_ID>: "<ENDPOINT>",
        1: "https://mainnet.infura.io/v3/YOUR_PROJECT_ID",
        137: "https://polygon-mainnet.infura.io/v3/YOUR_PROJECT_ID",
      },
    });

    const count = await provider.request<string>({
      method: "eth_getTransactionCount",
      params: [
        "0x407d73d8a49eeb85d32cf465507dd71d507100c1",
      ],
    });

allowAccountsCaching
--------------------

If ``allowAccountsCaching`` option is ``true``, the provider caches information about the accounts in local storage so that you do not have to execute eth_requestAccounts each time you instantiate the provider.

.. code-block:: js

    const provider = new UnWalletProvider({
      allowAccountsCaching: true,
    });


.. _EIP712: https://eips.ethereum.org/EIPS/eip-712
.. _EIP1102: https://eips.ethereum.org/EIPS/eip-1102
.. _EIP1193: https://eips.ethereum.org/EIPS/eip-1193
.. _EIP3326: https://eips.ethereum.org/EIPS/eip-3326
.. _MetaMask: https://metamask.io
