PseudoNode 0.5.0
================

PseudoNode is a cryptocurrency full node "emulator".

To the network, PseudoNode behaves the same way as a full node by relaying
transactions, blocks, addresses, etc.  However, unlike a normal full
node, PseudoNode does *not* verify data (txs & blocks) itself.  Rather,
PseudoNode relies on neighboring peers (with configurable confidence levels)
to do the verification on PseudoNode's behalf.  As a result, PseudoNode is
very lightweight.

Compared to a normal full node:

* PseudoNode *does not require the blockchain to be downloaded*.
* PseudoNode can "sync" with the network in seconds.
* PseudoNode supports multiple cryptocurrencies that are bitcoin derivatives.
  Currently supported are testnet, litecoin, dogecoin, paycoin and flappycoin.
  It is reasonably trivial to extend the implementation to support more
  currencies.
* PseudoNode is *not* a wallet (cannot be used to store coins).
* PseudoNode uses no disk space (sans the executable), negligible RAM, and
  negligible CPU time.  PseudoNode also consumes less network resources
  (data usage/bandwidth) than a normal full node.

PseudoNode can be downloaded from here (the official release):

* [https://github.com/basil00/PseudoNode/releases](https://github.com/basil00/PseudoNode/releases)

PseudoNode is currently experimental software.  There are likely to be some
bugs.

Example Usage
=============

PseudoNode is currently implemented as a command-line tool.

By default it will connect to the bitcoin network:

    pseudonode

You can connect to different networks using the --coin=COIN option, e.g.:

    pseudonode --coin=testnet
    pseudonode --coin=litecoin
    pseudonode --coin=dogecoin
    pseudonode --coin=paycoin
    pseudonode --coin=flappycoin

The current implementation supports the following cryptocurrencies: bitcoin,
testnet (bitcoin), litecoin, dogecoin, paycoin, flappycoin.

By default, PseudoNode considers data (tx or blocks) valid if 2 other nodes
also believe so.  This value can be configured via the --threshold=VAL option,
e.g.:

    pseudonode --threshold=3

Higher values will slow the node down.  Lower values make it more likely the
node will relay erroneous data.  The default of 2 is a good compromise.

By default PseudoNode will fetch objects (txs and blocks) only if other nodes
explicitly request for them.  It is possible to configure PseudoNode to fetch
objects immediately via the option:

    pseudonode --prefetch

Note that enabling this option will cause PseudoNode to consume more bandwidth
(on par with a normal full node).

To run PseudoNode as a background process use the --server option.  Currently
only works for Linux:

    pseudonode --server

Building
========

For Linux simply run make:

    make

Windows can be built via Linux cross compilation and MinGW.  Run the command:

    make -f Makefile.windows

For MacOSX, run the command:

    make -f Makefile.macosx

Technical
=========

PseudoNode connects to the network just like a normal full node.  Each "inv"
message it receives is treated as a vote.  After the threshold number of votes
has been received, then PseudoNode will consider the data valid, and will
relay it.  This means that PseudoNode will not relay data unless the network
is already relaying it.

PseudoNode cannot handle some kinds of messages directly, e.g. "getdata" for a
non-recent block.  For this PseudoNode forwards the message to another node
and acts as a proxy server.

TODOs
=====

PseudoNode is currently alpha quality software.  Some ideas for future work:

* Bloom filters.
* Mining support?  However without the UTXO set only empty blocks can be
  safely mined.

FAQ
===

*Does PseudoNode harm the network?*

Short answer: No.

Long answer: Not unless the number of PseudoNodes significantly overwhelms the
number of normal full nodes.  Otherwise, if PseudoNode can connect to at least
some good nodes (default 2), then PseudoNode will act just like a normal node
and contribute network bandwidth.

*Can PseudoNode cause the network to fork?*

No, PseudoNode just follows what other nodes are doing.

*Can PseudoNode steal coins?*

No.

*Can PseudoNodes be banned from the network?*

Not easily.  Requests that PseudoNode cannot handle directly can always be
forwarded to other (cooperative) full nodes.

LICENSE
=======

PseudoNode has been released under the MIT license:

    Permission is hereby granted, free of charge, to any person obtaining a
    copy of this software and associated documentation files (the "Software"),
    to deal in the Software without restriction, including without limitation
    the rights to use, copy, modify, merge, publish, distribute, sublicense,
    and/or sell copies of the Software, and to permit persons to whom the
    Software is furnished to do so, subject to the following conditions:
    
    The above copyright notice and this permission notice shall be included in
    all copies or substantial portions of the Software.
    
    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
    FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
    DEALINGS IN THE SOFTWARE.

