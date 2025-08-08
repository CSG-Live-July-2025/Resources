# Guide: React Native and Expo - Universal Mobile App Development

## Introduction: React.js vs React Native

Before diving into mobile development, let's understand the key differences between React.js and React Native:

### React.js (Web Development)
- **Purpose**: Building web applications that run in browsers
- **Platform**: Web browsers (Chrome, Firefox, Safari, etc.)
- **Components**: Uses HTML elements (`<div>`, `<h1>`, `<p>`, `<button>`)
- **Styling**: Uses CSS stylesheets and CSS-in-JS
- **Navigation**: URL-based routing with React Router
- **Output**: Websites and web applications

### React Native (Mobile Development)
- **Purpose**: Building native mobile apps for iOS and Android
- **Platform**: Mobile devices (iPhone, Android phones, tablets)
- **Components**: Uses native mobile components (`<View>`, `<Text>`, `<ScrollView>`, `<TouchableOpacity>`)
- **Styling**: Uses StyleSheet objects (similar to CSS but with some differences)
- **Navigation**: Stack and tab-based navigation with React Navigation
- **Output**: Native mobile applications

### What is Expo?

[Expo](https://expo.dev/) is an ecosystem of tools that makes React Native development faster and easier:

- **Quick Setup**: No need to install Android Studio or Xcode to get started
- **Universal Apps**: Write once, run on iOS, Android, and web
- **Expo Go**: Test your app instantly on your phone during development
- **Built-in APIs**: Access to camera, location, notifications, and more
- **Easy Deployment**: Simplified build and app store submission process

### Why Use Expo?

1. **Faster Development**: Get started in minutes, not hours
2. **Cross-Platform**: One codebase for iOS, Android, and web
3. **No Native Setup**: Develop without Xcode or Android Studio initially
4. **Live Testing**: See changes instantly on your device
5. **Rich Ecosystem**: 75+ pre-built modules for common features

## When You Need an API vs When You Don't

### Apps That DON'T Need an API:
- **Calculator apps** - All logic happens locally
- **Note-taking apps** - Data stored on device only
- **Games** - Self-contained entertainment
- **Utility apps** - Like flashlight, timer, or unit converter
- **Offline-first apps** - Like meditation or fitness trackers

### Apps That DO Need an API:
- **Social media apps** - Sharing data between users
- **E-commerce apps** - Product catalogs, orders, payments
- **Chat applications** - Real-time messaging
- **News apps** - Fetching latest articles
- **User authentication** - Login/signup across devices

For this guide, we'll build a **Photos app** that connects to a Rails API (like your existing React guide), showing how mobile apps can work with backend services.

## Setup: Development Environment

### Prerequisites
- **Node.js** (version 18 or higher)
- **A smartphone** (iPhone or Android) for testing
- **Same WiFi network** for your computer and phone

### Step 1: Install Expo CLI

In your terminal:
```bash
npm install -g @expo/cli
```

### Step 2: Download Expo Go App

On your smartphone:
- **iOS**: Download "Expo Go" from the App Store
- **Android**: Download "Expo Go" from Google Play Store

### Step 3: Create Your First App

In your terminal, navigate to your Actualize directory and create a new project:
```bash
npx create-expo-app PhotosApp --template blank
```

Navigate to your project and start the development server:
```bash
cd PhotosApp
npm start
```

## Understanding Expo Templates

When creating a new Expo app, you have several template options. Let's explore what's available:

### Available Templates

#### 1. `blank` - Minimal Starting Point ✅ **Recommended for Learning**
```bash
npx create-expo-app MyApp --template blank
```
**What you get:**
- Just the basic App.js with a simple starter screen
- Minimal dependencies (only the essentials)
- Clean, empty project structure
- No pre-built navigation or complex features

#### 2. `blank-typescript` - Minimal with TypeScript
```bash
npx create-expo-app MyApp --template blank-typescript
```
**What you get:**
- Same as blank, but with TypeScript configured
- App.tsx instead of App.js
- TypeScript configuration files

#### 3. Default Template (no --template flag)
```bash
npx create-expo-app MyApp
```
**What you get:**
- Pre-built tab navigation with Home and Explore tabs
- Welcome screen with helpful getting-started steps
- Complex folder structure with `app/` directory and file-based routing
- TypeScript configured by default
- Many pre-built components and examples

#### 4. `tabs` - Tab Navigation Template  
```bash
npx create-expo-app MyApp --template tabs
```
**What you get:**
- Similar to default template but with different configuration
- Pre-built tab navigation with multiple screens
- Example screens and components

#### 5. `navigation` - Stack Navigation Template
```bash
npx create-expo-app MyApp --template navigation
```
**What you get:**
- Stack-based navigation between screens
- Multiple example screens
- Navigation patterns already implemented

### Why We Recommend `--template blank` for Beginners

**1. Clean Slate for Learning**
```jsx
// With blank template, you start with just this simple code:
export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <StatusBar style="auto" />
    </View>
  );
}
```

**2. Understand the Fundamentals**
- You learn how components work from scratch
- You understand styling without pre-made themes
- You see exactly what dependencies you're adding
- No hidden complexity or "magic" code

**3. Educational Value**
- Similar to starting with a blank HTML file in web development
- Every piece of code serves a clear purpose
- You build features step-by-step, understanding each concept

**4. Matches This Guide's Teaching Approach**
- We'll build navigation, styling, and features together
- You'll understand *why* certain patterns are used
- No mysterious pre-built code to decipher

### When to Use Other Templates

**Use `tabs` when:**
- You've completed this guide and understand the basics
- You want to prototype quickly
- Your app definitely needs tab navigation
- You want to see best practices implemented

**Use `navigation` when:**
- You're comfortable with React Native fundamentals
- You need stack navigation patterns
- You're building a multi-screen app

For now, stick with `--template blank` as you work through this guide. Once you're comfortable with React Native basics, you can create a new project with `--template tabs` to see how a more complete app is structured!

### Step 4: Test on Your Phone

1. Make sure your phone and computer are on the same WiFi
2. Open the Expo Go app on your phone
3. Scan the QR code from your terminal
4. Your app should open on your phone!

### Step 5: Open in Your Code Editor

```bash
code .
```

> In your terminal, use git to save a version of your code:
> ```bash
> git init
> git add --all
> git commit -m "Initial React Native Expo setup"
> ```

## Understanding the Folder Structure

The **blank template** creates a simple, minimal structure that's perfect for learning:

```
PhotosApp/
├── assets/               # Images, fonts, icons
│   ├── adaptive-icon.png
│   ├── favicon.png
│   ├── icon.png
│   └── splash.png
├── node_modules/         # Installed dependencies
├── .gitignore           # Git ignore file
├── App.js               # Main app component (single file!)
├── app.json             # Expo configuration
├── package.json         # Dependencies and scripts
└── package-lock.json    # Dependency lock file
```

### Key Differences from Web React:
- **Single App.js file** instead of multiple components initially
- **app.json** for Expo/mobile app configuration (like app name, icon, etc.)
- **assets/** for images and fonts (instead of public/ folder)
- **No src/ directory** - you start with just App.js
- **No index.html** - Expo handles the app shell

### What About the Complex Folder Structure?

If you create a project **without** specifying `--template blank` (the default), you get a much more complex structure:

```
PhotosApp/
├── app/                    # File-based routing (Expo Router)
│   ├── (tabs)/            # Tab-based navigation
│   │   ├── index.tsx      # Home screen
│   │   └── explore.tsx    # Second tab
│   └── _layout.tsx        # Root layout
├── components/            # Pre-built reusable components
├── constants/             # App constants (colors, etc.)
├── hooks/                 # Custom React hooks
└── # ... many more files
```

This is why we use `--template blank` - it gives you the simple structure shown in the first example, perfect for learning the fundamentals!

## React Native Components vs HTML Elements

Here's how React Native components compare to HTML:

| HTML (React.js) | React Native | Purpose |
|----------------|--------------|---------|
| `<div>` | `<View>` | Container element |
| `<p>`, `<h1>`, `<span>` | `<Text>` | Display text |
| `<button>` | `<TouchableOpacity>` | Clickable button |
| `<input>` | `<TextInput>` | Text input field |
| `<img>` | `<Image>` | Display images |
| `<ul>`, `<ol>` | `<FlatList>` | Scrollable lists |
| No equivalent | `<ScrollView>` | Scrollable container |
| No equivalent | `<SafeAreaView>` | Avoid phone notches |

### Example Comparison:

**Web React (HTML elements):**
```jsx
<div className="container">
  <h1>Hello World</h1>
  <p>Welcome to my website</p>
  <button onClick={handlePress}>Click me</button>
  <img src="photo.jpg" alt="Photo" />
</div>
```

**React Native (Mobile components):**
```jsx
<View style={styles.container}>
  <Text style={styles.title}>Hello World</Text>
  <Text style={styles.text}>Welcome to my app</Text>
  <TouchableOpacity style={styles.button} onPress={handlePress}>
    <Text style={styles.buttonText}>Click me</Text>
  </TouchableOpacity>
  <Image source={{uri: 'photo.jpg'}} style={styles.image} />
</View>
```

## Building Your First Component

Let's replace the default app with our own component:

- In your text editor, open **App.js** and replace the code with:

```jsx
import { View, Text, StyleSheet } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Photos App</Text>
      <Text style={styles.subtitle}>Welcome to React Native!</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  subtitle: {
    fontSize: 16,
    color: '#666',
  },
});
```

> Check your phone to see the changes automatically update!

## Styling in React Native

React Native uses **StyleSheet** objects instead of CSS files:

### Key Differences:
- **camelCase properties**: `backgroundColor` instead of `background-color`
- **No units**: Numbers are in density-independent pixels
- **Flexbox by default**: All views use flexbox layout
- **Limited CSS properties**: Not all CSS properties are available

### Common Style Properties:

```tsx
const styles = StyleSheet.create({
  container: {
    flex: 1,              // Take up available space
    backgroundColor: '#f0f0f0',  // Background color
    padding: 20,          // Padding (all sides)
    margin: 10,           // Margin (all sides)
  },
  text: {
    fontSize: 16,         // Font size
    fontWeight: 'bold',   // Font weight
    color: '#333',        // Text color
    textAlign: 'center',  // Text alignment
  },
  button: {
    backgroundColor: '#007AFF',
    paddingHorizontal: 20,
    paddingVertical: 10,
    borderRadius: 8,      // Rounded corners
  },
});
```

## Development Tip: Where Console Logs Appear

**Important for beginners**: Unlike web development where `console.log()` appears in the browser's developer console, in React Native/Expo your console logs appear in your **development server terminal** where you ran `npx expo start`.

```jsx
// Add this anywhere in your component to test
console.log('Hello from React Native!');
console.warn('This is a warning');
console.error('This is an error');
```

When you save your file:
- `console.log()` messages appear in your **terminal**
- `console.warn()` and `console.error()` show both in terminal AND as yellow/red boxes in your app
- Very helpful for debugging your code!

## Building a Photos CRUD App

Now let's build a photos app, similar to your React web guide. We'll start with mock data to focus on learning React Native components, then connect to your Rails API later.

### Step 1: Create a Photos List with Mock Data

First, let's get the basic display working with some beautiful sample photos from Unsplash.

### Step 2: Create the Photos Screen

Since we're using the blank template with a simple structure, we'll modify the existing **App.js** file to show our photos. Later, we can add navigation if needed.

- Open **App.js** and replace the content with:

```jsx
import React, { useState } from 'react';
import { 
  View, 
  Text, 
  FlatList, 
  TouchableOpacity, 
  Image, 
  StyleSheet, 
  SafeAreaView 
} from 'react-native';

export default function App() {
  // Mock data with beautiful Unsplash images
  const [photos, setPhotos] = useState([
    {
      id: 1, 
      name: "Mountain Lake", 
      url: "https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=400&h=300&fit=crop", 
      width: 400, 
      height: 300
    },
    {
      id: 2, 
      name: "City Skyline", 
      url: "https://images.unsplash.com/photo-1449824913935-59a10b8d2000?w=400&h=300&fit=crop", 
      width: 400, 
      height: 300
    },
    {
      id: 3, 
      name: "Forest Path", 
      url: "https://images.unsplash.com/photo-1441974231531-c6227db76b6e?w=400&h=300&fit=crop", 
      width: 400, 
      height: 300
    },
    {
      id: 4, 
      name: "Ocean Waves", 
      url: "https://images.unsplash.com/photo-1439066615861-d1af74d74000?w=400&h=300&fit=crop", 
      width: 400, 
      height: 300
    },
    {
      id: 5, 
      name: "Desert Sunset", 
      url: "https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=400&h=300&fit=crop", 
      width: 400, 
      height: 300
    }
  ]);

  const renderPhoto = ({ item }) => (
    <View style={styles.photoCard}>
      <Image 
        source={{ uri: item.url }} 
        style={styles.photo}
        resizeMode="cover"
      />
      <Text style={styles.photoName}>{item.name}</Text>
      <Text style={styles.photoDimensions}>
        {item.width} x {item.height}
      </Text>
    </View>
  );

  return (
    <SafeAreaView style={styles.container}>
      <Text style={styles.title}>All Photos ({photos.length})</Text>
      <FlatList
        data={photos}
        renderItem={renderPhoto}
        keyExtractor={(item) => item.id.toString()}
        style={styles.list}
        showsVerticalScrollIndicator={false}
      />
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    padding: 16,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 16,
    textAlign: 'center',
    color: '#333',
  },
  list: {
    flex: 1,
  },
  photoCard: {
    backgroundColor: '#f9f9f9',
    padding: 16,
    marginBottom: 12,
    borderRadius: 12,
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 5,
  },
  photo: {
    width: 300,
    height: 200,
    borderRadius: 8,
    marginBottom: 8,
  },
  photoName: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 4,
    color: '#333',
  },
  photoDimensions: {
    fontSize: 14,
    color: '#666',
  },
});
```

### Step 3: Test Your Photos Screen

> Check your phone - you should now see a beautiful list of photos with images from Unsplash! The mock data lets you focus on learning React Native components without worrying about network setup.

## Adding Create Functionality

Let's add the ability to create new photos:

### Step 1: Create a New Photo Modal

- Create a new folder **components/** and inside it create **PhotoForm.js**:

```jsx
import React, { useState } from 'react';
import {
  View,
  Text,
  TextInput,
  TouchableOpacity,
  Modal,
  StyleSheet,
  Alert,
} from 'react-native';

export default function PhotoForm({ visible, onClose, onSuccess }) {
  const [name, setName] = useState('');
  const [url, setUrl] = useState('');
  const [width, setWidth] = useState('');
  const [height, setHeight] = useState('');

  const handleSubmit = () => {
    if (!name || !url || !width || !height) {
      Alert.alert('Error', 'Please fill in all fields');
      return;
    }

    // Create new photo object
    const newPhoto = {
      name,
      url,
      width: parseInt(width),
      height: parseInt(height),
    };
    
    // Reset form
    setName('');
    setUrl('');
    setWidth('');
    setHeight('');
    
    // Call the success callback with the new photo
    onSuccess(newPhoto);
    onClose();
    Alert.alert('Success', 'Photo created successfully!');
  };

  return (
    <Modal visible={visible} animationType="slide" presentationStyle="pageSheet">
      <View style={styles.container}>
        <View style={styles.header}>
          <TouchableOpacity onPress={onClose}>
            <Text style={styles.cancelButton}>Cancel</Text>
          </TouchableOpacity>
          <Text style={styles.title}>New Photo</Text>
          <TouchableOpacity onPress={handleSubmit}>
            <Text style={styles.saveButton}>
              Save
            </Text>
          </TouchableOpacity>
        </View>

        <View style={styles.form}>
          <View style={styles.inputGroup}>
            <Text style={styles.label}>Name</Text>
            <TextInput
              style={styles.input}
              value={name}
              onChangeText={setName}
              placeholder="Enter photo name"
            />
          </View>

          <View style={styles.inputGroup}>
            <Text style={styles.label}>URL</Text>
            <TextInput
              style={styles.input}
              value={url}
              onChangeText={setUrl}
              placeholder="Enter photo URL"
              autoCapitalize="none"
            />
          </View>

          <View style={styles.row}>
            <View style={styles.halfInput}>
              <Text style={styles.label}>Width</Text>
              <TextInput
                style={styles.input}
                value={width}
                onChangeText={setWidth}
                placeholder="Width"
                keyboardType="numeric"
              />
            </View>

            <View style={styles.halfInput}>
              <Text style={styles.label}>Height</Text>
              <TextInput
                style={styles.input}
                value={height}
                onChangeText={setHeight}
                placeholder="Height"
                keyboardType="numeric"
              />
            </View>
          </View>
        </View>
      </View>
    </Modal>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#eee',
  },
  title: {
    fontSize: 18,
    fontWeight: 'bold',
  },
  cancelButton: {
    color: '#007AFF',
    fontSize: 16,
  },
  saveButton: {
    color: '#007AFF',
    fontSize: 16,
    fontWeight: 'bold',
  },

  form: {
    padding: 16,
  },
  inputGroup: {
    marginBottom: 16,
  },
  label: {
    fontSize: 16,
    fontWeight: '500',
    marginBottom: 8,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    borderRadius: 8,
    padding: 12,
    fontSize: 16,
  },
  row: {
    flexDirection: 'row',
    gap: 12,
  },
  halfInput: {
    flex: 1,
  },
});
```

### Step 2: Update Your App.js

Now let's add the create functionality to your existing App.js. We'll add the + button and the ability to show the PhotoForm modal.

- Update **App.js** by making these specific changes:

**1. Add new imports:**
```jsx
import React, { useState } from 'react';
import { 
  View, 
  Text, 
  FlatList, 
  TouchableOpacity, 
  Image, 
  StyleSheet, 
  SafeAreaView 
} from 'react-native';
// Add this new import:
import { Ionicons } from '@expo/vector-icons';
import PhotoForm from './components/PhotoForm';
```

**2. Add new state and function for handling photo creation:**
```jsx
export default function App() {
  // Keep your existing photos state
  const [photos, setPhotos] = useState([
    // ... your existing mock data
  ]);
  
  // Add this new state:
  const [showForm, setShowForm] = useState(false);

  // Add this function to handle when new photos are created:
  const handlePhotoCreated = (newPhoto) => {
    // Add the new photo to our existing list
    setPhotos([...photos, { ...newPhoto, id: Date.now() }]);
  };
```

**3. Update your return statement to include the header with + button:**
```jsx
  return (
    <SafeAreaView style={styles.container}>
      {/* Replace the single title with this header */}
      <View style={styles.header}>
        <Text style={styles.title}>All Photos ({photos.length})</Text>
        <TouchableOpacity 
          style={styles.addButton}
          onPress={() => setShowForm(true)}
        >
          <Ionicons name="add" size={24} color="#fff" />
        </TouchableOpacity>
      </View>

      {/* Keep your existing FlatList */}
      <FlatList
        data={photos}
        renderItem={renderPhoto}
        keyExtractor={(item) => item.id.toString()}
        style={styles.list}
        showsVerticalScrollIndicator={false}
      />

      {/* Add the PhotoForm modal */}
      <PhotoForm
        visible={showForm}
        onClose={() => setShowForm(false)}
        onSuccess={handlePhotoCreated}
      />
    </SafeAreaView>
  );
```

**4. Update your styles to improve the header layout:**

**Update your existing `title` style** by removing the `marginBottom` and adding `flex: 1`:
```jsx
title: {
  fontSize: 24,
  fontWeight: 'bold',
  textAlign: 'center',
  color: '#333',
  flex: 1,  // Add this line
  // Remove marginBottom: 16 - delete that line
},
```

**Add these new styles** to your StyleSheet:
```jsx
const styles = StyleSheet.create({
  // ... keep all your existing styles and add these:
  header: {
    flexDirection: 'row',
    justifyContent: 'center',
    alignItems: 'center',
    marginBottom: 20,
    position: 'relative',
  },
  addButton: {
    backgroundColor: '#007AFF',
    width: 40,
    height: 40,
    borderRadius: 20,
    justifyContent: 'center',
    alignItems: 'center',
    position: 'absolute',
    right: 12,
  },
});
```

> Check your phone - you should now be able to tap the + button to create new photos!

## Connecting to Your Rails API

Now that you understand the basics of React Native components and state management, let's connect to your actual Rails backend.

### Step 1: Set Up Your Rails Server

- **Important**: Start your Rails API with network access enabled:
  ```bash
  rails server -b 0.0.0.0 -p 3000
  ```
  The `-b 0.0.0.0` flag allows your phone to connect to your computer's Rails server over WiFi.

- Configure CORS to allow requests from your mobile app
- Test your API endpoints are working

### Step 2: Install Axios

```bash
npx expo install axios
```

### Step 3: Find Your Computer's IP Address

For your mobile app to connect to your Rails server, you need your computer's IP address:

**On Mac:**
```bash
ifconfig | grep "inet " | grep -v 127.0.0.1
```

**On Windows:**
```bash
ipconfig
```

Replace `YOUR_COMPUTER_IP` with your actual IP address (e.g., `192.168.1.100`).

### Step 4: Update App.js for API Calls

**Key Changes from Mock Data Version:**
- Replace mock data array with empty array and loading state
- Add `useEffect` to fetch data when component loads
- Update `handlePhotoCreated` to refresh from API (no parameter needed)
- Add loading state management
- Configure axios with your Rails server URL (this applies globally to all axios requests)

- Update your **App.js** to use real API data instead of mock data:

```jsx
import React, { useState, useEffect } from 'react';
import { 
  View, 
  Text, 
  FlatList, 
  TouchableOpacity, 
  Image, 
  StyleSheet, 
  SafeAreaView,
  Alert 
} from 'react-native';
import { Ionicons } from '@expo/vector-icons';
import axios from 'axios';
import PhotoForm from './components/PhotoForm';

// Configure axios for your Rails API (applies to all axios requests in the app)
axios.defaults.baseURL = 'http://YOUR_COMPUTER_IP:3000'; // Replace with your IP
axios.defaults.headers.common['Content-Type'] = 'application/json';

export default function App() {
  const [photos, setPhotos] = useState([]);
  const [loading, setLoading] = useState(true);
  const [showForm, setShowForm] = useState(false);

  const fetchPhotos = async () => {
    try {
      const response = await axios.get('/photos.json');
      setPhotos(response.data);
    } catch (error) {
      Alert.alert('Error', 'Failed to load photos');
      console.error(error);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchPhotos();
  }, []);

  const handlePhotoCreated = () => {
    fetchPhotos(); // Refresh the list from API
  };

  const renderPhoto = ({ item }) => (
    <View style={styles.photoCard}>
      <Image 
        source={{ uri: item.url }} 
        style={styles.photo}
        resizeMode="cover"
      />
      <Text style={styles.photoName}>{item.name}</Text>
      <Text style={styles.photoDimensions}>
        {item.width} x {item.height}
      </Text>
    </View>
  );

  if (loading) {
    return (
      <SafeAreaView style={styles.centerContainer}>
        <Text>Loading photos...</Text>
      </SafeAreaView>
    );
  }

  return (
    <SafeAreaView style={styles.container}>
      <View style={styles.header}>
        <Text style={styles.title}>All Photos ({photos.length})</Text>
        <TouchableOpacity 
          style={styles.addButton}
          onPress={() => setShowForm(true)}
        >
          <Ionicons name="add" size={24} color="#fff" />
        </TouchableOpacity>
      </View>

      <FlatList
        data={photos}
        renderItem={renderPhoto}
        keyExtractor={(item) => item.id.toString()}
        style={styles.list}
        showsVerticalScrollIndicator={false}
      />

      <PhotoForm
        visible={showForm}
        onClose={() => setShowForm(false)}
        onSuccess={handlePhotoCreated}
      />
    </SafeAreaView>
  );
}

// Add centerContainer and header styles to your existing styles:
const styles = StyleSheet.create({
  // ... keep all existing styles and add these:
  centerContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 16,
  },
  addButton: {
    backgroundColor: '#007AFF',
    width: 40,
    height: 40,
    borderRadius: 20,
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```

### Step 5: Update PhotoForm for API Calls

Now we need to update the PhotoForm component to make API calls instead of just creating local objects.

**Key Changes from Mock Data Version:**
- Add axios import (will use the same baseURL configured in App.js)
- Add loading state for async operations
- Make actual API POST request instead of creating local object
- Add error handling for network requests
- Call `onSuccess()` without parameter (parent will refresh from API)

**Note:** The axios configuration (`baseURL`) from App.js applies globally, so PhotoForm will automatically use the same server URL.

- Update your **components/PhotoForm.js** to use axios:

```jsx
import React, { useState } from 'react';
import {
  View,
  Text,
  TextInput,
  TouchableOpacity,
  Modal,
  StyleSheet,
  Alert,
} from 'react-native';
import axios from 'axios';

export default function PhotoForm({ visible, onClose, onSuccess }) {
  const [name, setName] = useState('');
  const [url, setUrl] = useState('');
  const [width, setWidth] = useState('');
  const [height, setHeight] = useState('');
  const [loading, setLoading] = useState(false);

  const handleSubmit = async () => {
    if (!name || !url || !width || !height) {
      Alert.alert('Error', 'Please fill in all fields');
      return;
    }

    setLoading(true);
    try {
      await axios.post('/photos.json', {
        name,
        url,
        width: parseInt(width),
        height: parseInt(height),
      });
      
      // Reset form
      setName('');
      setUrl('');
      setWidth('');
      setHeight('');
      
      onSuccess(); // No parameter needed - parent will refresh from API
      onClose();
      Alert.alert('Success', 'Photo created successfully!');
    } catch (error) {
      Alert.alert('Error', 'Failed to create photo');
      console.error(error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <Modal visible={visible} animationType="slide" presentationStyle="pageSheet">
      <View style={styles.container}>
        <View style={styles.header}>
          <TouchableOpacity onPress={onClose}>
            <Text style={styles.cancelButton}>Cancel</Text>
          </TouchableOpacity>
          <Text style={styles.title}>New Photo</Text>
          <TouchableOpacity onPress={handleSubmit} disabled={loading}>
            <Text style={[styles.saveButton, loading && styles.disabled]}>
              {loading ? 'Saving...' : 'Save'}
            </Text>
          </TouchableOpacity>
        </View>

        <View style={styles.form}>
          <View style={styles.inputGroup}>
            <Text style={styles.label}>Name</Text>
            <TextInput
              style={styles.input}
              value={name}
              onChangeText={setName}
              placeholder="Enter photo name"
            />
          </View>

          <View style={styles.inputGroup}>
            <Text style={styles.label}>URL</Text>
            <TextInput
              style={styles.input}
              value={url}
              onChangeText={setUrl}
              placeholder="Enter photo URL"
              autoCapitalize="none"
            />
          </View>

          <View style={styles.row}>
            <View style={styles.halfInput}>
              <Text style={styles.label}>Width</Text>
              <TextInput
                style={styles.input}
                value={width}
                onChangeText={setWidth}
                placeholder="Width"
                keyboardType="numeric"
              />
            </View>

            <View style={styles.halfInput}>
              <Text style={styles.label}>Height</Text>
              <TextInput
                style={styles.input}
                value={height}
                onChangeText={setHeight}
                placeholder="Height"
                keyboardType="numeric"
              />
            </View>
          </View>
        </View>
      </View>
    </Modal>
  );
}

// Add the disabled style back to your existing styles:
const styles = StyleSheet.create({
  // ... keep all your existing PhotoForm styles and add:
  disabled: {
    opacity: 0.5,
  },
});
```

### Step 6: Troubleshooting Network Connection

If your app can't connect to your Rails server:

1. **Verify Rails is running with network access:**
   ```bash
   rails server -b 0.0.0.0 -p 3000
   ```
   You should see: `Listening on http://0.0.0.0:3000`

2. **Test the connection in your browser:**
   - On your computer: Visit `http://YOUR_COMPUTER_IP:3000/photos.json`
   - You should see your JSON data

3. **Check WiFi connection:**
   - Make sure your phone and computer are on the same WiFi network
   - Some public or corporate WiFi networks block device-to-device communication

4. **Firewall issues:**
   - Your computer's firewall might be blocking the connection

### The Complete Transition

You've now successfully transitioned from:
- **Mock Data**: Local array with instant updates, great for learning React Native
- **API Integration**: Real network requests to your Rails backend

Your app now:
- ✅ Loads photos from your Rails API on app start
- ✅ Creates new photos via API calls
- ✅ Handles loading states and network errors
- ✅ Refreshes data after creating new photos

> Your app now loads real data from your Rails API and can create new photos that persist on the server!

## Running on Different Platforms

One of Expo's biggest advantages is running on multiple platforms:

### 1. Mobile Testing (Current Setup)
- **Expo Go app**: Instant testing on real devices
- **iOS Simulator**: If you have a Mac with Xcode
- **Android Emulator**: If you have Android Studio

### 2. Web Development
```bash
npx expo start --web
```
Your app will open in a web browser! The same code runs on web, iOS, and Android.

### 3. Production Builds

When ready for app stores:
```bash
npx expo build:android  # Android APK
npx expo build:ios      # iOS IPA
```

## Common React Native Patterns

### 1. Safe Area Handling
```tsx
import { SafeAreaView } from 'react-native-safe-area-context';

export default function Screen() {
  return (
    <SafeAreaView style={styles.container}>
      {/* Your content here */}
    </SafeAreaView>
  );
}
```

### 2. Loading States
```tsx
const [loading, setLoading] = useState(true);

if (loading) {
  return (
    <View style={styles.centerContainer}>
      <ActivityIndicator size="large" color="#007AFF" />
      <Text>Loading...</Text>
    </View>
  );
}
```

### 3. Error Handling
```tsx
try {
  await axios.get('/api/data');
} catch (error) {
  Alert.alert('Error', 'Something went wrong');
}
```

### 4. Pull to Refresh
```tsx
<FlatList
  data={data}
  renderItem={renderItem}
  refreshing={refreshing}
  onRefresh={onRefresh}
/>
```

## When to Use React Native vs Native Development

### Choose React Native When:
- You want to share code between iOS and Android
- Your team knows JavaScript/React
- You need to move fast and iterate quickly
- Your app is content-focused (social, e-commerce, news)
- You want to reach both platforms with one team

### Choose Native Development When:
- You need maximum performance (games, AR/VR)
- Your app uses cutting-edge platform features
- You have separate iOS and Android teams
- You're building complex animations or custom UI
- Platform-specific user experience is critical

## Next Steps

Now that you have a working React Native app with Expo:

1. **Add more CRUD operations** (edit, delete photos)
2. **Implement navigation** between screens
3. **Add local storage** for offline functionality
4. **Explore Expo APIs** (camera, location, notifications)
5. **Style your app** with custom themes and animations
6. **Prepare for production** with EAS Build

### Useful Resources:
- [Expo Documentation](https://docs.expo.dev/)
- [React Native Documentation](https://reactnative.dev/)
- [React Navigation](https://reactnavigation.org/)
- [Expo Examples](https://github.com/expo/examples)

> In your terminal, use git to save your progress:
> ```bash
> git add --all
> git commit -m "Add photos CRUD functionality"
> ```

Congratulations! You've built your first React Native app with Expo that runs on iOS, Android, and web from a single codebase. This foundation will serve you well as you build more complex mobile applications.

---

# Advanced: Building with the Default Template

Now that you understand React Native fundamentals, let's explore the power of the **default template** with TypeScript, file-based routing, and professional app structure. We'll build a more sophisticated **Task Manager app** that showcases advanced patterns.

## Why the Default Template?

The default template provides:
- **TypeScript** out of the box for better development experience
- **File-based routing** with Expo Router for navigation
- **Tab navigation** pre-configured
- **Professional folder structure** used in production apps
- **Pre-built components** and utilities
- **Better developer tools** and debugging

## Creating the Advanced App

### Step 1: Create a New Project with Default Template

```bash
npx create-expo-app TaskManagerPro
cd TaskManagerPro
npm start
```

Notice the difference - no `--template blank` flag means we get the full-featured default template!

### Step 2: Explore the Advanced Structure

Your project now has a sophisticated structure:

```
TaskManagerPro/
├── app/                     # File-based routing
│   ├── (tabs)/             # Tab navigation group
│   │   ├── index.tsx       # Home tab
│   │   └── explore.tsx     # Explore tab
│   ├── +not-found.tsx      # 404 page
│   └── _layout.tsx         # Root layout
├── components/             # Reusable UI components
│   ├── ui/                # UI primitives
│   ├── Collapsible.tsx    # Animated components
│   ├── ExternalLink.tsx   # Link component
│   ├── HelloWave.tsx      # Animated hello wave
│   ├── ParallaxScrollView.tsx  # Advanced scroll views
│   ├── ThemedText.tsx     # Themed text component
│   └── ThemedView.tsx     # Themed view component
├── constants/             # App-wide constants
│   └── Colors.ts          # Color themes
├── hooks/                 # Custom React hooks
│   ├── useColorScheme.ts  # Dark/light mode
│   ├── useColorScheme.web.ts
│   └── useThemeColor.ts   # Theme management
└── assets/               # Images, fonts, icons
```

### Step 3: Understanding File-Based Routing

Unlike our simple App.js approach, the default template uses **Expo Router**:

- **`app/(tabs)/index.tsx`** = Home tab screen
- **`app/(tabs)/explore.tsx`** = Explore tab screen  
- **`app/_layout.tsx`** = Root layout with navigation
- **Navigation is automatic** based on file structure

## Building the Task Manager App

Let's transform this into a professional task management app with multiple features.

### Step 4: Create the Tasks Tab

**Important Note about Safe Areas:** Mobile devices have dynamic islands, notches, and home indicators that can overlap with your content. We use `useSafeAreaInsets()` to get the safe area measurements and apply proper padding to avoid these areas.

**UI Design Note:** The task cards use clean, minimal design principles:
- Pure white cards on light gray background for maximum contrast
- Standard React Native components (View/Text) for consistent styling
- Simple circular checkboxes with green completion state
- Subtle priority badges with light gray backgrounds
- Minimal shadows for gentle depth without heaviness
- Explicit color values for perfect control over appearance
- Clean typography and generous spacing for readability

- Replace **`app/(tabs)/index.tsx`** with our tasks screen:

```tsx
import { StyleSheet, FlatList, Alert, View, Text, TouchableOpacity } from 'react-native';
import { useState, useEffect } from 'react';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { Ionicons } from '@expo/vector-icons';
import { useSafeAreaInsets } from 'react-native-safe-area-context';

interface Task {
  id: string;
  title: string;
  description: string;
  completed: boolean;
  priority: 'low' | 'medium' | 'high';
  createdAt: string;
}

export default function TasksScreen() {
  const insets = useSafeAreaInsets();
  
  const [tasks, setTasks] = useState<Task[]>([
    {
      id: '1',
      title: 'Learn React Native',
      description: 'Complete the Expo guide and build a mobile app',
      completed: false,
      priority: 'high',
      createdAt: new Date().toISOString(),
    },
    {
      id: '2', 
      title: 'Review TypeScript',
      description: 'Study TypeScript fundamentals for mobile development',
      completed: true,
      priority: 'medium',
      createdAt: new Date().toISOString(),
    },
    {
      id: '3',
      title: 'Plan next project',
      description: 'Research and plan the next mobile app project',
      completed: false,
      priority: 'low',
      createdAt: new Date().toISOString(),
    },
  ]);

  const toggleTask = (id: string) => {
    setTasks(tasks.map(task => 
      task.id === id ? { ...task, completed: !task.completed } : task
    ));
  };

  const deleteTask = (id: string) => {
    Alert.alert(
      'Delete Task',
      'Are you sure you want to delete this task?',
      [
        { text: 'Cancel', style: 'cancel' },
        { 
          text: 'Delete', 
          style: 'destructive',
          onPress: () => setTasks(tasks.filter(task => task.id !== id))
        },
      ]
    );
  };

  const getPriorityColor = (priority: string) => {
    switch (priority) {
      case 'high': return '#ff4444';
      case 'medium': return '#ffaa00';
      case 'low': return '#44ff44';
      default: return '#888';
    }
  };

  const renderTask = ({ item }: { item: Task }) => (
    <View style={styles.taskCard}>
      <TouchableOpacity 
        style={[
          styles.checkbox, 
          item.completed && styles.checkboxCompleted
        ]}
        onPress={() => toggleTask(item.id)}
      >
        {item.completed && (
          <Ionicons name="checkmark" size={16} color="#fff" />
        )}
      </TouchableOpacity>
      
      <View style={styles.taskContent}>
        <View style={styles.taskHeader}>
          <Text 
            style={[styles.taskTitle, item.completed && styles.completedTask]}
          >
            {item.title}
          </Text>
          <View style={styles.priorityBadge}>
            <View 
              style={[styles.priorityDot, { backgroundColor: getPriorityColor(item.priority) }]} 
            />
            <Text 
              style={[styles.priorityText, { color: getPriorityColor(item.priority) }]}
            >
              {item.priority}
            </Text>
          </View>
        </View>
        
        <Text style={styles.taskDescription}>
          {item.description}
        </Text>
        
        <View style={styles.taskFooter}>
          <Text style={styles.taskDate}>
            {new Date(item.createdAt).toLocaleDateString()}
          </Text>
          <TouchableOpacity 
            style={styles.deleteButton}
            onPress={() => deleteTask(item.id)}
          >
            <Ionicons name="trash-outline" size={18} color="#ff4444" />
          </TouchableOpacity>
        </View>
      </View>
    </View>
  );

  const completedCount = tasks.filter(task => task.completed).length;

  return (
    <View style={[styles.container, { paddingTop: insets.top + 20 }]}>
      <View style={styles.header}>
        <Text style={styles.title}>My Tasks</Text>
        <Text style={styles.subtitle}>
          {completedCount} of {tasks.length} completed
        </Text>
      </View>

      <FlatList
        data={tasks}
        renderItem={renderTask}
        keyExtractor={(item) => item.id}
        style={styles.taskList}
        showsVerticalScrollIndicator={false}
        contentContainerStyle={{ paddingBottom: insets.bottom + 20 }}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
    backgroundColor: '#f8f9fa',
  },
  header: {
    marginBottom: 20,
  },
  title: {
    fontSize: 28,
    fontWeight: 'bold',
    marginBottom: 4,
    color: '#1a1a1a',
  },
  subtitle: {
    fontSize: 16,
    opacity: 0.7,
    color: '#666666',
  },
  taskList: {
    flex: 1,
  },
  taskCard: {
    flexDirection: 'row',
    padding: 20,
    marginBottom: 16,
    borderRadius: 12,
    backgroundColor: '#ffffff',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.05,
    shadowRadius: 4,
    elevation: 2,
    alignItems: 'flex-start',
  },
  checkbox: {
    width: 24,
    height: 24,
    borderRadius: 12,
    borderWidth: 2,
    borderColor: '#ddd',
    marginRight: 16,
    marginTop: 2,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: 'transparent',
  },
  checkboxCompleted: {
    backgroundColor: '#4CAF50',
    borderColor: '#4CAF50',
  },
  taskContent: {
    flex: 1,
  },
  taskHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'flex-start',
    marginBottom: 8,
  },
  priorityBadge: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: '#f8f9fa',
    paddingHorizontal: 8,
    paddingVertical: 4,
    borderRadius: 8,
  },
  priorityDot: {
    width: 6,
    height: 6,
    borderRadius: 3,
    marginRight: 6,
  },
  priorityText: {
    fontSize: 11,
    textTransform: 'capitalize',
    fontWeight: '600',
  },
  taskTitle: {
    fontSize: 16,
    fontWeight: '600',
    marginBottom: 6,
    flex: 1,
    marginRight: 12,
    color: '#1a1a1a',
  },
  completedTask: {
    textDecorationLine: 'line-through',
    opacity: 0.6,
  },
  taskDescription: {
    fontSize: 14,
    opacity: 0.8,
    marginBottom: 12,
    lineHeight: 20,
    color: '#666666',
  },
  taskFooter: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
  },
  taskDate: {
    fontSize: 12,
    opacity: 0.6,
    color: '#999999',
  },
  deleteButton: {
    padding: 8,
    borderRadius: 6,
    backgroundColor: '#fff5f5',
  },
});
```

### Step 5: Create the Stats Tab

**Clean Styling Note:** Like the Tasks tab, we use standard React Native components (`View`/`Text`) instead of themed components to ensure clean white cards with explicit color control and consistent styling across all devices.

- Replace **`app/(tabs)/explore.tsx`** with a statistics screen:

```tsx
import { StyleSheet, Dimensions, ScrollView, View, Text } from 'react-native';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { Ionicons } from '@expo/vector-icons';
import { useSafeAreaInsets } from 'react-native-safe-area-context';

const { width } = Dimensions.get('window');

export default function StatsScreen() {
  const insets = useSafeAreaInsets();
  
  // Mock data - in a real app, this would come from your task data
  const stats = {
    totalTasks: 15,
    completed: 8,
    pending: 7,
    highPriority: 3,
    productivity: 73, // percentage
  };

  const StatCard = ({ 
    title, 
    value, 
    icon, 
    color 
  }: { 
    title: string; 
    value: string | number; 
    icon: string; 
    color: string; 
  }) => (
    <View style={[styles.statCard, { borderLeftColor: color }]}>
      <View style={styles.statHeader}>
        <Ionicons name={icon as any} size={24} color={color} />
        <Text style={styles.statValue}>{value}</Text>
      </View>
      <Text style={styles.statTitle}>{title}</Text>
    </View>
  );

  const ProgressBar = ({ 
    progress, 
    color 
  }: { 
    progress: number; 
    color: string 
  }) => (
    <View style={styles.progressBarContainer}>
      <View 
        style={[
          styles.progressBar, 
          { width: `${progress}%`, backgroundColor: color }
        ]} 
      />
    </View>
  );

  return (
    <View style={[styles.container, { paddingTop: insets.top + 20 }]}>
      <ScrollView 
        showsVerticalScrollIndicator={false}
        contentContainerStyle={{ paddingBottom: insets.bottom + 20 }}
      >
        <Text style={styles.title}>Statistics</Text>
        
        <View style={styles.statsGrid}>
          <StatCard
            title="Total Tasks"
            value={stats.totalTasks}
            icon="list-outline"
            color="#007AFF"
          />
          <StatCard
            title="Completed"
            value={stats.completed}
            icon="checkmark-circle-outline"
            color="#4CAF50"
          />
          <StatCard
            title="Pending"
            value={stats.pending}
            icon="time-outline"
            color="#ff9500"
          />
          <StatCard
            title="High Priority"
            value={stats.highPriority}
            icon="alert-circle-outline"
            color="#ff4444"
          />
        </View>

        <View style={styles.productivitySection}>
          <Text style={styles.sectionTitle}>Productivity</Text>
          <View style={styles.productivityCard}>
            <Text style={styles.productivityPercentage}>
              {stats.productivity}%
            </Text>
            <ProgressBar progress={stats.productivity} color="#4CAF50" />
            <Text style={styles.productivityLabel}>
              Great job! You're {stats.productivity}% productive this week.
            </Text>
          </View>
        </View>

        <View style={styles.achievementsSection}>
          <Text style={styles.sectionTitle}>Achievements</Text>
          <View style={styles.achievementCard}>
            <Ionicons name="trophy" size={32} color="#FFD700" />
            <View style={styles.achievementText}>
              <Text style={styles.achievementTitle}>Task Master</Text>
              <Text style={styles.achievementDescription}>
                Completed 5 tasks in a row!
              </Text>
            </View>
          </View>
        </View>
      </ScrollView>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
    backgroundColor: '#f8f9fa',
  },
  title: {
    fontSize: 28,
    fontWeight: 'bold',
    marginBottom: 20,
    color: '#1a1a1a',
  },
  statsGrid: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    justifyContent: 'space-between',
    marginBottom: 24,
  },
  statCard: {
    width: (width - 48) / 2,
    padding: 16,
    marginBottom: 16,
    borderRadius: 12,
    borderLeftWidth: 4,
    backgroundColor: '#ffffff',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.05,
    shadowRadius: 4,
    elevation: 2,
  },
  statHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 8,
  },
  statValue: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#1a1a1a',
  },
  statTitle: {
    fontSize: 14,
    opacity: 0.7,
    color: '#666666',
  },
  productivitySection: {
    marginBottom: 24,
  },
  sectionTitle: {
    fontSize: 20,
    fontWeight: '600',
    marginBottom: 12,
    color: '#1a1a1a',
  },
  productivityCard: {
    padding: 20,
    borderRadius: 12,
    backgroundColor: '#ffffff',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.05,
    shadowRadius: 4,
    elevation: 2,
    alignItems: 'center',
  },
  productivityPercentage: {
    fontSize: 48,
    fontWeight: 'bold',
    color: '#4CAF50',
    marginBottom: 12,
  },
  progressBarContainer: {
    width: '100%',
    height: 8,
    backgroundColor: '#e9ecef',
    borderRadius: 4,
    marginBottom: 12,
  },
  progressBar: {
    height: '100%',
    borderRadius: 4,
  },
  productivityLabel: {
    textAlign: 'center',
    opacity: 0.8,
    color: '#666666',
  },
  achievementsSection: {
    marginBottom: 24,
  },
  achievementCard: {
    flexDirection: 'row',
    padding: 16,
    borderRadius: 12,
    backgroundColor: '#ffffff',
    borderLeftWidth: 4,
    borderLeftColor: '#FFD700',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.05,
    shadowRadius: 4,
    elevation: 2,
    alignItems: 'center',
  },
  achievementText: {
    marginLeft: 16,
    flex: 1,
  },
  achievementTitle: {
    fontSize: 16,
    fontWeight: '600',
    marginBottom: 4,
    color: '#1a1a1a',
  },
  achievementDescription: {
    fontSize: 14,
    opacity: 0.8,
    color: '#666666',
  },
});
```

### Step 6: Update Tab Configuration

**Clean Tab Styling:** The tab configuration uses the default Expo styling which automatically provides clean, platform-appropriate tab bars that complement our white card design.

- Update **`app/(tabs)/_layout.tsx`** to reflect our new app:

```tsx
import { Tabs } from 'expo-router';
import React from 'react';
import { Platform } from 'react-native';

import { HapticTab } from '@/components/HapticTab';
import { IconSymbol } from '@/components/ui/IconSymbol';
import TabBarBackground from '@/components/ui/TabBarBackground';
import { Colors } from '@/constants/Colors';
import { useColorScheme } from '@/hooks/useColorScheme';

export default function TabLayout() {
  const colorScheme = useColorScheme();

  return (
    <Tabs
      screenOptions={{
        tabBarActiveTintColor: Colors[colorScheme ?? 'light'].tint,
        headerShown: false,
        tabBarButton: HapticTab,
        tabBarBackground: TabBarBackground,
        tabBarStyle: Platform.select({
          ios: {
            // Use a transparent background on iOS to show the blur effect
            position: 'absolute',
          },
          default: {},
        }),
      }}>
      <Tabs.Screen
        name="index"
        options={{
          title: 'Tasks',
          tabBarIcon: ({ color }) => <IconSymbol size={28} name="list.bullet" color={color} />,
        }}
      />
      <Tabs.Screen
        name="explore"
        options={{
          title: 'Stats',
          tabBarIcon: ({ color }) => <IconSymbol size={28} name="chart.bar.fill" color={color} />,
        }}
      />
    </Tabs>
  );
}
```

### Step 7: Add Functional Task Management with Local Storage

Now let's make the app fully functional with the ability to create, edit, and delete tasks, with data persisted locally on the device.

#### Install AsyncStorage for Local Storage

First, install the AsyncStorage package:

```bash
npx expo install @react-native-async-storage/async-storage
```

#### Update the Tasks Screen with Full CRUD Functionality

Replace **`app/(tabs)/index.tsx`** with this enhanced version:

```tsx
import { StyleSheet, FlatList, Alert, View, Text, TouchableOpacity, Modal, TextInput } from 'react-native';
import { useState, useEffect } from 'react';
import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { Ionicons } from '@expo/vector-icons';
import { useSafeAreaInsets } from 'react-native-safe-area-context';
import AsyncStorage from '@react-native-async-storage/async-storage';

interface Task {
  id: string;
  title: string;
  description: string;
  priority: 'low' | 'medium' | 'high';
  completed: boolean;
  createdAt: string;
}

export default function TasksScreen() {
  const insets = useSafeAreaInsets();
  
  const [tasks, setTasks] = useState<Task[]>([]);
  const [showForm, setShowForm] = useState(false);
  const [editingTask, setEditingTask] = useState<Task | null>(null);
  const [formData, setFormData] = useState({
    title: '',
    description: '',
    priority: 'medium' as 'low' | 'medium' | 'high'
  });

  // Load tasks from storage on app start
  useEffect(() => {
    loadTasks();
  }, []);

  const loadTasks = async () => {
    try {
      const storedTasks = await AsyncStorage.getItem('tasks');
      if (storedTasks) {
        setTasks(JSON.parse(storedTasks));
      }
    } catch (error) {
      console.error('Error loading tasks:', error);
    }
  };

  const saveTasks = async (newTasks: Task[]) => {
    try {
      await AsyncStorage.setItem('tasks', JSON.stringify(newTasks));
      setTasks(newTasks);
    } catch (error) {
      console.error('Error saving tasks:', error);
      Alert.alert('Error', 'Failed to save tasks');
    }
  };

  const toggleTask = (id: string) => {
    const updatedTasks = tasks.map(task =>
      task.id === id ? { ...task, completed: !task.completed } : task
    );
    saveTasks(updatedTasks);
  };

  const deleteTask = (id: string) => {
    Alert.alert(
      'Delete Task',
      'Are you sure you want to delete this task?',
      [
        { text: 'Cancel', style: 'cancel' },
        {
          text: 'Delete',
          style: 'destructive',
          onPress: () => {
            const updatedTasks = tasks.filter(task => task.id !== id);
            saveTasks(updatedTasks);
          }
        }
      ]
    );
  };

  const openAddForm = () => {
    setEditingTask(null);
    setFormData({ title: '', description: '', priority: 'medium' });
    setShowForm(true);
  };

  const openEditForm = (task: Task) => {
    setEditingTask(task);
    setFormData({
      title: task.title,
      description: task.description,
      priority: task.priority
    });
    setShowForm(true);
  };

  const handleSubmit = () => {
    if (!formData.title.trim()) {
      Alert.alert('Error', 'Please enter a task title');
      return;
    }

    if (editingTask) {
      // Edit existing task
      const updatedTasks = tasks.map(task =>
        task.id === editingTask.id
          ? { ...task, ...formData }
          : task
      );
      saveTasks(updatedTasks);
    } else {
      // Create new task
      const newTask: Task = {
        id: Date.now().toString(),
        ...formData,
        completed: false,
        createdAt: new Date().toISOString()
      };
      saveTasks([...tasks, newTask]);
    }

    setShowForm(false);
    setFormData({ title: '', description: '', priority: 'medium' });
  };

  const getPriorityColor = (priority: string) => {
    switch (priority) {
      case 'high': return '#ff4444';
      case 'medium': return '#ffaa00';
      case 'low': return '#44ff44';
      default: return '#888';
    }
  };

  const renderTask = ({ item }: { item: Task }) => (
    <View style={styles.taskCard}>
      <TouchableOpacity 
        style={[
          styles.checkbox, 
          item.completed && styles.checkboxCompleted
        ]}
        onPress={() => toggleTask(item.id)}
      >
        {item.completed && (
          <Ionicons name="checkmark" size={16} color="#fff" />
        )}
      </TouchableOpacity>
      
      <TouchableOpacity 
        style={styles.taskContent}
        onPress={() => openEditForm(item)}
      >
        <View style={styles.taskHeader}>
          <Text 
            style={[styles.taskTitle, item.completed && styles.completedTask]}
          >
            {item.title}
          </Text>
          <View style={styles.priorityBadge}>
            <View 
              style={[styles.priorityDot, { backgroundColor: getPriorityColor(item.priority) }]} 
            />
            <Text 
              style={[styles.priorityText, { color: getPriorityColor(item.priority) }]}
            >
              {item.priority}
            </Text>
          </View>
        </View>
        
        <Text style={styles.taskDescription}>
          {item.description}
        </Text>
        
        <View style={styles.taskFooter}>
          <Text style={styles.taskDate}>
            {new Date(item.createdAt).toLocaleDateString()}
          </Text>
          <TouchableOpacity 
            style={styles.deleteButton}
            onPress={() => deleteTask(item.id)}
          >
            <Ionicons name="trash-outline" size={18} color="#ff4444" />
          </TouchableOpacity>
        </View>
      </TouchableOpacity>
    </View>
  );

  const completedCount = tasks.filter(task => task.completed).length;

  return (
    <View style={[styles.container, { paddingTop: insets.top + 20 }]}>
      <View style={styles.header}>
        <Text style={styles.title}>My Tasks</Text>
        <Text style={styles.subtitle}>
          {completedCount} of {tasks.length} completed
        </Text>
        <TouchableOpacity style={styles.addButton} onPress={openAddForm}>
          <Ionicons name="add" size={24} color="#fff" />
        </TouchableOpacity>
      </View>

      <FlatList
        data={tasks}
        renderItem={renderTask}
        keyExtractor={(item) => item.id}
        style={styles.taskList}
        showsVerticalScrollIndicator={false}
        contentContainerStyle={{ paddingBottom: insets.bottom + 20 }}
        ListEmptyComponent={
          <View style={styles.emptyState}>
            <Ionicons name="checkmark-circle-outline" size={64} color="#ccc" />
            <Text style={styles.emptyTitle}>No tasks yet</Text>
            <Text style={styles.emptyDescription}>Tap the + button to create your first task</Text>
          </View>
        }
      />

      {/* Task Form Modal */}
      <Modal visible={showForm} animationType="slide" presentationStyle="pageSheet">
        <View style={[styles.modalContainer, { paddingTop: insets.top + 20 }]}>
          <View style={styles.modalHeader}>
            <TouchableOpacity onPress={() => setShowForm(false)}>
              <Text style={styles.cancelButton}>Cancel</Text>
            </TouchableOpacity>
            <Text style={styles.modalTitle}>
              {editingTask ? 'Edit Task' : 'New Task'}
            </Text>
            <TouchableOpacity onPress={handleSubmit}>
              <Text style={styles.saveButton}>Save</Text>
            </TouchableOpacity>
          </View>

          <View style={styles.form}>
            <View style={styles.inputGroup}>
              <Text style={styles.label}>Title</Text>
              <TextInput
                style={styles.input}
                value={formData.title}
                onChangeText={(title) => setFormData({ ...formData, title })}
                placeholder="Enter task title"
                multiline
              />
            </View>

            <View style={styles.inputGroup}>
              <Text style={styles.label}>Description</Text>
              <TextInput
                style={[styles.input, styles.textArea]}
                value={formData.description}
                onChangeText={(description) => setFormData({ ...formData, description })}
                placeholder="Enter task description"
                multiline
                numberOfLines={3}
              />
            </View>

            <View style={styles.inputGroup}>
              <Text style={styles.label}>Priority</Text>
              <View style={styles.priorityOptions}>
                {(['low', 'medium', 'high'] as const).map((priority) => (
                  <TouchableOpacity
                    key={priority}
                    style={[
                      styles.priorityOption,
                      formData.priority === priority && styles.priorityOptionSelected,
                      { borderColor: getPriorityColor(priority) }
                    ]}
                    onPress={() => setFormData({ ...formData, priority })}
                  >
                    <View 
                      style={[styles.priorityDot, { backgroundColor: getPriorityColor(priority) }]} 
                    />
                    <Text 
                      style={[
                        styles.priorityOptionText,
                        formData.priority === priority && { color: getPriorityColor(priority) }
                      ]}
                    >
                      {priority.charAt(0).toUpperCase() + priority.slice(1)}
                    </Text>
                  </TouchableOpacity>
                ))}
              </View>
            </View>
          </View>
        </View>
      </Modal>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
    backgroundColor: '#f8f9fa',
  },
  header: {
    marginBottom: 20,
    position: 'relative',
  },
  title: {
    fontSize: 28,
    fontWeight: 'bold',
    marginBottom: 4,
    color: '#1a1a1a',
    textAlign: 'center',
  },
  subtitle: {
    fontSize: 16,
    opacity: 0.7,
    color: '#666666',
    textAlign: 'center',
  },
  addButton: {
    position: 'absolute',
    right: 0,
    top: 8,
    backgroundColor: '#007AFF',
    width: 40,
    height: 40,
    borderRadius: 20,
    justifyContent: 'center',
    alignItems: 'center',
  },
  taskList: {
    flex: 1,
  },
  taskCard: {
    flexDirection: 'row',
    padding: 20,
    marginBottom: 16,
    borderRadius: 12,
    backgroundColor: '#ffffff',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.05,
    shadowRadius: 4,
    elevation: 2,
    alignItems: 'flex-start',
  },
  checkbox: {
    width: 24,
    height: 24,
    borderRadius: 12,
    borderWidth: 2,
    borderColor: '#ddd',
    marginRight: 16,
    marginTop: 2,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: 'transparent',
  },
  checkboxCompleted: {
    backgroundColor: '#4CAF50',
    borderColor: '#4CAF50',
  },
  taskContent: {
    flex: 1,
  },
  taskHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'flex-start',
    marginBottom: 8,
  },
  priorityBadge: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: '#f8f9fa',
    paddingHorizontal: 8,
    paddingVertical: 4,
    borderRadius: 8,
  },
  priorityDot: {
    width: 6,
    height: 6,
    borderRadius: 3,
    marginRight: 6,
  },
  priorityText: {
    fontSize: 11,
    textTransform: 'capitalize',
    fontWeight: '600',
  },
  taskTitle: {
    fontSize: 16,
    fontWeight: '600',
    marginBottom: 6,
    flex: 1,
    marginRight: 12,
    color: '#1a1a1a',
  },
  completedTask: {
    textDecorationLine: 'line-through',
    opacity: 0.6,
  },
  taskDescription: {
    fontSize: 14,
    opacity: 0.8,
    marginBottom: 12,
    lineHeight: 20,
    color: '#666666',
  },
  taskFooter: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
  },
  taskDate: {
    fontSize: 12,
    opacity: 0.6,
    color: '#999999',
  },
  deleteButton: {
    padding: 8,
    borderRadius: 6,
    backgroundColor: '#fff5f5',
  },
  emptyState: {
    alignItems: 'center',
    justifyContent: 'center',
    paddingVertical: 60,
  },
  emptyTitle: {
    fontSize: 18,
    fontWeight: '600',
    color: '#1a1a1a',
    marginTop: 16,
    marginBottom: 8,
  },
  emptyDescription: {
    fontSize: 14,
    color: '#666666',
    textAlign: 'center',
  },
  modalContainer: {
    flex: 1,
    backgroundColor: '#f8f9fa',
    padding: 16,
  },
  modalHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 24,
  },
  cancelButton: {
    fontSize: 16,
    color: '#666666',
  },
  modalTitle: {
    fontSize: 18,
    fontWeight: '600',
    color: '#1a1a1a',
  },
  saveButton: {
    fontSize: 16,
    color: '#007AFF',
    fontWeight: '600',
  },
  form: {
    flex: 1,
  },
  inputGroup: {
    marginBottom: 24,
  },
  label: {
    fontSize: 16,
    fontWeight: '600',
    color: '#1a1a1a',
    marginBottom: 8,
  },
  input: {
    backgroundColor: '#ffffff',
    borderRadius: 8,
    padding: 12,
    fontSize: 16,
    borderWidth: 1,
    borderColor: '#e0e0e0',
    color: '#1a1a1a',
  },
  textArea: {
    minHeight: 80,
    textAlignVertical: 'top',
  },
  priorityOptions: {
    flexDirection: 'row',
    gap: 12,
  },
  priorityOption: {
    flex: 1,
    flexDirection: 'row',
    alignItems: 'center',
    padding: 12,
    borderRadius: 8,
    borderWidth: 2,
    backgroundColor: '#ffffff',
  },
  priorityOptionSelected: {
    backgroundColor: '#f8f9fa',
  },
  priorityOptionText: {
    fontSize: 14,
    fontWeight: '500',
    color: '#666666',
  },
});
```

#### Key Features Added:

1. **AsyncStorage Integration**: All tasks are saved locally and persist between app sessions
2. **Create Tasks**: Tap the + button to add new tasks with title, description, and priority
3. **Edit Tasks**: Tap any task to edit its details
4. **Delete Tasks**: Tap the trash icon with confirmation dialog
5. **Complete Tasks**: Tap the checkbox to mark tasks as complete
6. **Empty State**: Shows helpful message when no tasks exist
7. **Form Validation**: Ensures required fields are filled
8. **Priority Selection**: Visual priority picker with color coding
9. **Modal Forms**: Clean sliding modal for creating/editing tasks

#### How Local Storage Works:

- **`loadTasks()`**: Retrieves tasks from AsyncStorage on app start
- **`saveTasks()`**: Saves tasks to AsyncStorage whenever data changes
- **Automatic persistence**: All CRUD operations automatically save to storage
- **Error handling**: Graceful error handling for storage operations

Your Task Manager app is now fully functional with persistent local storage! 🎉

## Advanced Features Demonstrated

This advanced version showcases:

### 1. **TypeScript Integration**
- Type-safe interfaces (`Task`, `StatCard` props)
- Proper typing for components and data
- Better IDE support and error catching

### 2. **Clean UI Components**
- Standard React Native components (`View`, `Text`) for consistent styling
- Explicit color schemes for reliable appearance
- Pure white cards with subtle shadows
- Responsive design with `Dimensions`

### 3. **Advanced React Patterns**
- Custom hooks for theme management
- Proper state management with TypeScript
- Component composition and reusability

### 4. **File-Based Navigation**
- Automatic routing based on file structure
- Tab navigation with icons and theming
- Professional navigation patterns

### 5. **Enhanced User Experience**
- Clean, minimal card-based design
- Haptic feedback on interactions
- Safe area handling for all devices
- Progress indicators and statistics
- Interactive task management
- Alert dialogs for confirmations

### 6. **Production-Ready Structure**
- Organized component hierarchy
- Clean, consistent styling with explicit colors
- Safe area handling for all devices
- Scalable architecture patterns
- Type safety throughout

## Key Differences from Basic Version

| Basic (Blank Template) | Advanced (Default Template) |
|----------------------|----------------------------|
| Single App.js file | Multiple screens with routing |
| JavaScript | TypeScript |
| Basic styling | Clean white cards with explicit colors |
| Simple navigation | Tab-based navigation |
| Manual state management | Professional patterns |
| Basic UI components | Modern card-based UI |

## Next Steps for Students

After completing this advanced section, students can:

1. **Add more screens** using file-based routing
2. **Implement search and filtering** functionality  
3. **Add local storage** to persist tasks
4. **Integrate with APIs** for backend synchronization
5. **Add animations** using Reanimated
6. **Implement push notifications** 
7. **Add user authentication** flows
8. **Deploy to app stores** using EAS Build

This advanced version demonstrates how the default template provides a powerful foundation for building production-ready mobile applications with React Native and Expo! 