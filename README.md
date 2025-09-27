# Coffee Shop Supply Chain System - Class Diagram
```mermaid
classDiagram
    %% Core Entities
    class User {
        -String userId
        -String username
        -String fullName
        -String email
        -String phone
        -UserRole role
        -UserStatus status
        -Date hireDate
        -Double salary
        +login()
        +logout()
        +changePassword()
        +updateProfile()
    }

    class Shop {
        -String shopId
        -String name
        -String address
        -String phone
        -String email
        -ShopStatus status
        +updateDetails()
        +activate()
        +deactivate()
    }

    class Product {
        -String productId
        -String name
        -String description
        -Double basePrice
        -String category
        -ProductStatus status
        -Date creationDate
        +updateDetails()
        +activate()
        +deactivate()
    }

    class Ingredient {
        -String ingredientId
        -String name
        -String unit
        -Double unitPrice
        -String category
        -String origin
        -Date expiryDate
        -IngredientStatus status
        +updateDetails()
        +checkExpiry()
    }

    class Supplier {
        -String supplierId
        -String name
        -String address
        -String phone
        -String email
        -SupplierStatus status
        +updateDetails()
        +activate()
    }

    class Order {
        -String orderId
        -Date orderDate
        -OrderStatus status
        -String customerId
        -Date estimatedTime
        +updateStatus()
        +addProduct()
        +calculateTotal()
    }

    class PurchaseOrder {
        -String poId
        -Date orderDate
        -Date expectedDeliveryDate
        -POStatus status
        -String supplierId
        -Double totalAmount
        +approve()
        +receive()
        +updateStatus()
    }

    class Stock {
        -String stockId
        -String shopId
        -Date stockDate
        -StockStatus status
        +updateStock()
        +checkLevels()
    }

    class InventoryIssue {
        -String issueId
        -Date issueDate
        -String createdBy
        -IssueStatus status
        -String notes
        +approve()
        +reject()
    }

    %% Junction/Bridge Tables
    class ProductIngredient {
        -String productId
        -String ingredientId
        -Double quantity
        -String unit
    }

    class OrderProduct {
        -String orderId
        -String productId
        -Integer quantity
        -Double unitPrice
    }

    class PurchaseItem {
        -String poId
        -String ingredientId
        -Double quantity
        -Double unitPrice
        -Double totalPrice
    }

    class StockIngredient {
        -String stockId
        -String ingredientId
        -String poId
        -Double quantity
        -Date expiryDate
    }

    class IssueIngredient {
        -String issueId
        -String ingredientId
        -Double quantity
        -String unit
    }

    %% Enumerations
    class UserRole {
        <<enumeration>>
        ADMIN
        HR
        INVENTORY_STAFF
        BARISTA
    }

    class OrderStatus {
        <<enumeration>>
        PENDING
        BREWING
        READY
        DELIVERED
        CANCELLED
    }

    class POStatus {
        <<enumeration>>
        PENDING
        APPROVED
        RECEIVED
        CANCELLED
    }

    class IssueStatus {
        <<enumeration>>
        PENDING
        APPROVED
        REJECTED
        COMPLETED
    }

    %% Main Relationships
    User ||--|| UserRole
    Shop ||--o{ Order
    Shop ||--o{ Stock
    
    Product ||--o{ ProductIngredient
    Product ||--o{ OrderProduct
    
    Order ||--o{ OrderProduct
    
    Ingredient ||--o{ ProductIngredient
    Ingredient ||--o{ PurchaseItem
    Ingredient ||--o{ StockIngredient
    Ingredient ||--o{ IssueIngredient
    
    Supplier ||--o{ PurchaseOrder
    PurchaseOrder ||--o{ PurchaseItem
    
    Stock ||--o{ StockIngredient
    
    InventoryIssue ||--o{ IssueIngredient
    User ||--o{ InventoryIssue : creates
    
    %% Status Relationships
    Order ||--|| OrderStatus
    PurchaseOrder ||--|| POStatus
    InventoryIssue ||--|| IssueStatus
