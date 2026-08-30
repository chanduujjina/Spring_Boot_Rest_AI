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
## 


```erDiagram
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
