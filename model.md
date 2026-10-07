```mermaid
erDiagram

    CATEGORY ||--o{ PRODUCT : contains
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ ORDER_ITEM : contains
    PRODUCT ||--o{ ORDER_ITEM : ordered_in
    
  
    CUSTOMER {
        string id PK
        string email
        string fullName
        string phoneNumber
        date createdAt
    }

    CATEGORY {
        string id PK
        string name
        string description
    }

    PRODUCT {
        string id PK
        string categoryId FK
        string title
        string description
        number price
    }

    ORDER {
        string id PK
        string customerId FK
        number totalAmount
        string status
        date createdAt
    }

    ORDER_ITEM {
        string id PK
        string orderId FK
        string productId FK
        number quantity
        number unitPrice
    }
```