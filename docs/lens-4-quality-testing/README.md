# Lens 4: Quality & Testing Excellence

## Overview

The Quality & Testing lens provides a comprehensive framework for implementing agentic testing strategies across functional, visual, accessibility, performance, security, and resilience testing. This module transforms testing from a manual, post-development activity into an automated, specification-driven process that runs throughout the development lifecycle.

## Core Technology Stack

| Technology | Purpose | Application |
|-----------|---------|-------------|
| **Claude Agentic SDK (Latest)** | AI orchestration | Test generation, multi-agent coordination, intelligent analysis |
| **LangChain** | Agent framework | Multi-agent test orchestration, workflow chains |
| **TDD (Test-Driven Development)** | Methodology | Red-green-refactor cycle, unit testing |
| **BDD (Behavior-Driven Development)** | Methodology | Gherkin scenarios, stakeholder collaboration |
| **Selenium MCP** | Browser automation (UI) | Cross-browser UI testing with natural language |
| **Playwright MCP** | Modern browser testing (UI) | Auto-waiting, network mocking, trace files, parallel execution |
| **Postman MCP** | API testing | REST/GraphQL API testing, collection management, automation |
| **Figma MCP** | Design-to-test automation | Generate tests from visual specifications |
| **AWS MCP Marketplace** | Cloud integration | AWS services testing (Lambda, API Gateway, S3, DynamoDB, etc.) |
| **Azure MCP Marketplace** | Cloud integration | Azure services testing (Functions, API Management, Storage, etc.) |

**Note:** Chaos Engineering (AWS FIS, Fault Injection) has been moved to **[Lens 3: Platform, Cloud & SRE](../lens-3-platform-cloud-sre/README.md)** as it is part of the deployment and infrastructure resilience strategy managed by DevOps/SRE teams.

## Table of Contents

1. [Introduction: The Agentic Testing Paradigm](#1-introduction-the-agentic-testing-paradigm)
2. [SDD → BDD → TDD Hierarchy](#2-sdd--bdd--tdd-hierarchy)
3. [Multi-Language TDD: Unit Testing for Existing & New Code](#3-multi-language-tdd-unit-testing-for-existing--new-code)
4. [Multi-Language BDD: Functional Testing](#4-multi-language-bdd-functional-testing)
5. [Browser MCP: Selenium & Playwright (UI Testing)](#5-browser-mcp-selenium--playwright-ui-testing)
6. [Postman MCP: API Testing Automation](#6-postman-mcp-api-testing-automation)
7. [Design-to-Test Automation with Figma MCP](#7-design-to-test-automation-with-figma-mcp)
8. [AWS/Azure Cloud Environment Testing](#8-awsazure-cloud-environment-testing)
9. [Multi-Agent Test Orchestration](#9-multi-agent-test-orchestration)
10. [Testing Existing Codebases: Retrofitting Strategies](#10-testing-existing-codebases-retrofitting-strategies)
11. [Performance & Load Testing](#11-performance--load-testing)
12. [Security & Vulnerability Testing](#12-security--vulnerability-testing)
13. [Best Practices & Patterns](#13-best-practices--patterns)

---

## 1. Introduction: The Agentic Testing Paradigm

### The Paradigm Shift

Traditional testing follows a reactive model:

```text
Spec → Code → Write Tests (often skipped due to time pressure)
```

Agentic testing inverts this:

```text
Spec → Auto-Generate Tests → Code to Pass Tests
```

**Result:** 100% test coverage from day one, tests serve as the specification.

### Why This Matters for FSI

Financial services institutions face unique testing challenges:

- **Regulatory Compliance**: SOC 2, PCI DSS, GDPR require comprehensive testing evidence
- **Zero-Error Tolerance**: Financial transactions must be 100% accurate
- **High Availability**: 99.9%+ uptime SLAs
- **Security Critical**: Constant threat landscape
- **Audit Requirements**: Complete traceability from requirements to tests to code

Agentic testing provides:

- ✅ Automated compliance evidence
- ✅ Comprehensive test coverage
- ✅ Continuous validation
- ✅ Complete audit trail
- ✅ Faster time-to-market

---

## 2. SDD → BDD → TDD Hierarchy

### The Three-Layer Testing Foundation

```text
┌──────────────────────────────────────────────────────────┐
│  Layer 1: SDD (Spec-Driven Development)                  │
│  Purpose: Define WHAT to build                           │
│  Artifact: requirements.md, design.md, spec.md           │
│  Stakeholder: Product Manager, Business Analyst          │
└─────────────────┬────────────────────────────────────────┘
                  │
                  ▼
┌──────────────────────────────────────────────────────────┐
│  Layer 2: BDD (Behavior-Driven Development)              │
│  Purpose: Define HOW it should behave                    │
│  Artifact: feature.spec (Gherkin)                        │
│  Stakeholder: QA Engineer, Product Manager, Developer    │
└─────────────────┬────────────────────────────────────────┘
                  │
                  ▼
┌──────────────────────────────────────────────────────────┐
│  Layer 3: TDD (Test-Driven Development)                  │
│  Purpose: Define implementation correctness              │
│  Artifact: unit.test.ts, integration.test.ts             │
│  Stakeholder: Developer                                  │
└──────────────────────────────────────────────────────────┘
```

### Gherkin: The Ubiquitous Language

**Gherkin** is the critical bridge between human-readable requirements and machine-executable tests. It provides a structured, stakeholder-accessible format that translates directly into automated test code.

**Gherkin Structure:**

```gherkin
Feature: [High-level capability]
  As a [role]
  I want to [action]
  So that [business value]

  Background:
    Given [precondition for all scenarios]

  Scenario: [Specific test case]
    Given [initial context]
    When [action]
    Then [expected outcome]
    And [additional expectation]

  Scenario Outline: [Parameterized test case]
    Given [context with <parameter>]
    When [action with <parameter>]
    Then [outcome with <parameter>]

    Examples:
      | parameter1 | parameter2 | expected |
      | value1     | value2     | result1  |
```

#### Example: Banking Transfer Feature

```gherkin
Feature: Inter-Account Fund Transfer
  As a banking customer
  I want to transfer funds between my accounts
  So that I can manage my finances efficiently

  Background:
    Given I am logged in to my account
    And I have a checking account with balance $5000
    And I have a savings account with balance $10000
    And my daily transfer limit is $2000

  Scenario: Successful transfer within limits
    When I transfer $500 from checking to savings
    Then the transfer should be completed
    And my checking balance should be $4500
    And my savings balance should be $10500
    And I should receive a confirmation notification

  Scenario: Transfer exceeds available balance
    When I transfer $6000 from checking to savings
    Then the transfer should be rejected
    And I should see error "Insufficient funds"
    And no account balances should change

  Scenario: Transfer exceeds daily limit
    Given I have already transferred $1800 today
    When I transfer $500 from checking to savings
    Then the transfer should be rejected
    And I should see error "Daily transfer limit exceeded"
    And I should see "Remaining limit: $200"

  Scenario Outline: Multiple transfer amounts
    When I transfer $<amount> from checking to savings
    Then the transfer should be <status>
    And my checking balance should be $<checking_final>

    Examples:
      | amount | status    | checking_final |
      | 100    | completed | 4900          |
      | 500    | completed | 4500          |
      | 5000   | rejected  | 5000          |
      | 0      | rejected  | 5000          |
```

### From Gherkin to Executable Tests

```typescript
// Auto-generated from Gherkin using Cucumber.js
import { Given, When, Then } from '@cucumber/cucumber';
import { expect } from '@playwright/test';

Given('I am logged in to my account', async function() {
  await this.page.goto('/login');
  await this.page.fill('[data-testid="email"]', 'user@example.com');
  await this.page.fill('[data-testid="password"]', 'SecurePass123!');
  await this.page.click('[data-testid="sign-in-button"]');
  await expect(this.page).toHaveURL('/dashboard');
});

Given('I have a checking account with balance ${int}', async function(balance) {
  // Seed test data via API
  await this.api.post('/test/seed-account', {
    accountType: 'checking',
    balance: balance
  });
});

When('I transfer ${int} from checking to savings', async function(amount) {
  await this.page.click('[data-testid="transfer-button"]');
  await this.page.selectOption('[data-testid="from-account"]', 'checking');
  await this.page.selectOption('[data-testid="to-account"]', 'savings');
  await this.page.fill('[data-testid="amount"]', amount.toString());
  await this.page.click('[data-testid="submit-transfer"]');
});

Then('the transfer should be completed', async function() {
  await expect(this.page.locator('[data-testid="success-message"]'))
    .toContainText('Transfer completed');
});

Then('my checking balance should be ${int}', async function(expectedBalance) {
  const balance = await this.page.locator('[data-testid="checking-balance"]')
    .textContent();
  expect(parseFloat(balance.replace('$', ''))).toBe(expectedBalance);
});
```

---

## 3. Multi-Language TDD: Unit Testing for Existing & New Code

### The TDD Workflow with AI Agents

Test-Driven Development (TDD) follows the **red-green-refactor cycle**. With AI agents, developers guide the agent through this cycle while providing architectural decisions and guardrails.

```text
┌─────────────────────────────────────────────────────────────┐
│  TDD Cycle with AI Agents                                   │
└─────────────────────────────────────────────────────────────┘

Step 1: RED (Write Failing Test)
Developer provides specification → Agent writes failing test → Run test ❌

Step 2: GREEN (Write Minimal Code)
Developer requests implementation → Agent writes code → Run test ✅

Step 3: REFACTOR (Improve Quality)
Developer requests improvements → Agent refactors → Run test ✅

Repeat for each feature/function
```

### TDD Examples by Language/Framework

#### JavaScript/TypeScript (Jest)

**Use Case:** New feature - Calculate compound interest

**Step 1: RED - Write Failing Test**

```typescript
// __tests__/financial.test.ts
import { calculateCompoundInterest } from '../src/financial';

describe('calculateCompoundInterest', () => {
  it('should calculate compound interest correctly', () => {
    const result = calculateCompoundInterest({
      principal: 10000,
      annualRate: 0.05,
      years: 10,
      compoundingFrequency: 12 // monthly
    });
    expect(result).toBeCloseTo(16470.09, 2);
  });

  it('should handle zero principal', () => {
    const result = calculateCompoundInterest({
      principal: 0,
      annualRate: 0.05,
      years: 10,
      compoundingFrequency: 12
    });
    expect(result).toBe(0);
  });

  it('should throw error for negative rate', () => {
    expect(() => {
      calculateCompoundInterest({
        principal: 10000,
        annualRate: -0.05,
        years: 10,
        compoundingFrequency: 12
      });
    }).toThrow('Interest rate cannot be negative');
  });
});
```

**Run:** `npm test` → ❌ FAILS (function doesn't exist)

**Step 2: GREEN - Implement Minimal Code**

```typescript
// src/financial.ts
interface CompoundInterestParams {
  principal: number;
  annualRate: number;
  years: number;
  compoundingFrequency: number;
}

export function calculateCompoundInterest(params: CompoundInterestParams): number {
  const { principal, annualRate, years, compoundingFrequency } = params;
  
  if (annualRate < 0) {
    throw new Error('Interest rate cannot be negative');
  }
  
  if (principal === 0) {
    return 0;
  }
  
  const rate = annualRate / compoundingFrequency;
  const periods = compoundingFrequency * years;
  const amount = principal * Math.pow(1 + rate, periods);
  
  return Math.round(amount * 100) / 100;
}
```

**Run:** `npm test` → ✅ PASSES

**Step 3: REFACTOR - Improve Quality**

```typescript
// src/financial.ts (refactored)
interface CompoundInterestParams {
  principal: number;
  annualRate: number;
  years: number;
  compoundingFrequency: number;
}

interface ValidationResult {
  valid: boolean;
  error?: string;
}

function validateParams(params: CompoundInterestParams): ValidationResult {
  if (params.annualRate < 0) {
    return { valid: false, error: 'Interest rate cannot be negative' };
  }
  if (params.principal < 0) {
    return { valid: false, error: 'Principal cannot be negative' };
  }
  if (params.years < 0) {
    return { valid: false, error: 'Years cannot be negative' };
  }
  if (params.compoundingFrequency <= 0) {
    return { valid: false, error: 'Compounding frequency must be positive' };
  }
  return { valid: true };
}

export function calculateCompoundInterest(params: CompoundInterestParams): number {
  const validation = validateParams(params);
  if (!validation.valid) {
    throw new Error(validation.error);
  }
  
  if (params.principal === 0) {
    return 0;
  }
  
  const rate = params.annualRate / params.compoundingFrequency;
  const periods = params.compoundingFrequency * params.years;
  const amount = params.principal * Math.pow(1 + rate, periods);
  
  return Math.round(amount * 100) / 100;
}
```

**Run:** `npm test` → ✅ PASSES (with better code quality)

---

#### Angular (Jasmine/Karma)

**Use Case:** Component testing for account summary

**Step 1: RED - Write Failing Test**

```typescript
// src/app/components/account-summary/account-summary.component.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { AccountSummaryComponent } from './account-summary.component';
import { AccountService } from '../../services/account.service';
import { of } from 'rxjs';

describe('AccountSummaryComponent', () => {
  let component: AccountSummaryComponent;
  let fixture: ComponentFixture<AccountSummaryComponent>;
  let mockAccountService: jasmine.SpyObj<AccountService>;

  beforeEach(async () => {
    mockAccountService = jasmine.createSpyObj('AccountService', ['getAccounts']);
    
    await TestBed.configureTestingModule({
      declarations: [ AccountSummaryComponent ],
      providers: [
        { provide: AccountService, useValue: mockAccountService }
      ]
    }).compileComponents();

    fixture = TestBed.createComponent(AccountSummaryComponent);
    component = fixture.componentInstance;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should load accounts on init', () => {
    const mockAccounts = [
      { id: '1', type: 'checking', balance: 5000 },
      { id: '2', type: 'savings', balance: 10000 }
    ];
    mockAccountService.getAccounts.and.returnValue(of(mockAccounts));
    
    fixture.detectChanges(); // triggers ngOnInit
    
    expect(component.accounts.length).toBe(2);
    expect(component.totalBalance).toBe(15000);
  });

  it('should calculate total balance correctly', () => {
    component.accounts = [
      { id: '1', type: 'checking', balance: 5000 },
      { id: '2', type: 'savings', balance: 10000 }
    ];
    
    const total = component.calculateTotalBalance();
    
    expect(total).toBe(15000);
  });
});
```

**Run:** `ng test` → ❌ FAILS (component doesn't exist)

**Step 2: GREEN - Implement Component**

```typescript
// src/app/components/account-summary/account-summary.component.ts
import { Component, OnInit } from '@angular/core';
import { AccountService } from '../../services/account.service';

interface Account {
  id: string;
  type: string;
  balance: number;
}

@Component({
  selector: 'app-account-summary',
  templateUrl: './account-summary.component.html',
  styleUrls: ['./account-summary.component.scss']
})
export class AccountSummaryComponent implements OnInit {
  accounts: Account[] = [];
  totalBalance: number = 0;

  constructor(private accountService: AccountService) {}

  ngOnInit(): void {
    this.loadAccounts();
  }

  loadAccounts(): void {
    this.accountService.getAccounts().subscribe(accounts => {
      this.accounts = accounts;
      this.totalBalance = this.calculateTotalBalance();
    });
  }

  calculateTotalBalance(): number {
    return this.accounts.reduce((sum, account) => sum + account.balance, 0);
  }
}
```

**Run:** `ng test` → ✅ PASSES

---

#### React (Jest + React Testing Library)

**Use Case:** Custom hook for form validation

**Step 1: RED - Write Failing Test**

```typescript
// src/hooks/__tests__/useFormValidation.test.ts
import { renderHook, act } from '@testing-library/react';
import { useFormValidation } from '../useFormValidation';

describe('useFormValidation', () => {
  const validationRules = {
    email: (value: string) => {
      if (!value) return 'Email is required';
      if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) return 'Invalid email format';
      return '';
    },
    amount: (value: string) => {
      const num = parseFloat(value);
      if (isNaN(num)) return 'Amount must be a number';
      if (num <= 0) return 'Amount must be positive';
      return '';
    }
  };

  it('should initialize with no errors', () => {
    const { result } = renderHook(() => useFormValidation(validationRules));
    
    expect(result.current.errors).toEqual({});
    expect(result.current.isValid).toBe(true);
  });

  it('should validate email field', () => {
    const { result } = renderHook(() => useFormValidation(validationRules));
    
    act(() => {
      result.current.validate('email', '');
    });
    
    expect(result.current.errors.email).toBe('Email is required');
    expect(result.current.isValid).toBe(false);
  });

  it('should validate multiple fields', () => {
    const { result } = renderHook(() => useFormValidation(validationRules));
    
    act(() => {
      result.current.validate('email', 'invalid-email');
      result.current.validate('amount', '-100');
    });
    
    expect(result.current.errors.email).toBe('Invalid email format');
    expect(result.current.errors.amount).toBe('Amount must be positive');
    expect(result.current.isValid).toBe(false);
  });

  it('should clear errors when validation passes', () => {
    const { result } = renderHook(() => useFormValidation(validationRules));
    
    act(() => {
      result.current.validate('email', 'invalid');
    });
    expect(result.current.errors.email).toBeTruthy();
    
    act(() => {
      result.current.validate('email', 'user@example.com');
    });
    expect(result.current.errors.email).toBeFalsy();
    expect(result.current.isValid).toBe(true);
  });
});
```

**Run:** `npm test` → ❌ FAILS (hook doesn't exist)

**Step 2: GREEN - Implement Hook**

```typescript
// src/hooks/useFormValidation.ts
import { useState, useCallback } from 'react';

type ValidationRule = (value: string) => string;
type ValidationRules = Record<string, ValidationRule>;
type Errors = Record<string, string>;

export function useFormValidation(rules: ValidationRules) {
  const [errors, setErrors] = useState<Errors>({});

  const validate = useCallback((field: string, value: string) => {
    const rule = rules[field];
    if (!rule) return;

    const error = rule(value);
    setErrors(prev => {
      if (!error) {
        const { [field]: _, ...rest } = prev;
        return rest;
      }
      return { ...prev, [field]: error };
    });
  }, [rules]);

  const isValid = Object.keys(errors).length === 0;

  return { errors, isValid, validate };
}
```

**Run:** `npm test` → ✅ PASSES

---

#### Java (JUnit 5)

**Use Case:** Service class for transaction processing

**Step 1: RED - Write Failing Test**

```java
// src/test/java/com/bank/service/TransactionServiceTest.java
package com.bank.service;

import com.bank.model.Account;
import com.bank.model.Transaction;
import com.bank.repository.AccountRepository;
import com.bank.repository.TransactionRepository;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

import java.math.BigDecimal;
import java.util.Optional;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class TransactionServiceTest {

    @Mock
    private AccountRepository accountRepository;

    @Mock
    private TransactionRepository transactionRepository;

    private TransactionService transactionService;

    @BeforeEach
    void setUp() {
        transactionService = new TransactionService(accountRepository, transactionRepository);
    }

    @Test
    void shouldTransferFundsSuccessfully() {
        // Arrange
        Account fromAccount = new Account("ACC001", new BigDecimal("5000.00"));
        Account toAccount = new Account("ACC002", new BigDecimal("10000.00"));
        BigDecimal amount = new BigDecimal("500.00");

        when(accountRepository.findById("ACC001")).thenReturn(Optional.of(fromAccount));
        when(accountRepository.findById("ACC002")).thenReturn(Optional.of(toAccount));
        when(transactionRepository.save(any(Transaction.class))).thenAnswer(i -> i.getArguments()[0]);

        // Act
        Transaction result = transactionService.transfer("ACC001", "ACC002", amount);

        // Assert
        assertNotNull(result);
        assertEquals(new BigDecimal("4500.00"), fromAccount.getBalance());
        assertEquals(new BigDecimal("10500.00"), toAccount.getBalance());
        verify(accountRepository, times(2)).save(any(Account.class));
        verify(transactionRepository, times(1)).save(any(Transaction.class));
    }

    @Test
    void shouldThrowExceptionWhenInsufficientFunds() {
        // Arrange
        Account fromAccount = new Account("ACC001", new BigDecimal("100.00"));
        Account toAccount = new Account("ACC002", new BigDecimal("10000.00"));
        BigDecimal amount = new BigDecimal("500.00");

        when(accountRepository.findById("ACC001")).thenReturn(Optional.of(fromAccount));
        when(accountRepository.findById("ACC002")).thenReturn(Optional.of(toAccount));

        // Act & Assert
        assertThrows(InsufficientFundsException.class, () -> {
            transactionService.transfer("ACC001", "ACC002", amount);
        });

        verify(accountRepository, never()).save(any(Account.class));
        verify(transactionRepository, never()).save(any(Transaction.class));
    }

    @Test
    void shouldThrowExceptionWhenAccountNotFound() {
        // Arrange
        BigDecimal amount = new BigDecimal("500.00");
        when(accountRepository.findById("ACC001")).thenReturn(Optional.empty());

        // Act & Assert
        assertThrows(AccountNotFoundException.class, () -> {
            transactionService.transfer("ACC001", "ACC002", amount);
        });
    }
}
```

**Run:** `mvn test` → ❌ FAILS (service doesn't exist)

**Step 2: GREEN - Implement Service**

```java
// src/main/java/com/bank/service/TransactionService.java
package com.bank.service;

import com.bank.model.Account;
import com.bank.model.Transaction;
import com.bank.repository.AccountRepository;
import com.bank.repository.TransactionRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.math.BigDecimal;
import java.time.LocalDateTime;

@Service
public class TransactionService {

    private final AccountRepository accountRepository;
    private final TransactionRepository transactionRepository;

    public TransactionService(AccountRepository accountRepository, 
                            TransactionRepository transactionRepository) {
        this.accountRepository = accountRepository;
        this.transactionRepository = transactionRepository;
    }

    @Transactional
    public Transaction transfer(String fromAccountId, String toAccountId, BigDecimal amount) {
        Account fromAccount = accountRepository.findById(fromAccountId)
            .orElseThrow(() -> new AccountNotFoundException(fromAccountId));
        
        Account toAccount = accountRepository.findById(toAccountId)
            .orElseThrow(() -> new AccountNotFoundException(toAccountId));

        if (fromAccount.getBalance().compareTo(amount) < 0) {
            throw new InsufficientFundsException(fromAccountId, amount);
        }

        fromAccount.setBalance(fromAccount.getBalance().subtract(amount));
        toAccount.setBalance(toAccount.getBalance().add(amount));

        accountRepository.save(fromAccount);
        accountRepository.save(toAccount);

        Transaction transaction = new Transaction();
        transaction.setFromAccountId(fromAccountId);
        transaction.setToAccountId(toAccountId);
        transaction.setAmount(amount);
        transaction.setTimestamp(LocalDateTime.now());
        transaction.setStatus("COMPLETED");

        return transactionRepository.save(transaction);
    }
}
```

**Run:** `mvn test` → ✅ PASSES

---

#### .NET (xUnit/NUnit)

**Use Case:** Account validation service

**Step 1: RED - Write Failing Test**

```csharp
// Tests/AccountValidationServiceTests.cs
using Xunit;
using Moq;
using BankingApp.Services;
using BankingApp.Models;
using BankingApp.Repositories;
using System;
using System.Threading.Tasks;

namespace BankingApp.Tests.Services
{
    public class AccountValidationServiceTests
    {
        private readonly Mock<IAccountRepository> _mockRepository;
        private readonly AccountValidationService _service;

        public AccountValidationServiceTests()
        {
            _mockRepository = new Mock<IAccountRepository>();
            _service = new AccountValidationService(_mockRepository.Object);
        }

        [Fact]
        public async Task ValidateAccount_ShouldReturnTrue_WhenAccountIsValid()
        {
            // Arrange
            var account = new Account
            {
                Id = "ACC001",
                Balance = 5000m,
                Status = AccountStatus.Active,
                CustomerId = "CUST001"
            };

            _mockRepository.Setup(r => r.GetByIdAsync("ACC001"))
                .ReturnsAsync(account);

            // Act
            var result = await _service.ValidateAccountAsync("ACC001");

            // Assert
            Assert.True(result.IsValid);
            Assert.Empty(result.Errors);
        }

        [Fact]
        public async Task ValidateAccount_ShouldReturnFalse_WhenAccountIsFrozen()
        {
            // Arrange
            var account = new Account
            {
                Id = "ACC001",
                Balance = 5000m,
                Status = AccountStatus.Frozen,
                CustomerId = "CUST001"
            };

            _mockRepository.Setup(r => r.GetByIdAsync("ACC001"))
                .ReturnsAsync(account);

            // Act
            var result = await _service.ValidateAccountAsync("ACC001");

            // Assert
            Assert.False(result.IsValid);
            Assert.Contains("Account is frozen", result.Errors);
        }

        [Fact]
        public async Task ValidateAccount_ShouldReturnFalse_WhenAccountNotFound()
        {
            // Arrange
            _mockRepository.Setup(r => r.GetByIdAsync("ACC999"))
                .ReturnsAsync((Account)null);

            // Act
            var result = await _service.ValidateAccountAsync("ACC999");

            // Assert
            Assert.False(result.IsValid);
            Assert.Contains("Account not found", result.Errors);
        }

        [Theory]
        [InlineData(-100)]
        [InlineData(-0.01)]
        public async Task ValidateAccount_ShouldReturnFalse_WhenBalanceIsNegative(decimal balance)
        {
            // Arrange
            var account = new Account
            {
                Id = "ACC001",
                Balance = balance,
                Status = AccountStatus.Active,
                CustomerId = "CUST001"
            };

            _mockRepository.Setup(r => r.GetByIdAsync("ACC001"))
                .ReturnsAsync(account);

            // Act
            var result = await _service.ValidateAccountAsync("ACC001");

            // Assert
            Assert.False(result.IsValid);
            Assert.Contains("Negative balance detected", result.Errors);
        }
    }
}
```

**Run:** `dotnet test` → ❌ FAILS (service doesn't exist)

**Step 2: GREEN - Implement Service**

```csharp
// Services/AccountValidationService.cs
using BankingApp.Models;
using BankingApp.Repositories;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace BankingApp.Services
{
    public class AccountValidationService
    {
        private readonly IAccountRepository _accountRepository;

        public AccountValidationService(IAccountRepository accountRepository)
        {
            _accountRepository = accountRepository;
        }

        public async Task<ValidationResult> ValidateAccountAsync(string accountId)
        {
            var result = new ValidationResult();
            
            var account = await _accountRepository.GetByIdAsync(accountId);
            
            if (account == null)
            {
                result.IsValid = false;
                result.Errors.Add("Account not found");
                return result;
            }

            if (account.Status == AccountStatus.Frozen)
            {
                result.IsValid = false;
                result.Errors.Add("Account is frozen");
            }

            if (account.Status == AccountStatus.Closed)
            {
                result.IsValid = false;
                result.Errors.Add("Account is closed");
            }

            if (account.Balance < 0)
            {
                result.IsValid = false;
                result.Errors.Add("Negative balance detected");
            }

            result.IsValid = result.Errors.Count == 0;
            return result;
        }
    }

    public class ValidationResult
    {
        public bool IsValid { get; set; } = true;
        public List<string> Errors { get; set; } = new List<string>();
    }
}
```

**Run:** `dotnet test` → ✅ PASSES

---

#### Python (pytest)

**Use Case:** Interest calculator with business rules

**Step 1: RED - Write Failing Test**

```python
# tests/test_interest_calculator.py
import pytest
from decimal import Decimal
from src.interest_calculator import InterestCalculator
from src.exceptions import InvalidParameterError

class TestInterestCalculator:
    
    @pytest.fixture
    def calculator(self):
        return InterestCalculator()
    
    def test_calculate_simple_interest(self, calculator):
        """Test simple interest calculation"""
        result = calculator.calculate_simple_interest(
            principal=Decimal('10000'),
            rate=Decimal('0.05'),
            years=5
        )
        assert result == Decimal('2500.00')
    
    def test_calculate_compound_interest_annually(self, calculator):
        """Test compound interest with annual compounding"""
        result = calculator.calculate_compound_interest(
            principal=Decimal('10000'),
            rate=Decimal('0.05'),
            years=5,
            frequency='annually'
        )
        assert result == pytest.approx(Decimal('2762.82'), abs=Decimal('0.01'))
    
    def test_calculate_compound_interest_monthly(self, calculator):
        """Test compound interest with monthly compounding"""
        result = calculator.calculate_compound_interest(
            principal=Decimal('10000'),
            rate=Decimal('0.05'),
            years=5,
            frequency='monthly'
        )
        assert result == pytest.approx(Decimal('2833.59'), abs=Decimal('0.01'))
    
    def test_negative_principal_raises_error(self, calculator):
        """Test that negative principal raises error"""
        with pytest.raises(InvalidParameterError, match='Principal cannot be negative'):
            calculator.calculate_simple_interest(
                principal=Decimal('-1000'),
                rate=Decimal('0.05'),
                years=5
            )
    
    def test_negative_rate_raises_error(self, calculator):
        """Test that negative rate raises error"""
        with pytest.raises(InvalidParameterError, match='Interest rate cannot be negative'):
            calculator.calculate_simple_interest(
                principal=Decimal('10000'),
                rate=Decimal('-0.05'),
                years=5
            )
    
    def test_zero_years_returns_zero_interest(self, calculator):
        """Test that zero years returns zero interest"""
        result = calculator.calculate_simple_interest(
            principal=Decimal('10000'),
            rate=Decimal('0.05'),
            years=0
        )
        assert result == Decimal('0.00')
    
    @pytest.mark.parametrize('principal,rate,years,expected', [
        (Decimal('5000'), Decimal('0.04'), 3, Decimal('600.00')),
        (Decimal('15000'), Decimal('0.06'), 2, Decimal('1800.00')),
        (Decimal('20000'), Decimal('0.03'), 10, Decimal('6000.00')),
    ])
    def test_simple_interest_multiple_scenarios(self, calculator, principal, rate, years, expected):
        """Test simple interest with multiple scenarios"""
        result = calculator.calculate_simple_interest(principal, rate, years)
        assert result == expected
```

**Run:** `pytest` → ❌ FAILS (calculator doesn't exist)

**Step 2: GREEN - Implement Calculator**

```python
# src/interest_calculator.py
from decimal import Decimal, ROUND_HALF_UP
from typing import Literal
from src.exceptions import InvalidParameterError

FrequencyType = Literal['annually', 'semi-annually', 'quarterly', 'monthly', 'daily']

class InterestCalculator:
    
    FREQUENCY_MAP = {
        'annually': 1,
        'semi-annually': 2,
        'quarterly': 4,
        'monthly': 12,
        'daily': 365
    }
    
    def _validate_parameters(self, principal: Decimal, rate: Decimal, years: int) -> None:
        """Validate input parameters"""
        if principal < 0:
            raise InvalidParameterError('Principal cannot be negative')
        if rate < 0:
            raise InvalidParameterError('Interest rate cannot be negative')
        if years < 0:
            raise InvalidParameterError('Years cannot be negative')
    
    def calculate_simple_interest(
        self, 
        principal: Decimal, 
        rate: Decimal, 
        years: int
    ) -> Decimal:
        """
        Calculate simple interest: I = P * R * T
        
        Args:
            principal: Initial amount
            rate: Annual interest rate (as decimal, e.g., 0.05 for 5%)
            years: Number of years
            
        Returns:
            Interest amount rounded to 2 decimal places
        """
        self._validate_parameters(principal, rate, years)
        
        interest = principal * rate * Decimal(years)
        return interest.quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)
    
    def calculate_compound_interest(
        self,
        principal: Decimal,
        rate: Decimal,
        years: int,
        frequency: FrequencyType = 'annually'
    ) -> Decimal:
        """
        Calculate compound interest: A = P(1 + r/n)^(nt) - P
        
        Args:
            principal: Initial amount
            rate: Annual interest rate (as decimal)
            years: Number of years
            frequency: Compounding frequency
            
        Returns:
            Interest amount rounded to 2 decimal places
        """
        self._validate_parameters(principal, rate, years)
        
        if frequency not in self.FREQUENCY_MAP:
            raise InvalidParameterError(f'Invalid frequency: {frequency}')
        
        n = Decimal(self.FREQUENCY_MAP[frequency])
        t = Decimal(years)
        
        # A = P(1 + r/n)^(nt)
        amount = principal * ((1 + rate / n) ** (n * t))
        interest = amount - principal
        
        return interest.quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)
```

```python
# src/exceptions.py
class InvalidParameterError(ValueError):
    """Raised when invalid parameters are provided"""
    pass
```

**Run:** `pytest` → ✅ PASSES

---

### Key TDD Principles Across All Languages

1. **Write Test First**: Always write the test before implementation
2. **Red-Green-Refactor**: Follow the cycle strictly
3. **Small Steps**: Test one behavior at a time
4. **Fast Tests**: Unit tests should run in milliseconds
5. **Isolated Tests**: No dependencies on external systems
6. **Mock External Dependencies**: Use mocking frameworks
7. **Parameterized Tests**: Test multiple scenarios efficiently
8. **Descriptive Names**: Test names should describe the behavior

---

## 4. Multi-Language BDD: Functional Testing

### BDD: Bridging Business Requirements and Technical Implementation

Behavior-Driven Development (BDD) uses **Gherkin** as a ubiquitous language that all stakeholders understand—from product managers to developers to QA engineers. This ensures everyone shares the same understanding of system behavior.

### BDD Workflow with AI Agents

```text
┌─────────────────────────────────────────────────────────────┐
│  BDD Workflow with AI Agents                                │
└─────────────────────────────────────────────────────────────┘

Step 1: Write Gherkin Scenarios
Product Manager/BA writes features in plain English (Given-When-Then)

Step 2: Agent Generates Step Definitions
AI agent converts Gherkin into executable test code

Step 3: Implement Application Code
Developers implement features to pass the scenarios

Step 4: Continuous Validation
Tests run on every commit, providing living documentation
```

### BDD Examples by Language/Framework

#### JavaScript/TypeScript (Cucumber.js + Playwright)

**Feature File:**

```gherkin
# features/fund_transfer.feature
Feature: Inter-Account Fund Transfer
  As a banking customer
  I want to transfer funds between my accounts
  So that I can manage my finances efficiently

  Background:
    Given I am logged in as "john.doe@example.com"
    And I have a checking account "CHK-001" with balance $5000
    And I have a savings account "SAV-001" with balance $10000
    And my daily transfer limit is $2000

  Scenario: Successful transfer within limits
    When I navigate to the transfer page
    And I select "CHK-001" as the source account
    And I select "SAV-001" as the destination account
    And I enter transfer amount $500
    And I click the "Transfer" button
    Then I should see a success message "Transfer completed successfully"
    And the checking account "CHK-001" balance should be $4500
    And the savings account "SAV-001" balance should be $10500
    And I should receive an email confirmation

  Scenario: Transfer exceeds available balance
    When I navigate to the transfer page
    And I select "CHK-001" as the source account
    And I select "SAV-001" as the destination account
    And I enter transfer amount $6000
    And I click the "Transfer" button
    Then I should see an error message "Insufficient funds"
    And the checking account "CHK-001" balance should remain $5000
    And the savings account "SAV-001" balance should remain $10000

  Scenario Outline: Multiple transfer scenarios
    When I navigate to the transfer page
    And I select "CHK-001" as the source account
    And I select "SAV-001" as the destination account
    And I enter transfer amount $<amount>
    And I click the "Transfer" button
    Then I should see <message_type> message "<message>"
    And the checking account "CHK-001" balance should be $<final_balance>

    Examples:
      | amount | message_type | message                      | final_balance |
      | 100    | success      | Transfer completed          | 4900          |
      | 500    | success      | Transfer completed          | 4500          |
      | 5000   | error        | Insufficient funds          | 5000          |
      | 0      | error        | Amount must be greater than 0 | 5000          |
      | -100   | error        | Amount must be greater than 0 | 5000          |
```

**Step Definitions (Auto-generated by AI Agent):**

```typescript
// features/step_definitions/fund_transfer.steps.ts
import { Given, When, Then, Before, After } from '@cucumber/cucumber';
import { expect } from '@playwright/test';
import { Page } from '@playwright/test';

// World context to share state between steps
class TransferWorld {
  page!: Page;
  accountBalances: Map<string, number> = new Map();
  userEmail!: string;
}

Before(async function(this: TransferWorld) {
  // Setup: Launch browser, navigate to app
  const { chromium } = require('@playwright/test');
  const browser = await chromium.launch();
  const context = await browser.newContext();
  this.page = await context.newPage();
});

After(async function(this: TransferWorld) {
  // Cleanup: Close browser
  await this.page.close();
});

Given('I am logged in as {string}', async function(this: TransferWorld, email: string) {
  this.userEmail = email;
  await this.page.goto('https://bankapp.example.com/login');
  await this.page.fill('[data-testid="email"]', email);
  await this.page.fill('[data-testid="password"]', 'Test123!');
  await this.page.click('[data-testid="login-button"]');
  await expect(this.page).toHaveURL(/.*dashboard/);
});

Given('I have a checking account {string} with balance ${int}', 
  async function(this: TransferWorld, accountId: string, balance: number) {
    // Seed test data via API
    await this.page.request.post('/api/test/seed-account', {
      data: {
        accountId: accountId,
        type: 'checking',
        balance: balance,
        userId: this.userEmail
      }
    });
    this.accountBalances.set(accountId, balance);
});

Given('I have a savings account {string} with balance ${int}', 
  async function(this: TransferWorld, accountId: string, balance: number) {
    await this.page.request.post('/api/test/seed-account', {
      data: {
        accountId: accountId,
        type: 'savings',
        balance: balance,
        userId: this.userEmail
      }
    });
    this.accountBalances.set(accountId, balance);
});

Given('my daily transfer limit is ${int}', 
  async function(this: TransferWorld, limit: number) {
    await this.page.request.post('/api/test/set-transfer-limit', {
      data: {
        userId: this.userEmail,
        dailyLimit: limit
      }
    });
});

When('I navigate to the transfer page', async function(this: TransferWorld) {
  await this.page.click('[data-testid="transfer-menu"]');
  await expect(this.page).toHaveURL(/.*transfer/);
});

When('I select {string} as the source account', 
  async function(this: TransferWorld, accountId: string) {
    await this.page.selectOption('[data-testid="from-account"]', accountId);
});

When('I select {string} as the destination account', 
  async function(this: TransferWorld, accountId: string) {
    await this.page.selectOption('[data-testid="to-account"]', accountId);
});

When('I enter transfer amount ${int}', async function(this: TransferWorld, amount: number) {
  await this.page.fill('[data-testid="amount"]', amount.toString());
});

When('I click the {string} button', async function(this: TransferWorld, buttonText: string) {
  await this.page.click(`button:has-text("${buttonText}")`);
});

Then('I should see a success message {string}', 
  async function(this: TransferWorld, message: string) {
    const successMessage = await this.page.locator('[data-testid="success-message"]');
    await expect(successMessage).toContainText(message);
});

Then('I should see an error message {string}', 
  async function(this: TransferWorld, message: string) {
    const errorMessage = await this.page.locator('[data-testid="error-message"]');
    await expect(errorMessage).toContainText(message);
});

Then('the checking account {string} balance should be ${int}', 
  async function(this: TransferWorld, accountId: string, expectedBalance: number) {
    const balanceText = await this.page.locator(`[data-account="${accountId}"] .balance`).textContent();
    const balance = parseFloat(balanceText!.replace('$', '').replace(',', ''));
    expect(balance).toBe(expectedBalance);
});

Then('the checking account {string} balance should remain ${int}', 
  async function(this: TransferWorld, accountId: string, expectedBalance: number) {
    // Same as above - balance should not have changed
    const balanceText = await this.page.locator(`[data-account="${accountId}"] .balance`).textContent();
    const balance = parseFloat(balanceText!.replace('$', '').replace(',', ''));
    expect(balance).toBe(expectedBalance);
});
```

**Run:** `npm run test:bdd` → ✅ PASSES (after implementation)

---

#### Java (Cucumber-JVM + Selenium)

**Feature File:**

```gherkin
# src/test/resources/features/account_management.feature
Feature: Account Management
  As a bank administrator
  I want to manage customer accounts
  So that I can provide excellent customer service

  Background:
    Given I am logged in as an administrator
    And the following accounts exist:
      | Account ID | Customer Name | Type     | Balance | Status |
      | ACC-001    | John Doe      | Checking | 5000.00 | Active |
      | ACC-002    | Jane Smith    | Savings  | 10000.00| Active |
      | ACC-003    | Bob Johnson   | Checking | 250.00  | Frozen |

  Scenario: View account details
    When I search for account "ACC-001"
    Then I should see the account details:
      | Field         | Value    |
      | Customer Name | John Doe |
      | Type          | Checking |
      | Balance       | 5000.00  |
      | Status        | Active   |

  Scenario: Freeze suspicious account
    When I search for account "ACC-001"
    And I click the "Freeze Account" button
    And I enter freeze reason "Suspicious activity detected"
    And I confirm the freeze action
    Then the account status should be "Frozen"
    And I should see a notification "Account ACC-001 has been frozen"
    And an email should be sent to "john.doe@example.com"

  Scenario: Cannot modify frozen account
    When I search for account "ACC-003"
    And I attempt to update the balance to 500.00
    Then I should see an error "Cannot modify frozen account"
    And the balance should remain 250.00
```

**Step Definitions:**

```java
// src/test/java/com/bank/stepdefs/AccountManagementSteps.java
package com.bank.stepdefs;

import io.cucumber.java.en.Given;
import io.cucumber.java.en.When;
import io.cucumber.java.en.Then;
import io.cucumber.datatable.DataTable;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.By;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.openqa.selenium.support.ui.ExpectedConditions;
import static org.junit.jupiter.api.Assertions.*;

import java.util.List;
import java.util.Map;

public class AccountManagementSteps {
    
    private final WebDriver driver;
    private final WebDriverWait wait;
    private final TestContext context;
    
    public AccountManagementSteps(TestContext context) {
        this.context = context;
        this.driver = context.getDriver();
        this.wait = new WebDriverWait(driver, 10);
    }
    
    @Given("I am logged in as an administrator")
    public void iAmLoggedInAsAdministrator() {
        driver.get("https://bankapp.example.com/admin/login");
        driver.findElement(By.id("username")).sendKeys("admin@example.com");
        driver.findElement(By.id("password")).sendKeys("AdminPass123!");
        driver.findElement(By.id("login-button")).click();
        
        wait.until(ExpectedConditions.urlContains("/admin/dashboard"));
    }
    
    @Given("the following accounts exist:")
    public void theFollowingAccountsExist(DataTable dataTable) {
        List<Map<String, String>> accounts = dataTable.asMaps();
        
        for (Map<String, String> account : accounts) {
            // Seed test data via API
            context.getApiClient().post("/api/test/seed-account", account);
        }
    }
    
    @When("I search for account {string}")
    public void iSearchForAccount(String accountId) {
        driver.findElement(By.id("account-search")).sendKeys(accountId);
        driver.findElement(By.id("search-button")).click();
        
        wait.until(ExpectedConditions.visibilityOfElementLocated(
            By.cssSelector("[data-account-id='" + accountId + "']")
        ));
        
        context.setCurrentAccountId(accountId);
    }
    
    @Then("I should see the account details:")
    public void iShouldSeeTheAccountDetails(DataTable dataTable) {
        Map<String, String> expectedDetails = dataTable.asMap(String.class, String.class);
        
        for (Map.Entry<String, String> entry : expectedDetails.entrySet()) {
            String field = entry.getKey();
            String expectedValue = entry.getValue();
            
            WebElement element = driver.findElement(
                By.cssSelector("[data-field='" + field.toLowerCase().replace(" ", "-") + "']")
            );
            String actualValue = element.getText();
            
            assertEquals(expectedValue, actualValue, 
                "Field '" + field + "' does not match");
        }
    }
    
    @When("I click the {string} button")
    public void iClickTheButton(String buttonText) {
        WebElement button = driver.findElement(
            By.xpath("//button[contains(text(), '" + buttonText + "')]")
        );
        button.click();
    }
    
    @When("I enter freeze reason {string}")
    public void iEnterFreezeReason(String reason) {
        wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("freeze-reason")));
        driver.findElement(By.id("freeze-reason")).sendKeys(reason);
    }
    
    @When("I confirm the freeze action")
    public void iConfirmTheFreezeAction() {
        driver.findElement(By.id("confirm-freeze")).click();
        wait.until(ExpectedConditions.invisibilityOfElementLocated(By.id("freeze-dialog")));
    }
    
    @Then("the account status should be {string}")
    public void theAccountStatusShouldBe(String expectedStatus) {
        WebElement statusElement = driver.findElement(By.cssSelector("[data-field='status']"));
        String actualStatus = statusElement.getText();
        assertEquals(expectedStatus, actualStatus);
    }
    
    @Then("I should see a notification {string}")
    public void iShouldSeeANotification(String expectedMessage) {
        WebElement notification = wait.until(
            ExpectedConditions.visibilityOfElementLocated(By.className("notification"))
        );
        String actualMessage = notification.getText();
        assertTrue(actualMessage.contains(expectedMessage), 
            "Expected notification to contain: " + expectedMessage);
    }
    
    @When("I attempt to update the balance to {double}")
    public void iAttemptToUpdateTheBalanceTo(double newBalance) {
        driver.findElement(By.id("edit-balance")).click();
        driver.findElement(By.id("new-balance")).clear();
        driver.findElement(By.id("new-balance")).sendKeys(String.valueOf(newBalance));
        driver.findElement(By.id("save-balance")).click();
    }
    
    @Then("I should see an error {string}")
    public void iShouldSeeAnError(String expectedError) {
        WebElement errorElement = wait.until(
            ExpectedConditions.visibilityOfElementLocated(By.className("error-message"))
        );
        String actualError = errorElement.getText();
        assertTrue(actualError.contains(expectedError));
    }
    
    @Then("the balance should remain {double}")
    public void theBalanceShouldRemain(double expectedBalance) {
        WebElement balanceElement = driver.findElement(By.cssSelector("[data-field='balance']"));
        String balanceText = balanceElement.getText().replace("$", "").replace(",", "");
        double actualBalance = Double.parseDouble(balanceText);
        assertEquals(expectedBalance, actualBalance, 0.01);
    }
}
```

**Run:** `mvn test -Dcucumber.filter.tags="@account"` → ✅ PASSES

---

#### .NET (SpecFlow + Selenium)

**Feature File:**

```gherkin
# Features/LoanApplication.feature
Feature: Loan Application Process
  As a bank customer
  I want to apply for a personal loan
  So that I can finance my needs

  Background:
    Given I am on the loan application page
    And I am logged in as "customer@example.com"

  Scenario: Submit valid loan application
    When I fill in the loan application form:
      | Field           | Value   |
      | Loan Amount     | 50000   |
      | Loan Purpose    | Home Improvement |
      | Annual Income   | 120000  |
      | Employment Status | Employed Full-Time |
      | Credit Score    | 750     |
    And I agree to the terms and conditions
    And I submit the loan application
    Then I should see a success message "Your application has been submitted"
    And I should receive an application reference number
    And the application status should be "Under Review"

  Scenario: Loan amount exceeds maximum limit
    When I fill in the loan application form:
      | Field           | Value   |
      | Loan Amount     | 150000  |
      | Loan Purpose    | Home Improvement |
      | Annual Income   | 120000  |
    And I submit the loan application
    Then I should see an error "Loan amount exceeds maximum limit of $100,000"
    And the application should not be submitted

  Scenario Outline: Loan application validation
    When I fill in the loan application form:
      | Field           | Value          |
      | Loan Amount     | <loan_amount>  |
      | Annual Income   | <annual_income>|
      | Credit Score    | <credit_score> |
    And I submit the loan application
    Then I should see <result_type> message "<message>"

    Examples:
      | loan_amount | annual_income | credit_score | result_type | message                              |
      | 50000       | 120000        | 750          | success     | Your application has been submitted  |
      | 50000       | 30000         | 750          | error       | Income insufficient for requested amount |
      | 50000       | 120000        | 550          | warning     | Application may require manual review |
      | 0           | 120000        | 750          | error       | Loan amount must be greater than $1,000 |
```

**Step Definitions:**

```csharp
// StepDefinitions/LoanApplicationSteps.cs
using TechTalk.SpecFlow;
using TechTalk.SpecFlow.Assist;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using OpenQA.Selenium.Support.UI;
using NUnit.Framework;
using System;
using System.Collections.Generic;

namespace BankApp.Tests.StepDefinitions
{
    [Binding]
    public class LoanApplicationSteps
    {
        private readonly IWebDriver _driver;
        private readonly WebDriverWait _wait;
        private readonly ScenarioContext _scenarioContext;
        
        public LoanApplicationSteps(ScenarioContext scenarioContext)
        {
            _scenarioContext = scenarioContext;
            _driver = new ChromeDriver();
            _wait = new WebDriverWait(_driver, TimeSpan.FromSeconds(10));
        }
        
        [Given(@"I am on the loan application page")]
        public void GivenIAmOnTheLoanApplicationPage()
        {
            _driver.Navigate().GoToUrl("https://bankapp.example.com/loans/apply");
        }
        
        [Given(@"I am logged in as ""(.*)""")]
        public void GivenIAmLoggedInAs(string email)
        {
            _driver.Navigate().GoToUrl("https://bankapp.example.com/login");
            _driver.FindElement(By.Id("email")).SendKeys(email);
            _driver.FindElement(By.Id("password")).SendKeys("Test123!");
            _driver.FindElement(By.Id("login-button")).Click();
            
            _wait.Until(d => d.Url.Contains("/dashboard"));
            _driver.Navigate().GoToUrl("https://bankapp.example.com/loans/apply");
        }
        
        [When(@"I fill in the loan application form:")]
        public void WhenIFillInTheLoanApplicationForm(Table table)
        {
            var formData = table.Rows.ToDictionary();
            
            foreach (var field in formData)
            {
                string fieldName = field.Key.Replace(" ", "-").ToLower();
                IWebElement element = _driver.FindElement(By.CssSelector($"[name='{fieldName}']"));
                
                if (element.TagName == "select")
                {
                    new SelectElement(element).SelectByText(field.Value);
                }
                else
                {
                    element.Clear();
                    element.SendKeys(field.Value);
                }
            }
            
            _scenarioContext["FormData"] = formData;
        }
        
        [When(@"I agree to the terms and conditions")]
        public void WhenIAgreeToTheTermsAndConditions()
        {
            IWebElement checkbox = _driver.FindElement(By.Id("terms-checkbox"));
            if (!checkbox.Selected)
            {
                checkbox.Click();
            }
        }
        
        [When(@"I submit the loan application")]
        public void WhenISubmitTheLoanApplication()
        {
            _driver.FindElement(By.Id("submit-application")).Click();
            System.Threading.Thread.Sleep(1000); // Wait for submission
        }
        
        [Then(@"I should see a success message ""(.*)""")]
        public void ThenIShouldSeeASuccessMessage(string expectedMessage)
        {
            IWebElement successElement = _wait.Until(d => 
                d.FindElement(By.CssSelector(".success-message")));
            
            string actualMessage = successElement.Text;
            Assert.That(actualMessage, Does.Contain(expectedMessage));
        }
        
        [Then(@"I should receive an application reference number")]
        public void ThenIShouldReceiveAnApplicationReferenceNumber()
        {
            IWebElement refElement = _driver.FindElement(By.CssSelector("[data-ref-number]"));
            string refNumber = refElement.GetAttribute("data-ref-number");
            
            Assert.That(refNumber, Is.Not.Null.And.Not.Empty);
            Assert.That(refNumber, Does.Match(@"^LOAN-\d{10}$"));
            
            _scenarioContext["ReferenceNumber"] = refNumber;
        }
        
        [Then(@"the application status should be ""(.*)""")]
        public void ThenTheApplicationStatusShouldBe(string expectedStatus)
        {
            IWebElement statusElement = _driver.FindElement(By.CssSelector("[data-status]"));
            string actualStatus = statusElement.Text;
            
            Assert.That(actualStatus, Is.EqualTo(expectedStatus));
        }
        
        [Then(@"I should see an error ""(.*)""")]
        public void ThenIShouldSeeAnError(string expectedError)
        {
            IWebElement errorElement = _wait.Until(d => 
                d.FindElement(By.CssSelector(".error-message")));
            
            string actualError = errorElement.Text;
            Assert.That(actualError, Does.Contain(expectedError));
        }
        
        [Then(@"the application should not be submitted")]
        public void ThenTheApplicationShouldNotBeSubmitted()
        {
            // Verify we're still on the application page (not redirected)
            Assert.That(_driver.Url, Does.Contain("/loans/apply"));
            
            // Verify no success message is displayed
            var successElements = _driver.FindElements(By.CssSelector(".success-message"));
            Assert.That(successElements, Is.Empty);
        }
        
        [AfterScenario]
        public void Cleanup()
        {
            _driver?.Quit();
            _driver?.Dispose();
        }
    }
}
```

**Run:** `dotnet test --filter "Category=LoanApplication"` → ✅ PASSES

---

#### Python (Behave + Selenium)

**Feature File:**

```gherkin
# features/transaction_history.feature
Feature: Transaction History
  As a banking customer
  I want to view my transaction history
  So that I can track my spending and income

  Background:
    Given I am logged in as "user@example.com"
    And I have the following transactions:
      | Date       | Description      | Amount  | Type   | Category    |
      | 2025-01-15 | Grocery Store    | -125.50 | Debit  | Groceries   |
      | 2025-01-14 | Salary Deposit   | 5000.00 | Credit | Income      |
      | 2025-01-13 | Electric Bill    | -89.00  | Debit  | Utilities   |
      | 2025-01-12 | Coffee Shop      | -5.75   | Debit  | Dining      |
      | 2025-01-10 | ATM Withdrawal   | -200.00 | Debit  | Cash        |

  Scenario: View all transactions
    When I navigate to the transaction history page
    Then I should see 5 transactions
    And the transactions should be sorted by date descending

  Scenario: Filter transactions by date range
    When I navigate to the transaction history page
    And I filter transactions from "2025-01-13" to "2025-01-15"
    Then I should see 3 transactions
    And all transactions should be within the date range

  Scenario: Filter transactions by category
    When I navigate to the transaction history page
    And I filter transactions by category "Debit"
    Then I should see 4 transactions
    And all transactions should be of type "Debit"

  Scenario: Search transactions by description
    When I navigate to the transaction history page
    And I search for transactions containing "Coffee"
    Then I should see 1 transaction
    And the transaction description should contain "Coffee Shop"

  Scenario: Export transactions to CSV
    When I navigate to the transaction history page
    And I click the "Export to CSV" button
    Then a CSV file should be downloaded
    And the CSV should contain 5 transactions
    And the CSV should have columns "Date,Description,Amount,Type,Category"
```

**Step Definitions:**

```python
# features/steps/transaction_history_steps.py
from behave import given, when, then
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.options import Options
import time
import csv
import os

@given('I am logged in as "{email}"')
def step_impl(context, email):
    options = Options()
    options.add_argument('--headless')
    context.driver = webdriver.Chrome(options=options)
    context.wait = WebDriverWait(context.driver, 10)
    
    context.driver.get('https://bankapp.example.com/login')
    context.driver.find_element(By.ID, 'email').send_keys(email)
    context.driver.find_element(By.ID, 'password').send_keys('Test123!')
    context.driver.find_element(By.ID, 'login-button').click()
    
    context.wait.until(EC.url_contains('/dashboard'))

@given('I have the following transactions')
def step_impl(context):
    """Seed test data via API"""
    transactions = []
    for row in context.table:
        transactions.append({
            'date': row['Date'],
            'description': row['Description'],
            'amount': float(row['Amount']),
            'type': row['Type'],
            'category': row['Category']
        })
    
    # Call API to seed transactions
    import requests
    response = requests.post(
        'https://bankapp.example.com/api/test/seed-transactions',
        json={'transactions': transactions},
        cookies=context.driver.get_cookies()
    )
    assert response.status_code == 200
    context.seeded_transactions = transactions

@when('I navigate to the transaction history page')
def step_impl(context):
    context.driver.get('https://bankapp.example.com/transactions')
    context.wait.until(EC.presence_of_element_located((By.CLASS_NAME, 'transaction-list')))

@then('I should see {count:d} transactions')
def step_impl(context, count):
    transactions = context.driver.find_elements(By.CLASS_NAME, 'transaction-item')
    assert len(transactions) == count, f'Expected {count} transactions, found {len(transactions)}'

@then('the transactions should be sorted by date descending')
def step_impl(context):
    transactions = context.driver.find_elements(By.CLASS_NAME, 'transaction-item')
    dates = [t.find_element(By.CLASS_NAME, 'transaction-date').text for t in transactions]
    
    # Convert to comparable format and check descending order
    from datetime import datetime
    parsed_dates = [datetime.strptime(d, '%Y-%m-%d') for d in dates]
    assert parsed_dates == sorted(parsed_dates, reverse=True), 'Transactions not sorted by date descending'

@when('I filter transactions from "{start_date}" to "{end_date}"')
def step_impl(context, start_date, end_date):
    context.driver.find_element(By.ID, 'start-date').send_keys(start_date)
    context.driver.find_element(By.ID, 'end-date').send_keys(end_date)
    context.driver.find_element(By.ID, 'apply-filter').click()
    time.sleep(1)  # Wait for filter to apply

@then('all transactions should be within the date range')
def step_impl(context):
    transactions = context.driver.find_elements(By.CLASS_NAME, 'transaction-item')
    dates = [t.find_element(By.CLASS_NAME, 'transaction-date').text for t in transactions]
    
    from datetime import datetime
    start_date = datetime.strptime(
        context.driver.find_element(By.ID, 'start-date').get_attribute('value'),
        '%Y-%m-%d'
    )
    end_date = datetime.strptime(
        context.driver.find_element(By.ID, 'end-date').get_attribute('value'),
        '%Y-%m-%d'
    )
    
    for date_str in dates:
        transaction_date = datetime.strptime(date_str, '%Y-%m-%d')
        assert start_date <= transaction_date <= end_date, \
            f'Transaction date {date_str} not within range {start_date} to {end_date}'

@when('I filter transactions by category "{category}"')
def step_impl(context, category):
    context.driver.find_element(By.ID, 'category-filter').click()
    context.driver.find_element(By.XPATH, f'//option[text()="{category}"]').click()
    context.driver.find_element(By.ID, 'apply-filter').click()
    time.sleep(1)

@then('all transactions should be of type "{transaction_type}"')
def step_impl(context, transaction_type):
    transactions = context.driver.find_elements(By.CLASS_NAME, 'transaction-item')
    for transaction in transactions:
        actual_type = transaction.find_element(By.CLASS_NAME, 'transaction-type').text
        assert actual_type == transaction_type, \
            f'Expected type {transaction_type}, found {actual_type}'

@when('I search for transactions containing "{search_term}"')
def step_impl(context, search_term):
    search_box = context.driver.find_element(By.ID, 'transaction-search')
    search_box.clear()
    search_box.send_keys(search_term)
    context.driver.find_element(By.ID, 'search-button').click()
    time.sleep(1)

@then('the transaction description should contain "{expected_text}"')
def step_impl(context, expected_text):
    transaction = context.driver.find_element(By.CLASS_NAME, 'transaction-item')
    description = transaction.find_element(By.CLASS_NAME, 'transaction-description').text
    assert expected_text in description, \
        f'Expected description to contain "{expected_text}", found "{description}"'

@when('I click the "{button_text}" button')
def step_impl(context, button_text):
    button = context.driver.find_element(By.XPATH, f'//button[contains(text(), "{button_text}")]')
    button.click()
    time.sleep(2)  # Wait for download

@then('a CSV file should be downloaded')
def step_impl(context):
    # Check download directory for CSV file
    download_dir = os.path.expanduser('~/Downloads')
    csv_files = [f for f in os.listdir(download_dir) if f.endswith('.csv')]
    assert len(csv_files) > 0, 'No CSV file found in downloads'
    context.downloaded_csv = os.path.join(download_dir, csv_files[0])

@then('the CSV should contain {count:d} transactions')
def step_impl(context, count):
    with open(context.downloaded_csv, 'r') as f:
        reader = csv.reader(f)
        rows = list(reader)
        # Subtract 1 for header row
        assert len(rows) - 1 == count, f'Expected {count} transactions, found {len(rows) - 1}'

@then('the CSV should have columns "{expected_columns}"')
def step_impl(context, expected_columns):
    with open(context.downloaded_csv, 'r') as f:
        reader = csv.reader(f)
        headers = next(reader)
        assert ','.join(headers) == expected_columns, \
            f'Expected columns "{expected_columns}", found "{",".join(headers)}"'

def after_scenario(context, scenario):
    """Cleanup after each scenario"""
    if hasattr(context, 'driver'):
        context.driver.quit()
    if hasattr(context, 'downloaded_csv') and os.path.exists(context.downloaded_csv):
        os.remove(context.downloaded_csv)
```

**Run:** `behave` → ✅ PASSES

---

### Key BDD Principles Across All Languages

1. **Gherkin as Ubiquitous Language**: All stakeholders understand the same scenarios
2. **Living Documentation**: Features serve as executable specifications
3. **Outside-In Development**: Start with behavior, then implement
4. **Data Tables**: Use tables for complex test data
5. **Scenario Outlines**: Parameterize scenarios for multiple test cases
6. **Background**: Avoid duplication with common setup steps
7. **Tags**: Organize and filter scenarios by category
8. **Separation of Concerns**: Feature files separate from step definitions

---

## 5. Browser MCP: Selenium & Playwright (UI Testing)

### The Testing MCP Revolution

Browser MCP servers enable **natural language testing**—QA engineers and even product managers can issue test commands without writing code.

### Selenium MCP

**Traditional Selenium:**

```java
// Complex, brittle code
WebDriver driver = new ChromeDriver();
driver.get("https://example.com/login");
WebElement emailField = driver.findElement(By.id("email"));
emailField.sendKeys("user@example.com");
WebElement passwordField = driver.findElement(By.id("password"));
passwordField.sendKeys("password123");
WebElement loginButton = driver.findElement(By.xpath("//button[@type='submit']"));
loginButton.click();
WebDriverWait wait = new WebDriverWait(driver, 10);
wait.until(ExpectedConditions.urlContains("/dashboard"));
```

**Agentic Approach with Selenium MCP:**

```text
QA Engineer or Product Manager (in natural language):

"Navigate to the login page, enter 'user@example.com' in the email field, 
enter 'password123' in the password field, and click the login button"

↓ Selenium MCP Server translates ↓

Executable Selenium commands → Test execution
```

**Key Innovation:**
Instead of writing complex test scripts, a QA engineer or product manager can issue **natural language commands**. The Selenium MCP server translates these commands into executable Selenium actions.

### Playwright MCP: Modern Alternative

**Advantages over Selenium:**

- ✅ Built-in auto-waiting (no explicit waits needed)
- ✅ Better handling of modern web apps (SPAs, PWAs)
- ✅ Network interception and mocking
- ✅ Multiple browser contexts
- ✅ Video recording and trace files
- ✅ Better performance
- ✅ Parallel execution
- ✅ Auto-retry on flaky tests

**Example with Playwright MCP:**

```typescript
// Natural language → Playwright MCP → Executable test
Agent: "Test the login flow with network offline simulation"

Generated Playwright code:
test('Login with offline network', async ({ page, context }) => {
  // Simulate offline
  await context.setOffline(true);
  
  await page.goto('/login');
  
  // Expect offline message
  await expect(page.locator('.offline-message')).toBeVisible();
  
  // Restore network
  await context.setOffline(false);
  
  // Login should now work
  await page.fill('[data-testid="email"]', 'user@example.com');
  await page.fill('[data-testid="password"]', 'password123');
  await page.click('[data-testid="sign-in-button"]');
  
  await expect(page).toHaveURL('/dashboard');
});
```

### Enabled QA Workflows

#### 1. AI-Powered Test Execution

```text
Agent Prompt:
"Test the checkout flow:
1. Click the 'Add to Cart' button on the first product
2. Click the cart icon in the header
3. Click 'Proceed to Checkout'
4. Fill out the shipping form with test data
5. Select 'Credit Card' as payment method
6. Verify the order summary shows correct totals
7. Click 'Place Order'"

Agent executes via Selenium/Playwright MCP → Full test run with screenshots
```

#### 2. Faster Debugging & Root Cause Analysis

```text
Test Failure:
"Login test failed at step 3: Click login button"

Agent with Browser MCP access:
- Retrieves browser console logs
- Captures network requests
- Takes screenshot at failure point
- Analyzes DOM state
- Provides RCA: "Login button was disabled due to email validation error"
```

#### 3. AI-Assisted Test Case Generation

```text
Agent Prompt:
"Analyze the last 10 login test executions and generate 5 new edge case 
test scenarios that weren't covered"

Agent Output (via Browser MCP analysis):
1. Test login with email containing special characters (+, .)
2. Test login with maximum password length (128 characters)
3. Test login during session timeout
4. Test login with caps lock warning
5. Test login with autofill credentials
```

---

## 6. Postman MCP: API Testing Automation

### The API Testing Revolution with Postman MCP

Postman MCP brings natural language API testing to the agentic workflow, enabling QA engineers and developers to create, execute, and automate API tests without manual collection management.

### Key Capabilities

- **Natural Language Test Creation**: Describe API tests in plain English
- **Collection Management**: Auto-generate and organize test collections
- **Environment Management**: Dynamic environment switching (dev, staging, prod)
- **Test Automation**: Integrate with CI/CD pipelines
- **Pre-request Scripts**: Auto-generate setup logic
- **Test Assertions**: Intelligent validation of responses
- **Mock Servers**: Create mock APIs for parallel development

### Agentic API Testing Workflow

```text
┌─────────────────────────────────────────────────────────────┐
│  Agentic API Testing with Postman MCP                       │
└─────────────────────────────────────────────────────────────┘

Step 1: Describe API Test Intent
Developer/QA: "Test the fund transfer API with valid and invalid scenarios"

Step 2: Agent Generates Postman Collection
AI Agent → Postman MCP → Creates collection with requests and tests

Step 3: Execute Tests
Agent runs collection against environment (dev/staging/prod)

Step 4: Validate Results
Agent analyzes responses, generates report

Step 5: Continuous Integration
Tests run automatically on every commit
```

### Example: Banking API Testing

#### JavaScript/TypeScript (Postman MCP + Newman)

**Natural Language Request:**

```text
QA Engineer: "Create a Postman collection to test the Banking API:
1. User authentication (POST /api/auth/login)
2. Get account details (GET /api/accounts/{id})
3. Transfer funds (POST /api/transfers)
4. Get transaction history (GET /api/transactions)

For each endpoint, include:
- Success scenarios
- Validation error scenarios
- Authentication error scenarios
- Use environment variables for base URL and auth tokens"
```

**Agent-Generated Postman Collection:**

```json
{
  "info": {
    "name": "Banking API Test Suite",
    "description": "Comprehensive tests for Banking API endpoints",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "variable": [
    {
      "key": "baseUrl",
      "value": "https://api.bank.example.com",
      "type": "string"
    },
    {
      "key": "authToken",
      "value": "",
      "type": "string"
    },
    {
      "key": "accountId",
      "value": "",
      "type": "string"
    }
  ],
  "item": [
    {
      "name": "Authentication",
      "item": [
        {
          "name": "Login - Success",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test('Status code is 200', function () {",
                  "    pm.response.to.have.status(200);",
                  "});",
                  "",
                  "pm.test('Response contains auth token', function () {",
                  "    const jsonData = pm.response.json();",
                  "    pm.expect(jsonData).to.have.property('token');",
                  "    pm.expect(jsonData.token).to.be.a('string');",
                  "    pm.collectionVariables.set('authToken', jsonData.token);",
                  "});",
                  "",
                  "pm.test('Response time is acceptable', function () {",
                  "    pm.expect(pm.response.responseTime).to.be.below(2000);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"email\": \"user@example.com\",\n  \"password\": \"SecurePass123!\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/auth/login",
              "host": ["{{baseUrl}}"],
              "path": ["api", "auth", "login"]
            }
          }
        },
        {
          "name": "Login - Invalid Credentials",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test('Status code is 401', function () {",
                  "    pm.response.to.have.status(401);",
                  "});",
                  "",
                  "pm.test('Error message is descriptive', function () {",
                  "    const jsonData = pm.response.json();",
                  "    pm.expect(jsonData).to.have.property('error');",
                  "    pm.expect(jsonData.error).to.include('Invalid credentials');",
                  "});",
                  "",
                  "pm.test('No token in response', function () {",
                  "    const jsonData = pm.response.json();",
                  "    pm.expect(jsonData).to.not.have.property('token');",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"email\": \"user@example.com\",\n  \"password\": \"WrongPassword\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/auth/login",
              "host": ["{{baseUrl}}"],
              "path": ["api", "auth", "login"]
            }
          }
        }
      ]
    },
    {
      "name": "Account Operations",
      "item": [
        {
          "name": "Get Account Details - Success",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test('Status code is 200', function () {",
                  "    pm.response.to.have.status(200);",
                  "});",
                  "",
                  "pm.test('Account structure is valid', function () {",
                  "    const jsonData = pm.response.json();",
                  "    pm.expect(jsonData).to.have.property('accountId');",
                  "    pm.expect(jsonData).to.have.property('balance');",
                  "    pm.expect(jsonData).to.have.property('type');",
                  "    pm.expect(jsonData).to.have.property('status');",
                  "});",
                  "",
                  "pm.test('Balance is a number', function () {",
                  "    const jsonData = pm.response.json();",
                  "    pm.expect(jsonData.balance).to.be.a('number');",
                  "    pm.expect(jsonData.balance).to.be.at.least(0);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "GET",
            "header": [
              {
                "key": "Authorization",
                "value": "Bearer {{authToken}}"
              }
            ],
            "url": {
              "raw": "{{baseUrl}}/api/accounts/{{accountId}}",
              "host": ["{{baseUrl}}"],
              "path": ["api", "accounts", "{{accountId}}"]
            }
          }
        },
        {
          "name": "Get Account - Unauthorized",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test('Status code is 401', function () {",
                  "    pm.response.to.have.status(401);",
                  "});",
                  "",
                  "pm.test('Error indicates missing auth', function () {",
                  "    const jsonData = pm.response.json();",
                  "    pm.expect(jsonData.error).to.include('Unauthorized');",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "GET",
            "header": [],
            "url": {
              "raw": "{{baseUrl}}/api/accounts/{{accountId}}",
              "host": ["{{baseUrl}}"],
              "path": ["api", "accounts", "{{accountId}}"]
            }
          }
        }
      ]
    },
    {
      "name": "Fund Transfer",
      "item": [
        {
          "name": "Transfer Funds - Success",
          "event": [
            {
              "listen": "prerequest",
              "script": {
                "exec": [
                  "// Pre-request: Verify account has sufficient balance",
                  "pm.collectionVariables.set('transferAmount', 500);",
                  "pm.collectionVariables.set('fromAccount', 'CHK-001');",
                  "pm.collectionVariables.set('toAccount', 'SAV-001');"
                ],
                "type": "text/javascript"
              }
            },
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test('Status code is 200', function () {",
                  "    pm.response.to.have.status(200);",
                  "});",
                  "",
                  "pm.test('Transfer completed successfully', function () {",
                  "    const jsonData = pm.response.json();",
                  "    pm.expect(jsonData).to.have.property('transactionId');",
                  "    pm.expect(jsonData.status).to.equal('COMPLETED');",
                  "});",
                  "",
                  "pm.test('Transaction ID is valid format', function () {",
                  "    const jsonData = pm.response.json();",
                  "    pm.expect(jsonData.transactionId).to.match(/^TXN-[A-Z0-9]{10}$/);",
                  "    pm.collectionVariables.set('lastTransactionId', jsonData.transactionId);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Authorization",
                "value": "Bearer {{authToken}}"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"fromAccount\": \"{{fromAccount}}\",\n  \"toAccount\": \"{{toAccount}}\",\n  \"amount\": {{transferAmount}},\n  \"description\": \"Test transfer\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/transfers",
              "host": ["{{baseUrl}}"],
              "path": ["api", "transfers"]
            }
          }
        },
        {
          "name": "Transfer - Insufficient Funds",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test('Status code is 400', function () {",
                  "    pm.response.to.have.status(400);",
                  "});",
                  "",
                  "pm.test('Error indicates insufficient funds', function () {",
                  "    const jsonData = pm.response.json();",
                  "    pm.expect(jsonData.error).to.include('Insufficient funds');",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Authorization",
                "value": "Bearer {{authToken}}"
              },
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"fromAccount\": \"CHK-001\",\n  \"toAccount\": \"SAV-001\",\n  \"amount\": 999999,\n  \"description\": \"Test insufficient funds\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/transfers",
              "host": ["{{baseUrl}}"],
              "path": ["api", "transfers"]
            }
          }
        }
      ]
    }
  ]
}
```

**Run with Newman (CLI):**

```bash
# Install Newman
npm install -g newman

# Run collection against dev environment
newman run banking-api-collection.json \
  --environment dev-environment.json \
  --reporters cli,html \
  --reporter-html-export report.html

# Run with specific iteration data
newman run banking-api-collection.json \
  --environment dev-environment.json \
  --iteration-data test-data.csv \
  --reporters cli,json,html
```

**CI/CD Integration (GitHub Actions):**

```yaml
# .github/workflows/api-tests.yml
name: API Tests with Postman

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  api-tests:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Install Newman
        run: npm install -g newman newman-reporter-htmlextra
      
      - name: Run API Tests (Dev)
        run: |
          newman run postman/banking-api-collection.json \
            --environment postman/dev-environment.json \
            --reporters cli,htmlextra \
            --reporter-htmlextra-export reports/api-test-report.html
      
      - name: Upload Test Report
        uses: actions/upload-artifact@v3
        if: always()
        with:
          name: api-test-report
          path: reports/api-test-report.html
      
      - name: Fail Build if Tests Failed
        if: failure()
        run: exit 1
```

---

### Python (Postman MCP + Requests)

**Agent-Generated Test Script:**

```python
# tests/api/test_banking_api.py
import pytest
import requests
import os
from datetime import datetime

class TestBankingAPI:
    
    @pytest.fixture(scope='class')
    def base_url(self):
        return os.getenv('API_BASE_URL', 'https://api.bank.example.com')
    
    @pytest.fixture(scope='class')
    def auth_token(self, base_url):
        """Authenticate and return token"""
        response = requests.post(
            f'{base_url}/api/auth/login',
            json={
                'email': 'user@example.com',
                'password': 'SecurePass123!'
            }
        )
        assert response.status_code == 200
        return response.json()['token']
    
    def test_login_success(self, base_url):
        """Test successful login"""
        response = requests.post(
            f'{base_url}/api/auth/login',
            json={
                'email': 'user@example.com',
                'password': 'SecurePass123!'
            }
        )
        
        assert response.status_code == 200
        assert 'token' in response.json()
        assert isinstance(response.json()['token'], str)
        assert response.elapsed.total_seconds() < 2
    
    def test_login_invalid_credentials(self, base_url):
        """Test login with invalid credentials"""
        response = requests.post(
            f'{base_url}/api/auth/login',
            json={
                'email': 'user@example.com',
                'password': 'WrongPassword'
            }
        )
        
        assert response.status_code == 401
        assert 'error' in response.json()
        assert 'Invalid credentials' in response.json()['error']
        assert 'token' not in response.json()
    
    def test_get_account_details_success(self, base_url, auth_token):
        """Test retrieving account details"""
        account_id = 'ACC-001'
        response = requests.get(
            f'{base_url}/api/accounts/{account_id}',
            headers={'Authorization': f'Bearer {auth_token}'}
        )
        
        assert response.status_code == 200
        data = response.json()
        assert 'accountId' in data
        assert 'balance' in data
        assert 'type' in data
        assert 'status' in data
        assert isinstance(data['balance'], (int, float))
        assert data['balance'] >= 0
    
    def test_get_account_unauthorized(self, base_url):
        """Test account access without authentication"""
        account_id = 'ACC-001'
        response = requests.get(
            f'{base_url}/api/accounts/{account_id}'
        )
        
        assert response.status_code == 401
        assert 'error' in response.json()
        assert 'Unauthorized' in response.json()['error']
    
    def test_transfer_funds_success(self, base_url, auth_token):
        """Test successful fund transfer"""
        response = requests.post(
            f'{base_url}/api/transfers',
            headers={
                'Authorization': f'Bearer {auth_token}',
                'Content-Type': 'application/json'
            },
            json={
                'fromAccount': 'CHK-001',
                'toAccount': 'SAV-001',
                'amount': 500,
                'description': 'Test transfer'
            }
        )
        
        assert response.status_code == 200
        data = response.json()
        assert 'transactionId' in data
        assert data['status'] == 'COMPLETED'
        assert data['transactionId'].startswith('TXN-')
    
    def test_transfer_insufficient_funds(self, base_url, auth_token):
        """Test transfer with insufficient funds"""
        response = requests.post(
            f'{base_url}/api/transfers',
            headers={
                'Authorization': f'Bearer {auth_token}',
                'Content-Type': 'application/json'
            },
            json={
                'fromAccount': 'CHK-001',
                'toAccount': 'SAV-001',
                'amount': 999999,
                'description': 'Test insufficient funds'
            }
        )
        
        assert response.status_code == 400
        assert 'error' in response.json()
        assert 'Insufficient funds' in response.json()['error']
    
    @pytest.mark.parametrize('amount,expected_status', [
        (100, 200),
        (500, 200),
        (0, 400),
        (-100, 400),
    ])
    def test_transfer_various_amounts(self, base_url, auth_token, amount, expected_status):
        """Test transfers with various amounts"""
        response = requests.post(
            f'{base_url}/api/transfers',
            headers={
                'Authorization': f'Bearer {auth_token}',
                'Content-Type': 'application/json'
            },
            json={
                'fromAccount': 'CHK-001',
                'toAccount': 'SAV-001',
                'amount': amount,
                'description': f'Test transfer {amount}'
            }
        )
        
        assert response.status_code == expected_status
```

**Run:** `pytest tests/api/ -v --html=report.html` → ✅ PASSES

---

### Key Postman MCP Benefits

1. **Natural Language Test Creation**: Describe tests in plain English
2. **Auto-Generated Collections**: AI agent creates comprehensive test suites
3. **Environment Management**: Seamless switching between dev/staging/prod
4. **CI/CD Integration**: Automated API testing on every commit
5. **Mock Server Creation**: Parallel development with mock APIs
6. **Contract Testing**: Validate API contracts between services
7. **Performance Monitoring**: Track API response times and throughput
8. **Documentation**: Collections serve as living API documentation

---

## 7. Design-to-Test Automation with Figma MCP

### Figma as the Origin Point of Testable UI

The Figma MCP server's **true architectural significance** is not just in code generation, but in its role as the **origin point for automated, design-first testing**.

**Key Insight:**
The server provides **programmatic access** to all:

- UI elements
- Interactions
- Flow information
- Layout data

This structured data is **precisely what is needed to generate test cases**.

### Revolutionary Workflow

```text
BDD scenarios are generated from the design
BEFORE implementation code has been written
```

### Step-by-Step: From Figma to Automated Tests

#### Step 1: Design Contains Flow Information

#### Example: User Login Flow in Figma

```text
Frame: "Login Screen"
  ├─ Input: "Email" (required, type=email)
  ├─ Input: "Password" (required, type=password)
  ├─ Checkbox: "Remember me"
  ├─ Button: "Sign In" (onClick → "Dashboard Screen")
  └─ Link: "Forgot password?" (onClick → "Password Reset Screen")

Frame: "Dashboard Screen"
  ├─ Header: "Welcome, {user.name}"
  └─ ...
```

#### Step 2: Agent Extracts Flow via MCP

```python
# Pseudo-code for test generation agent
agent_prompt = """
Extract all user flows from the Figma file and generate BDD test scenarios.
For each interactive element, identify:
- User action (click, type, select)
- Expected outcome (navigation, state change, validation)
- Error states
"""

figma_data = mcp_client.query(
    tool="extract_design_context",
    file_url="https://figma.com/file/banking-app"
)

flows = extract_flows(figma_data)
```

#### Step 3: Generate Gherkin Test Scenarios

**Auto-Generated BDD Test:**

```gherkin
Feature: User Authentication
  As a banking customer
  I want to securely log in to my account
  So that I can access my financial information

  Background:
    Given I am on the login page

  Scenario: Successful login with valid credentials
    When I enter "user@example.com" in the "Email" field
    And I enter "SecurePass123!" in the "Password" field
    And I click the "Sign In" button
    Then I should be redirected to the "Dashboard" page
    And I should see "Welcome, John Doe"

  Scenario: Failed login with invalid email format
    When I enter "invalid-email" in the "Email" field
    And I click the "Sign In" button
    Then I should see an error message "Please enter a valid email address"
    And I should remain on the login page

  Scenario: Failed login with wrong password
    When I enter "user@example.com" in the "Email" field
    And I enter "WrongPassword" in the "Password" field
    And I click the "Sign In" button
    Then I should see an error message "Invalid email or password"
    And the "Password" field should be cleared
    And I should remain on the login page

  Scenario: Password reset flow
    When I click the "Forgot password?" link
    Then I should be redirected to the "Password Reset" page

  Scenario: Remember me functionality
    When I enter valid credentials
    And I check the "Remember me" checkbox
    And I click the "Sign In" button
    And I log out
    And I return to the login page
    Then the "Email" field should be pre-filled with "user@example.com"
```

#### Step 4: Generate Executable Test Code

```typescript
// Auto-generated from BDD scenarios using Playwright
import { test, expect } from '@playwright/test';

test.describe('User Authentication', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('Successful login with valid credentials', async ({ page }) => {
    await page.fill('[data-testid="email-input"]', 'user@example.com');
    await page.fill('[data-testid="password-input"]', 'SecurePass123!');
    await page.click('[data-testid="sign-in-button"]');
    
    await expect(page).toHaveURL('/dashboard');
    await expect(page.locator('[data-testid="welcome-message"]'))
      .toContainText('Welcome, John Doe');
  });

  test('Failed login with invalid email format', async ({ page }) => {
    await page.fill('[data-testid="email-input"]', 'invalid-email');
    await page.click('[data-testid="sign-in-button"]');
    
    await expect(page.locator('[data-testid="email-error"]'))
      .toContainText('Please enter a valid email address');
    await expect(page).toHaveURL('/login');
  });
});
```

### Benefits of Design-First Testing

#### 1. Tests Written Before Code

**Traditional Workflow:**

```text
Design → Code → Write Tests (often skipped due to time pressure)
```

**Agentic Workflow:**

```text
Design → Auto-Generate Tests → Code to Pass Tests
```

**Result:** 100% test coverage from day one, tests serve as the specification.

#### 2. Design as Living Documentation

The Figma file becomes the **single source of truth**:

- Product requirements
- Visual specification
- Test scenarios
- Implementation checklist

**Version Control Integration:**

```text
Each Figma version → New test suite generated
Developers can see exactly what changed and what tests need updating
```

#### 3. Compliance and Audit Trail

For FSI organizations:

**Audit Question:** "How do you ensure your login flow meets security requirements?"

**Answer with Evidence:**

```text
1. Security requirements documented in Figma (annotations)
2. Test scenarios auto-generated from design (Gherkin files in git)
3. Test execution reports (Playwright HTML reports)
4. Implementation code (React components in git)

Complete trail: Requirement → Design → Test → Code
```

---

## 5. Multi-Faceted Testing Strategy

### 1. Functional Verification

#### Agentic TDD (Test-Driven Development)

A developer can guide an agent through the **"red-green-refactor" TDD loop**. The developer provides **critical architectural decisions** and uses guardrails like pre-commit scripts to keep the agent on track.

**The TDD Loop with AI Agents:**

```text
Step 1: RED (Write Failing Test)
──────────────────────────────────
Developer: "Write a unit test for the calculateMonthlyPayment function 
            that validates loan amortization calculations"

Agent generates:
describe('calculateMonthlyPayment', () => {
  it('should calculate correct monthly payment for 30-year mortgage', () => {
    const result = calculateMonthlyPayment({
      principal: 300000,
      annualRate: 0.04,
      years: 30
    });
    expect(result).toBeCloseTo(1432.25, 2);
  });
});

Run test → ❌ FAILS (function doesn't exist)

Step 2: GREEN (Write Minimal Code to Pass)
──────────────────────────────────────────
Developer: "Implement the calculateMonthlyPayment function to pass the test"

Agent implements:
function calculateMonthlyPayment({ principal, annualRate, years }) {
  const monthlyRate = annualRate / 12;
  const numPayments = years * 12;
  const payment = principal * 
    (monthlyRate * Math.pow(1 + monthlyRate, numPayments)) / 
    (Math.pow(1 + monthlyRate, numPayments) - 1);
  return payment;
}

Run test → ✅ PASSES

Step 3: REFACTOR (Improve Code Quality)
───────────────────────────────────────
Developer: "Refactor for readability and add edge case handling"

Agent refactors with TypeScript, validation, edge cases
Run test → ✅ PASSES (with better code)
```

#### Agentic BDD (Behavior-Driven Development)

Frameworks like **BDDTestAIGen** use LLMs to automate the creation of BDD tests directly from high-level natural language requirements.

**Traditional BDD Challenge:**

```text
Problem: Writing comprehensive Gherkin scenarios requires:
- Deep understanding of the application
- Knowledge of edge cases
- Consistent language and structure
- Significant time investment

Result: Incomplete test coverage, inconsistent scenarios
```

**BDDTestAIGen Solution:**

```text
Input (Natural Language Requirement):
"Users should be able to transfer funds between their checking and savings 
accounts. The transfer should validate sufficient balance, apply daily 
transfer limits, and send confirmation notifications."

↓ BDDTestAIGen Agent ↓

Output: Complete Gherkin scenarios with edge cases, error conditions,
        happy paths, and boundary conditions
```

**Key Benefits:**

- ✅ Comprehensive Coverage - Agent generates scenarios you might miss
- ✅ Consistent Structure - All scenarios follow the same pattern
- ✅ Edge Case Discovery - Agent thinks of boundary conditions
- ✅ Fast Iteration - Generate scenarios in seconds, not hours
- ✅ Stakeholder Accessibility - Non-technical team members can contribute

### 2. Visual Validation: Beyond Pixel-Perfect Testing

**Traditional Visual Testing:**

```text
❌ Problem: Pixel-perfect snapshot comparison

Test fails if:
- Font rendering differs slightly between OS
- Browser renders 1px differently
- Dynamic content changes (dates, user names)
- Loading states differ in timing
- Minor CSS tweaks (1px padding change)

Result: 50%+ false positive rate → Developers ignore visual tests
```

**AI-Powered Visual Validation:**

AI-native testing platforms like **Mabl** employ AI for **semantic understanding of UI components**.

```text
✅ Solution: Semantic understanding of UI components

Mabl AI validates:
- Component structure (button is still a button)
- Text content (ignoring minor formatting)
- Layout relationships (button is still below input)
- Visual hierarchy (headings are still prominent)
- Functional elements (clickable areas intact)

Ignores:
- Minor pixel shifts
- Font rendering differences
- Dynamic content (timestamps, user-specific data)
- Intentional design updates

Result: <5% false positive rate → Tests are trusted
```

**Example:**

```typescript
// AI-powered visual validation (robust)
test('Login form maintains structure and functionality', async ({ page }) => {
  await page.goto('/login');
  
  await mabl.visualValidation(page, {
    validateComponents: [
      { type: 'input', label: 'Email', required: true },
      { type: 'input', label: 'Password', required: true },
      { type: 'button', text: 'Sign In', primary: true },
      { type: 'link', text: 'Forgot password?' }
    ],
    validateLayout: {
      emailAbovePassword: true,
      buttonBelowInputs: true,
      forgotPasswordRightAligned: true
    },
    ignoreContent: ['timestamp', 'username'],
    tolerance: 'semantic'
  });
});
```

### 3. Accessibility Testing: Automated WCAG Compliance

Accessibility is **no longer a separate, manual audit**. Modern AI test tools integrate **automated WCAG-based validation** directly into the test execution flow.

**The Problem with Traditional A11y Testing:**

```text
❌ Manual audits (expensive, infrequent)
❌ Separate test suites (out of sync with main tests)
❌ Requires specialized expertise
❌ Often tested at the end (too late to fix easily)
❌ Incomplete coverage
```

**Agentic Accessibility Testing:**

```typescript
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test.describe('Login Page Accessibility', () => {
  test('should not have any WCAG violations', async ({ page }) => {
    await page.goto('/login');
    
    const accessibilityScanResults = await new AxeBuilder({ page })
      .withTags(['wcag2a', 'wcag2aa', 'wcag21a', 'wcag21aa'])
      .analyze();
    
    expect(accessibilityScanResults.violations).toEqual([]);
  });
  
  test('should support full keyboard navigation', async ({ page }) => {
    await page.goto('/login');
    
    await page.keyboard.press('Tab'); // Email input
    await expect(page.locator('[data-testid="email-input"]')).toBeFocused();
    
    await page.keyboard.press('Tab'); // Password input
    await expect(page.locator('[data-testid="password-input"]')).toBeFocused();
    
    await page.keyboard.press('Tab'); // Sign in button
    await expect(page.locator('[data-testid="sign-in-button"]')).toBeFocused();
    
    await page.keyboard.press('Enter'); // Submit
  });
  
  test('should have proper ARIA labels', async ({ page }) => {
    await page.goto('/login');
    
    const emailInput = page.locator('input[type="email"]');
    await expect(emailInput).toHaveAttribute('aria-label', /email/i);
    
    const passwordInput = page.locator('input[type="password"]');
    await expect(passwordInput).toHaveAttribute('aria-label', /password/i);
  });
});
```

**FSI Compliance Benefits:**

```text
Banking Application A11y Requirements:
- ADA compliance (Americans with Disabilities Act)
- WCAG 2.1 AA minimum (often AAA for critical flows)
- Section 508 compliance (government banking)

Agentic Testing ensures:
✅ Automated validation on every commit
✅ Compliance evidence for audits
✅ Reduced legal risk
✅ Better user experience for all customers
```

---

## 10. Testing Existing Codebases: Retrofitting Strategies

### The Legacy Code Challenge

Most enterprise applications have extensive existing codebases without comprehensive test coverage. This section provides strategies for retrofitting tests to legacy code without breaking existing functionality.

### Key Challenges

- **Tight Coupling**: Hard dependencies, singletons, static methods
- **No Interfaces**: Direct instantiation of concrete classes
- **Side Effects**: Database calls, file I/O, network calls in business logic
- **Large Methods**: 500+ line methods mixing concerns
- **Hidden Dependencies**: Global state, service locators
- **No Clear Entry Points**: Unclear how to invoke code in isolation

### Retrofitting Strategies

#### 1. Characterization Tests (Golden Master Testing)

**Purpose:** Capture existing behavior before refactoring

**JavaScript/TypeScript Example:**

```typescript
// legacy/order-processor.ts
export class OrderProcessor {
  // 300 lines of complex legacy code with database calls
  processOrder(orderId: string): string {
    // Complex logic with side effects
    const order = this.db.getOrder(orderId);
    const customer = this.db.getCustomer(order.customerId);
    const inventory = this.checkInventory(order.items);
    const price = this.calculatePrice(order, customer);
    const tax = this.calculateTax(price, customer.address.state);
    this.sendEmail(customer.email, order);
    return this.formatResponse(order, price, tax);
  }
}
```

**Characterization Test:**

```typescript
// __tests__/characterization/order-processor.characterization.test.ts
import { OrderProcessor } from '../../legacy/order-processor';

describe('OrderProcessor Characterization Tests', () => {
  let processor: OrderProcessor;
  let goldenResults: Map<string, string>;

  beforeAll(() => {
    // Load golden master results (run once to capture)
    goldenResults = new Map([
      ['ORD-001', '{"orderId":"ORD-001","total":1250.50,"status":"CONFIRMED"}'],
      ['ORD-002', '{"orderId":"ORD-002","total":750.00,"status":"CONFIRMED"}'],
    ]);
  });

  beforeEach(() => {
    processor = new OrderProcessor();
  });

  test('processOrder maintains existing behavior for ORD-001', () => {
    const result = processor.processOrder('ORD-001');
    const expected = goldenResults.get('ORD-001');
    
    // First run: console.log(result) to capture golden master
    // Subsequent runs: verify behavior hasn't changed
    expect(result).toEqual(expected);
  });

  test('processOrder maintains existing behavior for ORD-002', () => {
    const result = processor.processOrder('ORD-002');
    const expected = goldenResults.get('ORD-002');
    
    expect(result).toEqual(expected);
  });
});
```

**Key Principle:** Characterization tests lock in existing behavior, allowing safe refactoring.

---

#### 2. Dependency Breaking Techniques

**Extract and Override**

```java
// Before: Tight coupling
public class LoanCalculator {
    public double calculateMonthlyPayment(double principal, int years) {
        // Direct database call - hard to test
        double interestRate = DatabaseConnection.getInstance()
            .query("SELECT rate FROM rates WHERE type='mortgage'")
            .getDouble(0);
        
        return calculatePayment(principal, interestRate, years);
    }
}

// After: Extract to protected method, override in tests
public class LoanCalculator {
    public double calculateMonthlyPayment(double principal, int years) {
        double interestRate = getInterestRate();
        return calculatePayment(principal, interestRate, years);
    }
    
    protected double getInterestRate() {
        return DatabaseConnection.getInstance()
            .query("SELECT rate FROM rates WHERE type='mortgage'")
            .getDouble(0);
    }
    
    private double calculatePayment(double principal, double rate, int years) {
        // Pure calculation logic
        double monthlyRate = rate / 12 / 100;
        int months = years * 12;
        return principal * (monthlyRate * Math.pow(1 + monthlyRate, months))
                / (Math.pow(1 + monthlyRate, months) - 1);
    }
}

// Test with subclass
public class LoanCalculatorTest {
    @Test
    public void testCalculateMonthlyPayment() {
        LoanCalculator calculator = new LoanCalculator() {
            @Override
            protected double getInterestRate() {
                return 3.5; // Stub out database call
            }
        };
        
        double payment = calculator.calculateMonthlyPayment(300000, 30);
        assertEquals(1347.13, payment, 0.01);
    }
}
```

**Introduce Interface**

```csharp
// Before: Concrete dependency
public class AccountService
{
    private EmailSender emailSender = new EmailSender();
    
    public void OpenAccount(Account account)
    {
        // Business logic
        ValidateAccount(account);
        SaveToDatabase(account);
        
        // Hard to test - sends real emails
        emailSender.SendWelcomeEmail(account.Email);
    }
}

// After: Interface-based dependency
public interface IEmailSender
{
    void SendWelcomeEmail(string email);
}

public class AccountService
{
    private readonly IEmailSender emailSender;
    
    // Constructor injection
    public AccountService(IEmailSender emailSender)
    {
        this.emailSender = emailSender;
    }
    
    public void OpenAccount(Account account)
    {
        ValidateAccount(account);
        SaveToDatabase(account);
        emailSender.SendWelcomeEmail(account.Email);
    }
}

// Test with mock
public class AccountServiceTests
{
    [Fact]
    public async Task OpenAccount_SendsWelcomeEmail()
    {
        var mockEmailSender = new Mock<IEmailSender>();
        var service = new AccountService(mockEmailSender.Object);
        
        var account = new Account { Email = "user@example.com" };
        service.OpenAccount(account);
        
        mockEmailSender.Verify(e => e.SendWelcomeEmail("user@example.com"), Times.Once);
    }
}
```

---

#### 3. Gradual Coverage Improvement

**Strategy: Start with High-Value, Low-Complexity Code**

```python
# Identify code to test (priority order)
# 1. Pure functions (no side effects)
# 2. Business logic with clear inputs/outputs
# 3. Recently changed code
# 4. Bug-prone areas

# Example: Gradual coverage for legacy financial module

# Phase 1: Test pure calculation functions
def test_calculate_compound_interest():
    # Pure function - easy to test
    result = calculate_compound_interest(principal=1000, rate=5, years=2)
    assert result == 1102.50

# Phase 2: Test business logic with mocked dependencies
def test_process_transaction_success(mocker):
    # Mock database and external services
    mock_db = mocker.patch('legacy.database.get_account')
    mock_db.return_value = {'balance': 5000, 'status': 'ACTIVE'}
    
    mock_email = mocker.patch('legacy.email.send_notification')
    
    result = process_transaction(account_id='ACC-001', amount=100)
    
    assert result['status'] == 'SUCCESS'
    mock_email.assert_called_once()

# Phase 3: Integration tests for complex workflows
@pytest.mark.integration
def test_end_to_end_loan_application(test_database):
    # Use test database
    application = submit_loan_application({
        'applicant': 'John Doe',
        'amount': 50000,
        'term': 60
    })
    
    assert application['status'] == 'PENDING_REVIEW'
    assert application['application_id'].startswith('LOAN-')
```

**Coverage Goals:**
- Month 1: 20% coverage (pure functions, critical paths)
- Month 3: 40% coverage (business logic with mocks)
- Month 6: 60% coverage (integration tests)
- Month 12: 80% coverage (comprehensive suite)

---

#### 4. Sprout Method/Class Pattern

**When:** Need to add new feature to legacy code

```java
// Before: Adding feature to legacy class
public class LegacyPaymentProcessor {
    public void processPayment(Payment payment) {
        // 500 lines of legacy code
        // No tests exist
        // Adding fraud detection would be risky
    }
}

// After: Sprout new tested class
public class FraudDetectionService {
    private final FraudScoreCalculator calculator;
    private final BlacklistChecker blacklistChecker;
    
    public FraudDetectionService(FraudScoreCalculator calculator, 
                                 BlacklistChecker blacklistChecker) {
        this.calculator = calculator;
        this.blacklistChecker = blacklistChecker;
    }
    
    public FraudCheckResult checkForFraud(Payment payment) {
        double riskScore = calculator.calculateRiskScore(payment);
        boolean isBlacklisted = blacklistChecker.isBlacklisted(payment.getCardNumber());
        
        if (isBlacklisted || riskScore > 0.8) {
            return FraudCheckResult.reject("High fraud risk");
        }
        return FraudCheckResult.approve();
    }
}

// Legacy class calls new tested class
public class LegacyPaymentProcessor {
    private final FraudDetectionService fraudDetection;
    
    public void processPayment(Payment payment) {
        // New feature - fully tested
        FraudCheckResult fraudCheck = fraudDetection.checkForFraud(payment);
        if (!fraudCheck.isApproved()) {
            throw new FraudException(fraudCheck.getReason());
        }
        
        // 500 lines of legacy code (unchanged)
    }
}

// Test new class thoroughly
public class FraudDetectionServiceTest {
    @Test
    public void rejectsBlacklistedCards() {
        var calculator = mock(FraudScoreCalculator.class);
        var blacklist = mock(BlacklistChecker.class);
        when(blacklist.isBlacklisted("1234567890")).thenReturn(true);
        
        var service = new FraudDetectionService(calculator, blacklist);
        var payment = new Payment().setCardNumber("1234567890");
        
        var result = service.checkForFraud(payment);
        
        assertFalse(result.isApproved());
        assertEquals("High fraud risk", result.getReason());
    }
}
```

---

#### 5. Wrap Method Pattern

**When:** Need to add behavior before/after legacy code

```typescript
// Before: Legacy method with side effects
class LegacyAccountManager {
  updateBalance(accountId: string, amount: number): void {
    // 200 lines of legacy code
    // Updates database directly
  }
}

// After: Wrap with new tested method
class LegacyAccountManager {
  updateBalance(accountId: string, amount: number): void {
    // 200 lines of legacy code (unchanged)
  }
  
  // New method - wraps legacy behavior
  updateBalanceWithAudit(accountId: string, amount: number, userId: string): void {
    this.auditService.logBalanceChange(accountId, amount, userId);
    this.updateBalance(accountId, amount);
  }
}

// Test new behavior
describe('LegacyAccountManager - New Audit Feature', () => {
  test('logs audit trail when updating balance', () => {
    const mockAuditService = {
      logBalanceChange: jest.fn()
    };
    const manager = new LegacyAccountManager();
    manager.auditService = mockAuditService;
    
    manager.updateBalanceWithAudit('ACC-001', 500, 'USER-123');
    
    expect(mockAuditService.logBalanceChange).toHaveBeenCalledWith(
      'ACC-001',
      500,
      'USER-123'
    );
  });
});
```

---

### Testing Strategy for Legacy Codebases

**Phase 1: Stabilization (Weeks 1-4)**
- ✅ Identify high-risk areas (bug history, complexity metrics)
- ✅ Write characterization tests for core workflows
- ✅ Set up CI/CD pipeline with test automation
- ✅ Establish baseline code coverage (even if low)

**Phase 2: Strategic Coverage (Months 2-4)**
- ✅ Test pure functions and business logic
- ✅ Apply dependency breaking techniques
- ✅ Mock external dependencies (database, APIs, filesystem)
- ✅ Aim for 40-50% coverage in critical modules

**Phase 3: Incremental Refactoring (Months 5-12)**
- ✅ Extract interfaces, introduce dependency injection
- ✅ Break large methods into smaller, testable units
- ✅ Add integration tests for key workflows
- ✅ Aim for 70-80% coverage across application

**Phase 4: Continuous Improvement (Ongoing)**
- ✅ Every bug fix includes a regression test
- ✅ Every new feature includes comprehensive tests
- ✅ Refactor untested code when touching it
- ✅ Monitor and improve code quality metrics

---

### Tools for Legacy Code Testing

**Code Coverage:**
- **JavaScript/TypeScript:** Istanbul, c8, Jest coverage
- **Java:** JaCoCo, Cobertura
- **.NET:** Coverlet, OpenCover
- **Python:** Coverage.py, pytest-cov

**Mutation Testing:**
- **JavaScript:** Stryker
- **Java:** PITest
- **.NET:** Stryker.NET
- **Python:** mutmut

**Static Analysis:**
- **JavaScript:** ESLint, SonarQube
- **Java:** SonarQube, SpotBugs
- **.NET:** ReSharper, SonarQube
- **Python:** Pylint, Bandit, SonarQube

**Dependency Analysis:**
- **JavaScript:** Madge (circular dependencies)
- **Java:** JDepend, ArchUnit
- **.NET:** NDepend
- **Python:** pydeps

---

### Key Principles for Testing Legacy Code

1. **Don't Rewrite Without Tests**: Write characterization tests first
2. **Small Steps**: One refactoring at a time, always keep tests green
3. **Prioritize High-Value Areas**: Test critical business logic first
4. **Mock External Dependencies**: Database, filesystem, network calls
5. **Boy Scout Rule**: Leave code better than you found it
6. **Fail Fast**: Add tests before fixing bugs
7. **Measure Progress**: Track coverage and quality metrics
8. **Automate Everything**: CI/CD, test execution, coverage reports

---

## 11. Performance & Load Testing

```gherkin
Feature: Transaction API Performance
  As a system architect
  I want to ensure the transaction API meets performance SLAs
  So that users experience fast, reliable service

  Scenario: Handle baseline load
    Given the system is in a healthy state
    When 100 concurrent users request transaction history
    Then 95th percentile response time should be under 200ms
    And 99th percentile response time should be under 500ms
    And error rate should be 0%

  Scenario: Handle peak load
    Given the system is in a healthy state
    When 1000 concurrent users request transaction history for 5 minutes
    Then 95th percentile response time should be under 1000ms
    And error rate should be under 0.1%

  Scenario: Graceful degradation under extreme load
    When 5000 concurrent users request transaction history
    Then the system should return 429 (Too Many Requests) for excess traffic
    And existing users should maintain sub-1s response times
    And no data corruption should occur
```

**Agent Implementation with k6:**

```typescript
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate } from 'k6/metrics';

const errorRate = new Rate('errors');

export const options = {
  stages: [
    { duration: '2m', target: 100 },  // Baseline
    { duration: '5m', target: 1000 }, // Peak
    { duration: '2m', target: 5000 }, // Extreme
    { duration: '2m', target: 0 },    // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<1000', 'p(99)<2000'],
    errors: ['rate<0.001'],
  },
};

export default function () {
  const response = http.get('https://api.bank.com/transactions', {
    headers: { 'Authorization': 'Bearer ${TOKEN}' },
  });
  
  check(response, {
    'status is 200': (r) => r.status === 200,
    'response time < 1s': (r) => r.timings.duration < 1000,
  }) || errorRate.add(1);
  
  sleep(1);
}
```

---

## 7. Security & Vulnerability Testing

```gherkin
Feature: API Security
  As a security engineer
  I want to ensure the API is protected against common vulnerabilities
  So that customer data remains secure

  Scenario: Prevent SQL injection
    When I send a request with SQL injection payload in the email parameter
    Then the request should be rejected with 400 Bad Request
    And no database queries should be executed
    And the incident should be logged

  Scenario: Enforce rate limiting
    When I send 100 requests in 10 seconds
    Then requests 51-100 should return 429 Too Many Requests
    And a rate limit header should indicate reset time

  Scenario: Validate JWT token expiration
    Given I have an expired JWT token
    When I send a request with the expired token
    Then the request should return 401 Unauthorized
    And the response should indicate "Token expired"
```

---

## 8. Chaos Engineering & Fault Injection → See Lens 3

**Chaos Engineering and Fault Injection** (AWS FIS, Gremlin, Chaos Mesh) have been moved to **[Lens 3: Platform, Cloud & SRE](../lens-3-platform-cloud-sre/README.md)** as they are part of the deployment and infrastructure resilience strategy.

### Why Chaos Engineering Belongs in Lens 3 (Platform & SRE)

**Deployment & Infrastructure Focus:**
- Chaos experiments target infrastructure: pods, nodes, databases, network
- Managed by DevOps/SRE teams with production access
- Part of deployment readiness and SLO validation
- Requires infrastructure-level permissions (AWS FIS, Kubernetes)

**Platform Concerns:**
- Multi-region failover testing
- Availability zone outages
- Database replication lag
- Network partitions
- Resource exhaustion (CPU, memory, disk)

**Lens 4 (Quality & Testing) focuses on:**
- Application-level testing (unit, integration, functional)
- Code quality and test automation
- Test-Driven Development (TDD)
- Behavior-Driven Development (BDD)
- API and UI testing

For chaos engineering strategies, fault injection experiments, and resilience testing, see:

👉 **[Lens 3: Platform, Cloud & SRE](../lens-3-platform-cloud-sre/README.md)**

---

## 9. Multi-Agent Test Orchestration

### Coordinating Multiple AI Agents for Comprehensive Testing

Modern applications require testing across multiple dimensions: UI, API, database, performance, security. Multi-agent test orchestration uses Claude Agentic SDK and LangChain to coordinate specialized test agents working together.

### Architecture

```text
┌──────────────────────────────────────────────────────────────┐
│  Multi-Agent Test Orchestration                              │
└──────────────────────────────────────────────────────────────┘

┌─────────────────┐
│ Orchestrator    │ ← Claude Agentic SDK
│ Agent           │    LangChain Workflow
└────────┬────────┘
         │
    ┌────┴─────┬────────┬────────┬────────┬────────┐
    │          │        │        │        │        │
┌───▼───┐  ┌──▼───┐ ┌──▼───┐ ┌──▼───┐ ┌──▼───┐ ┌──▼───┐
│  UI   │  │ API  │ │  DB  │ │ Perf │ │ Sec  │ │ Data │
│ Agent │  │Agent │ │Agent │ │Agent │ │Agent │ │Agent │
└───────┘  └──────┘ └──────┘ └──────┘ └──────┘ └──────┘
    │          │        │        │        │        │
    │     Shared Context & State Management         │
    └──────────────────┬───────────────────────────┘
                       │
               ┌───────▼────────┐
               │ Test Results   │
               │ Repository     │
               └────────────────┘
```

### Example: Fund Transfer E2E Testing with Multiple Agents

#### TypeScript (Claude Agentic SDK + LangChain)

```typescript
// orchestration/fund-transfer-test.ts
import { ClaudeAgenticSDK, Agent } from '@anthropic-ai/sdk';
import { LangChain, SequentialChain, ParallelChain } from 'langchain';
import { PlaywrightMCP } from '@playwright/mcp';
import { PostmanMCP } from '@postman/mcp';
import { AWSMCP } from '@aws/mcp';

interface TestContext {
  userId: string;
  fromAccount: string;
  toAccount: string;
  amount: number;
  transactionId?: string;
  uiScreenshots?: string[];
  apiResponses?: any[];
  dbSnapshots?: any[];
}

// Orchestrator Agent
class TestOrchestrator {
  private sdk: ClaudeAgenticSDK;
  private uiAgent: UITestAgent;
  private apiAgent: APITestAgent;
  private dbAgent: DatabaseAgent;
  private perfAgent: PerformanceAgent;
  
  constructor() {
    this.sdk = new ClaudeAgenticSDK({ apiKey: process.env.CLAUDE_API_KEY });
    this.uiAgent = new UITestAgent();
    this.apiAgent = new APITestAgent();
    this.dbAgent = new DatabaseAgent();
    this.perfAgent = new PerformanceAgent();
  }
  
  async orchestrateTest(scenario: string): Promise<TestResult> {
    // Create shared context
    const context: TestContext = {
      userId: 'USER-001',
      fromAccount: 'CHK-001',
      toAccount: 'SAV-001',
      amount: 500
    };
    
    // Phase 1: Setup (Parallel)
    await this.runPhase('Setup', async () => {
      await Promise.all([
        this.uiAgent.setup(context),
        this.apiAgent.setup(context),
        this.dbAgent.captureBaseline(context)
      ]);
    });
    
    // Phase 2: Execute Transfer (Sequential)
    await this.runPhase('Execute', async () => {
      // Step 1: UI initiates transfer
      const uiResult = await this.uiAgent.initiateTransfer(context);
      context.transactionId = uiResult.transactionId;
      
      // Step 2: API validates transaction
      const apiResult = await this.apiAgent.validateTransaction(context);
      context.apiResponses = apiResult.responses;
      
      // Step 3: DB verifies state
      const dbResult = await this.dbAgent.verifyBalances(context);
      context.dbSnapshots = dbResult.snapshots;
    });
    
    // Phase 3: Validate (Parallel)
    const results = await this.runPhase('Validate', async () => {
      return await Promise.all([
        this.uiAgent.validateUI(context),
        this.apiAgent.validateAPI(context),
        this.dbAgent.validateConsistency(context),
        this.perfAgent.validatePerformance(context)
      ]);
    });
    
    return this.aggregateResults(results);
  }
  
  private async runPhase(name: string, fn: () => Promise<any>): Promise<any> {
    console.log(`🔄 Starting phase: ${name}`);
    const result = await fn();
    console.log(`✅ Completed phase: ${name}`);
    return result;
  }
}

// UI Agent (Playwright MCP)
class UITestAgent {
  private browser: PlaywrightMCP;
  
  async setup(context: TestContext): Promise<void> {
    this.browser = await PlaywrightMCP.launch();
    await this.browser.login(context.userId);
  }
  
  async initiateTransfer(context: TestContext): Promise<{ transactionId: string }> {
    const page = await this.browser.page();
    
    // Navigate to transfer page
    await page.goto('/transfers/new');
    
    // Fill form
    await page.fill('#fromAccount', context.fromAccount);
    await page.fill('#toAccount', context.toAccount);
    await page.fill('#amount', context.amount.toString());
    
    // Submit and capture transaction ID
    await page.click('button[type="submit"]');
    await page.waitForSelector('.transaction-id');
    const transactionId = await page.textContent('.transaction-id');
    
    // Capture screenshot
    context.uiScreenshots = [await page.screenshot({ path: 'transfer-complete.png' })];
    
    return { transactionId };
  }
  
  async validateUI(context: TestContext): Promise<ValidationResult> {
    const page = await this.browser.page();
    
    // Verify success message
    const successMessage = await page.textContent('.success-message');
    const balanceUpdated = await page.textContent('#account-balance');
    
    return {
      passed: successMessage.includes('Transfer successful'),
      details: { balanceUpdated }
    };
  }
}

// API Agent (Postman MCP)
class APITestAgent {
  private client: PostmanMCP;
  
  async setup(context: TestContext): Promise<void> {
    this.client = new PostmanMCP({ environment: 'staging' });
  }
  
  async validateTransaction(context: TestContext): Promise<{ responses: any[] }> {
    const responses = [];
    
    // Get transaction details
    const txnResponse = await this.client.get(`/api/transactions/${context.transactionId}`);
    responses.push(txnResponse);
    
    // Verify transaction status
    expect(txnResponse.status).toBe(200);
    expect(txnResponse.data.status).toBe('COMPLETED');
    expect(txnResponse.data.amount).toBe(context.amount);
    
    // Get updated account balances
    const fromAccountResponse = await this.client.get(`/api/accounts/${context.fromAccount}`);
    const toAccountResponse = await this.client.get(`/api/accounts/${context.toAccount}`);
    responses.push(fromAccountResponse, toAccountResponse);
    
    return { responses };
  }
  
  async validateAPI(context: TestContext): Promise<ValidationResult> {
    // Verify API response structure
    const validation = await this.client.validateSchema(
      context.apiResponses[0],
      'TransactionResponse'
    );
    
    return {
      passed: validation.valid,
      details: { schemaErrors: validation.errors }
    };
  }
}

// Database Agent (AWS MCP - DynamoDB/RDS)
class DatabaseAgent {
  private client: AWSMCP;
  private baseline: any;
  
  async captureBaseline(context: TestContext): Promise<void> {
    this.client = new AWSMCP({ service: 'dynamodb' });
    
    // Capture account balances before transfer
    this.baseline = {
      fromBalance: await this.getBalance(context.fromAccount),
      toBalance: await this.getBalance(context.toAccount)
    };
  }
  
  async verifyBalances(context: TestContext): Promise<{ snapshots: any[] }> {
    const snapshots = [];
    
    // Get updated balances
    const fromBalance = await this.getBalance(context.fromAccount);
    const toBalance = await this.getBalance(context.toAccount);
    
    snapshots.push({ account: context.fromAccount, balance: fromBalance });
    snapshots.push({ account: context.toAccount, balance: toBalance });
    
    // Verify correct deduction and addition
    expect(fromBalance).toBe(this.baseline.fromBalance - context.amount);
    expect(toBalance).toBe(this.baseline.toBalance + context.amount);
    
    return { snapshots };
  }
  
  async validateConsistency(context: TestContext): Promise<ValidationResult> {
    // Verify transaction record exists
    const txnRecord = await this.client.query({
      TableName: 'Transactions',
      KeyConditionExpression: 'transactionId = :id',
      ExpressionAttributeValues: { ':id': context.transactionId }
    });
    
    return {
      passed: txnRecord.Items.length === 1,
      details: { record: txnRecord.Items[0] }
    };
  }
  
  private async getBalance(accountId: string): Promise<number> {
    const result = await this.client.getItem({
      TableName: 'Accounts',
      Key: { accountId }
    });
    return result.Item.balance;
  }
}

// Performance Agent
class PerformanceAgent {
  async validatePerformance(context: TestContext): Promise<ValidationResult> {
    // Check that transaction completed within SLA
    const metrics = await this.getMetrics(context.transactionId);
    
    return {
      passed: metrics.totalTime < 2000, // 2 second SLA
      details: {
        uiTime: metrics.uiTime,
        apiTime: metrics.apiTime,
        dbTime: metrics.dbTime,
        totalTime: metrics.totalTime
      }
    };
  }
  
  private async getMetrics(transactionId: string): Promise<any> {
    // Query CloudWatch or application metrics
    return {
      uiTime: 150,
      apiTime: 50,
      dbTime: 25,
      totalTime: 225
    };
  }
}

// Run orchestrated test
const orchestrator = new TestOrchestrator();
const result = await orchestrator.orchestrateTest('Fund Transfer E2E');

console.log('Test Result:', result);
// Output:
// 🔄 Starting phase: Setup
// ✅ Completed phase: Setup
// 🔄 Starting phase: Execute
// ✅ Completed phase: Execute
// 🔄 Starting phase: Validate
// ✅ Completed phase: Validate
// Test Result: { passed: true, phases: [...], duration: 2.3s }
```

### Key Benefits of Multi-Agent Orchestration

1. **Parallel Execution**: Setup and validation phases run concurrently
2. **Shared Context**: All agents access consistent test data
3. **Specialized Agents**: Each agent focuses on its domain (UI, API, DB, performance)
4. **Coordinated Workflow**: Orchestrator manages dependencies and sequencing
5. **Comprehensive Coverage**: Tests UI, API, database, and performance together
6. **Single Test Run**: One execution validates entire system, not isolated components

---

## 12. Best Practices & Patterns

### Test Organization

**Directory Structure:**

```text
tests/
├── functional/
│   ├── login.spec.ts
│   ├── transfer.spec.ts
│   └── dashboard.spec.ts
├── visual/
│   ├── components.visual.ts
│   └── pages.visual.ts
├── accessibility/
│   ├── wcag2aa.spec.ts
│   └── keyboard-nav.spec.ts
├── performance/
│   ├── load-test.k6.js
│   └── stress-test.k6.js
├── security/
│   ├── injection.spec.ts
│   └── auth.spec.ts
└── chaos/
    ├── pod-failure.spec.ts
    └── network-latency.spec.ts
```

### Test Data Management

**Use APIs for test data:**

```typescript
test.beforeEach(async ({ request }) => {
  await request.post('/api/test/seed', {
    data: {
      user: { email: 'test@example.com', balance: 5000 },
      accounts: [
        { type: 'checking', balance: 5000 },
        { type: 'savings', balance: 10000 }
      ]
    }
  });
});

test.afterEach(async ({ request }) => {
  await request.delete('/api/test/cleanup');
});
```

### Continuous Testing Strategy

```text
Commit → Unit Tests (30s)
   ↓
Push → Integration Tests (5min)
   ↓
PR → Visual Tests (10min)
   ↓
Merge → E2E Tests (20min)
   ↓
Deploy → Smoke Tests (5min)
   ↓
Production → Continuous Monitoring
```

---

## Related Modules

- **[Lens 1: Product Development & Architecture](../lens-1-product-dev-architecture/README.md)** - Design and development workflows
- **[Lens 3: Platform, Cloud & SRE](../lens-3-platform-cloud-sre/README.md)** - Infrastructure reliability
- **[Lens 5: AI, Inference & Agentic Automation](../lens-5-ai-inference-automation/README.md)** - Advanced AI orchestration

---

## Additional Resources

- **[Complete Workflow Guide](../../AGENTIC_WORKFLOW_PRACTICE.md)** - Full platform documentation
- **[Banking Use Case](../../AGENTIC_BANK_FSI.md)** - Financial services implementation
- **[Main README](../../README.md)** - Platform overview

---

**Status**: Full content migration complete

**Last Updated**: 2025-01-17

**Maintainers**: QA & Platform Architecture Team
