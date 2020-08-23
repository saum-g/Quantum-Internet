Here we describe the basic functionality we expect the Network layer to perform. 

## Link Layer interface

The interface the network layer expects from the link layer is:

- **Creation of entangled pairs between adjacent nodes** : The link layer has the information of the nodes it is directly connected to and is tasked with the creation of entangled pairs between adjacent nodes in the network. The transport layer requires interface to specify the end-node for creating a link between the adjacent nodes.

- **Entanglement swap** : The link layer should provide an interface by which the transport layer can specify the two qubits in the memory of the node between which an entanglement swap is to be performed. These qubits can either be specified through their locations in memory or the link-layer ids of the links. Though in a practical situation the link layer will not have information about the end-nodes since there can be entanglement swapping occuring at the end nodes too, it suffices for us to abstract the link-layer ids to the end-nodes.

- **Qubit decoherence** : The network layer protocols need information of whether a pre-existing pair of qubits has decohered beyond what is useful for us. This depends a lot on the fidelity and storage conditions determined at the physical layer, but can either be provided as an interface or maintained by the network layer itself. 

## Interface to Transport layer

We currently expect the link layer to connect any two nodes in the network provided the minimum fidelity to be achieved in the least time possible.

## Information maintained at Network layer

- **Network Graph** : Let us call it `G(N,E)`. The set `N` consists of all the nodes in the network and the edge set `E` contains the pairs of nodes adjacent to each other. Each edge `e` can have functions defined on it describing the fidelity of a link created across it, number of entangled links it can create per second, etc. For the sake of simplicity of the initial simulation, we assume the fidelity of a newly created link to be `F` and link creation time to be `t` for every edge `e`.

- **Virtual Graph** : Let us call it `Gv(N,Ev)`. Here the edge set `Ev` consists of edges connecting nodes which share an entangled pair at the given moment. Two nodes can have multiple entangled links and hence multiple edges between them. The pre-existence of entangled links in the network reduce the diameter of the graph and aid in the reduction of latency. Refer to [1] for more details.

In a practical scenario no node will have complete information of either the virtual or the network graph. The amount of information a node will have has direct impacts on the protocol used at the network layer and will depend on the final protocol employed.

## Steps taken at network layer

According to [1], the steps followed at the network layer to fulfill a demand of link creation are as follows:

1. **Path Discovery** : This step involves deciding the chain of nodes which will act as repeaters in the link creation process. 
2. **Entanglement Reservation** : This step involves simultaneous generation of entangled links between all adjacent pair of nodes along the discovered path.
3. **Entanglement Distribution** : Entanglement swapping is performed at every intermediate node in the chain to finally yield the requested entangled pair.

Steps 2 and 3 above combined constitute the *Prepare and Swap* protocol for entanglement swapping. It was arrived upon independently in #19 and is also discussed in [2].
