# 🔧 Functions - Small, Focused, and Powerful

Functions are the building blocks of clean code! Think of them as small, specialized tools in your toolbox - each one does one thing really well.

## 🌟 Why Small Functions Matter

Imagine a Swiss Army knife vs. a specialized screwdriver. The Swiss Army knife can do many things, but it's not great at any one thing. A good function is like a specialized tool - it does one thing perfectly!

```javascript
// ❌ Bad - Does too many things
function processUserData(userData) {
    // Validate input
    if (!userData || !userData.email) {
        throw new Error('Invalid user data');
    }

    // Format email
    const formattedEmail = userData.email.toLowerCase().trim();

    // Save to database
    database.save(formattedEmail);

    // Send welcome email
    emailService.send(formattedEmail, 'Welcome!');

    // Log the action
    logger.log('User processed: ' + formattedEmail);

    return { success: true, email: formattedEmail };
}

// ✅ Good - Each function has one responsibility
function validateUserData(userData) {
    if (!userData || !userData.email) {
        throw new Error('Invalid user data');
    }
}

function formatEmail(email) {
    return email.toLowerCase().trim();
}

function saveUser(email) {
    return database.save(email);
}

function sendWelcomeEmail(email) {
    return emailService.send(email, 'Welcome!');
}

function logUserAction(email) {
    logger.log('User processed: ' + email);
}

function processUserData(userData) {
    validateUserData(userData);
    const formattedEmail = formatEmail(userData.email);
    saveUser(formattedEmail);
    sendWelcomeEmail(formattedEmail);
    logUserAction(formattedEmail);
    return { success: true, email: formattedEmail };
}
```

## 📏 The Single Responsibility Principle

Each function should have **one reason to change**. If you can't describe what a function does in one sentence without using "and", it's probably doing too much!

```javascript
// ❌ Bad - Multiple responsibilities
function calculateAndDisplayAndSaveTotal(items) {
    let total = 0;
    for (let item of items) {
        total += item.price * item.quantity;
    }

    document.getElementById('total').innerHTML = total;
    localStorage.setItem('total', total);
    return total;
}

// ✅ Good - Single responsibility each
function calculateTotal(items) {
    return items.reduce((total, item) => total + (item.price * item.quantity), 0);
}

function displayTotal(total) {
    document.getElementById('total').innerHTML = total;
}

function saveTotal(total) {
    localStorage.setItem('total', total);
}

function calculateAndDisplayAndSaveTotal(items) {
    const total = calculateTotal(items);
    displayTotal(total);
    saveTotal(total);
    return total;
}
```

## 📝 Function Parameters

### The Magic Number: 3 or Fewer
Functions with more than 3 parameters are hard to understand and use. If you need more, consider using an object!

```javascript
// ❌ Bad - Too many parameters
function createUser(firstName, lastName, email, phone, address, city, state, zipCode, country, dateOfBirth) {
    // Function body
}

// ✅ Good - Use an object for many parameters
function createUser(userData) {
    const { firstName, lastName, email, phone, address, city, state, zipCode, country, dateOfBirth } = userData;
    // Function body
}

// Even better - Group related data
function createUser(personalInfo, contactInfo, addressInfo) {
    // Function body
}
```

### Parameter Order Matters
Put the most important parameters first, and optional ones last:

```javascript
// ✅ Good - Required first, optional last
function sendEmail(recipient, subject, body, attachments = [], priority = 'normal') {
    // Function body
}

// Usage
sendEmail('user@example.com', 'Hello', 'How are you?');
sendEmail('user@example.com', 'Hello', 'How are you?', ['file.pdf'], 'high');
```

## 🔄 Return Values

### Be Consistent
Always return the same type of data, or nothing at all:

```javascript
// ❌ Bad - Inconsistent return types
function processData(data) {
    if (data.length === 0) {
        return null;
    }
    if (data.length === 1) {
        return data[0];
    }
    return data; // Array
}

// ✅ Good - Consistent return type
function processData(data) {
    if (data.length === 0) {
        return [];
    }
    return data.map(item => transformItem(item));
}
```

### Avoid Side Effects
Functions should either:
- **Do something** (change state, send email, save data)
- **Return something** (calculate, transform, validate)

But not both!

```javascript
// ❌ Bad - Side effect + return value
function calculateAndSaveTotal(items) {
    const total = items.reduce((sum, item) => sum + item.price, 0);
    database.save('total', total); // Side effect
    return total; // Return value
}

// ✅ Good - Separate concerns
function calculateTotal(items) {
    return items.reduce((sum, item) => sum + item.price, 0);
}

function saveTotal(total) {
    database.save('total', total);
}
```

## 🎯 Function Naming

### Use Verbs for Actions
Function names should clearly describe what they do:

```javascript
// ✅ Good - Clear action verbs
function calculateTotal() { }
function validateInput() { }
function sendEmail() { }
function getUserById() { }
function isUserLoggedIn() { }
function hasPermission() { }
```

### Avoid Generic Names
```javascript
// ❌ Bad - Too generic
function process() { }
function handle() { }
function doStuff() { }
function work() { }

// ✅ Good - Specific and clear
function validateUserInput() { }
function sendWelcomeEmail() { }
function calculateOrderTotal() { }
function formatPhoneNumber() { }
```

## 🧪 Function Length

### The 20-Line Rule
Most functions should be 20 lines or less. If they're longer, they're probably doing too much:

```javascript
// ❌ Bad - Too long and complex
function processOrder(order) {
    // Validate order
    if (!order || !order.items || order.items.length === 0) {
        throw new Error('Invalid order');
    }

    // Check inventory
    for (let item of order.items) {
        const stock = inventory.getStock(item.productId);
        if (stock < item.quantity) {
            throw new Error(`Insufficient stock for ${item.productId}`);
        }
    }

    // Calculate total
    let total = 0;
    for (let item of order.items) {
        const product = products.getById(item.productId);
        total += product.price * item.quantity;
    }

    // Apply discount
    if (order.customer.isVip) {
        total *= 0.9; // 10% discount
    }

    // Process payment
    const paymentResult = paymentProcessor.charge(order.paymentMethod, total);
    if (!paymentResult.success) {
        throw new Error('Payment failed');
    }

    // Update inventory
    for (let item of order.items) {
        inventory.reduceStock(item.productId, item.quantity);
    }

    // Send confirmation
    emailService.send(order.customer.email, `Order confirmed for $${total}`);

    return { success: true, orderId: generateOrderId(), total };
}

// ✅ Good - Broken into smaller functions
function validateOrder(order) {
    if (!order || !order.items || order.items.length === 0) {
        throw new Error('Invalid order');
    }
}

function checkInventory(items) {
    for (let item of items) {
        const stock = inventory.getStock(item.productId);
        if (stock < item.quantity) {
            throw new Error(`Insufficient stock for ${item.productId}`);
        }
    }
}

function calculateOrderTotal(items, customer) {
    let total = items.reduce((sum, item) => {
        const product = products.getById(item.productId);
        return sum + (product.price * item.quantity);
    }, 0);

    if (customer.isVip) {
        total *= 0.9; // 10% discount
    }

    return total;
}

function processPayment(paymentMethod, amount) {
    const result = paymentProcessor.charge(paymentMethod, amount);
    if (!result.success) {
        throw new Error('Payment failed');
    }
    return result;
}

function updateInventory(items) {
    items.forEach(item => {
        inventory.reduceStock(item.productId, item.quantity);
    });
}

function sendOrderConfirmation(customer, total) {
    emailService.send(customer.email, `Order confirmed for $${total}`);
}

function processOrder(order) {
    validateOrder(order);
    checkInventory(order.items);
    const total = calculateOrderTotal(order.items, order.customer);
    processPayment(order.paymentMethod, total);
    updateInventory(order.items);
    sendOrderConfirmation(order.customer, total);

    return { success: true, orderId: generateOrderId(), total };
}
```

## 🔍 Error Handling in Functions

### Fail Fast
Check for errors early and throw meaningful exceptions:

```javascript
// ✅ Good - Fail fast with clear messages
function divide(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
        throw new Error('Both arguments must be numbers');
    }
    if (b === 0) {
        throw new Error('Cannot divide by zero');
    }
    return a / b;
}
```

### Use Specific Exception Types
```javascript
// ✅ Good - Specific exceptions
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = 'ValidationError';
    }
}

class BusinessLogicError extends Error {
    constructor(message) {
        super(message);
        this.name = 'BusinessLogicError';
    }
}

function validateUser(user) {
    if (!user.email) {
        throw new ValidationError('Email is required');
    }
    if (user.age < 18) {
        throw new BusinessLogicError('User must be 18 or older');
    }
}
```

## 🎨 Function Composition

### Build Complex Behavior from Simple Functions
```javascript
// ✅ Good - Composing simple functions
function isAdult(user) {
    return user.age >= 18;
}

function hasValidEmail(user) {
    return user.email && user.email.includes('@');
}

function canCreateAccount(user) {
    return isAdult(user) && hasValidEmail(user);
}

// Usage
if (canCreateAccount(user)) {
    createAccount(user);
}
```

## 🧪 Testing Functions

### Make Functions Easy to Test
```javascript
// ✅ Good - Pure function, easy to test
function calculateTax(amount, taxRate) {
    return amount * taxRate;
}

// Test
console.assert(calculateTax(100, 0.1) === 10);
console.assert(calculateTax(200, 0.05) === 10);
```

## 🎯 Quick Checklist

- [ ] Does the function do only one thing?
- [ ] Can you describe it in one sentence?
- [ ] Does it have 3 or fewer parameters?
- [ ] Is it 20 lines or less?
- [ ] Does it have a clear, descriptive name?
- [ ] Does it return consistent data types?
- [ ] Does it handle errors appropriately?
- [ ] Is it easy to test?

## 💡 Pro Tips

1. **Start with the name** - If you can't name it clearly, it's probably too complex
2. **Write the test first** - This helps you think about what the function should do
3. **Refactor mercilessly** - Don't be afraid to break big functions into smaller ones
4. **Use your IDE's refactoring tools** - They make splitting functions safe and easy
5. **Read your code out loud** - If it sounds awkward, it probably is!

## 🚀 Common Patterns

### The Command Pattern
```javascript
// ✅ Good - Command pattern for actions
function createUserCommand(userData) {
    return {
        execute: () => userService.create(userData),
        undo: () => userService.delete(userData.id),
        description: `Create user ${userData.email}`
    };
}
```

### The Strategy Pattern
```javascript
// ✅ Good - Strategy pattern for algorithms
const paymentStrategies = {
    creditCard: (amount) => creditCardProcessor.charge(amount),
    paypal: (amount) => paypalProcessor.charge(amount),
    bitcoin: (amount) => bitcoinProcessor.charge(amount)
};

function processPayment(method, amount) {
    const strategy = paymentStrategies[method];
    if (!strategy) {
        throw new Error(`Unsupported payment method: ${method}`);
    }
    return strategy(amount);
}
```

---

*"The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that."* - Robert C. Martin

Remember: Good functions are like good friends - they're reliable, do what they say they'll do, and make your life easier! 🎉
