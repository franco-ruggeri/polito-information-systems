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
  class Address
  class Employee
  class Car
  class "Car Model" as CarModel
  CARS o-- "*" ParkingSite
  CARS -- "*" CarModel : offers >
  ParkingSite -- "*" Car : is parked in <
  ParkingSite -- "*" Employee : works in <
  ParkingSite -- Address : located at >
  Car "*" -- CarModel : describes <
  Car : +Tag
  ParkingSite : +ID
  Address : +City
  Address : +Type
  Address : +Name
  Employee : +ID
  Employee : +Name
  Employee : +Surname
  
  ' Insurances
  class Insurer
  class Insurance
  Insurance "*" -- Insurer : provides >
  Insurance "*" -- Car : covered by <
  Insurer : +ID
  Insurer : +Name
  Insurance : +Type
  
  ' Customer
  class Customer
  class "Driver license" as DriverLicense
  Customer -- DriverLicense
  Customer : +Name
  Customer : +Surname
  Customer : +ID document
  DriverLicense : +ID
  DriverLicense : +Expiration date
  
  ' Reservation
  class Reservation
  Reservation "*" -- Customer : reserves <
  Reservation -- CarModel : reserves >
  Reservation : +From date
  Reservation : +To date
  
  ' Check-out - Part 1
  class Contract
  class "Credit card" as CreditCard
  class Payment
  Contract "*" -- Customer : signs <
  Contract "0..1" -- "0..1" Reservation : relative to >
  Customer -- CreditCard
  Contract -- Payment : has payment >
  Contract -- Car : assigned to <
  Contract : +From date
  Contract : +To date
  Contract : +Customer's signature
  CreditCard : +Number
  CreditCard : +Expiration date
  Payment : +Damage deposit
  Payment : +Partial fee
  Payment : +Total fee
  
  ' Check-out - Part 2
  class "Check-out" as Checkout
  class Damage
  Checkout -- Contract : attached to >
  Checkout -- "*" Damage : has >
  Checkout "*" -- Customer : signs <
  Checkout "*" -- Employee : performs <
  Checkout : +Customer's signature
  Checkout : +Timestamp
  Damage : +Description
  
  ' Check-in
  class "Check-in" as Checkin
  Checkin -- Contract : relative to >
  Checkin -- "*" Damage : has new >
  Checkin : +Timestamp
  
  ' Invoice
  class Invoice
  Invoice -- Payment
  Invoice : +ID
  ``` 
</details>

![](plantuml/car_rental.svg)
