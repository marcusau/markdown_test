# AI POC UML Component Diagram Generation

## Problem Statement

The first step of the AI POC as outlined during the Initial POC Meeting by AIGC is to use Design Documents and AI to generate UML Diagrams. In this step, I have received the SRS Document from Erwin, and 5 Design Documents from Jason, namely

1.	01.CLPS-GTAMS-需求清单
2.	02.CLPS-GTAMS-系统架构设计
3.	03.CLPS-APE-GTAMS-技术组件选型 Technical Component List
4.	04.CLPS-APE-GTAMS-工程目录结构说明
5.	08.Resume-Mgr-详细设计

For simplicity, I shall refer to each of these documents as Document 1/2/3/4/5/6, in accordance with the assignment above.

I will be uploading these documents to ChatGPT 4o with the following prompt:
Please generate a UML Component Diagram in PlantUML code based on the following document(s).

## UML Diagram with SRS Only

```plaintext
@startuml
skinparam componentStyle rectangle
skinparam shadowing false
skinparam component {
  BackgroundColor<<UI>> #DDEEFF
  BackgroundColor<<Service>> #FFEEDD
  BackgroundColor<<External>> #DDFFDD
  BorderColor black
}

package "GTAMS Talent Management System" {
  
  [Web UI - Talent库界面] <<UI>> 
  [Resume Upload Service] <<Service>>
  [Resume Parsing Service] <<Service>>
  [Resume Deduplication Logic] <<Service>>
  [Talent Database Service] <<Service>>
  [Advanced Search Service] <<Service>>
  [User Management Service] <<Service>>

  [Third-party Parser (小析)] <<External>>

  ' UI interactions
  [Web UI - Talent库界面] --> [Resume Upload Service]
  [Web UI - Talent库界面] --> [Advanced Search Service]
  [Web UI - Talent库界面] --> [Talent Database Service]
  [Web UI - Talent库界面] --> [Resume Deduplication Logic]

  ' Resume Upload interactions
  [Resume Upload Service] --> [Resume Parsing Service]
  [Resume Upload Service] --> [Resume Deduplication Logic]

  ' Parsing service talks to external parser
  [Resume Parsing Service] --> [Third-party Parser (小析)]

  ' Deduplication logic interacts with DB
  [Resume Deduplication Logic] --> [Talent Database Service]

  ' Advanced Search and DB
  [Advanced Search Service] --> [Talent Database Service]

  ' Resume Uploads tied to user info
  [Resume Upload Service] --> [User Management Service]
  [Web UI - Talent库界面] --> [User Management Service]
}

@enduml
```

## UML Diagram with Documents 12345

```plaintext
@startuml
skinparam componentStyle rectangle

package "Frontend" {
  [Web Client (Vue.js + ElementPlus)]
  [Mobile App (planned)]
  [Mini Program (planned)]
}

package "API Gateway" {
  [Spring Cloud Gateway] as Gateway
}

package "Microservices" {
  [User Management Service]
  [Role/Permission Service]
  [Talent Management Service]
  [Resume Management Service]
  [Order Management Service]
  [Report Service]
  [Notification Service]
  [File Storage Service]
  [Logging Service]
}

package "Infrastructure" {
  [MySQL]
  [Redis]
  [Elasticsearch]
  [MinIO]
  [MQ (RocketMQ)]
  [ELK Stack]
  [SkyWalking]
  [Prometheus]
  [Grafana]
}

' External interaction
[Web Client (Vue.js + ElementPlus)] --> Gateway
[Mobile App (planned)] --> Gateway
[Mini Program (planned)] --> Gateway

' API Gateway routes to Microservices
Gateway --> [User Management Service]
Gateway --> [Role/Permission Service]
Gateway --> [Talent Management Service]
Gateway --> [Resume Management Service]
Gateway --> [Order Management Service]
Gateway --> [Report Service]
Gateway --> [Notification Service]
Gateway --> [File Storage Service]
Gateway --> [Logging Service]

' Microservices and Infrastructure
[User Management Service] --> [MySQL]
[Role/Permission Service] --> [MySQL]
[Talent Management Service] --> [MySQL]
[Resume Management Service] --> [MySQL]
[Order Management Service] --> [MySQL]
[Report Service] --> [Elasticsearch]
[Notification Service] --> [MQ (RocketMQ)]
[File Storage Service] --> [MinIO]
[Logging Service] --> [ELK Stack]

' Monitoring and Observability
[User Management Service] ..> [SkyWalking]
[Talent Management Service] ..> [SkyWalking]
[Resume Management Service] ..> [SkyWalking]
[Prometheus] --> [Grafana]

@enduml
```

## UML Diagram with Documents 1234

```plaintext
@startuml
title GTAMS UML Component Diagram

package "Frontend" {
  [Web App (Vue.js)] --> [API Gateway]
  [Mobile App / Mini Program] --> [API Gateway]
}

package "API Gateway" {
  [API Gateway] --> [System Management Service]
  [API Gateway] --> [Talent Management Service]
  [API Gateway] --> [Resume Management Service]
  [API Gateway] --> [Order Management Service]
  [API Gateway] --> [Report & Portal Service]
  [API Gateway] --> [Notification Service]
  [API Gateway] --> [File Storage Service]
}

package "Microservices" {
  [System Management Service] --> [MySQL DB]
  [Talent Management Service] --> [MySQL DB]
  [Resume Management Service] --> [MySQL DB]
  [Order Management Service] --> [MySQL DB]
  [Report & Portal Service] --> [MySQL DB]
  [Notification Service] --> [Redis] 
  [Notification Service] --> [RocketMQ]
  [Resume Management Service] --> [Elasticsearch]
  [File Storage Service] --> [MinIO]
}

package "Infrastructure" {
  [Spring Security & OAuth2]
  [Redis]
  [Elasticsearch]
  [MinIO]
  [RocketMQ]
  [Prometheus + Grafana]
  [ELK Stack (Logstash + Kibana)]
  [SkyWalking]

  [System Management Service] --> [Spring Security & OAuth2]
  [All Services] --> [Prometheus + Grafana]
  [All Services] --> [ELK Stack (Logstash + Kibana)]
  [All Services] --> [SkyWalking]
}

@enduml
```

## UML Diagram with Documents 1235
```plaintext
@startuml
title GTAMS 系统 UML 组件图

package "前端客户端" {
  [Web前端(Vue.js)] <<component>>
  [小程序/移动端] <<component>>
}

package "API接入层" {
  [API网关服务 (Spring Cloud Gateway)] <<component>>
}

package "基础服务" {
  [文件存储服务 (MinIO)] <<component>>
  [消息提醒服务] <<component>>
  [日志服务] <<component>>
}

package "业务微服务" {
  [系统管理服务] <<component>>
  [简历管理服务] <<component>>
  [人才管理服务] <<component>>
  [订单交付服务] <<component>>
  [报表统计服务] <<component>>
}

package "数据层" {
  [MySQL数据库] <<database>>
  [Redis缓存] <<database>>
  [Elasticsearch检索] <<database>>
  [对象存储 (MinIO)] <<database>>
}

package "运维监控" {
  [日志采集 (ELK)] <<component>>
  [性能监控 (Prometheus + Grafana)] <<component>>
  [链路追踪 (SkyWalking)] <<component>>
}

' 客户端交互
[Web前端(Vue.js)] --> [API网关服务 (Spring Cloud Gateway)]
[小程序/移动端] --> [API网关服务 (Spring Cloud Gateway)]

' 网关转发请求到各微服务
[API网关服务 (Spring Cloud Gateway)] --> [系统管理服务]
[API网关服务 (Spring Cloud Gateway)] --> [简历管理服务]
[API网关服务 (Spring Cloud Gateway)] --> [人才管理服务]
[API网关服务 (Spring Cloud Gateway)] --> [订单交付服务]
[API网关服务 (Spring Cloud Gateway)] --> [报表统计服务]

' 各业务服务访问基础服务
[简历管理服务] --> [文件存储服务 (MinIO)]
[系统管理服务] --> [消息提醒服务]
[系统管理服务] --> [日志服务]

' 各服务访问数据库
[系统管理服务] --> [MySQL数据库]
[简历管理服务] --> [MySQL数据库]
[人才管理服务] --> [MySQL数据库]
[订单交付服务] --> [MySQL数据库]
[报表统计服务] --> [Elasticsearch检索]
[人才管理服务] --> [Redis缓存]

' 监控系统连接所有微服务
[日志采集 (ELK)] ..> [系统管理服务]
[性能监控 (Prometheus + Grafana)] ..> [简历管理服务]
[链路追踪 (SkyWalking)] ..> [API网关服务 (Spring Cloud Gateway)]

@enduml
```

## UML Diagram with Documents 1245

```plaintext
@startuml
title GTAMS UML Component Diagram

package "Frontend Clients" {
  [Web Client]
  [Mobile App]
  [WeChat Mini Program]
}

package "API Gateway" {
  [Spring Cloud Gateway] <<component>>
}

package "Microservices" {
  [System Management Service] <<component>>
  [Talent Management Service] <<component>>
  [Resume Management Service] <<component>>
  [Order Management Service] <<component>>
  [Report & Portal Service] <<component>>
  [Notification Service] <<component>>
  [File Storage Service] <<component>>
}

package "Infrastructure" {
  [MySQL] <<database>>
  [Redis] <<database>>
  [Elasticsearch] <<database>>
  [MinIO] <<storage>>
  [RocketMQ] <<queue>>
  [ELK Stack]
  [Prometheus]
  [Grafana]
  [SkyWalking]
}

' Frontend to Gateway
[Web Client] --> [Spring Cloud Gateway]
[Mobile App] --> [Spring Cloud Gateway]
[WeChat Mini Program] --> [Spring Cloud Gateway]

' Gateway to Microservices
[Spring Cloud Gateway] --> [System Management Service]
[Spring Cloud Gateway] --> [Talent Management Service]
[Spring Cloud Gateway] --> [Resume Management Service]
[Spring Cloud Gateway] --> [Order Management Service]
[Spring Cloud Gateway] --> [Report & Portal Service]
[Spring Cloud Gateway] --> [Notification Service]
[Spring Cloud Gateway] --> [File Storage Service]

' Microservices and Infra
[System Management Service] --> [MySQL]
[System Management Service] --> [Redis]
[Resume Management Service] --> [MySQL]
[Resume Management Service] --> [MinIO]
[Talent Management Service] --> [MySQL]
[Talent Management Service] --> [Redis]
[Order Management Service] --> [MySQL]
[Order Management Service] --> [RocketMQ]
[Report & Portal Service] --> [Elasticsearch]
[Notification Service] --> [RocketMQ]

' Monitoring and Logging
[System Management Service] --> [ELK Stack]
[System Management Service] --> [Prometheus]
[System Management Service] --> [SkyWalking]
[Resume Management Service] --> [SkyWalking]
[Talent Management Service] --> [Grafana]

@enduml
```

## UML Diagram with Documents 1345

```plaintext
@startuml
skinparam componentStyle rectangle

package "GTAMS System" {
  [Frontend (Vue3 + Vite)] --> [API Gateway (Spring Cloud Gateway)]

  [API Gateway (Spring Cloud Gateway)] --> [gtams-system-mgr]
  [API Gateway (Spring Cloud Gateway)] --> [gtams-cv-resume]
  [API Gateway (Spring Cloud Gateway)] --> [gtams-talent]
  [API Gateway (Spring Cloud Gateway)] --> [gtams-order]
  [API Gateway (Spring Cloud Gateway)] --> [gtams-report]
  [API Gateway (Spring Cloud Gateway)] --> [gtams-notice]

  component "gtams-system-mgr" {
    [User Mgmt] 
    [Role & Auth Mgmt]
    [Menu Mgmt]
  }

  component "gtams-cv-resume" {
    [Resume Basic Info]
    [Resume File Mgmt]
    [Intention Mgmt]
    [Education History]
    [Work Experience]
  }

  component "gtams-talent" {
    [Talent Pool]
    [Talent Folder]
    [Delivery Logs]
  }

  component "gtams-order" {
    [Job Order Mgmt]
  }

  component "gtams-report" {
    [Dashboard & Reports]
  }

  component "gtams-notice" {
    [Message Push]
    [Email Alerts]
  }
}

database "MySQL" {
  [TalentDB]
  [ResumeDB]
  [SystemDB]
}

[gtams-cv-resume] --> [ResumeDB]
[gtams-talent] --> [TalentDB]
[gtams-system-mgr] --> [SystemDB]

[Redis Cache] ..> [API Gateway (Spring Cloud Gateway)]
[Elasticsearch] ..> [gtams-cv-resume]

@enduml
```

## UML Diagram with Documents 2345

```plaintext
@startuml
skinparam componentStyle rectangle

package "Frontend Clients" {
  [PC Web (Vue3 + ElementPlus)] <<component>>
  [Mobile/Web App (Vant/WeChat)] <<component>>
}

[Spring Cloud Gateway] <<component>>

package "Business Microservices" {
  [System Management Service] <<component>>
  [Resume Management Service] <<component>>
  [Talent Management Service] <<component>>
  [Order Management Service] <<component>>
  [Report & Dashboard Service] <<component>>
  [Message Notification Service] <<component>>
}

package "Infrastructure Services" {
  [File Storage Service (MinIO)] <<component>>
  [Logging Service (ELK)] <<component>>
  [Monitoring Service (Prometheus/Grafana)] <<component>>
  [Trace Service (SkyWalking)] <<component>>
}

package "Data Layer" {
  [MySQL] <<database>>
  [Redis] <<database>>
  [Elasticsearch] <<database>>
}

' External dependencies
[RocketMQ] <<queue>>

' Connections
[PC Web (Vue3 + ElementPlus)] --> [Spring Cloud Gateway]
[Mobile/Web App (Vant/WeChat)] --> [Spring Cloud Gateway]

[Spring Cloud Gateway] --> [System Management Service]
[Spring Cloud Gateway] --> [Resume Management Service]
[Spring Cloud Gateway] --> [Talent Management Service]
[Spring Cloud Gateway] --> [Order Management Service]
[Spring Cloud Gateway] --> [Report & Dashboard Service]
[Spring Cloud Gateway] --> [Message Notification Service]

[System Management Service] --> [MySQL]
[Resume Management Service] --> [MySQL]
[Talent Management Service] --> [MySQL]
[Order Management Service] --> [MySQL]
[Report & Dashboard Service] --> [MySQL]

[All Microservices] --> [Redis]
[Resume Management Service] --> [Elasticsearch]

[Resume Management Service] --> [File Storage Service (MinIO)]
[Message Notification Service] --> [RocketMQ]

[All Microservices] --> [Logging Service (ELK)]
[All Microservices] --> [Monitoring Service (Prometheus/Grafana)]
[All Microservices] --> [Trace Service (SkyWalking)]

@enduml
```

## UML Diagram with Documents 123

```plaintext
@startuml
title GTAMS - Component Diagram

package "Frontend" {
  [Web_Client] 
  [Mobile_Client]
}

package "API Gateway" {
  [API_Gateway]
}

package "Backend Services" {
  [User_Management_Service]
  [Permission_Management_Service]
  [Talent_Management_Service]
  [Resume_Management_Service]
  [Order_Delivery_Service]
  [Report_Portal_Service]
  [Message_Notification_Service]
  [File_Storage_Service]
}

package "Databases_And_Storage" {
  [MySQL]
  [Redis]
  [Elasticsearch]
  [MinIO]
}

package "External Systems" {
  [CRM_System]
  [Resume_Parser]
}

package "Monitoring_And_DevOps" {
  [ELK_Stack]
  [Prometheus]
  [Grafana]
  [SkyWalking]
  [Kubernetes]
  [GitLab_CI_CD]
}

Web_Client --> API_Gateway
Mobile_Client --> API_Gateway

API_Gateway --> User_Management_Service
API_Gateway --> Permission_Management_Service
API_Gateway --> Talent_Management_Service
API_Gateway --> Resume_Management_Service
API_Gateway --> Order_Delivery_Service
API_Gateway --> Report_Portal_Service
API_Gateway --> Message_Notification_Service
API_Gateway --> File_Storage_Service

Talent_Management_Service --> Resume_Management_Service
Order_Delivery_Service --> Talent_Management_Service
Resume_Management_Service --> File_Storage_Service
Resume_Management_Service --> Resume_Parser
Order_Delivery_Service --> CRM_System

User_Management_Service --> MySQL
Permission_Management_Service --> MySQL
Talent_Management_Service --> MySQL
Resume_Management_Service --> MySQL
Order_Delivery_Service --> MySQL
Report_Portal_Service --> MySQL
Message_Notification_Service --> MySQL
File_Storage_Service --> MinIO

User_Management_Service --> Redis
Permission_Management_Service --> Redis
Talent_Management_Service --> Redis
Resume_Management_Service --> Redis
Order_Delivery_Service --> Redis

Talent_Management_Service --> Elasticsearch
Resume_Management_Service --> Elasticsearch

User_Management_Service --> ELK_Stack
All_Backend_Services --> Prometheus
All_Backend_Services --> Grafana
All_Backend_Services --> SkyWalking

All_Backend_Services --> Kubernetes
All_Backend_Services --> GitLab_CI_CD

@enduml
```

## UML Diagram with Documents 124

```plaintext
@startuml
skinparam componentStyle rectangle

package "Frontend" {
  [Vue App] --> [API Gateway]
}

package "API Gateway" {
  component "Spring Cloud Gateway" as Gateway
}

package "Microservices" {
  component "System Management Service" as SystemMgr
  component "Resume Management Service" as ResumeMgr
  component "Talent Management Service" as TalentMgr
  component "Order & Delivery Service" as OrderMgr
  component "Report & Portal Service" as ReportMgr
  component "Message Service" as MsgSvc
  component "File Storage Service" as FileSvc
}

package "Infrastructure" {
  [MySQL]
  [Redis]
  [Elasticsearch]
  [MinIO]
  [Prometheus]
  [Grafana]
  [SkyWalking]
  [ELK Stack]
}

' Gateway forwards requests
[API Gateway] --> SystemMgr
[API Gateway] --> ResumeMgr
[API Gateway] --> TalentMgr
[API Gateway] --> OrderMgr
[API Gateway] --> ReportMgr
[API Gateway] --> MsgSvc
[API Gateway] --> FileSvc

' Each service accesses shared infra
SystemMgr --> [MySQL]
ResumeMgr --> [MySQL]
TalentMgr --> [MySQL]
OrderMgr --> [MySQL]
ReportMgr --> [MySQL]

SystemMgr --> [Redis]
ResumeMgr --> [Elasticsearch]
FileSvc --> [MinIO]

' Monitoring and logging
SystemMgr --> [Prometheus]
SystemMgr --> [Grafana]
SystemMgr --> [SkyWalking]
SystemMgr --> [ELK Stack]

MsgSvc --> [Redis]
MsgSvc --> [ELK Stack]

@enduml
```

## UML Diagram with Documents 125

```plaintext
@startuml
skinparam componentStyle rectangle

package "Frontend" {
  [Vue App] --> [API Gateway]
}

package "Gateway Layer" {
  [API Gateway] --> [User Service API]
  [API Gateway] --> [Resume Service API]
  [API Gateway] --> [Talent Service API]
  [API Gateway] --> [Order Service API]
  [API Gateway] --> [Report Service API]
  [API Gateway] --> [Message Service API]
  [API Gateway] --> [File Storage Service]
}

package "Microservices" {
  [User Service API] --> [User Service Impl]
  [Resume Service API] --> [Resume Service Impl]
  [Talent Service API] --> [Talent Service Impl]
  [Order Service API] --> [Order Service Impl]
  [Report Service API] --> [Report Service Impl]
  [Message Service API] --> [Message Service Impl]
  [File Storage Service] --> [MinIO]
}

package "Database Layer" {
  [User Service Impl] --> [System DB]
  [Resume Service Impl] --> [Resume DB]
  [Talent Service Impl] --> [Talent DB]
  [Order Service Impl] --> [Order DB]
  [Report Service Impl] --> [Analytics DB]
  [Message Service Impl] --> [Notification DB]
}

note right of [File Storage Service]
MinIO is used to store resume
files, avatars, and other uploads.
end note

note bottom of [Resume Service API]
Handles resume parsing,
education, experience, and skills.
end note

note bottom of [Talent Service API]
Manages talent records, pools,
delivery logs, and folders.
end note

note bottom of [Order Service API]
Covers customers, contacts,
managers, and order tracking.
end note
@enduml
```

## UML Diagram with Documents 134

```plaintext
@startuml
skinparam componentStyle rectangle

package "GTAMS Platform" {
  [Frontend (Vue3 + TS)] as FE
  [API Gateway\n(Spring Cloud Gateway)] as Gateway
  [System Management\n(gtams-system-mgr)] as SystemMgr
  [Talent Management\n(gtams-talent)] as TalentMgr
  [Resume Management\n(gtams-cv-resume)] as ResumeMgr
  [Order Management\n(gtams-order)] as OrderMgr
  [Report Management\n(gtams-report)] as ReportMgr
  [Notification Service\n(gtams-notice)] as NoticeMgr

  [MySQL Database] as DB
  [Redis Cache] as Redis
  [Elasticsearch] as ES
  [MinIO\n(Resume/File Storage)] as MinIO
  [RocketMQ] as MQ
}

FE --> Gateway : REST API
Gateway --> SystemMgr
Gateway --> TalentMgr
Gateway --> ResumeMgr
Gateway --> OrderMgr
Gateway --> ReportMgr
Gateway --> NoticeMgr

SystemMgr --> DB
TalentMgr --> DB
ResumeMgr --> DB
OrderMgr --> DB
ReportMgr --> DB
NoticeMgr --> DB

TalentMgr --> Redis
ResumeMgr --> MinIO
ResumeMgr --> ES
NoticeMgr --> MQ

@enduml
```

## UML Diagram with Documents 135

```plaintext
@startuml
title GTAMS UML Component Diagram

package "Frontend" {
  [Web UI (Vue3 + ElementPlus)] --> [API Gateway]
}

package "Backend Services" {
  [API Gateway] --> [Resume Management Service]
  [API Gateway] --> [Talent Management Service]
  [API Gateway] --> [Order Management Service]
  [API Gateway] --> [User & Auth Service]
  [API Gateway] --> [Dictionary & Log Service]
}

package "Resume Management Service" {
  [Resume Management Service] --> [MySQL (Resume DB)]
  [Resume Management Service] --> [MinIO]
  [Resume Management Service] --> [Elasticsearch]
}

package "Talent Management Service" {
  [Talent Management Service] --> [MySQL (Talent DB)]
  [Talent Management Service] --> [Redis]
}

package "Order Management Service" {
  [Order Management Service] --> [CRM System]
  [Order Management Service] --> [MySQL (Order DB)]
}

package "User & Auth Service" {
  [User & Auth Service] --> [Spring Security + OAuth2]
  [User & Auth Service] --> [Redis]
}

package "System Infrastructure" {
  [Spring Cloud Config] --> [All Services]
  [Consul Service Registry] --> [All Services]
  [RocketMQ] --> [All Services]
  [Logstash + Kibana] --> [All Services]
  [Prometheus + Grafana] --> [All Services]
}

@enduml
```

## UML Diagram with Documents 145

```plaintext
@startuml
left to right direction
skinparam componentStyle rectangle

component "Frontend (Web/App/MiniApp)" as Frontend
component "API Gateway" as Gateway
component "System Management\n(gtams-system-mgr)" as SystemMgr
component "Talent Management\n(gtams-talent)" as TalentMgr
component "Resume Management\n(gtams-cv-resume)" as ResumeMgr
component "Order Management\n(gtams-order)" as OrderMgr
component "Report Management\n(gtams-report)" as ReportMgr
component "Notification Management\n(gtams-notice)" as NoticeMgr
component "APE Framework" as APEFramework
component "MySQL/Redis" as Database
component "Redis MQ" as MQ
component "External CRM/EHR APIs" as ExternalAPI

Frontend --> Gateway
Gateway --> SystemMgr
Gateway --> TalentMgr
Gateway --> ResumeMgr
Gateway --> OrderMgr
Gateway --> ReportMgr
Gateway --> NoticeMgr

SystemMgr --> Database
TalentMgr --> Database
ResumeMgr --> Database
OrderMgr --> Database
ReportMgr --> Database
NoticeMgr --> Database

SystemMgr --> MQ
NoticeMgr --> MQ

OrderMgr --> ExternalAPI

SystemMgr --> APEFramework
TalentMgr --> APEFramework
ResumeMgr --> APEFramework
OrderMgr --> APEFramework
ReportMgr --> APEFramework
NoticeMgr --> APEFramework
@enduml
```

## UML Diagram with Documents 234

```plaintext
@startuml
title GTAMS UML Component Diagram

package "Frontend" {
  [PC Web (Vue3 + ElementPlus)]
  [Mobile App / Mini Program (Vant / uni-app)]
}

package "API Gateway" {
  [Spring Cloud Gateway] as Gateway
}

package "Microservices" {
  [System Management Service]
  [Resume Management Service]
  [Talent Management Service]
  [Order Management Service]
  [Report & Portal Service]
  [Notification Service]
  [File Storage Service]
}

package "Infrastructure Services" {
  [MySQL]
  [Redis]
  [Elasticsearch]
  [MinIO]
  [RocketMQ]
}

package "Monitoring & Logging" {
  [ELK Stack]
  [Prometheus]
  [Grafana]
  [SkyWalking]
}

' Frontend to Gateway
[PC Web (Vue3 + ElementPlus)] --> Gateway
[Mobile App / Mini Program (Vant / uni-app)] --> Gateway

' Gateway routes to microservices
Gateway --> [System Management Service]
Gateway --> [Resume Management Service]
Gateway --> [Talent Management Service]
Gateway --> [Order Management Service]
Gateway --> [Report & Portal Service]
Gateway --> [Notification Service]
Gateway --> [File Storage Service]

' Services interact with infrastructure
[Resume Management Service] --> [MySQL]
[Talent Management Service] --> [MySQL]
[Order Management Service] --> [MySQL]
[System Management Service] --> [MySQL]
[Notification Service] --> [RocketMQ]
[Resume Management Service] --> [Elasticsearch]
[All Microservices] --> [Redis]
[File Storage Service] --> [MinIO]

' Monitoring tools
[All Microservices] --> [ELK Stack]
[All Microservices] --> [Prometheus]
[Prometheus] --> [Grafana]
[All Microservices] --> [SkyWalking]

@enduml
```

## UML Diagram with Documents 235

```plaintext
@startuml
skinparam componentStyle rectangle

package "Frontend" {
  [Vue.js UI] --> [API Gateway]
}

package "API Gateway" {
  [API Gateway] --> [Resume Service]
  [API Gateway] --> [File Service]
  [API Gateway] --> [Integration Service]
  [API Gateway] --> [System Service]
}

package "Microservices" {
  [Resume Service]
  [File Service]
  [Integration Service]
  [System Service]
}

package "Infrastructure" {
  [MySQL]
  [Redis]
  [Elasticsearch]
  [MinIO]
  [RocketMQ]
  [SkyWalking]
}

[Resume Service] --> [MySQL]
[Resume Service] --> [Redis]
[Resume Service] --> [Elasticsearch]
[Resume Service] --> [File Service]
[Resume Service] --> [Integration Service]
[Resume Service] --> [System Service]

[File Service] --> [MinIO]
[Resume Service] --> [RocketMQ]
[Resume Service] --> [SkyWalking]

@enduml
```

## UML Diagram with Documents 245

```plaintext
@startuml
' Specify diagram type
!define COMPONENT_STYLE rectangle
left to right direction
title GTAMS Component Diagram

' Client layer
component "Web Frontend\n(Vue.js)" as WebUI
component "Mobile App / Mini Program" as MobileApp

' Gateway
component "Spring Cloud Gateway" as Gateway

' Services
component "System Management Service" as SysMgr
component "Resume Management Service" as ResumeMgr
component "Talent Management Service" as TalentMgr
component "Order & Delivery Service" as OrderMgr
component "Report Service" as ReportMgr
component "Notification Service" as NoticeMgr
component "File Storage Service\n(MinIO)" as FileMgr

' Infrastructure
database "MySQL DB" as MySQL
component "Redis Cache" as Redis
component "ELK Stack" as ELK
component "Prometheus" as Prometheus
component "Grafana" as Grafana
component "SkyWalking" as SkyWalking

' Connections
WebUI --> Gateway
MobileApp --> Gateway

Gateway --> SysMgr
Gateway --> ResumeMgr
Gateway --> TalentMgr
Gateway --> OrderMgr
Gateway --> ReportMgr
Gateway --> NoticeMgr
Gateway --> FileMgr

SysMgr --> MySQL
ResumeMgr --> MySQL
TalentMgr --> MySQL
OrderMgr --> MySQL

SysMgr --> Redis
ResumeMgr --> Redis

SysMgr --> ELK
SysMgr --> Prometheus
Prometheus --> Grafana
Gateway --> SkyWalking

@enduml
```

## UML Diagram with Documents 345

```plaintext
@startuml
skinparam componentStyle rectangle
skinparam defaultTextAlignment center

package "前端" {
  [Resume Web] <<component>> 
}

package "后端 API 接口层" {
  [Resume AdminController] <<component>>
  [Resume AppController] <<component>>
}

package "后端服务层" {
  [ResumeService] <<component>>
  [EducationService] <<component>>
  [WorkExperienceService] <<component>>
  [ProjectExperienceService] <<component>>
  [IntentionService] <<component>>
}

package "后端数据访问层" {
  [ResumeMapper] <<component>>
  [EducationMapper] <<component>>
  [WorkExperienceMapper] <<component>>
  [ProjectExperienceMapper] <<component>>
  [IntentionMapper] <<component>>
  [RedisDAO] <<component>>
}

package "文件处理" {
  [MinIO File Storage] <<external>>
}

package "外部系统" {
  [MySQL] <<database>>
  [Redis] <<database>>
}

package "基础框架组件" {
  [Spring Boot]
  [MyBatis-Plus]
  [Spring Security]
  [OAuth2]
  [Spring Cloud Gateway]
}

' Frontend to Backend
[Resume Web] --> [Resume AdminController] : REST API\n/admin-api/resume/...

' Controller to Service
[Resume AdminController] --> [ResumeService]
[Resume AdminController] --> [EducationService]
[Resume AdminController] --> [WorkExperienceService]
[Resume AdminController] --> [ProjectExperienceService]
[Resume AdminController] --> [IntentionService]

' Service to Mapper
[ResumeService] --> [ResumeMapper]
[EducationService] --> [EducationMapper]
[WorkExperienceService] --> [WorkExperienceMapper]
[ProjectExperienceService] --> [ProjectExperienceMapper]
[IntentionService] --> [IntentionMapper]

' File Upload/Download
[ResumeService] --> [MinIO File Storage]

' DAO to Infrastructure
[ResumeMapper] --> [MySQL]
[EducationMapper] --> [MySQL]
[WorkExperienceMapper] --> [MySQL]
[ProjectExperienceMapper] --> [MySQL]
[IntentionMapper] --> [MySQL]
[RedisDAO] --> [Redis]

' Services use common frameworks
[ResumeService] ..> [Spring Boot]
[ResumeMapper] ..> [MyBatis-Plus]
[Resume AdminController] ..> [Spring Security]
[Resume AdminController] ..> [OAuth2]
[Resume Web] --> [Spring Cloud Gateway]

@enduml
```

## UML Diagram with Documents 12

```plaintext
@startuml
title CLPS GTAMS - UML Component Diagram

package "Frontend" {
  [Vue Web App] <<Component>>
  [Mobile App] <<Component>>
  [WeChat Mini Program] <<Component>>
}

package "API Gateway" {
  [Spring Cloud Gateway] <<Component>>
}

package "Microservices" {
  [System Management Service] <<Component>>
  [Talent Management Service] <<Component>>
  [Resume Management Service] <<Component>>
  [Order and Delivery Service] <<Component>>
  [Report and Portal Service] <<Component>>
  [Message Notification Service] <<Component>>
  [File Storage Service] <<Component>>
}

package "Data Layer" {
  [MySQL] <<Database>>
  [Redis] <<Database>>
  [Elasticsearch] <<Database>>
  [MinIO] <<Database>>
}

package "Monitoring & Logging" {
  [Prometheus] <<Component>>
  [Grafana] <<Component>>
  [SkyWalking] <<Component>>
  [ELK Stack] <<Component>>
}

[Vue Web App] --> [Spring Cloud Gateway]
[Mobile App] --> [Spring Cloud Gateway]
[WeChat Mini Program] --> [Spring Cloud Gateway]

[Spring Cloud Gateway] --> [System Management Service]
[Spring Cloud Gateway] --> [Talent Management Service]
[Spring Cloud Gateway] --> [Resume Management Service]
[Spring Cloud Gateway] --> [Order and Delivery Service]
[Spring Cloud Gateway] --> [Report and Portal Service]
[Spring Cloud Gateway] --> [Message Notification Service]
[Spring Cloud Gateway] --> [File Storage Service]

[System Management Service] --> [MySQL]
[Talent Management Service] --> [MySQL]
[Resume Management Service] --> [MySQL]
[Order and Delivery Service] --> [MySQL]
[Report and Portal Service] --> [Elasticsearch]
[Message Notification Service] --> [Redis]
[File Storage Service] --> [MinIO]

[System Management Service] --> [Redis]
[Resume Management Service] --> [Elasticsearch]
[Talent Management Service] --> [Elasticsearch]

[System Management Service] --> [SkyWalking]
[Talent Management Service] --> [SkyWalking]
[Order and Delivery Service] --> [SkyWalking]
[Resume Management Service] --> [SkyWalking]

[SkyWalking] --> [ELK Stack]
[Prometheus] --> [Grafana]

@enduml
```

## UML Diagram with Documents 13

```plaintext
@startuml
title GTAMS 系统 UML Component Diagram

package "Frontend" {
  [Vue.js 3.4.x] <<framework>>
  [Vite / Webpack] <<builder>>
  [Element Plus / Vant] <<UI>>
  [Pinia] <<State Management>>
  [Vue Router] <<Routing>>
  [Axios] <<HTTP>>
  [Vue i18n] <<i18n>>
}

package "Backend" {
  [Spring Boot 3.2.x] <<framework>>
  [Spring Cloud 2022.0.x] <<microservice>>
  [Spring Security & OAuth2] <<auth>>
  [MyBatis-Plus 3.5.x] <<ORM>>
  [Spring Cloud Gateway] <<gateway>>
  [Spring Data JPA] <<ORM>>
  [XXL-Job] <<scheduler>>
  [Swagger 3.0.x] <<doc>>
}

package "Data & Storage" {
  [MySQL 8.0.x] <<database>>
  [Redis 7.2.x] <<cache>>
  [Elasticsearch 8.11.x] <<search>>
  [MinIO 2024.02.x] <<object storage>>
}

package "Monitoring & DevOps" {
  [Prometheus + Grafana] <<monitoring>>
  [SkyWalking] <<tracing>>
  [GitLab CI/CD] <<CI/CD>>
  [Kubernetes] <<orchestration>>
  [SonarQube] <<quality>>
}

package "Messaging & Config" {
  [RocketMQ] <<message queue>>
  [Consul] <<service discovery>>
  [Spring Cloud Config] <<config>>
}

package "Modules" {
  [System Management]
  [Talent Management]
  [Resume Management]
  [Order Management]
  [Report & Portal]
  [Notification Management]
}

' Connections
[Frontend] --> [Spring Cloud Gateway]
[Spring Cloud Gateway] --> [Spring Boot 3.2.x]
[Spring Boot 3.2.x] --> [System Management]
[Spring Boot 3.2.x] --> [Talent Management]
[Spring Boot 3.2.x] --> [Resume Management]
[Spring Boot 3.2.x] --> [Order Management]
[Spring Boot 3.2.x] --> [Report & Portal]
[Spring Boot 3.2.x] --> [Notification Management]

[System Management] --> [MySQL 8.0.x]
[Talent Management] --> [Elasticsearch 8.11.x]
[Resume Management] --> [MinIO 2024.02.x]
[Order Management] --> [Redis 7.2.x]
[Notification Management] --> [RocketMQ]

[Spring Boot 3.2.x] --> [Spring Security & OAuth2]
[Spring Boot 3.2.x] --> [Spring Data JPA]
[Spring Boot 3.2.x] --> [MyBatis-Plus 3.5.x]

[Spring Boot 3.2.x] --> [Prometheus + Grafana]
[Spring Boot 3.2.x] --> [SkyWalking]
[Spring Boot 3.2.x] --> [Swagger 3.0.x]
[Spring Boot 3.2.x] --> [XXL-Job]

@enduml
```

## UML Diagram with Documents 14

```plaintext
@startuml
skinparam componentStyle rectangle
skinparam defaultTextAlignment center

package "Frontend (Vue + Vite)" {
  [Web UI] as UI
}

package "Backend Gateway" {
  [API Gateway] as Gateway
}

package "GTAMS Modules" {
  [System Management\n(gtams-system-mgr)] as SystemMgr
  [Resume Management\n(gtams-cv-resume)] as ResumeMgr
  [Talent Management\n(gtams-talent)] as TalentMgr
  [Order Management\n(gtams-order)] as OrderMgr
  [Report Management\n(gtams-report)] as ReportMgr
  [Notification Management\n(gtams-notice)] as NoticeMgr
}

package "Infrastructure" {
  [APE Framework\nape-spring-boot-starter-*] as Framework
  [APE Infra] as Infra
}

package "External Systems" {
  [CRM System]
  [TS System]
}

' Frontend to gateway
UI --> Gateway : HTTP (REST API)

' Gateway routes to backend modules
Gateway --> SystemMgr
Gateway --> ResumeMgr
Gateway --> TalentMgr
Gateway --> OrderMgr
Gateway --> ReportMgr
Gateway --> NoticeMgr

' Modules using shared infrastructure
SystemMgr --> Framework
ResumeMgr --> Framework
TalentMgr --> Framework
OrderMgr --> Framework
ReportMgr --> Framework
NoticeMgr --> Framework

' Backend modules to external systems
OrderMgr --> [CRM System] : Client Info Sync
OrderMgr --> [TS System] : Order Info Sync

@enduml
```

## UML Diagram with Documents 15

```plaintext
@startuml
skinparam componentStyle rectangle

package GTAMS {
  [UserManagement] 
  [RoleManagement] 
  [DepartmentManagement]
  [MenuPermission]
  [DataDictionary]
  [LogManagement]
}

package ResumeManager {
  [TalentBasicService]
  [ResumeAttachmentService]
  [IntentionService]
  [EducationService]
  [WorkExperienceService]
  [ProjectExperienceService]
  [AwardService]
  [SkillService]
  [TrainingService]
  [LanguageService]
  [AdditionalInfoService]
}

package OrderSystem {
  [ClientManagement]
  [ClientContactManagement]
  [AccountManagerService]
  [OrderManagement]
  [JobRequirementService]
  [TalentDeliveryService]
}

package Communication {
  [CommunicationRecordService]
  [TalentPoolService]
  [TalentCollectionService]
}

package PortalAndReports {
  [ReportService]
  [PortalDashboardService]
}

package Messaging {
  [EmailService]
  [MessageService]
  [AnnouncementService]
}

[UserManagement] --> [RoleManagement]
[UserManagement] --> [DepartmentManagement]
[UserManagement] --> [MenuPermission]

[ResumeAttachmentService] --> [TalentBasicService]
[IntentionService] --> [TalentBasicService]
[EducationService] --> [TalentBasicService]
[WorkExperienceService] --> [TalentBasicService]
[ProjectExperienceService] --> [WorkExperienceService]
[AwardService] --> [TalentBasicService]
[SkillService] --> [TalentBasicService]
[TrainingService] --> [TalentBasicService]
[LanguageService] --> [TalentBasicService]
[AdditionalInfoService] --> [TalentBasicService]

[OrderManagement] --> [ClientManagement]
[OrderManagement] --> [AccountManagerService]
[OrderManagement] --> [ClientContactManagement]
[JobRequirementService] --> [OrderManagement]
[TalentDeliveryService] --> [JobRequirementService]
[TalentDeliveryService] --> [TalentBasicService]

[CommunicationRecordService] --> [TalentBasicService]
[CommunicationRecordService] --> [OrderManagement]
[TalentPoolService] --> [TalentBasicService]
[TalentCollectionService] --> [TalentBasicService]

[ReportService] --> [OrderManagement]
[ReportService] --> [TalentBasicService]

[PortalDashboardService] --> [ReportService]

[EmailService] --> [TalentDeliveryService]
[MessageService] --> [UserManagement]
[AnnouncementService] --> [UserManagement]

@enduml
```

## UML Diagram with Documents 23

```plaintext
@startuml
skinparam componentStyle rectangle

package "Client Applications" {
  [PCWeb]
  [MobileApp]
  [WeChatMiniProgram]
}

package "Gateway Layer" {
  [APIGateway] <<SpringCloudGateway>>
}

package "Business Microservices" {
  [UserService]
  [PermissionService]
  [TalentService]
  [ResumeService]
  [OrderService]
  [ReportService]
  [MessageService]
}

package "Infrastructure Services" {
  [FileService] <<MinIO>>
  [LogService] <<ELK>>
  [MonitoringService] <<Prometheus/Grafana>>
  [TracingService] <<SkyWalking>>
  [MQService] <<RocketMQ>>
}

package "Data Layer" {
  [MySQL]
  [Redis]
  [Elasticsearch]
}

' Client to Gateway
[PCWeb] --> [APIGateway]
[MobileApp] --> [APIGateway]
[WeChatMiniProgram] --> [APIGateway]

' Gateway to Business Services
[APIGateway] --> [UserService]
[APIGateway] --> [PermissionService]
[APIGateway] --> [TalentService]
[APIGateway] --> [ResumeService]
[APIGateway] --> [OrderService]
[APIGateway] --> [ReportService]
[APIGateway] --> [MessageService]

' Business Services dependencies
[ResumeService] --> [FileService]
[ResumeService] --> [TalentService]
[TalentService] --> [OrderService]
[OrderService] --> [MessageService]
[UserService] --> [PermissionService]
[MessageService] --> [MQService]

' Business Services to Data Layer
[UserService] --> [MySQL]
[ResumeService] --> [MySQL]
[OrderService] --> [MySQL]
[TalentService] --> [Redis]
[ReportService] --> [Elasticsearch]

' Infra Integration
[All Services] --> [LogService]
[All Services] --> [MonitoringService]
[All Services] --> [TracingService]

@enduml
```

## UML Diagram with Documents 24

```plaintext
@startuml
skinparam componentStyle rectangle
left to right direction

' Frontend Clients
package "Client Applications" {
  [Web Frontend (Vue.js)]
  [Mobile App / Mini Program]
}

' API Gateway
[API Gateway (Spring Cloud Gateway)]

' Business Microservices
package "Business Services" {
  [System Service]
  [Resume Service]
  [Talent Service]
  [Order Service]
  [Report Service]
  [Message Service]
}

' Infrastructure Services
package "Infrastructure" {
  [File Storage Service]
  [Message Queue Service]
  [Logging & Monitoring Service]
}

' Data Layer
package "Data Stores" {
  [MySQL]
  [Redis]
  [Elasticsearch]
  [MinIO]
}

' Monitoring Tools
package "Observability" {
  [ELK Stack]
  [Prometheus]
  [Grafana]
  [SkyWalking]
}

' External Integration
[Resume Parsing API ("小析解析")]

' Connections
[Web Frontend (Vue.js)] --> [API Gateway (Spring Cloud Gateway)]
[Mobile App / Mini Program] --> [API Gateway (Spring Cloud Gateway)]

[API Gateway (Spring Cloud Gateway)] --> [System Service]
[API Gateway (Spring Cloud Gateway)] --> [Resume Service]
[API Gateway (Spring Cloud Gateway)] --> [Talent Service]
[API Gateway (Spring Cloud Gateway)] --> [Order Service]
[API Gateway (Spring Cloud Gateway)] --> [Report Service]
[API Gateway (Spring Cloud Gateway)] --> [Message Service]

[Resume Service] --> [Talent Service]
[Talent Service] --> [Resume Service]
[Order Service] --> [Talent Service]
[Report Service] --> [Order Service]

[Resume Service] --> [File Storage Service]
[Resume Service] --> [Resume Parsing API ("小析解析")]

[All Microservices] --> [Message Queue Service]
[All Microservices] --> [Logging & Monitoring Service]

[System Service] --> [MySQL]
[Resume Service] --> [MySQL]
[Talent Service] --> [MySQL]
[Order Service] --> [MySQL]
[Report Service] --> [MySQL]
[Message Service] --> [Redis]

[Resume Service] --> [Elasticsearch]
[File Storage Service] --> [MinIO]

[Logging & Monitoring Service] --> [ELK Stack]
[Logging & Monitoring Service] --> [Prometheus]
[Logging & Monitoring Service] --> [Grafana]
[Logging & Monitoring Service] --> [SkyWalking]
@enduml
```

## UML Diagram with Documents 25

```plaintext
@startuml
skinparam componentStyle rectangle

package "Frontend" {
  [VueApp] <<frontend>>
}

package "Gateway Layer" {
  [SpringCloudGateway] <<gateway>>
}

package "Resume Management Service" {
  [ResumeController] <<controller>>
  [ResumeService] <<service>>
  [ResumeRepository] <<repository>>
}

package "Integrated Service" {
  [ResumeParserClient] <<feign>>
  [MinIOClient] <<feign>>
}

package "Database & Storage" {
  [MySQL]
  [MinIO]
}

' Interactions
[VueApp] --> [SpringCloudGateway]
[SpringCloudGateway] --> [ResumeController]
[ResumeController] --> [ResumeService]
[ResumeService] --> [ResumeRepository]
[ResumeService] --> [ResumeParserClient]
[ResumeService] --> [MinIOClient]

[ResumeRepository] --> [MySQL]
[MinIOClient] --> [MinIO]
[ResumeParserClient] ..> [ExternalParserAPI] : uses

@enduml
```

## UML Diagram with Documents 34

```plaintext
@startuml
!define RECTANGLE style rectangle
skinparam componentStyle RECTANGLE

' Frontend Components
package "Frontend" {
  [VueApp] as VueApp
  [VueRouter] as VueRouter
  [PiniaStore] as PiniaStore
  [AxiosClient] as AxiosClient
  [ElementPlusUI] as ElementPlusUI
}

' Backend Components (Spring Boot Microservices)
package "Backend Services" {
  [GatewayService] as Gateway
  [SystemMgrService] as SystemMgr
  [ResumeService] as Resume
  [TalentService] as Talent
  [OrderService] as Order
  [ReportService] as Report
  [NoticeService] as Notice
}

' Infrastructure
package "Infrastructure" {
  [MySQL]
  [Redis]
  [RocketMQ]
  [Elasticsearch]
  [MinIO]
  [Consul]
  [SpringConfigServer] as ConfigServer
}

' Monitoring & DevOps
package "Monitoring & DevOps" {
  [Prometheus]
  [Grafana]
  [SkyWalking]
  [GitLabCI]
  [Kubernetes]
}

' Interactions
VueApp --> VueRouter
VueApp --> PiniaStore
VueApp --> ElementPlusUI
VueApp --> AxiosClient

AxiosClient --> Gateway

Gateway --> SystemMgr
Gateway --> Resume
Gateway --> Talent
Gateway --> Order
Gateway --> Report
Gateway --> Notice

SystemMgr --> MySQL
SystemMgr --> Redis
SystemMgr --> RocketMQ
SystemMgr --> Elasticsearch
SystemMgr --> MinIO
SystemMgr --> ConfigServer

Resume --> MySQL
Resume --> Redis
Resume --> MinIO

Talent --> MySQL
Talent --> RocketMQ

Order --> MySQL
Order --> RocketMQ

Report --> MySQL
Report --> Elasticsearch

Notice --> RocketMQ
Notice --> Redis

' Registration and Configuration
Gateway --> Consul
SystemMgr --> Consul
Resume --> Consul
Talent --> Consul
Order --> Consul
Report --> Consul
Notice --> Consul

SystemMgr --> ConfigServer
Resume --> ConfigServer
Talent --> ConfigServer
Order --> ConfigServer
Report --> ConfigServer
Notice --> ConfigServer

' Monitoring and DevOps
SystemMgr --> Prometheus
SystemMgr --> SkyWalking

Prometheus --> Grafana
GitLabCI --> Kubernetes

@enduml
```

## UML Diagram with Documents 35

```plaintext
@startuml
skinparam componentStyle rectangle

package "Frontend" {
  [Vue3_UI] <<component>>
  [ResumeAPIAdapter] <<component>>
}

package "Backend" {
  [ResumeController] <<component>>
  [TalentService] <<component>>
  [ResumeService] <<component>>
  [EducationService] <<component>>
  [WorkExperienceService] <<component>>
  [ProjectExperienceService] <<component>>
  [AwardService] <<component>>
  [SkillService] <<component>>
  [TrainingService] <<component>>
  [LanguageService] <<component>>
  [RelativeService] <<component>>
  [SelfAssessmentService] <<component>>
  [ResumeRepository] <<component>>
  [MySQL_DB] <<database>>
  [MinIO_Storage] <<component>>
}

package "Infrastructure" {
  [SpringBoot_App] <<framework>>
  [MyBatisPlus] <<framework>>
  [Redis_Cache] <<component>>
  [Elasticsearch] <<component>>
  [RocketMQ] <<component>>
}

[Vue3_UI] --> [ResumeAPIAdapter]

[ResumeAPIAdapter] --> [ResumeController]

[ResumeController] --> [TalentService]
[ResumeController] --> [ResumeService]
[ResumeController] --> [EducationService]
[ResumeController] --> [WorkExperienceService]
[ResumeController] --> [ProjectExperienceService]
[ResumeController] --> [AwardService]
[ResumeController] --> [SkillService]
[ResumeController] --> [TrainingService]
[ResumeController] --> [LanguageService]
[ResumeController] --> [RelativeService]
[ResumeController] --> [SelfAssessmentService]

[TalentService] --> [ResumeRepository]
[ResumeService] --> [ResumeRepository]
[EducationService] --> [ResumeRepository]
[WorkExperienceService] --> [ResumeRepository]
[ProjectExperienceService] --> [ResumeRepository]
[AwardService] --> [ResumeRepository]
[SkillService] --> [ResumeRepository]
[TrainingService] --> [ResumeRepository]
[LanguageService] --> [ResumeRepository]
[RelativeService] --> [ResumeRepository]
[SelfAssessmentService] --> [ResumeRepository]

[ResumeRepository] --> [MySQL_DB]
[ResumeRepository] --> [Redis_Cache]
[ResumeRepository] --> [Elasticsearch]

[ResumeService] --> [MinIO_Storage]
[ResumeService] --> [RocketMQ]

[SpringBoot_App] --> [ResumeController]
[SpringBoot_App] --> [MyBatisPlus]

@enduml
```

## UML Diagram with Documents 45

```plaintext
@startuml
!define RECTANGLE class
skinparam componentStyle rectangle

package "GTAMS Resume Management" {
  [ResumeController] <<REST Controller>>
  [ResumeService] <<Service>>
  [ResumeConvert] <<Convert>>
  [ResumeMapper] <<Mapper>>
  [ResumeDO] <<Data Object>>
  [ResumeApi] <<OpenFeign API>>

  [ResumeController] --> [ResumeService] : calls
  [ResumeService] --> [ResumeMapper] : uses
  [ResumeService] --> [ResumeConvert] : converts
  [ResumeConvert] --> [ResumeDO] : maps to/from
  [ResumeController] --> [ResumeApi] : delegates (external API)

  [ResumeService] ..> [RedisDAO] : optional caching
  [ResumeService] ..> [MQProducer] : optional async events
}

package "External Systems" {
  [RedisDAO] <<Redis>>
  [MQProducer] <<Message Queue>>
}

@enduml
```

## UML Diagram with Documents 1

```plaintext
@startuml
skinparam componentStyle rectangle
skinparam shadowing false

package GTAMS {

  [SystemManagement] as SystemManagement
  [TalentManagement] as TalentManagement
  [ResumeManagement] as ResumeManagement
  [OrderManagement] as OrderManagement
  [ReportPortalManagement] as ReportPortalManagement
  [NotificationManagement] as NotificationManagement

  [AuthenticationService] as AuthService
  [AuthorizationService] as AuthzService
  [UserService] as UserService
  [RoleService] as RoleService
  [OrganizationService] as OrgService
  [DictService] as DictService
  [LogService] as LogService

  [TalentPoolService] as TalentPool
  [CommunicationLogService] as CommLog
  [ResumeService] as Resume
  [ResumeTemplateService] as ResumeTemplate
  [CustomerService] as Customer
  [OrderService] as Order
  [PositionService] as Position
  [DispatchService] as Dispatch
  [ReportService] as Report
  [EmailService] as Email
  [MessageService] as Message
  [AnnouncementService] as Announcement

  SystemManagement --> AuthService
  SystemManagement --> AuthzService
  SystemManagement --> UserService
  SystemManagement --> RoleService
  SystemManagement --> OrgService
  SystemManagement --> DictService
  SystemManagement --> LogService

  TalentManagement --> TalentPool
  TalentManagement --> CommLog
  TalentManagement --> Resume

  ResumeManagement --> Resume
  ResumeManagement --> ResumeTemplate

  OrderManagement --> Customer
  OrderManagement --> Order
  OrderManagement --> Position
  OrderManagement --> Dispatch

  ReportPortalManagement --> Report

  NotificationManagement --> Email
  NotificationManagement --> Message
  NotificationManagement --> Announcement
}

@enduml
```

## UML Diagram with Documents 2

```plaintext
@startuml
!define AWSPUML https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v14.0/LATEST/AWSPUML
skinparam componentStyle rectangle

package "Frontend" {
  [PCWeb] as PC_Web
  [MobileApp] as Mobile_App
}

package "Gateway Layer" {
  [API_Gateway] <<component>>
}

package "Business Microservices" {
  [UserManagementService]
  [PermissionService]
  [TalentService]
  [ResumeService]
  [OrderService]
  [ReportService]
  [MessageService]
}

package "Infrastructure Services" {
  [FileStorageService]
  [LogService]
  [NotificationService]
  [IntegrateService]
}

package "Data Layer" {
  [MySQL]
  [Redis]
  [Elasticsearch]
  [MinIO]
}

package "Monitoring & Tracing" {
  [ELKStack]
  [Prometheus]
  [Grafana]
  [SkyWalking]
}

PC_Web --> API_Gateway
Mobile_App --> API_Gateway

API_Gateway --> UserManagementService
API_Gateway --> PermissionService
API_Gateway --> TalentService
API_Gateway --> ResumeService
API_Gateway --> OrderService
API_Gateway --> ReportService
API_Gateway --> MessageService

TalentService --> FileStorageService
TalentService --> IntegrateService
TalentService --> ResumeService
ResumeService --> MySQL
ResumeService --> Elasticsearch

OrderService --> MySQL
OrderService --> Redis

MessageService --> NotificationService

UserManagementService --> Redis
UserManagementService --> MySQL

FileStorageService --> MinIO
LogService --> ELKStack
MessageService --> Redis

AllServices --> SkyWalking
AllServices --> Prometheus
AllServices --> Grafana

note right of AllServices
All microservices include:
- User Management
- Talent Management
- Resume Management
- Order Management
- Report and Message Services
end note

@enduml
```

## UML Diagram with Documents 3

```plaintext
@startuml
skinparam componentStyle rectangle

package "Frontend" {
  [Vue.js_3.4.x]
  [TypeScript_5.x]
  [Vue_i18n_9.x]
  [Vite_5.x]
  [Webpack_5.x]
  [Pinia_2.x]
  [Vue_Router_4.x]
  [Element_Plus_2.4.x]
  [Vant_4.x]
  [Sass_1.69.x]
  [Tailwind_CSS_3.4.x]
  [PostCSS_8.x]
  [Axios_1.x]
  [ESLint_8.x]
  [Prettier_3.x]
  [Husky_8.x]
  [Vitest_1.x]
  [Cypress_12.x]
  [VUE-ECharts_5.x]
  [AntV_G2_5.x]
  [Lodash_4.x]
  [Day.js_1.x]
  [Iconify_3.x]
  [Font_Awesome_6.x]
  [uni-app_4.55.x]
  [Node.js_18.x_LTS]
  [pnpm_8.x]
  [npm_9.x]
}

package "Backend" {
  [JDK_21]
  [SpringBoot_3.2.x]
  [Spring_Cloud_2022.0.x]
  [Spring_Cloud_Gateway_3.1.x]
  [Spring_Data_JPA_3.2.x]
  [Spring_Security_OAuth2_6.2.x]
  [MyBatis_Plus_3.5.x]
  [MySQL_8.0.x]
  [Redis_7.2.x]
  [RocketMQ_4.9.x]
  [Consul_1.16.x]
  [Spring_Cloud_Config_4.1.x]
  [Elasticsearch_8.11.x]
  [Logstash_8.11.x]
  [Kibana_8.11.x]
  [Prometheus_2.44.x]
  [Grafana_10.1.x]
  [SkyWalking_9.5.x]
  [OAuth2_6.2.x]
  [XXL-job_3.0.x]
  [MinIO_2024.02.x]
  [GitLab_CI_CD_16.9.x]
  [Kubernetes_1.29.x]
  [SonarQube_10.2.x]
  [Swagger_3.0.x]
  [Nginx_1.25.x]
}

' Example of interactions
[Vue.js_3.4.x] --> [Axios_1.x]
[Axios_1.x] --> [Spring_Cloud_Gateway_3.1.x]
[Spring_Cloud_Gateway_3.1.x] --> [SpringBoot_3.2.x]
[SpringBoot_3.2.x] --> [MyBatis_Plus_3.5.x]
[SpringBoot_3.2.x] --> [Spring_Security_OAuth2_6.2.x]
[Spring_Security_OAuth2_6.2.x] --> [Redis_7.2.x]
[SpringBoot_3.2.x] --> [MySQL_8.0.x]
[SpringBoot_3.2.x] --> [Elasticsearch_8.11.x]
[SpringBoot_3.2.x] --> [MinIO_2024.02.x]
[GitLab_CI_CD_16.9.x] --> [Kubernetes_1.29.x]

@enduml
```

## UML Diagram with Documents 4

```plaintext
@startuml
skinparam componentStyle rectangle

package "Frontend (XxxFrontEnd)" {
  [api] --> [utils]
  [components] --> [api]
  [views] --> [router]
  [router] --> [store]
  [App.vue] --> [main.ts]
  [main.ts] --> [permission.ts]
}

package "Backend Modules" {
  package "Xxx-api" {
    [DeptApi]
    [UserApi]
    [ApiConstants]
    [ErrorCodeConstants]
    [LoginLogTypeEnum]
  }

  package "Xxx-biz" {
    [DeptApiImpl] --> [DeptApi]
    [UserController] --> [UserService]
    [UserServiceImpl] --> [UserService]
    [DeptServiceImpl] --> [DeptService]
    [DeptService] --> [DeptMapper]
    [AdminUserMapper] --> [AdminUserDO]
    [RedisCaptchaServiceImpl] --> [RedisKeyConstants]
    [OAuth2AccessTokenRedisDAO] --> [Redis]
    [MailProducer] --> [MailSendMessage]
    [SmsProducer] --> [SmsSendMessage]
  }

  [UserController] --> [DeptService]
  [DeptController] --> [PostService]
}

package "APE Framework" {
  [ape-common]
  [ape-spring-boot-starter-mybatis] --> [MyBatis]
  [ape-spring-boot-starter-redis] --> [Redis]
  [ape-spring-boot-starter-security] --> [Spring Security]
  [ape-spring-boot-starter-job] --> [Quartz]
  [ape-spring-boot-starter-web] --> [Web]
}

[UserController] --> [ape-spring-boot-starter-web]
[DeptServiceImpl] --> [ape-spring-boot-starter-mybatis]
[RedisCaptchaServiceImpl] --> [ape-spring-boot-starter-redis]

package "Infrastructure" {
  [ape-gateway] --> [UserController]
  [ape-infra]
}

@enduml
```

## UML Diagram with Documents 5
```plaintext
@startuml

package "ResumeManagementSystem" {
  [ResumeController] --> [TalentBasicService]
  [ResumeController] --> [OriginalFileService]
  [ResumeController] --> [IntentionService]
  [ResumeController] --> [EducationService]
  [ResumeController] --> [WorkExperienceService]
  [ResumeController] --> [ProjectExperienceService]
  [ResumeController] --> [AwardService]
  [ResumeController] --> [SkillService]
  [ResumeController] --> [TrainingService]
  [ResumeController] --> [LanguageAbilityService]
  [ResumeController] --> [MoreInfoService]
  [ResumeController] --> [RelativeService]
  [ResumeController] --> [SelfAssessmentService]
  [ResumeController] --> [AdvancedQueryService]
}

[ResumeController] - [MySQLDB] : uses
[OriginalFileService] --> [FileStorageService]

@enduml
```

