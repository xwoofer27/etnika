# Firebase Firestore Setup Guide

## Overview
Your admin panel is now integrated with Firebase Firestore to manage products from the `ethnika-dbm` Firebase project.

## How to Get Your Firebase Configuration

1. **Go to Firebase Console**
   - Visit: https://console.firebase.google.com/
   - Select your project: `ethnika-dbm`

2. **Get Your Project Settings**
   - Click the ⚙️ (Settings) icon at the top left
   - Select "Project Settings"
   - Go to the "General" tab
   - Scroll down to "Your apps" section
   - Click on the Web app (if you don't have one, click "Add app" and select Web)

3. **Copy Your Firebase Config**
   - You'll see a `firebaseConfig` object that looks like:
   ```javascript
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "ethnika-dbm.firebaseapp.com",
     projectId: "ethnika-dbm",
     storageBucket: "ethnika-dbm.appspot.com",
     messagingSenderId: "123456789...",
     appId: "1:123456789:web:abc123..."
   };
   ```

## How to Update Your Admin Panel

1. **Open** `admin/admin.html`

2. **Find** the Firebase Configuration section (around line 580-590):
   ```javascript
   // ===================== FIREBASE CONFIGURATION =====================
   // TODO: Replace with your Firebase project credentials from Firebase Console
   const firebaseConfig = {
       apiKey: "YOUR_API_KEY",
       authDomain: "ethnika-dbm.firebaseapp.com",
       projectId: "ethnika-dbm",
       storageBucket: "ethnika-dbm.appspot.com",
       messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
       appId: "YOUR_APP_ID"
   };
   ```

3. **Replace** the placeholder values with your actual credentials from step 3 above

4. **Save** the file

## Setting Up Firestore Database

1. **Create a "products" Collection**
   - In Firebase Console, go to "Firestore Database"
   - Click "Create Database"
   - Choose production mode for security rules
   - Select your location (e.g., us-central1)
   - Click "Create"

2. **Create the "products" Collection**
   - Once your database is created, click "Create Collection"
   - Name it: `products`
   - Click "Create collection"
   - Click "Auto ID" to add your first document, then click "Save"
   - You can now delete this test document if you wish

## Setting Up Firestore Security Rules

To allow your admin panel to read and write to the products collection, update your Firestore Security Rules:

1. **Go to Firestore Database** → **Rules** tab
2. **Replace** the default rules with:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /products/{document=**} {
      // Allow anyone to read products
      allow read;
      // Allow authenticated users to write/delete products
      // For development, you can use: allow write;
      // For production, implement proper authentication
      allow write: if request.auth != null;
    }
  }
}
```

3. **Publish** the rules

### Note on Authentication
- If you want to allow unauthenticated access (development only), change `allow write;` to just `allow write;`
- For production, implement proper authentication (Firebase Auth)

## Product Data Structure

Documents in the `products` collection should have this structure:

```javascript
{
  name: "Royal Maroon Silk Churidhar",           // Product name (string, required)
  image: "https://example.com/image.jpg",        // Image URL (string, required)
  category: "Silk Blend",                        // Category (string)
  badge: "Bestseller",                           // Badge: Bestseller, New, Trending, Premium, or empty
  price: 2499,                                   // Selling price (number, required)
  originalPrice: 3999,                           // Original price for discount display (number)
  sizes: ["M", "L", "XL"],                       // Available sizes array (array of strings)
  createdAt: "2024-01-15T10:30:00Z",            // Creation timestamp (auto-generated)
  updatedAt: "2024-01-15T10:30:00Z"             // Last update timestamp (auto-generated)
}
```

## Features

✅ **Read**: View all products from Firestore  
✅ **Create**: Add new products to the database  
✅ **Update**: Edit existing products  
✅ **Delete**: Remove products with confirmation  
✅ **Export**: Copy all product data as JSON to clipboard  
✅ **Real-time Updates**: Changes are reflected immediately  
✅ **Error Handling**: Toast notifications for all operations  
✅ **Loading States**: Visual feedback during operations  

## Troubleshooting

### "Firebase not initialized" Error
- Check that your Firebase config credentials are correct
- Ensure you've created the Firestore database
- Check browser console for specific error messages

### Cannot read/write to Firestore
- Verify your Security Rules allow the operations
- Check that the "products" collection exists
- Make sure your credentials have proper permissions

### CORS Issues
- This is expected in development from `file://` protocol
- Use a local server like Python's `python -m http.server` or Node's `npx serve`
- Or deploy to a hosting service (Firebase Hosting, Netlify, Vercel, etc.)

## Testing

1. Open the admin panel in your browser
2. Check the browser console (F12) for any errors
3. Try adding a new product
4. Verify it appears in your Firestore console
5. Edit or delete it to test all operations

## Next Steps

- Set up Firebase Authentication for secure access
- Add image upload instead of URL input (Firebase Storage)
- Add batch operations for importing multiple products
- Create a public-facing catalog page
