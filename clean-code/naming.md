# 🏷️ Naming Things - The Art of Good Names

Naming is one of the most important skills in programming! Good names make your code self-documenting and easy to understand.

## 🌟 Why Good Names Matter

Think of variable names like street signs - they help you navigate your code without getting lost!

```javascript
// ❌ Bad - What does this mean?
const d = new Date();
const u = users.filter(x => x.a > 18);

// ✅ Good - Crystal clear!
const currentDate = new Date();
const adultUsers = users.filter(user => user.age > 18);
```

## 📝 Rules for Great Names

### 1. Use Intention-Revealing Names
```javascript
// ❌ Bad
const d; // What kind of data?
const elapsedTimeInDays; // Too verbose

// ✅ Good
const daysSinceCreation;
const userAccount;
const isLoggedIn;
```

### 2. Avoid Disinformation
```javascript
// ❌ Bad - This is NOT an array!
const accountList = {};

// ✅ Good
const accountMap = {};
const accounts = [];
```

### 3. Make Meaningful Distinctions
```javascript
// ❌ Bad - No real difference
const user1, user2, user3;

// ✅ Good - Clear purpose
const currentUser, targetUser, adminUser;
```

### 4. Use Pronounceable Names
```javascript
// ❌ Bad
const genymdhms;
const modymdhms;

// ✅ Good
const generationTimestamp;
const modificationTimestamp;
```

### 5. Use Searchable Names
```javascript
// ❌ Bad - Hard to find
const MAX_CLASSES_PER_STUDENT = 7;

// ✅ Good - Easy to search for
const MAX_CLASSES_PER_STUDENT = 7;
```

## 🎯 Naming Conventions

### Variables and Functions
```javascript
// Use camelCase for variables and functions
const userName = "john_doe";
const isUserActive = true;

function calculateTotalPrice() {
    // function body
}
```

### Constants
```javascript
// Use UPPER_SNAKE_CASE for constants
const MAX_RETRY_ATTEMPTS = 3;
const API_BASE_URL = "https://api.example.com";
```

### Classes
```javascript
// Use PascalCase for classes
class UserAccount {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }
}
```

## 🚫 Common Naming Mistakes

### 1. Single Letter Variables (except in loops)
```javascript
// ❌ Bad
const a = users.filter(u => u.a > 18);

// ✅ Good
const adultUsers = users.filter(user => user.age > 18);
```

### 2. Abbreviations
```javascript
// ❌ Bad
const usr, pwd, db, cfg;

// ✅ Good
const user, password, database, config;
```

### 3. Hungarian Notation
```javascript
// ❌ Bad
const strUserName;
const intUserAge;
const boolIsActive;

// ✅ Good
const userName;
const userAge;
const isActive;
```

## 🎨 Naming Patterns

### Boolean Variables
```javascript
// Use is, has, can, should prefixes
const isLoggedIn = true;
const hasPermission = false;
const canEdit = true;
const shouldValidate = false;
```

### Functions
```javascript
// Use verbs for actions
function calculateTotal() { }
function validateInput() { }
function sendEmail() { }
function getUserById() { }
```

### Classes
```javascript
// Use nouns for classes
class User { }
class EmailService { }
class DatabaseConnection { }
```

## 🧪 Practice Examples

### Before (Messy Names)
```javascript
function calc(u, p) {
    const t = 0;
    for (let i = 0; i < u.length; i++) {
        t += u[i] * p[i];
    }
    return t;
}
```

### After (Clean Names)
```javascript
function calculateTotalPrice(unitPrices, quantities) {
    const totalPrice = 0;
    for (let i = 0; i < unitPrices.length; i++) {
        totalPrice += unitPrices[i] * quantities[i];
    }
    return totalPrice;
}
```

## 🎯 Quick Checklist

- [ ] Does the name reveal its purpose?
- [ ] Is it easy to pronounce?
- [ ] Can you search for it easily?
- [ ] Does it avoid abbreviations?
- [ ] Is it consistent with the codebase?
- [ ] Would a new developer understand it?

## 💡 Pro Tips

1. **Spend time thinking about names** - Good names save time in the long run
2. **Don't be afraid to rename** - If you find a better name, change it!
3. **Use your IDE's refactoring tools** - They make renaming safe and easy
4. **Ask yourself: "What does this represent?"** - The answer should be in the name

---

*"There are only two hard things in Computer Science: cache invalidation and naming things."* - Phil Karlton

Remember: Good names are like good friends - they make life easier and more enjoyable! 🎉