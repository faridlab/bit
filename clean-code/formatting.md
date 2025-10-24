# 🎨 Formatting - Making Your Code Beautiful

Code formatting is like good handwriting - it makes everything easier to read and understand! Consistent formatting helps your brain process code faster and reduces cognitive load.

## 🌟 Why Formatting Matters

Think of code formatting like organizing your room. When everything is in its proper place and neatly arranged, you can find what you need quickly and feel more comfortable working there!

```javascript
// ❌ Bad - Inconsistent and hard to read
function calculateTotal(items){let total=0;for(let i=0;i<items.length;i++){total+=items[i].price*items[i].quantity;}return total;}

// ✅ Good - Clean and readable
function calculateTotal(items) {
    let total = 0;
    for (let i = 0; i < items.length; i++) {
        total += items[i].price * items[i].quantity;
    }
    return total;
}
```

## 📏 Indentation - The Foundation

### Use Consistent Indentation
Pick either spaces or tabs and stick with it throughout your project:

```javascript
// ✅ Good - 4 spaces (most common)
function processUser(user) {
    if (user.isActive) {
        const result = validateUser(user);
        if (result.isValid) {
            return saveUser(user);
        }
    }
    return null;
}

// ✅ Good - 2 spaces (also common)
function processUser(user) {
  if (user.isActive) {
    const result = validateUser(user);
    if (result.isValid) {
      return saveUser(user);
    }
  }
  return null;
}
```

### Indentation Rules
```javascript
// ✅ Good - Proper indentation for different constructs
class UserService {
    constructor(database) {
        this.database = database;
    }

    async createUser(userData) {
        try {
            const validatedUser = this.validateUser(userData);
            const savedUser = await this.database.save(validatedUser);
            return this.formatUserResponse(savedUser);
        } catch (error) {
            this.handleError(error);
            throw error;
        }
    }

    validateUser(userData) {
        if (!userData.email) {
            throw new Error('Email is required');
        }
        return userData;
    }
}
```

## 🔤 Spacing - Breathing Room for Code

### Around Operators
```javascript
// ❌ Bad - No spacing
const total=price*quantity+tax;

// ✅ Good - Proper spacing
const total = price * quantity + tax;
```

### Around Keywords
```javascript
// ❌ Bad - Inconsistent spacing
if(condition){
    doSomething();
}else{
    doSomethingElse();
}

// ✅ Good - Consistent spacing
if (condition) {
    doSomething();
} else {
    doSomethingElse();
}
```

### In Function Declarations
```javascript
// ❌ Bad - Inconsistent spacing
function calculateTotal(items,taxRate){
    // function body
}

// ✅ Good - Proper spacing
function calculateTotal(items, taxRate) {
    // function body
}
```

### Around Braces
```javascript
// ❌ Bad - Inconsistent brace placement
function example()
{
    if (condition)
    {
        doSomething();
    }
}

// ✅ Good - Consistent brace placement
function example() {
    if (condition) {
        doSomething();
    }
}
```

## 📝 Line Length - Keep It Reasonable

### The 80-120 Character Rule
Most teams use 80-120 characters per line. This makes code readable on different screen sizes:

```javascript
// ❌ Bad - Too long line
const result = this.calculateTotalPrice(items, taxRate, discountCode, customerType, isVip, hasLoyaltyCard, isFirstTimeCustomer);

// ✅ Good - Broken into readable chunks
const result = this.calculateTotalPrice(
    items,
    taxRate,
    discountCode,
    customerType,
    isVip,
    hasLoyaltyCard,
    isFirstTimeCustomer
);
```

### Breaking Long Lines
```javascript
// ✅ Good - Break at logical points
const user = await userService.createUser({
    name: userData.name,
    email: userData.email,
    phone: userData.phone,
    address: userData.address,
    preferences: userData.preferences
});

// ✅ Good - Break function calls
const result = await database.query(
    'SELECT * FROM users WHERE active = ? AND created_at > ?',
    [true, startDate]
);
```

## 🎯 Blank Lines - Visual Separation

### Use Blank Lines to Group Related Code
```javascript
// ✅ Good - Logical grouping with blank lines
class UserService {
    constructor(database) {
        this.database = database;
        this.cache = new Map();
    }

    async getUserById(id) {
        // Check cache first
        if (this.cache.has(id)) {
            return this.cache.get(id);
        }

        // Fetch from database
        const user = await this.database.findById(id);
        if (!user) {
            throw new Error('User not found');
        }

        // Cache the result
        this.cache.set(id, user);

        return user;
    }

    clearCache() {
        this.cache.clear();
    }
}
```

### Don't Overuse Blank Lines
```javascript
// ❌ Bad - Too many blank lines
function processData(data) {

    const result = [];

    for (let item of data) {

        if (item.isValid) {

            result.push(item);

        }

    }

    return result;

}

// ✅ Good - Appropriate blank lines
function processData(data) {
    const result = [];

    for (let item of data) {
        if (item.isValid) {
            result.push(item);
        }
    }

    return result;
}
```

## 🔧 Braces and Parentheses

### Consistent Brace Style
```javascript
// ✅ Good - K&R style (opening brace on same line)
if (condition) {
    doSomething();
} else {
    doSomethingElse();
}

// ✅ Good - Allman style (opening brace on new line)
if (condition)
{
    doSomething();
}
else
{
    doSomethingElse();
}
```

### Parentheses for Clarity
```javascript
// ❌ Bad - Ambiguous precedence
const result = a + b * c / d - e;

// ✅ Good - Clear precedence
const result = a + (b * c / d) - e;

// ✅ Good - Even clearer
const result = a + ((b * c) / d) - e;
```

## 📁 File Organization

### Import/Export Formatting
```javascript
// ✅ Good - Grouped and sorted imports
// External libraries
import React from 'react';
import { useState, useEffect } from 'react';
import axios from 'axios';

// Internal modules
import { UserService } from '../services/UserService';
import { validateEmail } from '../utils/validation';
import { API_BASE_URL } from '../config/constants';

// Types
import type { User, UserCreateRequest } from '../types/User';
```

### Class Member Ordering
```javascript
// ✅ Good - Logical member ordering
class UserService {
    // Static properties
    static readonly MAX_RETRY_ATTEMPTS = 3;

    // Instance properties
    private database: Database;
    private cache: Map<string, User>;

    // Constructor
    constructor(database: Database) {
        this.database = database;
        this.cache = new Map();
    }

    // Public methods
    async getUserById(id: string): Promise<User> {
        // Implementation
    }

    async createUser(userData: UserCreateRequest): Promise<User> {
        // Implementation
    }

    // Private methods
    private validateUser(userData: UserCreateRequest): void {
        // Implementation
    }

    private handleError(error: Error): void {
        // Implementation
    }
}
```

## 🎨 Naming and Formatting

### Consistent Naming
```javascript
// ✅ Good - Consistent naming conventions
const userName = 'john_doe';
const isUserActive = true;
const MAX_RETRY_ATTEMPTS = 3;

class UserAccount {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }

    getDisplayName() {
        return this.name.toUpperCase();
    }
}
```

### String Formatting
```javascript
// ✅ Good - Consistent string formatting
const message = 'Hello, world!';
const template = `User ${userName} has ${itemCount} items`;
const multiline = `
    This is a multiline string
    that spans multiple lines
    for better readability
`;
```

## 🧪 Code Blocks and Control Structures

### If Statements
```javascript
// ✅ Good - Clear if statements
if (user.isActive) {
    processUser(user);
}

if (user.isActive && user.hasPermission) {
    processUser(user);
}

if (user.isActive) {
    processUser(user);
} else if (user.isPending) {
    queueUser(user);
} else {
    rejectUser(user);
}
```

### Loops
```javascript
// ✅ Good - Clear loop formatting
for (let i = 0; i < items.length; i++) {
    processItem(items[i]);
}

for (const item of items) {
    if (item.isValid) {
        processItem(item);
    }
}

items.forEach(item => {
    if (item.isValid) {
        processItem(item);
    }
});
```

### Switch Statements
```javascript
// ✅ Good - Clear switch formatting
switch (user.status) {
    case 'active':
        processActiveUser(user);
        break;

    case 'pending':
        queueUser(user);
        break;

    case 'suspended':
        logSuspension(user);
        break;

    default:
        throw new Error(`Unknown user status: ${user.status}`);
}
```

## 🎯 Object and Array Formatting

### Object Literals
```javascript
// ✅ Good - Clear object formatting
const user = {
    id: 1,
    name: 'John Doe',
    email: 'john@example.com',
    isActive: true,
    preferences: {
        theme: 'dark',
        notifications: true,
        language: 'en'
    }
};

// ✅ Good - Long object formatting
const config = {
    apiBaseUrl: 'https://api.example.com',
    timeout: 5000,
    retryAttempts: 3,
    cacheEnabled: true,
    debugMode: false
};
```

### Arrays
```javascript
// ✅ Good - Clear array formatting
const items = [
    'apple',
    'banana',
    'cherry',
    'date'
];

// ✅ Good - Long array formatting
const permissions = [
    'read:users',
    'write:users',
    'delete:users',
    'read:posts',
    'write:posts',
    'delete:posts'
];
```

## 🚀 Function Formatting

### Function Declarations
```javascript
// ✅ Good - Clear function formatting
function calculateTotal(items, taxRate) {
    const subtotal = items.reduce((sum, item) => sum + item.price, 0);
    const tax = subtotal * taxRate;
    return subtotal + tax;
}

// ✅ Good - Long parameter list formatting
function createUser(
    name,
    email,
    phone,
    address,
    preferences,
    isVip,
    hasLoyaltyCard
) {
    // Implementation
}
```

### Arrow Functions
```javascript
// ✅ Good - Simple arrow functions
const double = x => x * 2;
const isEven = n => n % 2 === 0;

// ✅ Good - Complex arrow functions
const processUsers = users => {
    return users
        .filter(user => user.isActive)
        .map(user => ({
            id: user.id,
            name: user.name,
            email: user.email
        }));
};
```

## 🎨 Comments and Documentation

### Comment Formatting
```javascript
// ✅ Good - Single-line comments
// This function calculates the total price including tax
function calculateTotal(items, taxRate) {
    // Implementation
}

// ✅ Good - Multi-line comments
/*
 * This function processes user data and validates it
 * before saving to the database. It handles various
 * edge cases and provides detailed error messages.
 */
function processUserData(userData) {
    // Implementation
}
```

### JSDoc Formatting
```javascript
/**
 * Calculates the total price for a list of items
 * @param {Array} items - Array of item objects with price property
 * @param {number} taxRate - Tax rate as a decimal (e.g., 0.08 for 8%)
 * @returns {number} The total price including tax
 * @throws {Error} When items array is empty or taxRate is negative
 */
function calculateTotal(items, taxRate) {
    // Implementation
}
```

## 🎯 Quick Checklist

- [ ] Is indentation consistent throughout the file?
- [ ] Are there appropriate blank lines between logical sections?
- [ ] Are lines kept to a reasonable length (80-120 characters)?
- [ ] Is spacing consistent around operators and keywords?
- [ ] Are braces and parentheses used consistently?
- [ ] Are imports/exports properly organized?
- [ ] Are comments properly formatted?
- [ ] Is the overall structure easy to scan?

## 💡 Pro Tips

1. **Use a formatter** - Tools like Prettier, ESLint, or your IDE's formatter can handle most formatting automatically
2. **Set up your IDE** - Configure your editor to format on save
3. **Use a style guide** - Follow established style guides like Airbnb, Google, or Standard
4. **Be consistent** - Pick a style and stick with it throughout the project
5. **Review formatting in code reviews** - Make formatting part of your review process

## 🚀 Tools and Automation

### Prettier Configuration
```json
{
    "semi": true,
    "trailingComma": "es5",
    "singleQuote": true,
    "printWidth": 80,
    "tabWidth": 4,
    "useTabs": false
}
```

### ESLint Configuration
```json
{
    "extends": ["eslint:recommended", "prettier"],
    "rules": {
        "indent": ["error", 4],
        "quotes": ["error", "single"],
        "semi": ["error", "always"],
        "no-trailing-spaces": "error",
        "eol-last": "error"
    }
}
```

## 🎨 Language-Specific Guidelines

### JavaScript/TypeScript
```javascript
// ✅ Good - JavaScript formatting
const user = {
    name: 'John',
    age: 30,
    isActive: true
};

function greetUser(user) {
    return `Hello, ${user.name}!`;
}
```

### Python
```python
# ✅ Good - Python formatting
def calculate_total(items, tax_rate):
    subtotal = sum(item.price for item in items)
    tax = subtotal * tax_rate
    return subtotal + tax
```

### Java
```java
// ✅ Good - Java formatting
public class UserService {
    private Database database;

    public UserService(Database database) {
        this.database = database;
    }

    public User getUserById(String id) {
        return database.findById(id);
    }
}
```

---

*"Programs must be written for people to read, and only incidentally for machines to execute."* - Harold Abelson

Remember: Beautiful code is not just about functionality - it's about making your code a joy to read and work with! 🎉
