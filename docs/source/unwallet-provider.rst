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
      params: [{
        "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
        "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
        "gas": "0x76c0",
        "gasPrice": "0x9184e72a000",
        "value": "0x9184e72a",
        "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675",
      }],
    });


Wrapping with other libraries
=============================

ethers.js
---------

.. code-block:: js

    import { ethers } from "ethers";

    const web3Provider = new ethers.providers.Web3Provider(provider);


Supported RPC methods
=====================

eth_requestAccounts
-------------------

Parameters
^^^^^^^^^^

none

Returns
^^^^^^^

``Array`` of ``DATA (20 Bytes)`` - the addresses that the user approved to access

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

``Array`` of ``DATA (20 Bytes)`` - the addresses that the user approved to access

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

eth_sign
--------

Parameters
^^^^^^^^^^

#. ``DATA (20 Bytes)`` - the address of the signer account
#. ``DATA`` - the message to sign

Returns
^^^^^^^

``DATA`` - The signature

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

TODO

eth_signTypedData_v4
--------------------

TODO

eth_sendTransaction
-------------------

Parameters
^^^^^^^^^^

#. ``Object`` - the transaction object

  - ``from``: ``DATA (20 Bytes)`` - (optional) the address the transaction is send from
  - ``to``: ``DATA (20 Bytes)`` - the address the transaction is directed to
  - ``gas``: ``QUANTITY`` - (optional) integer of the gas provided for the transaction execution
  - ``gasPrice``: ``QUANTITY`` - (optional) integer of the gas price used for each paid gas
  - ``value``: ``QUANTITY`` - (optional) integer of the value sent with this transaction
  - ``data``: ``DATA`` - (optional) the hash of the invoked method signature and encoded parameters

Returns
^^^^^^^

``DATA (32 Bytes)`` - the transaction hash

Example
^^^^^^^

.. code-block:: js

    const txHash = await provider.request<string>({
      method: "eth_sendTransaction",
      params: [{
        "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
        "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
        "gas": "0x76c0",
        "gasPrice": "0x9184e72a000",
        "value": "0x9184e72a",
        "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675",
      }],
    });


.. _EIP1102: https://eips.ethereum.org/EIPS/eip-1102
.. _EIP1193: https://eips.ethereum.org/EIPS/eip-1193