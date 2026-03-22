graph TD
    %% Node Styling and Definition
    classDef node fill:#fff,stroke:#333,stroke-width:2px,rx:10,ry:10,font-family:'Arial';
    classDef layerTitle fill:#eee,stroke:#999,stroke-width:1px,stroke-dasharray: 5 5,rx:5,ry:5,font-family:'Arial',font-weight:bold;
    classDef annotation font-style:italic,color:#666,font-family:'Arial';

    %% Title and Labels
    Title[KumbhNet: Offline Mesh Communication System Architecture]:::node
    Title --> PeerLabel("Peer-to-Peer"):::annotation
    Title --> MeshLabel("Mesh Network"):::annotation
    Title --> DTNLabel("Delay-Tolerant Network (DTN)"):::annotation

    %% Diagram Container
    subgraph " "
        direction TB

        %% Device 1 (Origin)
        subgraph Device1 [Smartphone Node 1]
            direction TB
            A1(Chat / SOS / Groups):::layerTitle
            AppLayer1["Application Layer"] --> A1

            A2(Routing / Msg Queue / TTL):::layerTitle
            AppLayer1 --> NetworkingLayer1["Networking Layer"] --> A2

            A3(Bluetooth / WiFi Direct):::layerTitle
            NetworkingLayer1 --> CommLayer1["Communication Layer"] --> A3
        end

        %% Device 2 (Hop)
        subgraph Device2 [Smartphone Node 2 (Store & Forward)]
            direction TB
            B1(Chat / SOS / Groups):::layerTitle
            AppLayer2["Application Layer"] --> B1

            B2(Routing / Msg Queue / TTL):::layerTitle
            AppLayer2 --> NetworkingLayer2["Networking Layer"] --> B2

            B3(Bluetooth / WiFi Direct):::layerTitle
            NetworkingLayer2 --> CommLayer2["Communication Layer"] --> B3
        end

        %% Device 3 (Destination)
        subgraph Device3 [Smartphone Node 3]
            direction TB
            C1(Chat / SOS / Groups):::layerTitle
            AppLayer3["Application Layer"] --> C1

            C2(Routing / Msg Queue / TTL):::layerTitle
            AppLayer3 --> NetworkingLayer3["Networking Layer"] --> C2

            C3(Bluetooth / WiFi Direct):::layerTitle
            NetworkingLayer3 --> CommLayer3["Communication Layer"] --> C3
        end

        %% Message Flow Connections (Comm Layer to Comm Layer)
        CommLayer1 -->|Bluetooth/WiFi Direct Connection| CommLayer2
        CommLayer2 -->|Bluetooth/WiFi Direct Connection| CommLayer3

        %% Explicit Multi-Hop Routing Flow
        %% (Representing the decision made at the Networking Layer)
        NetworkingLayer1 -.->|1. Route Discovery/Message Created| A3
        A3 -.->|2. Transmit Packet| B3
        B3 -.->|3. Receive Packet| NetworkingLayer2
        NetworkingLayer2 -.->|4. Store & Check TTL/Next Hop| B2
        B2 -.->|5. Forward Packet| B3
        B3 -.->|6. Transmit Packet| C3
        C3 -.->|7. Receive Packet| NetworkingLayer3
        NetworkingLayer3 -.->|8. Deliver to App (Final Hop)| C1

    end
