# Intro to TypeScript Guide

## What is TypeScript?

**TypeScript is JavaScript with types.** Think of it as JavaScript's helpful assistant that catches mistakes before your code runs.

### Why use TypeScript?

- **Catch errors early:** Find bugs while writing code, not when users find them
- **Better IDE support:** Get autocomplete, refactoring, and navigation features
- **Self-documenting code:** Types serve as inline documentation
- **Easier refactoring:** Confidently rename variables and functions across large codebases
- **Team collaboration:** Types help teammates understand what your functions expect

### JavaScript vs TypeScript

**JavaScript (error-prone):**
```javascript
function greetUser(user) {
  return "Hello, " + user.name;
}

greetUser("John"); // Runtime error: Cannot read property 'name' of string
```

**TypeScript (catches errors early):**
```typescript
function greetUser(user: { name: string }) {
  return "Hello, " + user.name;
}

greetUser("John"); // TypeScript error: Argument of type 'string' is not assignable to parameter of type '{ name: string; }'
```

TypeScript catches this mistake **before** your code runs!

## Project Setup

Let's create a React TypeScript project to learn with practical examples.

- In your terminal, navigate to your Actualize directory and create a new project:
  ```
  npm create vite@latest
  ```

  _When prompted, enter a project name like `typescript-intro`, select the React framework, select the **TypeScript** variant_

- In the terminal, go to your new directory, install dependencies, and start the dev server:
  ```
  cd typescript-intro
  npm install
  npm run dev
  ```

- In the terminal, open your text editor:
  ```
  code .
  ```

- In your text editor, open the file **src/index.css** and replace all the CSS code with the following:

  ```css
  html {
    scroll-behavior: smooth;
  }
  ```

- In your browser, visit http://localhost:5173 to make sure your app is working!

  > In your terminal, use git to save a version of your code.
  > ```bash
  > git init
  > git add --all
  > git commit -m "Initial TypeScript project setup"
  > ```

## Understanding the Differences

Let's explore your new project structure. Notice these TypeScript-specific files:

- **src/App.tsx** (not .jsx) - TypeScript React components use `.tsx` extension
- **src/main.tsx** (not .jsx) - TypeScript entry point
- **tsconfig.json** - TypeScript configuration file

### File Extensions

| JavaScript | TypeScript | Purpose |
|------------|------------|---------|
| `.js` | `.ts` | Regular TypeScript files |
| `.jsx` | `.tsx` | React components with JSX |

## Basic Types

Let's start with the fundamentals. TypeScript adds type annotations to JavaScript.

## Understanding Type Inference

Before we dive into examples, let's understand **when TypeScript can figure out types automatically** (inference) vs **when you need to tell it explicitly**.

### When TypeScript CAN infer types (you don't need to write them):

```typescript
// ✅ TypeScript knows these are automatically:
const name = "Alice";           // TypeScript infers: string
const age = 25;                 // TypeScript infers: number
const isStudent = true;         // TypeScript infers: boolean
const numbers = [1, 2, 3];      // TypeScript infers: number[]
const fruits = ["apple", "banana"]; // TypeScript infers: string[]

// ✅ Function return types (often inferred):
function addNumbers(a: number, b: number) {
  return a + b; // TypeScript infers return type is 'number'
}
```

### When TypeScript CAN'T infer types well (you should write them):

```typescript
// ❌ TypeScript can't read your mind for function parameters:
function greetUser(name) { // Error: Parameter 'name' implicitly has an 'any' type
  return "Hello, " + name;
}

// ✅ You must specify parameter types:
function greetUser(name: string) {
  return "Hello, " + name; // Return type 'string' is inferred
}

// ❌ Empty arrays - TypeScript doesn't know what goes in them:
const items = []; // TypeScript infers: any[]

// ✅ Tell TypeScript what the array will contain:
const items: string[] = []; // or const items = [] as string[];

// ❌ Variables without initial values:
let userId; // TypeScript infers: any

// ✅ Specify the type:
let userId: number;
```

### Why declare types even when TypeScript can infer them?

**1. Documentation & Clarity:**
```typescript
// Less clear - what is this function supposed to return?
function calculateTax(amount) {
  return amount * 0.1;
}

// More clear - obvious this takes a number and returns a number
function calculateTax(amount: number): number {
  return amount * 0.1;
}
```

**2. Catch Errors Earlier:**
```typescript
// Without explicit typing, errors happen later:
const userAge = "25"; // Oops, this should be a number
const nextYear = userAge + 1; // Result: "251" (string concatenation!)

// With explicit typing, errors happen immediately:
const userAge: number = "25"; // Error: Type 'string' is not assignable to type 'number'
```

**3. Better IDE Support:**
When you explicitly type things, your editor can give you better autocomplete and error detection.

**4. Future-Proofing:**
```typescript
// If you later change how this array is used, explicit typing helps:
const userIds: number[] = []; // Clear intent: this will hold numbers

// Later in your code:
userIds.push("user123"); // Error caught immediately!
```

### 1. Basic Type Annotations

First, create the examples directory:
```bash
mkdir src/examples
```

Create a new file **src/examples/BasicTypes.tsx**:

```typescript
import React from 'react';

export function BasicTypes() {
  // Explicit types (you write the type annotation)
  const message: string = "Hello, TypeScript!";
  const count: number = 42;
  const isActive: boolean = true;
  
  // Inferred types (TypeScript figures it out automatically)
  const inferredString = "I'm automatically a string"; // TypeScript infers: string
  const inferredNumber = 100; // TypeScript infers: number
  const inferredBoolean = false; // TypeScript infers: boolean

  // Arrays - when to be explicit vs when inference works
  const explicitNumbers: number[] = [1, 2, 3, 4, 5]; // Explicit
  const inferredFruits = ["apple", "banana", "orange"]; // Inferred as string[]
  
  // Empty arrays need explicit types
  const emptyItems: string[] = []; // Must be explicit - TypeScript can't guess
  
  // Two ways to write array types (both are valid and identical):
  const colors: string[] = ["red", "green", "blue"];        // Method 1: type[]
  const sizes: Array<number> = [10, 20, 30];               // Method 2: Array<type>

  // Function parameters always need types, return types often inferred
  const calculateArea = (width: number, height: number) => {
    return width * height; // Return type 'number' is inferred
  };

  const area = calculateArea(10, 5); // TypeScript knows 'area' is a number

  // The 'any' type - turns off TypeScript checking (use sparingly!)
  const anything: any = "I can be anything!";
  const alsoAnything: any = 42;
  const stillAnything: any = { name: "John", age: 30 };
  // With 'any', TypeScript won't check types or give errors

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>Basic Types & Inference Example</h2>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>Explicit Types:</h3>
        <p><strong>Message:</strong> {message}</p>
        <p><strong>Count:</strong> {count}</p>
        <p><strong>Is Active:</strong> {isActive ? 'Yes' : 'No'}</p>
      </div>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>Inferred Types:</h3>
        <p><strong>Inferred String:</strong> {inferredString}</p>
        <p><strong>Inferred Number:</strong> {inferredNumber}</p>
        <p><strong>Inferred Boolean:</strong> {inferredBoolean ? 'Yes' : 'No'}</p>
      </div>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>Arrays:</h3>
        <p><strong>Explicit Numbers:</strong> [{explicitNumbers.join(', ')}]</p>
        <p><strong>Inferred Fruits:</strong> [{inferredFruits.join(', ')}]</p>
        <p><strong>Empty Items:</strong> [{emptyItems.join(', ') || 'Empty array'}]</p>
        <p><strong>Colors (Method 1):</strong> [{colors.join(', ')}]</p>
        <p><strong>Sizes (Method 2):</strong> [{sizes.join(', ')}]</p>
      </div>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>Function Example:</h3>
        <p><strong>Area (10 × 5):</strong> {area}</p>
      </div>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>The 'any' Type (use carefully!):</h3>
        <p><strong>Anything:</strong> {String(anything)}</p>
        <p><strong>Also Anything:</strong> {String(alsoAnything)}</p>
        <p><strong>Still Anything:</strong> {(stillAnything as any).name} (age: {(stillAnything as any).age})</p>
      </div>
      
      <div style={{ 
        marginTop: '20px', 
        padding: '15px', 
        backgroundColor: '#e6f3ff', 
        borderRadius: '5px' 
      }}>
        <h4>📋 Array Syntax: Two Ways, Same Result</h4>
        <div style={{ fontSize: '14px' }}>
          <p><strong>Method 1:</strong> <code>type[]</code> (most common)</p>
          <p><strong>Method 2:</strong> <code>Array&lt;type&gt;</code> (generic syntax)</p>
          <p><strong>Both are identical!</strong> Most developers prefer <code>number[]</code> because it's shorter and more readable. Use whichever you prefer, but be consistent in your project.</p>
        </div>
      </div>
      
      <div style={{ 
        marginTop: '15px', 
        padding: '15px', 
        backgroundColor: '#fff3cd', 
        borderRadius: '5px' 
      }}>
        <h4>⚠️ About the 'any' Type</h4>
        <div style={{ fontSize: '14px' }}>
          <p><strong>What it does:</strong> <code>any</code> tells TypeScript "this can be anything, don't check it"</p>
          <p><strong>When you might see it:</strong> Old JavaScript code, third-party libraries, or when you're not sure of the type</p>
          <p><strong>Why to avoid it:</strong> You lose all the benefits of TypeScript - no autocomplete, no error checking!</p>
          <p><strong>Better alternatives:</strong> Use specific types like <code>string</code>, <code>unknown</code>, or create an interface</p>
        </div>
      </div>
    </div>
  );
}
```

### 2. What is an Interface?

Before we dive into examples, let's understand **what an interface is** and **why we use them**.

**Think of an interface like a contract or blueprint** that describes what an object should look like.

#### JavaScript vs TypeScript Objects

**In JavaScript (no structure enforcement):**
```javascript
// Anyone can create any object shape - no consistency!
const user1 = { name: "Alice", age: 25 };
const user2 = { firstName: "Bob", years: 30 }; // Different structure!
const user3 = { name: "Charlie" }; // Missing age!

// This leads to errors:
function greetUser(user) {
  return `Hello, ${user.name}!`; // What if user.name doesn't exist?
}
```

**In TypeScript with interfaces (structure enforced):**
```typescript
// Define the "contract" - what every User object must have
interface User {
  name: string;
  age: number;
}

// Now ALL users must follow this structure
const user1: User = { name: "Alice", age: 25 }; // ✅ Valid
const user2: User = { firstName: "Bob", years: 30 }; // ❌ Error - wrong properties!
const user3: User = { name: "Charlie" }; // ❌ Error - missing age!

// Functions can trust the structure
function greetUser(user: User) {
  return `Hello, ${user.name}!`; // ✅ We KNOW user.name exists
}
```

#### Why Use Interfaces?

1. **Consistency** - All objects of the same type have the same structure
2. **Error Prevention** - TypeScript catches mistakes before your code runs
3. **Documentation** - Interfaces show other developers what properties an object should have
4. **Autocomplete** - Your editor knows what properties are available
5. **Refactoring Safety** - If you change an interface, TypeScript shows you everywhere that needs updating

#### Simple Interface Example

```typescript
// The interface (blueprint)
interface Car {
  brand: string;
  model: string;
  year: number;
}

// Using the interface
const myCar: Car = {
  brand: "Toyota",
  model: "Camry", 
  year: 2020
}; // ✅ Follows the Car interface

const invalidCar: Car = {
  brand: "Honda",
  // Missing model and year - TypeScript will complain!
}; // ❌ Error!
```

**Think of it this way:** If you're building a house, you follow blueprints to make sure it has all the required rooms and features. Interfaces are like blueprints for your objects!

### 3. Object Types and Interfaces in Practice

Create **src/examples/ObjectTypes.tsx**:

```typescript
import React from 'react';

// Step 1: Start with a simple interface (our "blueprint")
interface Person {
  name: string;
  age: number;
}

// Step 2: A more complex interface with different data types
interface User {
  id: number;
  name: string;
  email: string;
  isAdmin: boolean;
}

// Step 3: An interface with arrays and more properties
interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  inStock: boolean;
  tags: string[]; // Array of strings
}

export function ObjectTypes() {
  // Using our simple Person interface
  const person: Person = {
    name: "John Doe",
    age: 28
  }; // ✅ Must have exactly these properties!
  
  // Using the User interface - TypeScript ensures we have all required properties
  const user: User = {
    id: 1,
    name: "Alice Johnson",
    email: "alice@example.com",
    isAdmin: true
  }; // ✅ All User properties present!
  
  // Using the Product interface with array data
  const product: Product = {
    id: 101,
    name: "Wireless Headphones",
    price: 99.99,
    category: "Electronics",
    inStock: true,
    tags: ["wireless", "bluetooth", "audio"] // String array as defined in interface
  }; // ✅ Follows Product interface exactly!

  // This would cause an error (uncomment to see):
  // const invalidUser: User = {
  //   name: "Bob"
  //   // Missing: id, email, isAdmin - TypeScript will complain!
  // };

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>Interfaces in Action</h2>
      
      <div style={{ marginBottom: '20px' }}>
        <h3>Simple Person Interface</h3>
        <p><strong>Name:</strong> {person.name}</p>
        <p><strong>Age:</strong> {person.age}</p>
      </div>
      
      <div style={{ marginBottom: '20px' }}>
        <h3>User Interface (with more properties)</h3>
        <p><strong>ID:</strong> {user.id}</p>
        <p><strong>Name:</strong> {user.name}</p>
        <p><strong>Email:</strong> {user.email}</p>
        <p><strong>Admin:</strong> {user.isAdmin ? 'Yes' : 'No'}</p>
      </div>
      
      <div>
        <h3>Product Interface (with arrays)</h3>
        <p><strong>Name:</strong> {product.name}</p>
        <p><strong>Price:</strong> ${product.price}</p>
        <p><strong>Category:</strong> {product.category}</p>
        <p><strong>In Stock:</strong> {product.inStock ? 'Yes' : 'No'}</p>
        <p><strong>Tags:</strong> {product.tags.join(', ')}</p>
      </div>
      
      <div style={{ 
        marginTop: '20px', 
        padding: '15px', 
        backgroundColor: '#f0f8ff', 
        borderRadius: '5px' 
      }}>
        <h4>💡 Key Takeaway</h4>
        <p>
          Interfaces ensure that every object of the same type has the same structure. 
          If you try to create a User without an email, or a Product without a price, 
          TypeScript will catch the error before your code runs!
        </p>
      </div>
    </div>
  );
}
```

## Organizing Types in Separate Files

As your TypeScript projects grow, you'll often want to organize your types in separate files instead of mixing them with your components. This is a common best practice!

### Why Separate Type Files?

- **Reusability** - Use the same types across multiple components
- **Organization** - Keep your component files focused on logic, not type definitions
- **Team Collaboration** - Team members can easily find and update shared types
- **Maintainability** - Changes to types are centralized in one place

### Creating a Types File

Let's organize our interfaces into a separate file:

First, create the types directory:
```bash
mkdir src/types
```

Create **src/types/index.ts**:

```typescript
// src/types/index.ts
// This file contains all our shared type definitions

// User-related types
export interface User {
  id: number;
  name: string;
  email: string;
  isAdmin: boolean;
}

export interface UserProfile extends User {
  avatar?: string;
  bio?: string;
  joinDate: Date;
}

// Product-related types
export interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  inStock: boolean;
  tags: string[];
}

// API Response types
export interface ApiResponse<T> {
  data: T;
  success: boolean;
  message: string;
  timestamp: Date;
}

// Common utility types
export type Status = 'loading' | 'success' | 'error';
export type UserRole = 'admin' | 'editor' | 'viewer';
export type ID = string | number;
```

### Using Types from Separate Files

Now you can import and use these types in any component:

Create **src/examples/TypeImports.tsx**:

```typescript
import React, { useState } from 'react';
// Import types from our separate types file
import type { User, Product, ApiResponse, Status } from '../types';

export function TypeImports() {
  // Using imported types
  const [user, setUser] = useState<User | null>(null);
  const [products, setProducts] = useState<Product[]>([]);
  const [status, setStatus] = useState<Status>('loading');

  // Example data using our imported types
  const sampleUser: User = {
    id: 1,
    name: "Alice Johnson",
    email: "alice@example.com",
    isAdmin: false
  };

  const sampleProducts: Product[] = [
    {
      id: 1,
      name: "Laptop",
      price: 999.99,
      category: "Electronics",
      inStock: true,
      tags: ["computer", "portable"]
    },
    {
      id: 2,
      name: "Coffee Mug",
      price: 15.99,
      category: "Kitchen",
      inStock: false,
      tags: ["ceramic", "beverage"]
    }
  ];

  // Example API response using generic type
  const apiResponse: ApiResponse<Product[]> = {
    data: sampleProducts,
    success: true,
    message: "Products fetched successfully",
    timestamp: new Date()
  };

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>Using Types from Separate Files</h2>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>User Data (imported User type):</h3>
        <p><strong>Name:</strong> {sampleUser.name}</p>
        <p><strong>Email:</strong> {sampleUser.email}</p>
        <p><strong>Admin:</strong> {sampleUser.isAdmin ? 'Yes' : 'No'}</p>
      </div>

      <div style={{ marginBottom: '15px' }}>
        <h3>Products (imported Product[] type):</h3>
        {sampleProducts.map(product => (
          <div key={product.id} style={{ marginBottom: '10px', paddingLeft: '20px' }}>
            <p><strong>{product.name}</strong> - ${product.price}</p>
            <p>Category: {product.category} | In Stock: {product.inStock ? 'Yes' : 'No'}</p>
          </div>
        ))}
      </div>

      <div style={{ marginBottom: '15px' }}>
        <h3>API Response (imported ApiResponse type):</h3>
        <p><strong>Success:</strong> {apiResponse.success ? 'Yes' : 'No'}</p>
        <p><strong>Message:</strong> {apiResponse.message}</p>
        <p><strong>Data Count:</strong> {apiResponse.data.length} products</p>
      </div>

      <div style={{ 
        padding: '15px', 
        backgroundColor: '#e8f5e8', 
        borderRadius: '5px' 
      }}>
        <h4>💡 File Organization Tips</h4>
        <ul style={{ marginBottom: 0, fontSize: '14px' }}>
          <li><strong>src/types/index.ts</strong> - Main types file for shared interfaces</li>
          <li><strong>src/types/api.ts</strong> - API-specific types (requests, responses)</li>
          <li><strong>src/types/user.ts</strong> - User-related types if you have many</li>
          <li>Use <strong>export type</strong> for type aliases, <strong>export interface</strong> for object shapes</li>
          <li>Import with <strong>import type</strong> for better build optimization</li>
        </ul>
      </div>
    </div>
  );
}
```

### Import Syntax Explained

```typescript
// Regular import (imports both types and values)
import { User, Product } from '../types';

// Type-only import (better for performance, only imports types)
import type { User, Product } from '../types';

// You can mix them if needed
import { someFunction } from '../utils';
import type { User } from '../types';
```

**Pro Tip:** Use `import type` when you're only importing TypeScript types/interfaces. This tells TypeScript bundlers that these imports can be safely removed from the final JavaScript bundle!

> Check the browser to see how types can be organized and reused across components!
> 
> In your terminal, use git to save your progress:
> ```bash
> git add --all
> git commit -m "Add type organization and separate files example"
> ```

### 4. Functions with Types

Before we dive into examples, let's understand the TypeScript function syntax:

#### Function Type Syntax Breakdown

```typescript
// 1. Basic function structure
function functionName(parameter: type): returnType {
  return something;
}

// 2. Real example
function addTwo(num: number): number {
//     ↑         ↑      ↑       ↑
//  function   param   param   return
//    name     name    type     type
  return num + 2;
}

// 3. Multiple parameters
function greet(name: string, age: number): string {
//             ↑              ↑             ↑
//         parameter 1    parameter 2   return type
  return `Hello ${name}, you are ${age} years old`;
}

// 4. Arrow function (same rules)
const multiply = (a: number, b: number): number => a * b;
//                ↑                        ↑
//            parameters              return type
```

**Key Points:**
- **Parameters always need types** - TypeScript can't guess what you'll pass in
- **Return type comes after the `)`** - tells TypeScript what the function gives back
- **Same rules for regular functions and arrow functions**

Create **src/examples/FunctionTypes.tsx**:

```typescript
import React, { useState } from 'react';

// Basic function syntax: function name(parameter: type): returnType
//                                              ↑              ↑
//                                      parameter type    return type
function addNumbers(a: number, b: number): number {
  return a + b;
}

// Default parameter values: parameter: type = defaultValue
function greetUser(name: string, isVip: boolean = false): string {
  //                              ↑
  //                    default value (optional parameter)
  if (isVip) {
    return `Welcome, VIP ${name}!`;
  }
  return `Hello, ${name}!`;
}

// Functions that don't return anything use 'void'
function logMessage(message: string): void {
  //                                   ↑
  //                              'void' means "returns nothing"
  console.log(message);
}

// Arrow functions: same parameter types, same return type syntax
const multiplyNumbers = (x: number, y: number): number => {
  //    ↑ variable name    ↑ parameters        ↑ return type
  return x * y;
};

// Advanced: Functions that accept OTHER functions as parameters
function processNumber(num: number, operation: (n: number) => number): number {
  //                                          ↑
  //                              This parameter IS a function!
  //                              It takes a number and returns a number
  return operation(num);
}

export function FunctionTypes() {
  // useState with explicit type: useState<string>
  const [result, setResult] = useState<string>("");

  // Function that uses all our typed functions
  const handleCalculation = () => {
    // Calling our typed functions - TypeScript ensures we pass correct types
    const sum = addNumbers(10, 5);           // ✅ Two numbers
    const product = multiplyNumbers(4, 3);   // ✅ Two numbers  
    const greeting = greetUser("John", true); // ✅ String and boolean
    
    // Using the higher-order function with an inline function
    const doubled = processNumber(8, (n) => n * 2); 
    //                             ↑  ↑
    //                          number  function that doubles
    
    logMessage("Calculation performed!"); // ✅ String parameter
    
    setResult(`Sum: ${sum}, Product: ${product}, Doubled: ${doubled}`);
  };

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>Function Types Example</h2>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>Try it out:</h3>
        <button onClick={handleCalculation} style={{ padding: '10px', marginBottom: '10px' }}>
          Run Calculations (check console too!)
        </button>
      </div>
      
      <div>
        <h3>Function calls with different parameters:</h3>
        <p><strong>Basic greeting:</strong> {greetUser("Alice")}</p>
        <p><strong>VIP greeting:</strong> {greetUser("Bob", true)}</p>
        {result && <p><strong>Calculation results:</strong> {result}</p>}
      </div>
      
      <div style={{ 
        marginTop: '15px', 
        padding: '10px', 
        backgroundColor: '#fff3cd', 
        borderRadius: '4px' 
      }}>
        <h4>🔍 What TypeScript is checking:</h4>
        <ul style={{ marginBottom: 0, fontSize: '14px' }}>
          <li><strong>Parameter types:</strong> You can't pass a string where a number is expected</li>
          <li><strong>Parameter count:</strong> You must provide all required parameters</li>
          <li><strong>Return types:</strong> Functions return what they promise to return</li>
          <li><strong>Function signatures:</strong> Functions passed as parameters must match the expected signature</li>
        </ul>
      </div>
    </div>
  );
}
```

Now let's update **src/App.tsx** to display our examples:

```typescript
import React from 'react';
import { BasicTypes } from './examples/BasicTypes';
import { ObjectTypes } from './examples/ObjectTypes';
import { FunctionTypes } from './examples/FunctionTypes';
import './App.css';

function App() {
  return (
    <div className="App">
      <header style={{ padding: '20px', textAlign: 'center' }}>
        <h1>TypeScript Learning Examples</h1>
        <p>Explore TypeScript concepts with interactive examples</p>
      </header>
      
      <main>
        <BasicTypes />
        <ObjectTypes />
        <FunctionTypes />
      </main>
    </div>
  );
}

export default App;
```

### 5. Inference in Real-World Scenarios

Create **src/examples/InferenceExamples.tsx**:

```typescript
import React, { useState } from 'react';

export function InferenceExamples() {
  // ✅ React useState - TypeScript infers from initial value
  const [count, setCount] = useState(0); // TypeScript infers: number
  const [name, setName] = useState("John"); // TypeScript infers: string
  
  // ❌ But sometimes useState needs help with complex types
  // const [user, setUser] = useState(null); // TypeScript infers: null (not helpful!)
  
  // ✅ Better - tell TypeScript what the state will eventually contain
  interface User {
    id: number;
    name: string;
  }
  const [user, setUser] = useState<User | null>(null); // Explicit type needed
  
  // ✅ Object inference works well when initialized
  const product = {
    id: 1,
    name: "Laptop",
    price: 999.99
  }; // TypeScript infers: { id: number; name: string; price: number; }
  
  // ❌ But dynamic object properties need help
  const dynamicData: { [key: string]: any } = {}; // Must be explicit
  
  // ✅ Array methods - TypeScript is smart about these
  const numbers = [1, 2, 3, 4, 5];
  const doubled = numbers.map(n => n * 2); // TypeScript infers: number[]
  const strings = numbers.map(n => n.toString()); // TypeScript infers: string[]
  
  // ✅ Event handlers - TypeScript knows React event types
  const handleButtonClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    console.log("Button clicked!", event.currentTarget.innerText);
  };
  
  // ❌ But generic event handlers need type help
  const handleInputChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    console.log("Input value:", event.target.value);
  };
  
  // ✅ Async functions - return type inferred from what you return
  const fetchUserData = async (id: number) => {
    // Return type automatically inferred as: Promise<User>
    return {
      id: id,
      name: "Alice"
    };
  };

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>Type Inference in Real Scenarios</h2>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>React State (hover to see inferred types):</h3>
        <p><strong>Count:</strong> {count}</p>
        <button onClick={() => setCount(count + 1)}>Increment</button>
        <p><strong>Name:</strong> {name}</p>
        <input 
          value={name} 
          onChange={handleInputChange}
          placeholder="Type a name"
        />
      </div>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>Object Inference:</h3>
        <p><strong>Product:</strong> {product.name} - ${product.price}</p>
      </div>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>Array Method Inference:</h3>
        <p><strong>Original:</strong> [{numbers.join(', ')}]</p>
        <p><strong>Doubled:</strong> [{doubled.join(', ')}]</p>
        <p><strong>As Strings:</strong> [{strings.join(', ')}]</p>
      </div>
      
      <div>
        <h3>Event Handling:</h3>
        <button onClick={handleButtonClick}>
          Click me (check console)
        </button>
      </div>
      
      <div style={{ 
        marginTop: '15px', 
        padding: '10px', 
        backgroundColor: '#e7f3ff', 
        borderRadius: '4px' 
      }}>
        <h4>💡 Pro Tip: When to be explicit vs let TypeScript infer</h4>
        <ul style={{ marginBottom: 0 }}>
          <li><strong>Let TypeScript infer:</strong> Variable assignments, array methods, simple function returns</li>
          <li><strong>Be explicit:</strong> Function parameters, empty arrays, complex state, API responses</li>
          <li><strong>Rule of thumb:</strong> If TypeScript can't figure it out or the type isn't obvious to other developers, be explicit!</li>
        </ul>
      </div>
    </div>
  );
}
```

Update **src/App.tsx** to include this new example:

```typescript
import React from 'react';
import { BasicTypes } from './examples/BasicTypes';
import { InferenceExamples } from './examples/InferenceExamples';
import { ObjectTypes } from './examples/ObjectTypes';
import { TypeImports } from './examples/TypeImports';
import { FunctionTypes } from './examples/FunctionTypes';
import './App.css';

function App() {
  return (
    <div className="App">
      <header style={{ padding: '20px', textAlign: 'center' }}>
        <h1>TypeScript Learning Examples</h1>
        <p>Explore TypeScript concepts with interactive examples</p>
      </header>
      
      <main>
        <BasicTypes />
        <InferenceExamples />
        <ObjectTypes />
        <TypeImports />
        <FunctionTypes />
      </main>
    </div>
  );
}

export default App;
```

> Check the browser to see your TypeScript examples in action!
> 
> In your terminal, use git to save your progress:
> ```bash
> git add --all
> git commit -m "Add basic TypeScript type examples"
> ```

## Advanced TypeScript Concepts

### 6. Union Types and Optional Properties

Create **src/examples/AdvancedTypes.tsx**:

```typescript
import React, { useState } from 'react';

// Union types - a value can be one of several types
type Status = 'loading' | 'success' | 'error';
type ID = string | number;

// Optional properties with ?
interface BlogPost {
  id: ID;
  title: string;
  content: string;
  author: string;
  publishedDate?: Date; // Optional - might not exist
  tags?: string[];     // Optional array
}

// Type for different user roles
type UserRole = 'admin' | 'editor' | 'viewer';

interface Employee {
  id: number;
  name: string;
  role: UserRole;
  department: string;
  startDate: Date;
  salary?: number; // Optional - private information
}

export function AdvancedTypes() {
  const [status, setStatus] = useState<Status>('loading');
  
  // Example blog post
  const blogPost: BlogPost = {
    id: "post-123",
    title: "Learning TypeScript",
    content: "TypeScript makes JavaScript development much safer...",
    author: "Jane Doe",
    // publishedDate is optional, so we can omit it
    tags: ["typescript", "javascript", "programming"]
  };
  
  // Example employee
  const employee: Employee = {
    id: 1,
    name: "John Smith",
    role: "editor",
    department: "Engineering",
    startDate: new Date('2023-01-15')
    // salary is optional and omitted for privacy
  };

  const handleStatusChange = (newStatus: Status) => {
    setStatus(newStatus);
  };

  // Function that accepts different ID types
  const findItem = (id: ID): string => {
    return `Looking for item with ID: ${id}`;
  };

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>Advanced Types Example</h2>
      
      <div style={{ marginBottom: '20px' }}>
        <h3>Status Management</h3>
        <p><strong>Current Status:</strong> {status}</p>
        <div>
          <button onClick={() => handleStatusChange('loading')}>Loading</button>
          <button onClick={() => handleStatusChange('success')}>Success</button>
          <button onClick={() => handleStatusChange('error')}>Error</button>
        </div>
      </div>

      <div style={{ marginBottom: '20px' }}>
        <h3>Blog Post</h3>
        <p><strong>Title:</strong> {blogPost.title}</p>
        <p><strong>Author:</strong> {blogPost.author}</p>
        <p><strong>Tags:</strong> {blogPost.tags?.join(', ') || 'No tags'}</p>
        <p><strong>Published:</strong> {blogPost.publishedDate?.toDateString() || 'Not published yet'}</p>
      </div>

      <div style={{ marginBottom: '20px' }}>
        <h3>Employee Info</h3>
        <p><strong>Name:</strong> {employee.name}</p>
        <p><strong>Role:</strong> {employee.role}</p>
        <p><strong>Department:</strong> {employee.department}</p>
        <p><strong>Start Date:</strong> {employee.startDate.toDateString()}</p>
      </div>

      <div>
        <h3>ID Examples</h3>
        <p>{findItem(123)}</p>
        <p>{findItem("user-456")}</p>
      </div>
    </div>
  );
}
```

### 7. Type Assertions - "Trust Me, I Know What This Is"

Sometimes you know more about a value's type than TypeScript does. Type assertions let you tell TypeScript: "Trust me, I know this is actually this type."

#### When You Might Need Type Assertions

Create **src/examples/TypeAssertions.tsx**:

```typescript
import React, { useState } from 'react';

export function TypeAssertions() {
  const [userInfo, setUserInfo] = useState<string>('');

  // Example 1: DOM elements
  // TypeScript doesn't know what specific HTML element getElementById returns
  const setupButtonClick = () => {
    const button = document.getElementById('myButton'); // Type: HTMLElement | null
    
    // ❌ This would error because HTMLElement doesn't have 'disabled'
    // button.disabled = true; // Error: Property 'disabled' does not exist on type 'HTMLElement'
    
    // ✅ Use type assertion to tell TypeScript it's specifically a button
    const buttonElement = document.getElementById('myButton') as HTMLButtonElement;
    if (buttonElement) {
      buttonElement.disabled = true; // ✅ Works! HTMLButtonElement has 'disabled'
      setUserInfo(`Button element found and disabled! Type: ${button?.constructor.name || 'null'}`);
    } else {
      setUserInfo('Button with ID "myButton" not found in this demo');
    }
  };

  // Example 2: API responses
  // Sometimes APIs return 'any' but you know the actual structure
  const handleApiResponse = () => {
    // Imagine this comes from an API call
    const apiData: any = {
      id: 1,
      name: "John Doe",
      email: "john@example.com"
    };
    
    // ❌ TypeScript doesn't know what properties exist on 'any'
    // console.log(apiData.name); // No autocomplete, no type checking
    
    // ✅ Assert the type so you get proper typing
    interface User {
      id: number;
      name: string;
      email: string;
    }
    
    const user = apiData as User;
    console.log(user.name); // ✅ Full type safety and autocomplete!
    setUserInfo(`${user.name} (${user.email})`);
  };

  // Example 3: Assertion syntax demonstration
  const demonstrateAssertionSyntax = () => {
    const someValue: unknown = "Hello, TypeScript!";
    
    // Method 1: 'as' syntax (recommended, works everywhere)
    const str1 = someValue as string;
    
    // Method 2: angle bracket syntax (doesn't work in JSX/TSX files)
    // Note: This is commented out because it causes JSX parsing errors
    // const str2 = <string>someValue; // ❌ Doesn't work in .tsx files!
    const str2 = "Angle brackets don't work in JSX files";
    
    setUserInfo(`Method 1: ${str1} | Note: ${str2}`);
  };

  // Example 4: Non-null assertion (!)
  const handleNonNullAssertion = () => {
    const users = ['Alice', 'Bob', 'Charlie'];
    
    // ❌ TypeScript thinks find() might return undefined
    // const firstUser = users.find(user => user.startsWith('A'));
    // console.log(firstUser.toUpperCase()); // Error: Object is possibly 'undefined'
    
    // ✅ If you KNOW it will find something, use non-null assertion
    const firstUser = users.find(user => user.startsWith('A'))!;
    console.log(firstUser.toUpperCase()); // ✅ Works, but be careful!
    
    setUserInfo(`Found user: ${firstUser}`);
  };

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>Type Assertions Example</h2>
      
      <div style={{ marginBottom: '15px' }}>
        <h3>Try the examples:</h3>
        <button onClick={setupButtonClick} style={{ margin: '5px', padding: '8px' }}>
          DOM Element Assertion
        </button>
        <button onClick={handleApiResponse} style={{ margin: '5px', padding: '8px' }}>
          API Response Assertion
        </button>
        <button onClick={demonstrateAssertionSyntax} style={{ margin: '5px', padding: '8px' }}>
          Assertion Syntax
        </button>
        <button onClick={handleNonNullAssertion} style={{ margin: '5px', padding: '8px' }}>
          Non-null Assertion
        </button>
      </div>
      
      {/* Demo button for DOM assertion example */}
      <div style={{ marginBottom: '15px', padding: '10px', backgroundColor: '#f8f9fa', borderRadius: '4px' }}>
        <h4>Demo Button (for DOM example):</h4>
        <button id="myButton" style={{ padding: '8px', margin: '5px' }}>
          I'm the target button (ID: myButton)
        </button>
        <p style={{ fontSize: '12px', color: '#666' }}>
          Click "DOM Element Assertion" above to find and disable this button
        </p>
      </div>
      
      {userInfo && (
        <div style={{ marginBottom: '15px', padding: '10px', backgroundColor: '#f0f8ff' }}>
          <strong>Result:</strong> {userInfo}
        </div>
      )}
      
      <div style={{ 
        padding: '15px', 
        backgroundColor: '#fff3cd', 
        borderRadius: '5px' 
      }}>
        <h4>⚠️ Type Assertion Safety Tips</h4>
        <ul style={{ marginBottom: 0, fontSize: '14px' }}>
          <li><strong>Use sparingly:</strong> Only when you truly know more than TypeScript</li>
          <li><strong>Prefer type guards:</strong> Use <code>typeof</code> checks when possible</li>
          <li><strong>Be careful with '!':</strong> Non-null assertion can cause runtime errors if you're wrong</li>
          <li><strong>Document why:</strong> Add comments explaining why the assertion is safe</li>
        </ul>
      </div>
      
      <div style={{ 
        marginTop: '15px',
        padding: '15px', 
        backgroundColor: '#e8f5e8', 
        borderRadius: '5px' 
      }}>
        <h4>📖 Type Assertion Syntax</h4>
        <div style={{ fontSize: '14px', fontFamily: 'monospace' }}>
          <p><strong>As syntax:</strong> <code>value as Type</code> (recommended)</p>
          <p><strong>Angle brackets:</strong> <code>&lt;Type&gt;value</code> (❌ avoid in JSX/TSX files)</p>
          <p><strong>Non-null assertion:</strong> <code>value!</code> (use carefully)</p>
        </div>
      </div>
    </div>
  );
}
```

> Check the browser to see type assertions in action!
> 
> In your terminal, use git to save your progress:
> ```bash
> git add --all
> git commit -m "Add array syntax explanation and type assertions"
> ```

### 8. Generics - Reusable Type Components

Create **src/examples/Generics.tsx**:

```typescript
import React, { useState } from 'react';

// Generic function - works with any type
function getFirstItem<T>(items: T[]): T | undefined {
  return items[0];
}

// Generic interface
interface ApiResponse<T> {
  data: T;
  success: boolean;
  message: string;
}

// Generic class
class Storage<T> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  getAll(): T[] {
    return this.items;
  }

  clear(): void {
    this.items = [];
  }
}

// Example types for our API responses
interface User {
  id: number;
  name: string;
  email: string;
}

interface Product {
  id: number;
  name: string;
  price: number;
}

export function Generics() {
  const [selectedNumber, setSelectedNumber] = useState<number | undefined>();
  const [selectedFruit, setSelectedFruit] = useState<string | undefined>();

  // Using generics with different types
  const numbers = [1, 2, 3, 4, 5];
  const fruits = ["apple", "banana", "cherry"];
  const users: User[] = [
    { id: 1, name: "Alice", email: "alice@example.com" },
    { id: 2, name: "Bob", email: "bob@example.com" }
  ];

  // Generic storage examples
  const numberStorage = new Storage<number>();
  const stringStorage = new Storage<string>();
  
  // Add some items
  numberStorage.add(10);
  numberStorage.add(20);
  stringStorage.add("hello");
  stringStorage.add("world");

  // API response examples
  const userResponse: ApiResponse<User[]> = {
    data: users,
    success: true,
    message: "Users fetched successfully"
  };

  const productResponse: ApiResponse<Product> = {
    data: { id: 1, name: "Laptop", price: 999.99 },
    success: true,
    message: "Product fetched successfully"
  };

  const handleGetFirst = () => {
    setSelectedNumber(getFirstItem(numbers));
    setSelectedFruit(getFirstItem(fruits));
  };

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>Generics Example</h2>
      
      <div style={{ marginBottom: '20px' }}>
        <h3>Generic Functions</h3>
        <button onClick={handleGetFirst}>Get First Items</button>
        <p><strong>First number:</strong> {selectedNumber ?? 'None'}</p>
        <p><strong>First fruit:</strong> {selectedFruit ?? 'None'}</p>
        <p><strong>First user:</strong> {getFirstItem(users)?.name ?? 'None'}</p>
      </div>

      <div style={{ marginBottom: '20px' }}>
        <h3>Generic Storage</h3>
        <p><strong>Numbers in storage:</strong> [{numberStorage.getAll().join(', ')}]</p>
        <p><strong>Strings in storage:</strong> [{stringStorage.getAll().join(', ')}]</p>
      </div>

      <div>
        <h3>Generic API Responses</h3>
        <p><strong>User Response:</strong> {userResponse.message} (Success: {userResponse.success ? 'Yes' : 'No'})</p>
        <p><strong>Users Count:</strong> {userResponse.data.length}</p>
        <p><strong>Product Response:</strong> {productResponse.message}</p>
        <p><strong>Product Name:</strong> {productResponse.data.name} - ${productResponse.data.price}</p>
      </div>
    </div>
  );
}
```

Update **src/App.tsx** to include the new examples:

```typescript
import React from 'react';
import { BasicTypes } from './examples/BasicTypes';
import { InferenceExamples } from './examples/InferenceExamples';
import { ObjectTypes } from './examples/ObjectTypes';
import { TypeImports } from './examples/TypeImports';
import { FunctionTypes } from './examples/FunctionTypes';
import { AdvancedTypes } from './examples/AdvancedTypes';
import { TypeAssertions } from './examples/TypeAssertions';
import { Generics } from './examples/Generics';
import './App.css';

function App() {
  return (
    <div className="App">
      <header style={{ padding: '20px', textAlign: 'center' }}>
        <h1>TypeScript Learning Examples</h1>
        <p>Explore TypeScript concepts with interactive examples</p>
      </header>
      
      <main>
        <BasicTypes />
        <InferenceExamples />
        <ObjectTypes />
        <TypeImports />
        <FunctionTypes />
        <AdvancedTypes />
        <TypeAssertions />
        <Generics />
      </main>
    </div>
  );
}

export default App;
```

> Check the browser to see your expanded TypeScript examples!
> 
> In your terminal, use git to save your progress:
> ```bash
> git add --all
> git commit -m "Add advanced TypeScript concepts and generics"
> ```

## Real-World Example: Todo App with TypeScript

Let's build a practical todo app to demonstrate TypeScript in action.

### 9. Building a Todo App

Create **src/examples/TodoApp.tsx**:

```typescript
import React, { useState, useEffect } from 'react';

// Define interfaces for our todo application
interface Todo {
  id: number;
  text: string;
  completed: boolean;
  createdAt: Date;
  priority: 'low' | 'medium' | 'high';
}

interface TodoStats {
  total: number;
  completed: number;
  pending: number;
}

// Props interface for TodoItem component
interface TodoItemProps {
  todo: Todo;
  onToggle: (id: number) => void;
  onDelete: (id: number) => void;
}

// TodoItem component with typed props
function TodoItem({ todo, onToggle, onDelete }: TodoItemProps) {
  const priorityColors = {
    low: '#28a745',
    medium: '#ffc107', 
    high: '#dc3545'
  };

  return (
    <div style={{ 
      padding: '10px', 
      border: '1px solid #ddd', 
      marginBottom: '5px',
      borderLeft: `4px solid ${priorityColors[todo.priority]}`,
      backgroundColor: todo.completed ? '#f8f9fa' : 'white'
    }}>
      <div style={{ display: 'flex', alignItems: 'center', gap: '10px' }}>
        <input
          type="checkbox"
          checked={todo.completed}
          onChange={() => onToggle(todo.id)}
        />
        <span style={{ 
          textDecoration: todo.completed ? 'line-through' : 'none',
          flex: 1
        }}>
          {todo.text}
        </span>
        <span style={{ 
          fontSize: '12px', 
          color: priorityColors[todo.priority],
          fontWeight: 'bold'
        }}>
          {todo.priority.toUpperCase()}
        </span>
        <button 
          onClick={() => onDelete(todo.id)}
          style={{ color: 'red', border: 'none', background: 'none' }}
        >
          ✕
        </button>
      </div>
      <div style={{ fontSize: '12px', color: '#666', marginTop: '5px' }}>
        Created: {todo.createdAt.toLocaleDateString()}
      </div>
    </div>
  );
}

export function TodoApp() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [newTodoText, setNewTodoText] = useState<string>('');
  const [newTodoPriority, setNewTodoPriority] = useState<Todo['priority']>('medium');
  const [filter, setFilter] = useState<'all' | 'completed' | 'pending'>('all');

  // Add initial todos when component mounts
  useEffect(() => {
    const initialTodos: Todo[] = [
      {
        id: 1,
        text: "Learn TypeScript basics",
        completed: true,
        createdAt: new Date(),
        priority: 'high'
      },
      {
        id: 2,
        text: "Build a todo app",
        completed: false,
        createdAt: new Date(),
        priority: 'medium'
      }
    ];
    setTodos(initialTodos);
  }, []);

  // Add new todo
  const addTodo = (): void => {
    if (newTodoText.trim() === '') return;

    const newTodo: Todo = {
      id: Date.now(), // Simple ID generation
      text: newTodoText.trim(),
      completed: false,
      createdAt: new Date(),
      priority: newTodoPriority
    };

    setTodos(prevTodos => [...prevTodos, newTodo]);
    setNewTodoText('');
  };

  // Toggle todo completion
  const toggleTodo = (id: number): void => {
    setTodos(prevTodos =>
      prevTodos.map(todo =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };

  // Delete todo
  const deleteTodo = (id: number): void => {
    setTodos(prevTodos => prevTodos.filter(todo => todo.id !== id));
  };

  // Calculate stats
  const getStats = (): TodoStats => {
    const total = todos.length;
    const completed = todos.filter(todo => todo.completed).length;
    const pending = total - completed;
    return { total, completed, pending };
  };

  // Filter todos
  const getFilteredTodos = (): Todo[] => {
    switch (filter) {
      case 'completed':
        return todos.filter(todo => todo.completed);
      case 'pending':
        return todos.filter(todo => !todo.completed);
      default:
        return todos;
    }
  };

  const stats = getStats();
  const filteredTodos = getFilteredTodos();

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>TypeScript Todo App</h2>
      
      {/* Stats */}
      <div style={{ 
        padding: '10px', 
        backgroundColor: '#f8f9fa', 
        marginBottom: '20px',
        borderRadius: '4px'
      }}>
        <strong>Stats:</strong> {stats.total} total, {stats.completed} completed, {stats.pending} pending
      </div>

      {/* Add new todo */}
      <div style={{ marginBottom: '20px' }}>
        <div style={{ display: 'flex', gap: '10px', marginBottom: '10px' }}>
          <input
            type="text"
            value={newTodoText}
            onChange={(e) => setNewTodoText(e.target.value)}
            placeholder="Enter new todo..."
            style={{ flex: 1, padding: '8px' }}
            onKeyPress={(e) => e.key === 'Enter' && addTodo()}
          />
          <select
            value={newTodoPriority}
            onChange={(e) => setNewTodoPriority(e.target.value as Todo['priority'])}
            style={{ padding: '8px' }}
          >
            <option value="low">Low</option>
            <option value="medium">Medium</option>
            <option value="high">High</option>
          </select>
          <button onClick={addTodo} style={{ padding: '8px 16px' }}>
            Add Todo
          </button>
        </div>
      </div>

      {/* Filter buttons */}
      <div style={{ marginBottom: '20px' }}>
        <strong>Filter: </strong>
        {(['all', 'pending', 'completed'] as const).map(filterOption => (
          <button
            key={filterOption}
            onClick={() => setFilter(filterOption)}
            style={{
              marginLeft: '5px',
              padding: '5px 10px',
              backgroundColor: filter === filterOption ? '#007bff' : '#6c757d',
              color: 'white',
              border: 'none',
              borderRadius: '4px'
            }}
          >
            {filterOption.charAt(0).toUpperCase() + filterOption.slice(1)}
          </button>
        ))}
      </div>

      {/* Todo list */}
      <div>
        {filteredTodos.length === 0 ? (
          <p style={{ textAlign: 'center', color: '#666' }}>
            No todos {filter !== 'all' ? `(${filter})` : ''} found.
          </p>
        ) : (
          filteredTodos.map(todo => (
            <TodoItem
              key={todo.id}
              todo={todo}
              onToggle={toggleTodo}
              onDelete={deleteTodo}
            />
          ))
        )}
      </div>
    </div>
  );
}
```

Update **src/App.tsx** to include the TodoApp:

```typescript
import React from 'react';
import { BasicTypes } from './examples/BasicTypes';
import { InferenceExamples } from './examples/InferenceExamples';
import { ObjectTypes } from './examples/ObjectTypes';
import { TypeImports } from './examples/TypeImports';
import { FunctionTypes } from './examples/FunctionTypes';
import { AdvancedTypes } from './examples/AdvancedTypes';
import { TypeAssertions } from './examples/TypeAssertions';
import { Generics } from './examples/Generics';
import { TodoApp } from './examples/TodoApp';
import './App.css';

function App() {
  return (
    <div className="App">
      <header style={{ padding: '20px', textAlign: 'center' }}>
        <h1>TypeScript Learning Examples</h1>
        <p>Explore TypeScript concepts with interactive examples</p>
      </header>
      
      <main>
        <BasicTypes />
        <InferenceExamples />
        <ObjectTypes />
        <TypeImports />
        <FunctionTypes />
        <AdvancedTypes />
        <TypeAssertions />
        <Generics />
        <TodoApp />
      </main>
    </div>
  );
}

export default App;
```

> Check the browser to see your fully functional TypeScript todo app!
> 
> In your terminal, use git to save your progress:
> ```bash
> git add --all
> git commit -m "Add complete todo app example with TypeScript"
> ```

## Common TypeScript Errors and Solutions

### Understanding TypeScript Error Messages

Create **src/examples/ErrorExamples.tsx** to show common issues:

```typescript
import React from 'react';

export function ErrorExamples() {
  // ❌ Common Error 1: Type mismatch
  // const age: number = "25"; // Error: Type 'string' is not assignable to type 'number'
  const age: number = 25; // ✅ Correct

  // ❌ Common Error 2: Property doesn't exist
  interface Person {
    name: string;
    age: number;
  }
  
  const person: Person = { name: "John", age: 30 };
  // const city = person.city; // Error: Property 'city' does not exist on type 'Person'
  
  // ✅ Solutions for accessing optional/unknown properties:
  interface PersonWithCity {
    name: string;
    age: number;
    city?: string; // Optional property
  }
  
  const personWithCity: PersonWithCity = { name: "Jane", age: 25, city: "NYC" };
  const city = personWithCity.city; // ✅ Works because city is optional

  // ❌ Common Error 3: Null/undefined issues
  // const names: string[] = null; // Error if strictNullChecks is enabled
  const names: string[] = []; // ✅ Correct
  
  // ❌ Common Error 4: Function parameter issues
  function greet(name: string): string {
    return `Hello, ${name}!`;
  }
  
  // greet(); // Error: Expected 1 arguments, but got 0
  // greet(123); // Error: Argument of type 'number' is not assignable to parameter of type 'string'
  const greeting = greet("Alice"); // ✅ Correct

  // ❌ Common Error 5: Array method issues
  const numbers = [1, 2, 3];
  // const doubled = numbers.map(n => n * "2"); // Error: Operator '*' cannot be applied to types 'number' and 'string'
  const doubled = numbers.map(n => n * 2); // ✅ Correct

  return (
    <div style={{ padding: '20px', border: '1px solid #ccc', margin: '10px' }}>
      <h2>TypeScript Error Examples & Solutions</h2>
      
      <div style={{ backgroundColor: '#d4edda', padding: '15px', marginBottom: '10px', borderRadius: '4px' }}>
        <h3>✅ These examples show CORRECT TypeScript usage</h3>
        <p><strong>Age:</strong> {age}</p>
        <p><strong>Person:</strong> {person.name} is {person.age} years old</p>
        <p><strong>City:</strong> {city || 'No city specified'}</p>
        <p><strong>Greeting:</strong> {greeting}</p>
        <p><strong>Doubled numbers:</strong> [{doubled.join(', ')}]</p>
      </div>

      <div style={{ backgroundColor: '#f8d7da', padding: '15px', borderRadius: '4px' }}>
        <h3>❌ Common TypeScript Errors (see comments in code)</h3>
        <ul>
          <li><strong>Type mismatch:</strong> Assigning wrong type to variable</li>
          <li><strong>Property doesn't exist:</strong> Accessing undefined properties</li>
          <li><strong>Null/undefined:</strong> Not handling potential null values</li>
          <li><strong>Function parameters:</strong> Wrong number or type of arguments</li>
          <li><strong>Array methods:</strong> Type mismatches in array operations</li>
        </ul>
      </div>
    </div>
  );
}
```

## TypeScript Configuration

### Understanding tsconfig.json

Let's look at your project's **tsconfig.json** file:

```json
{
  "compilerOptions": {
    "target": "ES2020",                    // JavaScript version to compile to
    "useDefineForClassFields": true,       // Modern class field behavior
    "lib": ["ES2020", "DOM", "DOM.Iterable"], // Available APIs
    "module": "ESNext",                    // Module system
    "skipLibCheck": true,                  // Skip type checking of declaration files

    /* Bundler mode */
    "moduleResolution": "bundler",         // How to resolve modules
    "allowImportingTsExtensions": true,    // Allow importing .ts files
    "resolveJsonModule": true,             // Allow importing JSON files
    "isolatedModules": true,               // Each file is a separate module
    "noEmit": true,                       // Don't output files (Vite handles this)

    /* Linting */
    "strict": true,                       // Enable all strict type checking
    "noUnusedLocals": true,              // Error on unused local variables
    "noUnusedParameters": true,          // Error on unused parameters
    "noFallthroughCasesInSwitch": true   // Error on switch fallthrough
  },
  "include": ["src"],                     // Include all files in src directory
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

**Key settings explained:**
- **`"strict": true`** - Enables the strictest type checking (recommended)
- **`"noEmit": true`** - Vite handles compilation, so TypeScript just does type checking
- **`"lib": ["DOM"]`** - Provides browser API types (document, window, etc.)

## Best Practices

### 1. Type vs Interface

```typescript
// Use 'interface' for object shapes that might be extended
interface User {
  id: number;
  name: string;
}

interface AdminUser extends User {
  permissions: string[];
}

// Use 'type' for unions, primitives, or computed types
type Status = 'loading' | 'success' | 'error';
type UserKeys = keyof User; // 'id' | 'name'
```

### 2. Avoid 'any' Type

```typescript
// ❌ Avoid - loses all type safety
const data: any = fetchUserData();

// ✅ Better - use specific types
interface UserData {
  id: number;
  name: string;
  email: string;
}
const data: UserData = fetchUserData();

// ✅ Or use 'unknown' if truly unknown
const data: unknown = fetchUserData();
if (typeof data === 'object' && data !== null) {
  // Type guard to safely use data
}
```

### 3. Use Type Guards

```typescript
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

function processValue(value: unknown) {
  if (isString(value)) {
    // TypeScript now knows value is a string
    console.log(value.toUpperCase());
  }
}
```

Update **src/App.tsx** one final time to include error examples:

```typescript
import React from 'react';
import { BasicTypes } from './examples/BasicTypes';
import { InferenceExamples } from './examples/InferenceExamples';
import { ObjectTypes } from './examples/ObjectTypes';
import { TypeImports } from './examples/TypeImports';
import { FunctionTypes } from './examples/FunctionTypes';
import { AdvancedTypes } from './examples/AdvancedTypes';
import { TypeAssertions } from './examples/TypeAssertions';
import { Generics } from './examples/Generics';
import { TodoApp } from './examples/TodoApp';
import { ErrorExamples } from './examples/ErrorExamples';
import './App.css';

function App() {
  return (
    <div className="App">
      <header style={{ padding: '20px', textAlign: 'center' }}>
        <h1>TypeScript Learning Examples</h1>
        <p>Explore TypeScript concepts with interactive examples</p>
      </header>
      
      <main>
        <BasicTypes />
        <InferenceExamples />
        <ObjectTypes />
        <TypeImports />
        <FunctionTypes />
        <AdvancedTypes />
        <TypeAssertions />
        <Generics />
        <TodoApp />
        <ErrorExamples />
      </main>
    </div>
  );
}

export default App;
```

> Final check in the browser to see all your TypeScript examples!
> 
> In your terminal, save your final version:
> ```bash
> git add --all
> git commit -m "Complete TypeScript introduction with error examples and best practices"
> ```

## Summary

Congratulations! You've learned TypeScript fundamentals:

### ✅ What You've Learned
- **Basic types** - string, number, boolean, arrays
- **Object types & interfaces** - defining shape of objects
- **Function types** - parameter and return types
- **Advanced types** - unions, optionals, type guards
- **Generics** - reusable type components
- **Real-world application** - complete todo app
- **Error handling** - common issues and solutions
- **Best practices** - when to use interface vs type, avoiding 'any'

### 🚀 Next Steps
- **Try adding TypeScript to existing projects** - Start small with one file
- **Explore React TypeScript patterns** - Component props, hooks, event handlers
- **Learn about utility types** - `Pick`, `Omit`, `Partial`, `Required`
- **Study advanced patterns** - Conditional types, mapped types
- **Practice with real APIs** - Type your API responses

### 🔧 Tools & Resources
- **VS Code** - Excellent TypeScript support built-in
- **TypeScript Playground** - https://www.typescriptlang.org/play
- **Documentation** - https://www.typescriptlang.org/docs/

TypeScript makes JavaScript development safer, more maintainable, and more enjoyable. Start using it in your next project - your future self will thank you!
