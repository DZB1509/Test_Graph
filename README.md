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

    %% Junction Tables
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

    %% Relationships
    User ||--|| UserRole : has
    Shop ||--o{ Order : has
    Shop ||--o{ Stock : contains
    Product ||--o{ ProductIngredient : uses
    Product ||--o{ OrderProduct : in
    Order ||--o{ OrderProduct : contains
    Order ||--|| OrderStatus : has
    Ingredient ||--o{ ProductIngredient : part_of
    Ingredient ||--o{ PurchaseItem : ordered
    Ingredient ||--o{ StockIngredient : stored
    Ingredient ||--o{ IssueIngredient : issued
    Supplier ||--o{ PurchaseOrder : supplies
    PurchaseOrder ||--o{ PurchaseItem : contains
    PurchaseOrder ||--|| POStatus : has
    Stock ||--o{ StockIngredient : holds
    InventoryIssue ||--o{ IssueIngredient : issues
    InventoryIssue ||--|| IssueStatus : has
    User ||--o{ InventoryIssue : creates
