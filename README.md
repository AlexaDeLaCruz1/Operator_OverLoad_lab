/*
Author: Alexa De La Cruz
Date: 2/4/2026
Purpose: Enhancing the Bank Account Management System
*/

// The Header File
#ifndef BANKACCOUNT_H
#define BANKACCOUNT_H

#include <string>
#include <iostream>

class BankAccount {
private:
    int accountNumber;
    std::string accountHolder;
    double balance;

public:

    BankAccount();
    BankAccount(int accNum, std::string holder, double initialBalance);

    // rule of three
    BankAccount(const BankAccount& other);            
    BankAccount& operator=(const BankAccount& other); 
    ~BankAccount();                                   

    // operator Overloading
    BankAccount& operator+=(double amount); // Deposit
    BankAccount& operator-=(double amount); // Withdrawal
    bool operator==(const BankAccount& other) const;
    bool operator<(const BankAccount& other) const;
    bool operator>(const BankAccount& other) const;

    // static utility
    static void printAccount(const BankAccount& account);
    static BankAccount createAccountFromInput();

    double getBalance() const { return balance; }
};

#endif




// The Implementation File
#include "BankAccount.h"
#include <iostream>

BankAccount::BankAccount() : accountNumber(0), accountHolder("N/A"), balance(0.0) {}

BankAccount::BankAccount(int accNum, std::string holder, double initialBalance) 
    : accountNumber(accNum), accountHolder(holder), balance(initialBalance) {}

BankAccount::BankAccount(const BankAccount& other) {
    accountNumber = other.accountNumber;
    accountHolder = other.accountHolder;
    balance = other.balance;
}

// to prevent self-assignment
BankAccount& BankAccount::operator=(const BankAccount& other) {
    if (this != &other) { // to prevent self-assignment
        accountNumber = other.accountNumber;
        accountHolder = other.accountHolder;
        balance = other.balance;
    }
    return *this;
}

BankAccount::~BankAccount() {



BankAccount& BankAccount::operator+=(double amount) {
    if (amount > 0) balance += amount;
    return *this;
}

BankAccount& BankAccount::operator-=(double amount) {
    if (amount > 0 && balance >= amount) {
        balance -= amount;
    } else {
        std::cout << "Insufficient funds or invalid amount.\n";
    }
    return *this;
}  // :]

bool BankAccount::operator==(const BankAccount& other) const {
    return this->accountNumber == other.accountNumber;
}

bool BankAccount::operator<(const BankAccount& other) const {
    return this->balance < other.balance;
}

bool BankAccount::operator>(const BankAccount& other) const {
    return this->balance > other.balance;
}

// double check!!!! 
void BankAccount::printAccount(const BankAccount& account) {
    std::cout << "\n--- Account Details ---" << std::endl;
    std::cout << "Account #: " << account.accountNumber << std::endl;
    std::cout << "Holder:    " << account.accountHolder << std::endl;
    std::cout << "Balance:   $" << account.balance << std::endl;
}

BankAccount BankAccount::createAccountFromInput() {
    int id;
    std::string name;
    double bal;

    std::cout << "Enter Account Number: ";
    std::cin >> id;
    std::cin.ignore(); // Clear newline
    std::cout << "Enter Holder Name: ";
    std::getline(std::cin, name);
    std::cout << "Enter Initial Balance: ";
    std::cin >> bal;

    return BankAccount(id, name, bal);
}


// Main
#include "BankAccount.h"
#include <iostream>
#include <vector>

int main() {
    BankAccount myAccount = BankAccount::createAccountFromInput();
    int choice;

    do {
        std::cout << "\n1. Deposit (+=)\n2. Withdraw (-=)\n3. View Details (Static Print)\n4. Compare with another\n5. Exit\nChoice: ";
        std::cin >> choice;

        if (choice == 1) {
            double amt;
            std::cout << "Enter deposit amount: ";
            std::cin >> amt;
            myAccount += amt; // Using overloaded operator
        } else if (choice == 2) {
            double amt;
            std::cout << "Enter withdrawal amount: ";
            std::cin >> amt;
            myAccount -= amt; // Using overloaded operator
        } else if (choice == 3) {
            BankAccount::printAccount(myAccount); // Using static function
        } else if (choice == 4) {
            std::cout << "Create a second account for comparison:\n";
            BankAccount other = BankAccount::createAccountFromInput();
            
            if (myAccount == other) std::cout << "Same account numbers!\n";
            if (myAccount > other)  std::cout << "Primary account has a higher balance.\n";
            if (myAccount < other)  std::cout << "Primary account has a lower balance.\n";
        }
    } while (choice != 5);

    return 0;
}
