# Objective Create an rest api using Spring boot
## Spring boot project Strcture
```mermaid
treeView-beta
            restDemo/
                     src/
                         main/
                              java/
                                   com/
                                       cc/
                              test/
                                   com/
                                       cc/
                .gitignore
                pom.xml
                README.md
```
## state diagrm

```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ ORDER_ITEM : contains
    PRODUCT ||--o{ ORDER_ITEM : includes
    CUSTOMER {
        string id
        string name
        string email
    }
    ORDER {
        string id
        date orderDate
        string status
    }
    PRODUCT {
        string id
        string name
        float price
    }
    ORDER_ITEM {
        int quantity
        float price
    }
```

### sequence diagram
```mermaid
sequenceDiagram
    autonumber
    actor Scheduler
    participant BatchJob
    participant DataMergeService
    box DataSources
        participant CosmosReader
        participant OdsReader
        participant SynapseReader
    end  %% <-- Changed 'box' to 'end' here
    participant GroupingService
    participant NotificationPublisher

    Scheduler->>BatchJob: execute()
    activate BatchJob
    BatchJob->>DataMergeService: process()
    activate DataMergeService
    
    par Read From Data Sources
        DataMergeService->>CosmosReader: readByDateCriteria()
        DataMergeService->>OdsReader: readByReportId()
        DataMergeService->>SynapseReader: readByReportId()
    end
    
    Note over DataMergeService: merged = mergeByKeys(reportId, policyId)
    
    alt merged != null
        DataMergeService->>GroupingService: group(merged)
        GroupingService-->>DataMergeService: grouped
        
        alt user.preference == "SMS"
            DataMergeService->>NotificationPublisher: publish(smsTopic, grouped)
        else user.preference == "Email"
            DataMergeService->>NotificationPublisher: publish(emailTopic, grouped)
        end
    end
    
    deactivate DataMergeService
    deactivate BatchJob
```


