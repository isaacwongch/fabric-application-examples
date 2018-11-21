# Extending the Commercial Paper Network

![cp.network](./resource/develop.diagram.1.png)

The aim of this project is to walk you through every little step in creating a realistic fabric network

## An In-Depth Look on configtx.yaml

In the PaperNet network, we define a consortium called PapernetConsortium which includes three organizations -- DigiBank, MangetoCorp and HedgeMatic. On the other hand, we have another organization called MiddleBank to be the orderer organization. Note that MiddleBank is not part of the consortium, this servers act as legal indepedant unit that will not participate in any transaction.

```yaml
Profiles:

    SystemGenesis:
        Orderer:
            <<: *OrdererDefaults
            Organizations:
                - *MiddleBank
        Consortiums:
            PapernetConsortium:
                Organizations:
                    - *MagnetoCorp
                    - *DigiBank
                    - *HedgeMatic
    PapernetChannel:
        Consortium: PapernetConsortium
        Application:
            <<: *ApplicationDefaults
            Organizations:
                - *MagnetoCorp
                - *DigiBank
                - *HedgeMatic
```

A few points to note from the above YAML.
* You need a consortium to create a channel
* Organizations defined under PaperNetChannel must be part of the consortium linked

### Command

#### Create the orderer genesis block
```
../bin/configtxgen -profile SystemGenesis -outputBlock ./channel-artifacts/genesis.block
```

The created genesis block will be referenced by the docker-compose file while creating the orderer container (environemnt & volume)


## To-do List
* Storing off-chain data such as the certificate using IPFS
* A master-details data structure demonstration
* Using Fabric CA in the network
* Deploy the network on Kubernetes
* Use Kafka for the orderer
* Use of private collection
* Demonstrate service discovery capability