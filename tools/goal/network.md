# goal network

The `goal network` collection of commands are provided to support the creation and management of 'private networks.  These are fully-formed Algorand networks with private, custom Genesis ledgers running the current build of Algorand software.  Rather than creating a node instance based on the released genesis.json, these networks have their own and need to be manually connected.

The basic idea is that we create one or more data directories and wallets to form this network, specify which node owns which wallets, and can start/stop the network as a unit.  Each node is just like any other node running on TestNet or DevNet.

| Command | Usage |
|------------|-|
| create     | Create a private named network |
| delete     | Stops and Deletes a deployed private network |
| start      | Start a deployed private network |
| status     | Prints status for all nodes in a deployed private network |
| stop       | Stop a deployed private network |

## To create a new named network
> `goal network create -r ~/net1 -n private -t <path_to_template.json>`

This creates a collection of folders under ~/net1 that make up the entire private network named 'private' (simplifying cleanup).

## To start the private network
> `goal network start -r ~/net1`

## To stop the private network
> `goal network stop -r ~/net1`

## To check the status of all of the nodes running on the private network
> `goal network status -r ~/net1`

## And to delete the private network (stopping first if necessary)
> `goal network delete -r ~/net1`
#### NOTE: This does not prompt first - so be careful before you do this!

Once you have a private network running, you can create more accounts, create transactions, and otherwise treat it like any other set of running nodes.  It's entirely private, however.  If you want to open it up and allow others to join it, you can modify the config.json file and phonebook.json files to explicitly define listening ports and peer addresses.

## Sample Network Template JSON

To create a network, you first create a template file that defines the wallets and nodes comprising the network.  The wallet stake is specified in percent, and the percents should total 100%.
Online = 0 for wallets that will be marked as offline - they have no participation keys generated and cannot participate in consensus.  They will be eligible for offline rewards / incentives.  Online wallets are created with corresponding participation keys, good for the first 10 million blocks (for now).
IsRelay indicates the node is intended to be a relay - there must be at least one relay included in any network.  For now, all nodes will connect only to that relay -- support for including all nodes marked as relays is a future task.

```json
{
    "NetworkName": "",
    "GenesisData": [
        {
            "Name": "Wallet1",
            "Stake": 50,
            "Online": 1
        },
        {
            "Name": "Wallet2",
            "Stake": 40,
            "Online": 1
        },
        {
            "Name": "Wallet3",
            "Stake": 10,
            "Online": 0
        }
    ],
    "Nodes": [
        {
            "Name": "Primary",
            "IsRelay": true,
            "Wallets": [
                "Wallet1"
            ]
        },
        {
            "Name": "Node",
            "Wallets": [
                "Wallet2",
                "Wallet3"
            ]
        }
    ]
}