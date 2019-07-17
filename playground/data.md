The following can be understood as an Ethereum way of wrapping
data. Here data is the parameters of a transaction.

    <ethereum function="getLocality" contract="EntryToken" as="utf8">
        <data>
            <uint256 ref="tokenId"/>
        </data>
    </ethereum>

In turn, `<etehereum>` can be used in `<origin>` or `<transaction>`.

    <origin>
        <ethereum function="getLocality" contract="EntryToken" as="utf8">
            <data>
                <uint256 ref="tokenId"/>
            </data>
        </ethereum>
    </origin>

    <transaction>
        <ethereum>
            <to>0x7301CFA0e1756B71869E93d4e4Dca5c7d0eb0AA6</to>
            <value ref="amount"/>
            <data>....</data>
        </ethereum>
    </transaction>

In these cases, these <data> elements are ad-hoc - created and used
for a specific transaction or pure function call. But sometimes they
need to be portable. Let's consider, for example, a simple order
bidding a fungible token CRO with Ether:

    <data>
    <address ref="holdingContract"/>
    <uint256 ref="tokenId"/>
    <uint32 ref="expiry"/>
    <unit256 ref="bid"/>
    </data>

The order is signed by Alice (the signature is outside of
data). Imagine the following function in a smart contract that can
process the purchase:

    function buy(order, r, s, v)

Anyone who can compile a transaction that has the order and signature
as paramters, attach "bid" amount of Ether, to the smart contract
function `buy`, is able to finalise the transaction on behalf of Alice
and, as the result, Alice gets the CRO tokens. (Alice's address can be
calculated from the signature or explicitly stated in `<data>`).

The uses-case would be having Alice pre-sign a lot of different orders
to a market watcher who will finalise the trade for Alice only with
one of these orders.

