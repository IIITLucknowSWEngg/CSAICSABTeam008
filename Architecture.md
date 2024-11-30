# Architecture.md

## 1. System Context Diagram

![System Context Diagram](assets/systemcontextdiagram.png)

```plantuml
@startuml
actor "User" as User
actor "Admin" as Admin

package "Concord App" {
    rectangle "User Authentication \nand Profile Management" as AuthSystem
    rectangle "Text Communication" as TextComm
    rectangle "Voice Communication" as VoiceComm
    rectangle "Video Communication" as VideoComm
    rectangle "Server Management" as ServerMgmt
    rectangle "Moderation Tools" as ModerationTools
    rectangle "Community Engagement" as CommunityEngagement
}

User --> AuthSystem : Register/Login
User --> TextComm : Send Messages
User --> VoiceComm : Join Voice Channels
User --> VideoComm : Start Video Calls
User --> ServerMgmt : Manage Servers
User --> CommunityEngagement : Participate in Polls

Admin --> ServerMgmt : Manage Roles/Permissions
Admin --> ModerationTools : Moderate Activities
@enduml

```

## 2. Container Diagram

![Container Diagram](assets/containerdiagram.png)

```plantuml
@startuml
title Container Diagram - Concord

rectangle "Web/Mobile App" as App <<User Interface>> #lightblue
rectangle "API Gateway" as APIGateway <<API Gateway>> #lightgreen
rectangle "Auth Service" as AuthService <<Service>> #lightyellow
rectangle "Text Service" as TextService <<Service>> #lightyellow
rectangle "Voice Service" as VoiceService <<Service>> #lightyellow
rectangle "Video Service" as VideoService <<Service>> #lightyellow
rectangle "Server Management Service" as ServerMgmtService <<Service>> #lightyellow
rectangle "Moderation Service" as ModerationService <<Service>> #lightyellow
rectangle "Community Engagement Service" as CommunityService <<Service>> #lightyellow

database "User Database" as UserDB <<Database>> #lightpink
database "Server Database" as ServerDB <<Database>> #lightpink
database "Message Database" as MsgDB <<Database>> #lightpink

cloud "External Services" {
    rectangle "Notification Service" as NotificationService <<External Service>> #lightgray
}

App --> APIGateway : API Calls
APIGateway --> AuthService : User Authentication
APIGateway --> TextService : Text Communication
APIGateway --> VoiceService : Voice Communication
APIGateway --> VideoService : Video Communication
APIGateway --> ServerMgmtService : Server Management
APIGateway --> ModerationService : Moderation Tasks
APIGateway --> CommunityService : Polls/Surveys
APIGateway --> NotificationService : Notify Users

AuthService --> UserDB : Manage Users
TextService --> MsgDB : Store Messages
ServerMgmtService --> ServerDB : Manage Servers
@enduml

```
