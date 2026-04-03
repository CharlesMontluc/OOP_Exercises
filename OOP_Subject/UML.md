# Exercice 1 - Hospital Management System

## Task 1 - Basic Class Diagram
```mermaid
classDiagram
    class Patient {
        +id: String
        +name: String
        +email: String
        +password: String
        +post_problem()
        +make_payment()
    }

    class Doctor {
        +id: String
        +name: String
        +specialization: String
        +password: String
        +view_problem()
        +prescribe_solution()
    }

    class Organizer {
        +id: String
        +name: String
        +password: String
        +read_problem()
        +send_prescription()
        +pay_doctor()
        +create_member()
    }

    Patient --> Organizer : submits to
    Organizer --> Doctor : consults
    Organizer --> Patient : sends prescription
```

## Task 2 - Complete Class Diagram
```mermaid
classDiagram
    class Patient {
        +id: String
        +name: String
        +email: String
        +password: String
        +post_problem()
        +make_payment()
    }

    class Doctor {
        +id: String
        +name: String
        +specialization: String
        +password: String
        +view_problem()
        +prescribe_solution()
    }

    class Organizer {
        +id: String
        +name: String
        +password: String
        +read_problem()
        +send_prescription()
        +pay_doctor()
    }

    class HealthProblem {
        +id: String
        +description: String
        +status: String
        +get_details()
    }

    class Prescription {
        +id: String
        +diagnosis: String
        +medication: String
        +get_details()
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

    class CreditCardPayment {
        +card_number: String
        +process()
    }

    Patient --> HealthProblem : submits
    Organizer --> Doctor : consults
    Doctor --> Prescription : creates
    Organizer --> Patient : sends prescription
    Patient --> Payment : makes
    Payment <|-- CashPayment : inherits
    Payment <|-- CheckPayment : inherits
    Payment <|-- CreditCardPayment : inherits
```

## Task 2 - Sequence Diagram
```mermaid
sequenceDiagram
    actor Patient
    actor Organizer
    actor Doctor

    Patient->>Organizer: submit health problem
    Organizer->>Doctor: forward problem
    Doctor->>Organizer: send prescription
    Organizer->>Patient: deliver prescription
    Patient->>Organizer: make payment
    Organizer->>Doctor: forward payment
```


## Task 3 - Advanced Class Diagram
```mermaid
classDiagram
    class User {
        +id: String
        +name: String
        +email: String
        +password: String
        +login()
        +logout()
    }

    class Patient {
        +address: String
        +post_problem()
        +make_payment()
        +view_prescription()
    }

    class Doctor {
        +specialization: String
        +view_problem()
        +prescribe_solution()
    }

    class Organizer {
        +role: String
        +read_problem()
        +send_prescription()
        +pay_doctor()
        +create_member()
    }

    class HealthProblem {
        +id: String
        +description: String
        +status: String
        +updated_at: Date
        +get_details()
        +update_status()
    }

    class Prescription {
        +id: String
        +diagnosis: String
        +medication: String
        +expiry_date: Date
        +get_details()
    }

    class Payment {
        +id: String
        +amount: Float
        +status: String
        +date: Date
        +process()
    }

    class CashPayment {
        +process()
    }

    class CheckPayment {
        +check_number: String
        +process()
    }

    class CreditCardPayment {
        +card_number: String
        +process()
    }

    User <|-- Patient : inherits
    User <|-- Doctor : inherits
    User <|-- Organizer : inherits
    Patient --> HealthProblem : submits
    Organizer --> Doctor : consults
    Doctor --> Prescription : creates
    Organizer --> Patient : sends prescription
    Patient --> Payment : makes
    Payment <|-- CashPayment : inherits
    Payment <|-- CheckPayment : inherits
    Payment <|-- CreditCardPayment : inherits
```

## Sequence Diagram 1: Patient Registration
```mermaid
sequenceDiagram
    actor Patient
    actor Organizer

    Patient->>Organizer: request registration
    Organizer->>Organizer: generate ID and password
    Organizer->>Patient: send credentials
    Patient->>Organizer: login
    Organizer->>Patient: login successful
```

## Sequence Diagram 2: Consultation Flow
```mermaid
sequenceDiagram
    actor Patient
    actor Organizer
    actor Doctor

    Patient->>Organizer: post health problem
    Organizer->>Organizer: update status to IN_REVIEW
    Organizer->>Doctor: forward problem
    Doctor->>Doctor: analyze problem
    Doctor->>Organizer: send prescription
    Organizer->>Patient: deliver prescription
    Organizer->>Organizer: update status to RESOLVED
```

## Sequence Diagram 3: Payment Processing
```mermaid
sequenceDiagram
    actor Patient
    actor Organizer
    actor Doctor

    Patient->>Organizer: choose the payment method
    alt Cash
        Patient->>Organizer: pay cash
    else Check
        Patient->>Organizer: submit check
    else Credit Card
        Patient->>Organizer: submit card details
    end
    Organizer->>Organizer: record payment
    Organizer->>Doctor: forward payment
```



## Questions to consider

**1. How would you handle a patient consulting multiple doctors?**
A Patient can be linked to multiple Doctors through the Organizer. Moreover, we could add a list of doctors to the HealthProblem class, making it a one to many relationship between Patient and Doctor.

**2. Should prescriptions be reusable? How would you model that?**
Yes, for chronic conditions. We could add a `is_reusable: Boolean` and `expiry_date: Date` attribute to the Prescription class, and link it to multiple HealthProblems.

**3. What design pattern fits the payment system?**
The Strategy pattern fits well as Payment is the base class and CashPayment, CheckPayment, CreditCardPayment each implement their own `process()` method differently.

**4. How do you ensure data integrity when payments are processed?**
We can add a `status` attribute to Payment (pending, completed, failed) and only mark it complete once the Organizer confirms receipt. The Doctor only gets paid after the status is completed.


