# ⚠️ Error Handling - Graceful Failure and Recovery

Error handling is like having a safety net - it catches problems before they crash your application and helps users understand what went wrong. Good error handling makes your application robust and user-friendly!

## 🌟 Why Error Handling Matters

Think of error handling like a good friend who helps you when things go wrong. Instead of leaving you confused and frustrated, they explain what happened and help you figure out what to do next!

```javascript
// ❌ Bad - No error handling
function divide(a, b) {
    return a / b; // Crashes if b is 0!
}

// ✅ Good - Proper error handling
function divide(a, b) {
    if (b === 0) {
        throw new Error('Cannot divide by zero');
    }
    return a / b;
}
```

## 🎯 The Golden Rules of Error Handling

### 1. Fail Fast and Fail Loud
```javascript
// ✅ Good - Check for errors early
function processUser(user) {
    if (!user) {
        throw new Error('User is required');
    }
    if (!user.email) {
        throw new Error('User email is required');
    }
    if (!isValidEmail(user.email)) {
        throw new Error('Invalid email format');
    }

    // Process the user
    return saveUser(user);
}
```

### 2. Be Specific About Errors
```javascript
// ❌ Bad - Generic error
function validateUser(user) {
    if (!user || !user.email) {
        throw new Error('Invalid user');
    }
}

// ✅ Good - Specific error messages
function validateUser(user) {
    if (!user) {
        throw new Error('User object is required');
    }
    if (!user.email) {
        throw new Error('User email is required');
    }
    if (!isValidEmail(user.email)) {
        throw new Error(`Invalid email format: ${user.email}`);
    }
}
```

### 3. Handle Errors at the Right Level
```javascript
// ❌ Bad - Handling errors too low
function saveUser(user) {
    try {
        database.save(user);
    } catch (error) {
        // This is too low-level - the caller doesn't know what to do
        console.log('Database error');
    }
}

// ✅ Good - Let errors bubble up to the right level
function saveUser(user) {
    return database.save(user); // Let the caller handle the error
}

// Handle at the appropriate level
async function createUser(userData) {
    try {
        const user = await saveUser(userData);
        return { success: true, user };
    } catch (error) {
        logger.error('Failed to create user:', error);
        return { success: false, error: 'Unable to create user' };
    }
}
```

## 🚨 Types of Errors

### 1. Validation Errors
```javascript
// ✅ Good - Input validation
function validateEmail(email) {
    if (!email) {
        throw new ValidationError('Email is required');
    }
    if (typeof email !== 'string') {
        throw new ValidationError('Email must be a string');
    }
    if (!email.includes('@')) {
        throw new ValidationError('Email must contain @ symbol');
    }
    if (!email.includes('.')) {
        throw new ValidationError('Email must contain a domain');
    }
    return true;
}
```

### 2. Business Logic Errors
```javascript
// ✅ Good - Business rule validation
function processOrder(order) {
    if (order.items.length === 0) {
        throw new BusinessError('Order must contain at least one item');
    }
    if (order.total < 0) {
        throw new BusinessError('Order total cannot be negative');
    }
    if (order.customer.balance < order.total) {
        throw new BusinessError('Insufficient funds');
    }

    return processPayment(order);
}
```

### 3. System Errors
```javascript
// ✅ Good - System error handling
async function fetchUserData(userId) {
    try {
        const response = await fetch(`/api/users/${userId}`);
        if (!response.ok) {
            throw new SystemError(`HTTP ${response.status}: ${response.statusText}`);
        }
        return await response.json();
    } catch (error) {
        if (error instanceof SystemError) {
            throw error;
        }
        throw new SystemError('Network request failed');
    }
}
```

## 🛠️ Error Handling Patterns

### 1. Try-Catch Blocks
```javascript
// ✅ Good - Proper try-catch usage
async function processPayment(paymentData) {
    try {
        const result = await paymentService.charge(paymentData);
        return { success: true, transactionId: result.id };
    } catch (error) {
        if (error.code === 'INSUFFICIENT_FUNDS') {
            throw new PaymentError('Insufficient funds in account');
        }
        if (error.code === 'CARD_DECLINED') {
            throw new PaymentError('Credit card was declined');
        }
        if (error.code === 'NETWORK_ERROR') {
            throw new PaymentError('Payment service is temporarily unavailable');
        }
        throw new PaymentError('Payment processing failed');
    }
}
```

### 2. Error Boundaries (React)
```javascript
// ✅ Good - React error boundary
class ErrorBoundary extends React.Component {
    constructor(props) {
        super(props);
        this.state = { hasError: false, error: null };
    }

    static getDerivedStateFromError(error) {
        return { hasError: true, error };
    }

    componentDidCatch(error, errorInfo) {
        console.error('Error caught by boundary:', error, errorInfo);
        // Send error to logging service
        this.logError(error, errorInfo);
    }

    render() {
        if (this.state.hasError) {
            return (
                <div className="error-boundary">
                    <h2>Something went wrong</h2>
                    <p>We're sorry, but something unexpected happened.</p>
                    <button onClick={() => this.setState({ hasError: false })}>
                        Try again
                    </button>
                </div>
            );
        }

        return this.props.children;
    }
}
```

### 3. Promise Error Handling
```javascript
// ✅ Good - Promise error handling
async function fetchUserData(userId) {
    return fetch(`/api/users/${userId}`)
        .then(response => {
            if (!response.ok) {
                throw new Error(`HTTP ${response.status}`);
            }
            return response.json();
        })
        .catch(error => {
            console.error('Failed to fetch user:', error);
            throw new Error('Unable to load user data');
        });
}

// ✅ Good - Async/await error handling
async function fetchUserData(userId) {
    try {
        const response = await fetch(`/api/users/${userId}`);
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}`);
        }
        return await response.json();
    } catch (error) {
        console.error('Failed to fetch user:', error);
        throw new Error('Unable to load user data');
    }
}
```

## 🎨 Custom Error Classes

### Creating Specific Error Types
```javascript
// ✅ Good - Custom error classes
class ValidationError extends Error {
    constructor(message, field) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
    }
}

class BusinessError extends Error {
    constructor(message, code) {
        super(message);
        this.name = 'BusinessError';
        this.code = code;
    }
}

class SystemError extends Error {
    constructor(message, originalError) {
        super(message);
        this.name = 'SystemError';
        this.originalError = originalError;
    }
}

// Usage
function validateUser(user) {
    if (!user.email) {
        throw new ValidationError('Email is required', 'email');
    }
    if (user.age < 18) {
        throw new BusinessError('User must be 18 or older', 'AGE_REQUIREMENT');
    }
}
```

### Error Hierarchy
```javascript
// ✅ Good - Error hierarchy
class AppError extends Error {
    constructor(message, code) {
        super(message);
        this.name = 'AppError';
        this.code = code;
        this.timestamp = new Date().toISOString();
    }
}

class ValidationError extends AppError {
    constructor(message, field) {
        super(message, 'VALIDATION_ERROR');
        this.name = 'ValidationError';
        this.field = field;
    }
}

class BusinessError extends AppError {
    constructor(message, businessRule) {
        super(message, 'BUSINESS_ERROR');
        this.name = 'BusinessError';
        this.businessRule = businessRule;
    }
}
```

## 🔄 Error Recovery Strategies

### 1. Retry Logic
```javascript
// ✅ Good - Retry with exponential backoff
async function fetchWithRetry(url, maxRetries = 3) {
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            const response = await fetch(url);
            if (response.ok) {
                return await response.json();
            }
            throw new Error(`HTTP ${response.status}`);
        } catch (error) {
            if (attempt === maxRetries) {
                throw new Error(`Failed after ${maxRetries} attempts: ${error.message}`);
            }

            // Exponential backoff: 1s, 2s, 4s
            const delay = Math.pow(2, attempt - 1) * 1000;
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }
}
```

### 2. Fallback Values
```javascript
// ✅ Good - Fallback values
async function getUserPreferences(userId) {
    try {
        return await userService.getPreferences(userId);
    } catch (error) {
        console.warn('Failed to load user preferences, using defaults:', error);
        return getDefaultPreferences();
    }
}

function getDefaultPreferences() {
    return {
        theme: 'light',
        language: 'en',
        notifications: true
    };
}
```

### 3. Circuit Breaker Pattern
```javascript
// ✅ Good - Circuit breaker
class CircuitBreaker {
    constructor(threshold = 5, timeout = 60000) {
        this.failureCount = 0;
        this.threshold = threshold;
        this.timeout = timeout;
        this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
        this.nextAttempt = Date.now();
    }

    async execute(operation) {
        if (this.state === 'OPEN') {
            if (Date.now() < this.nextAttempt) {
                throw new Error('Circuit breaker is OPEN');
            }
            this.state = 'HALF_OPEN';
        }

        try {
            const result = await operation();
            this.onSuccess();
            return result;
        } catch (error) {
            this.onFailure();
            throw error;
        }
    }

    onSuccess() {
        this.failureCount = 0;
        this.state = 'CLOSED';
    }

    onFailure() {
        this.failureCount++;
        if (this.failureCount >= this.threshold) {
            this.state = 'OPEN';
            this.nextAttempt = Date.now() + this.timeout;
        }
    }
}
```

## 📝 Error Logging and Monitoring

### Structured Logging
```javascript
// ✅ Good - Structured error logging
class ErrorLogger {
    static logError(error, context = {}) {
        const errorInfo = {
            timestamp: new Date().toISOString(),
            message: error.message,
            stack: error.stack,
            name: error.name,
            code: error.code,
            context: context
        };

        // Send to logging service
        this.sendToLoggingService(errorInfo);

        // Also log to console in development
        if (process.env.NODE_ENV === 'development') {
            console.error('Error logged:', errorInfo);
        }
    }

    static sendToLoggingService(errorInfo) {
        // Implementation for sending to logging service
        // e.g., Sentry, LogRocket, etc.
    }
}

// Usage
try {
    await riskyOperation();
} catch (error) {
    ErrorLogger.logError(error, {
        userId: user.id,
        operation: 'riskyOperation',
        timestamp: Date.now()
    });
    throw error;
}
```

### Error Metrics
```javascript
// ✅ Good - Error metrics
class ErrorMetrics {
    static recordError(errorType, severity = 'medium') {
        // Record error metrics
        this.incrementCounter(`errors.${errorType}.${severity}`);
        this.incrementCounter('errors.total');
    }

    static incrementCounter(metric) {
        // Send to metrics service
        // e.g., DataDog, New Relic, etc.
    }
}

// Usage
try {
    await processPayment(paymentData);
} catch (error) {
    ErrorMetrics.recordError('payment_failed', 'high');
    throw error;
}
```

## 🎯 User-Friendly Error Messages

### Error Message Guidelines
```javascript
// ❌ Bad - Technical error messages
function processUser(user) {
    try {
        return database.save(user);
    } catch (error) {
        throw new Error('SQLSTATE[23000]: Integrity constraint violation');
    }
}

// ✅ Good - User-friendly error messages
function processUser(user) {
    try {
        return database.save(user);
    } catch (error) {
        if (error.code === 'DUPLICATE_EMAIL') {
            throw new Error('An account with this email already exists');
        }
        if (error.code === 'INVALID_DATA') {
            throw new Error('Please check your information and try again');
        }
        throw new Error('Unable to save your information. Please try again later');
    }
}
```

### Error Message Templates
```javascript
// ✅ Good - Error message templates
const ERROR_MESSAGES = {
    VALIDATION: {
        REQUIRED: (field) => `${field} is required`,
        INVALID_FORMAT: (field) => `Please enter a valid ${field}`,
        TOO_SHORT: (field, min) => `${field} must be at least ${min} characters`,
        TOO_LONG: (field, max) => `${field} must be no more than ${max} characters`
    },
    BUSINESS: {
        INSUFFICIENT_FUNDS: 'You don\'t have enough funds for this transaction',
        ITEM_OUT_OF_STOCK: 'This item is currently out of stock',
        ORDER_TOO_LARGE: 'Your order exceeds the maximum allowed quantity'
    },
    SYSTEM: {
        NETWORK_ERROR: 'Please check your internet connection and try again',
        SERVICE_UNAVAILABLE: 'This service is temporarily unavailable',
        TIMEOUT: 'The request took too long. Please try again'
    }
};

// Usage
function validateEmail(email) {
    if (!email) {
        throw new ValidationError(ERROR_MESSAGES.VALIDATION.REQUIRED('email'));
    }
    if (!email.includes('@')) {
        throw new ValidationError(ERROR_MESSAGES.VALIDATION.INVALID_FORMAT('email'));
    }
}
```

## 🧪 Testing Error Handling

### Testing Error Scenarios
```javascript
// ✅ Good - Testing error handling
describe('UserService', () => {
    describe('createUser', () => {
        it('should throw ValidationError when email is missing', async () => {
            const userData = { name: 'John' };

            await expect(userService.createUser(userData))
                .rejects
                .toThrow(ValidationError);
        });

        it('should throw BusinessError when user is underage', async () => {
            const userData = {
                name: 'John',
                email: 'john@example.com',
                age: 16
            };

            await expect(userService.createUser(userData))
                .rejects
                .toThrow(BusinessError);
        });

        it('should handle database errors gracefully', async () => {
            const userData = {
                name: 'John',
                email: 'john@example.com',
                age: 25
            };

            // Mock database to throw error
            database.save.mockRejectedValue(new Error('Database connection failed'));

            await expect(userService.createUser(userData))
                .rejects
                .toThrow('Unable to create user account');
        });
    });
});
```

## 🎯 Quick Checklist

- [ ] Are errors caught at the appropriate level?
- [ ] Are error messages specific and helpful?
- [ ] Are custom error classes used for different error types?
- [ ] Is error logging implemented?
- [ ] Are errors handled gracefully without crashing the app?
- [ ] Are user-friendly error messages provided?
- [ ] Is error recovery implemented where appropriate?
- [ ] Are error scenarios tested?

## 💡 Pro Tips

1. **Fail fast** - Check for errors early and throw meaningful exceptions
2. **Be specific** - Generic error messages don't help anyone
3. **Log everything** - You can't fix what you can't see
4. **Test error paths** - Error handling code needs testing too
5. **Use error boundaries** - Prevent one error from crashing the entire app
6. **Provide recovery options** - Give users a way to fix the problem
7. **Monitor errors** - Set up alerts for critical errors

## 🚀 Advanced Error Handling

### Global Error Handler
```javascript
// ✅ Good - Global error handler
window.addEventListener('error', (event) => {
    console.error('Global error:', event.error);
    ErrorLogger.logError(event.error, {
        type: 'javascript_error',
        filename: event.filename,
        lineno: event.lineno,
        colno: event.colno
    });
});

window.addEventListener('unhandledrejection', (event) => {
    console.error('Unhandled promise rejection:', event.reason);
    ErrorLogger.logError(event.reason, {
        type: 'unhandled_promise_rejection'
    });
});
```

### Error Recovery Strategies
```javascript
// ✅ Good - Multiple recovery strategies
class ResilientService {
    constructor() {
        this.circuitBreaker = new CircuitBreaker();
        this.retryCount = 0;
        this.maxRetries = 3;
    }

    async executeWithResilience(operation) {
        try {
            return await this.circuitBreaker.execute(operation);
        } catch (error) {
            if (this.retryCount < this.maxRetries) {
                this.retryCount++;
                await this.delay(Math.pow(2, this.retryCount) * 1000);
                return this.executeWithResilience(operation);
            }

            // Try fallback
            return this.fallbackOperation();
        }
    }

    async fallbackOperation() {
        // Implement fallback logic
        throw new Error('All recovery strategies failed');
    }

    delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
}
```

---

*"The best error handling is the kind that prevents errors from happening in the first place."* - Unknown

Remember: Good error handling is like a good friend - it's there when you need it, helps you understand what went wrong, and gives you a path forward! 🎉
