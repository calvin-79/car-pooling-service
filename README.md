# Car Pooling Smart Contract

## Overview

This repository contains a Move-based smart contract for a decentralized car pooling service. The car pooling system allows users to register as passengers or drivers, create trips, join trips, and handle payments securely on the blockchain. The contract is designed to ensure trust and automation between the parties involved in car pooling, with features such as trip management, fare collection, and withdrawals for both passengers and the car pooling service management.

## Features

- **Passenger Registration**: Users can register as passengers and deposit funds into their accounts.
- **Trip Creation**: Drivers can create new trips, specifying the destination and fare.
- **Trip Joining**: Passengers can join available trips and pay the fare.
- **Service Fees**: The car pooling service can charge a fee for managing trips.
- **Secure Payments**: Funds are held in a pool until the trip is completed, after which they are transferred to the driver.
- **Balance Management**: Passengers and service management can deposit or withdraw funds as needed.

## Smart Contract Structure

### 1. `ServiceCap`

This struct represents the car pooling service and manages the following:

- A list of all active trips.
- The service's wallet containing collected fees.
- The service fee rate.
- The management's address.

### 2. `Trip`

Represents an individual car pooling trip:

- Tracks passengers and driver.
- Stores destination and fare.
- Holds the pool of funds collected for the trip.
- Indicates whether the trip has been completed.

### 3. `Passenger`

Represents a passenger in the system:

- Tracks the passenger's address and their balance of deposited funds.

### Error Codes

- `ENotOwner`: Unauthorized access by someone other than the service management.
- `ENotPassenger`: Action attempted by someone who is not a registered passenger.
- `EInsufficientBalance`: When a passenger or service wallet does not have enough balance.
- `ETripCompleted`: Attempt to interact with a trip that has already been completed.

## Functions

### Initialization

- **`init(ctx: &mut TxContext)`**: Initializes the car pooling service. It creates a `ServiceCap` object that manages trips and collects service fees.

### Service Management

- **`set_service_fee(service: &mut ServiceCap, fee: u64, ctx: &mut TxContext)`**: Allows the service management to set or change the service fee.
- **`withdraw_wallet(service: &mut ServiceCap, amount: u64, ctx: &mut TxContext)`**: Withdraws funds from the service wallet by the management.

### Passenger Operations

- **`add_passenger(passenger: address, ctx: &mut TxContext)`**: Registers a new passenger with an initial balance of zero.
- **`deposit(passenger: &mut Passenger, amount: Coin<SUI>)`**: Deposits funds into the passenger's balance.
- **`withdraw(passenger: &mut Passenger, amount: u64, ctx: &mut TxContext)`**: Allows a passenger to withdraw funds from their balance.

### Trip Operations

- **`create_trip(service: &mut ServiceCap, driver: address, destination: String, fare: u64, ctx: &mut TxContext)`**: Creates a new trip managed by the service with a driver, destination, and fare.
- **`join_trip(trip: &mut Trip, passenger: &mut Passenger, ctx: &mut TxContext)`**: Allows a passenger to join a trip if they have sufficient balance to cover the fare.
- **`complete_trip(trip: &mut Trip, ctx: &mut TxContext)`**: Completes the trip and transfers the funds in the trip pool to the driver.

### Viewing Trips

- **`view_trips(service: &ServiceCap)`**: Returns a list of all active trips.

## Error Handling

The contract uses assertions to ensure safe execution. If any condition fails (e.g., insufficient balance, unauthorized access), the corresponding error code will be thrown, ensuring no unintended operations can occur.

## How to Deploy

1. Install the necessary tools for working with Move smart contracts.
2. Build the contract and deploy it to a SUI-based blockchain environment.
3. Interact with the smart contract by invoking the relevant functions for passenger registration, trip creation, and joining trips.

## Future Enhancements

- **Reputation System**: Introduce a reputation system for drivers and passengers based on completed trips.
- **Dynamic Pricing**: Implement dynamic fare adjustments based on demand and distance.
- **Cancellation Policy**: Add support for trip cancellation and refunds for passengers.
  
## License

This project is open-source and available under the MIT License.

---

Feel free to explore the codebase and customize it for your specific needs!
