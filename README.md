# peertopeerCommunicationWeb

Initial design ...



@startuml
skinparam shadowing false
skinparam linetype ortho

title P2P Communications App Architecture

package "Client (Web Browser)" {
  component "WebRTC Connection" as WebRTC
  component "Encryption (Web Crypto API)" as Encryption
  component "Signaling Client" as SignalingClient
  component "TURN Relay" as TURNRelay
  component "Local Data Storage (IndexedDB)" as LocalStorage
}

package "Anonymized Signaling Server" {
  component "Signaling Server" as SignalingServer
  component "Tor Proxy (Optional)" as TorProxy
}

package "TURN Server" {
  component "TURN Server (Coturn)" as TURNServer
}

node "Peer A" {
  [Browser: Peer A] --> WebRTC
  WebRTC --> TURNRelay : ICE Candidates (Relay Only)
  WebRTC --> Encryption
  WebRTC --> SignalingClient : Session Description
  Encryption --> LocalStorage : Encrypted Data
}

node "Peer B" {
  [Browser: Peer B] --> WebRTC
  WebRTC --> TURNRelay : ICE Candidates (Relay Only)
  WebRTC --> Encryption
  WebRTC --> SignalingClient : Session Description
  Encryption --> LocalStorage : Encrypted Data
}

WebRTC -down-> TURNServer : Relayed Media/Data Traffic
SignalingClient --> SignalingServer : ICE Candidate Exchange
SignalingServer --> TorProxy : Optional Anonymization

TURNServer -down-> TURNRelay : Forward Media/Data
@enduml


![image](https://github.com/user-attachments/assets/1e26ba7d-08d2-4c84-87af-ae3900ad42e1)
