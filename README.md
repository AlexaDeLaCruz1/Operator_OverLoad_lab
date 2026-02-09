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
    // Constructors
    BankAccount();
    BankAccount(int accNum, std::string holder, double initialBalance);

    // the rule of three 
    BankAccount(const BankAccount& other);            
    BankAccount& operator=(const BankAccount& other); 
    ~BankAccount();                                   

    // the operator Overloading
    BankAccount& operator+=(double amount); // deposit
    BankAccount& operator-=(double amount); // withdrawal
    bool operator==(const BankAccount& other) const;
    bool operator<(const BankAccount& other) const;
    bool operator>(const BankAccount& other) const;

    // Static utility functions 
    static void printAccount(const BankAccount& account);
    static BankAccount createAccountFromInput();

    // Getters
    double getBalance() const { return balance; }
};

#endif
