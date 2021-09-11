# Car rental

## Description

Car rental companies own a number of cars and a number of sites where cars are parked when not in use. Customers rent cars for a period of time (having made a reservation, or not) and return them. 
We focus on the CARS company.

### AS IS process
A customer may reserve a car, using the company web site, or the call center (this step is optional). 
A customer steps into the office close to the rental car parking site and completes the first step of the check out. The contract for the rental is defined (period of rental, name of driver, related ID document and driving license, insurances, damage deposit, partial and total fees, credit card), signed by both parties, and the payment for the rental is completed (payment has two parts, rental and damage deposit – the latter is normally returned at the end of the rental).  Further, a specific car (identified by its tag) is assigned to the rental. 
Then the customer walks to the car parking site. Here the second part of the check out happens. 
An employee checks with the customer the car and lists all visible damages on the car in an annex to the contract. Also this annex is signed by both parties. Then the employee hands the car to the customer (this of course includes the keys) and the rental starts.
The final step is check in. The customer drives the car to parking site. An employee receives the car and the keys, checks with the customer for new damages. If there are damages another process starts (we leave this process out of this analysis). At this point the rental ends. The company issues an invoice and possibly returns the damage deposit to the customer.

### TO BE process
The idea is to improve the process by introducing the same innovations used by car sharing companies. 
A customer has first to define an account with CARS. In this step the customer uploads his documents (ID, driving license) and a credit card. If all is right CARS approves and the customer can later rent cars. This step can be performed on a PC or smart phone. In any case the customer has to install the CARS app on her smartphone. 
When a registered customer wants to rent a car she has to do a reservation (via app or PC). 
Check out works as follows. The customer walks directly to the rental car parking, via the app she signals that she wants to start the rental. The app answers with position and tag of the assigned car. 
When the customer is close to the car she asks, via the app, to open the car. The system opens the car (the car needs to be modified via a device connected to the cellular network and capable of controlling some car functions, like door open/close).  The keys are inside the car. The customer starts the car, and the rental. 
The check in is similar. The customer parks the car in the rental car parking, stops the car, exits, and asks the app to close the car. At this point the rental is over. 
Invoicing and payments proceed through the credit card. 
Damage deposits and possibly damage reimboursements are avoided, introducing by default an insurance to cover all.

## Lab 1 - Data model

<details>
  <summary>PlantUML</summary>
  
  ```plantuml:car_rental
  ' CARS company
  class CARS
  class "Parking site" as ParkingSite
  class Office
  class Employee
  class Car
  class "Car Model" as CarModel
  class "Call center" as CallCenter
  class "Web site" as WebSite
  CARS o-- "*" ParkingSite
  CARS o-- CallCenter
  CARS o-- WebSite
  CARS -- "*" CarModel : offers >
  ParkingSite -- Office : is close to <
  ParkingSite -- "*" Car : is parked in <
  Office -- "*" Employee : works in <
  Car "*" -- CarModel : is described by >
  Car -- "*" : Insurance
  Car : +Tag
  
  ' Insurances
  class Insurer
  class Insurance
  Insurance "*" -- Insurer : provides >
  Insurance "*" -- Car : covered by <
  Insurance : +Type
  
  ' Customer
  class Customer
  class "Credit card" as CreditCard
  Customer -- CreditCard
  Customer : +Name
  Customer : +ID document
  Customer : +Driving license
  CreditCard : +Number
  CreditCard : +Expiration date
  
  ' Reservation
  class Reservation
  class ReservationChannel
  ReservationChannel <|-- CallCenter
  ReservationChannel <|-- WebSite
  Reservation "*" -- Customer : reserves <
  Reservation -- ReservationChannel : done using >
  Reservation -- CarModel : reserves >
  
  ' Check-out - Part 1
  class Contract
  class Payment
  Contract "*" -- Customer : signs <
  Contract -- "0..1" Reservation : relative to >
  Contract -- Office : signs <
  Contract -- Payment : has payment >
  Contract -- Car : assigned to <
  Contract : +Period start
  Contract : +Period end
  Contract : +Customer's signature
  Contract : +Office's signature
  Payment : +Damage deposit
  Payment : +Partial fee
  Payment : +Total fee
  
  ' Check-out - Part 2
  class "Check-out damage" as CheckoutDamage
  class Damage
  CheckoutDamage -- Contract : attached to >
  CheckoutDamage -- "*" Damage : has >
  CheckoutDamage "*" -- Customer : signs <
  CheckoutDamage "*" -- Employee : signs <
  CheckoutDamage : +Customer's signature
  CheckoutDamage : +Employee's signature
  Damage : +Description
  
  ' Check-in
  class "Check-in damage" as CheckinDamage
  CheckinDamage -- Contract : relative to >
  CheckinDamage -- "*" Damage : has new >
  
  ' Invoice
  class Invoice
  Invoice -- Payment
  ``` 
</details>

![](plantuml/car_rental.svg)
