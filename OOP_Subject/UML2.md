# Exercice 2 - ABCD Records

## Level 1  

### Task 1.1: Basic Use Case Diagram
```mermaid
flowchart TD
    subgraph ABCD Records
        PO([Place Order])
        VM([Verify Membership])
        MP([Make Payment])
        PR([Process Order])
    end

    Member --> PO
    Member --> MP
    Clerk --> VM
    Clerk --> PR
```

### Task 1.2: Basic Class Diagram
```mermaid
classDiagram
    class Member {
        +id: String
        +name: String
        +address: String
        +memberType: String
        +place_order()
        +make_payment()
        +get_status()
    }

    class Order {
        +id: String
        +date: Date
        +total: Float
        +status: String
        +get_details()
        +calculate_total()
        +update_status()
    }

    class OrderProcessingClerk {
        +id: String
        +name: String
        +department: String
        +verify_membership()
        +process_order()
        +print_invoice()
    }

    Member "1" --> "0..*" Order : places
    OrderProcessingClerk "1" --> "0..*" Order : processes
    OrderProcessingClerk "1" --> "0..*" Member : verifies
```

## Level 2 

### Complete Use Case Diagram
```mermaid
flowchart TD
    subgraph ABCD Records
        PO([Place Order])
        VM([Verify Membership])
        PR([Process Order])
        MP([Make Payment])
        AD([Apply Discount])
        PI([Print Invoice])
        PS([Print Shipping List])
        OU([Order Unavailable Items])
        SM([Send Membership Application])
        VA([Verify Item Availability])
    end

    Member --> PO
    Member --> MP
    NonMember --> SM
    Clerk --> VM
    Clerk --> PR
    Clerk --> PI
    Clerk --> PS
    CollectionClerk --> MP

    PO -->|include| VM
    PO -->|include| VA
    PR -->|include| PI
    PR -->|include| PS
    PO -->|extend| AD
    PO -->|extend| OU
    VM -->|extend| SM
```


### Complete Class Diagram
```mermaid
classDiagram
    class Member {
        +id: String
        +name: String
        +address: String
        +place_order()
        +make_payment()
    }

    class RoyalMember {
        +discount: Float
        +order_unavailable()
        +apply_discount()
    }

    class RegularMember {
        +place_order()
    }

    class Order {
        +id: String
        +date: Date
        +status: String
        +calculate_total()
        +update_status()
    }

    class OrderLine {
        +quantity: Int
        +unit_price: Float
        +get_subtotal()
    }

    class Item {
        +id: String
        +name: String
        +price: Float
        +is_available()
    }

    class CD {
        +artist: String
        +format: String
    }

    class Tape {
        +duration: String
        +format: String
    }

    class Payment {
        +id: String
        +amount: Float
        +status: String
        +process()
    }

    class CashPayment {
        +process()
    }

    class CheckPayment {
        +check_number: String
        +process()
    }

    class BankDraft {
        +bank_name: String
        +process()
    }

    class Invoice {
        +id: String
        +date: Date
        +print()
    }

    class ShippingList {
        +id: String
        +date: Date
        +print()
    }

    class MembershipApplication {
        +id: String
        +status: String
        +send()
    }

    class OrderProcessingClerk {
        +id: String
        +name: String
        +verify_membership()
        +process_order()
    }

    Member <|-- RoyalMember : inherits
    Member <|-- RegularMember : inherits
    Item <|-- CD : inherits
    Item <|-- Tape : inherits
    Payment <|-- CashPayment : inherits
    Payment <|-- CheckPayment : inherits
    Payment <|-- BankDraft : inherits
    Member "1" --> "0..*" Order : places
    Order "1" --> "1..*" OrderLine : contains
    OrderLine "1" --> "1" Item : refers to
    Order "1" --> "1" Invoice : generates
    Order "1" --> "1" ShippingList : generates
    Order "1" --> "1" Payment : paid by
    OrderProcessingClerk --> Order : processes
    MembershipApplication --> Member : creates
```

## Level 3 


### Sequence Diagram 1: Complete Order Processing Flow
```mermaid
sequenceDiagram
    actor Member
    actor Clerk

    Member->>Clerk: submit order form
    Clerk->>Clerk: verify membership
    Clerk->>Clerk: check item availability
    alt item available
        Clerk->>Clerk: process order
        Clerk->>Clerk: apply discount if Royal
        Clerk->>Clerk: print invoice
        Clerk->>Clerk: print shipping list
        Clerk->>Member: send invoice
    else item unavailable
        alt Royal Member
            Clerk->>Clerk: add to backorder
            Clerk->>Member: notify delay
        else Regular Member
            Clerk->>Member: item unavailable
        end
    end
```

### Sequence Diagram 2: Membership Verification
```mermaid
sequenceDiagram
    actor Sender
    actor Clerk

    Sender->>Clerk: submit order form
    Clerk->>Clerk: check membership
    alt is Member
        alt Royal Member
            Clerk->>Clerk: enable discount
            Clerk->>Sender: proceed with Royal benefits
        else Regular Member
            Clerk->>Sender: proceed with standard processing
        end
    else not a Member
        Clerk->>Sender: send membership application
        Clerk->>Sender: return items
    end
```

### Sequence Diagram 3: Payment Processing
```mermaid
sequenceDiagram
    actor Member
    actor CollectionClerk

    Member->>CollectionClerk: request to pay
    CollectionClerk->>Member: show amount due
    Member->>CollectionClerk: choose payment method
    alt Cash
        Member->>CollectionClerk: pay cash
        CollectionClerk->>CollectionClerk: record payment
    else Check
        Member->>CollectionClerk: submit check
        CollectionClerk->>CollectionClerk: verify check
        alt check valid
            CollectionClerk->>CollectionClerk: record payment
        else check invalid
            CollectionClerk->>Member: choose another method
        end
    else Bank Draft
        Member->>CollectionClerk: submit bank draft
        CollectionClerk->>CollectionClerk: verify draft
        CollectionClerk->>CollectionClerk: record payment
    end
    CollectionClerk->>Member: send receipt
```

### Sequence Diagram 4: Item Reordering for Royal Members
```mermaid
sequenceDiagram
    actor Clerk
    actor RoyalMember

    Clerk->>Clerk: check item availability
    alt item unavailable
        Clerk->>Clerk: verify Royal membership
        Clerk->>Clerk: create backorder
        Clerk->>RoyalMember: notify item on backorder
        Clerk->>Clerk: update order status to PENDING
        Clerk->>RoyalMember: notify when item arrives
    else Regular Member
        Clerk->>Clerk: remove item from order
        Clerk->>Clerk: recalculate total
    end
```