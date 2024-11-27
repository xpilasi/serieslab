# SeriesLab 🎬  
**SeriesLab** is a web application developed as the final project for our Frontend Development certification at LaSalle. The goal is to provide a complete experience for users to search, review, and explore TV series.  

## 🌟 Features  
- 📚 **Series Catalog**: Get detailed information about your favorite TV series using the [TMDB](https://www.themoviedb.org/) API.  
- ✍️ **User Reviews**: Create and read series reviews with an active community.  
- 🔒 **Secure Authentication**: Firestore handles user login and registration.  
- 📂 **State Management**: Implemented with **Pinia** to efficiently handle the app's global state.  
- 🔍 **Dynamic Search**: Quickly search for TV series using an intuitive search bar.  

---

## 🛠️ Technologies Used  
### Frameworks and Libraries  
- **Vue.js**: For building the frontend.  
- **Pinia**: State management library.  

### Backend and Databases  
- **Firebase Firestore**: Used for user authentication and storing reviews.  
- **TMDB API**: To fetch detailed information about TV series.  

### Tools and Styling  
- **Tailwind CSS**: For modern and responsive UI design.  
- **Vite**: For fast and efficient development.  

---

## 🚀 Installation and Usage  

### 1. Clone the Repository  
```bash  
git clone https://github.com/your-username/serieslab.git  
cd serieslab  
```
### 2.  Install Dependencies
```bash  
npm install 
``` 
### 3  Configure Environment Variables
Create a .env file in the root directory and add the following keys:

VITE_FIREBASE_API_KEY=<your_firebase_api_key>  
VITE_FIREBASE_AUTH_DOMAIN=<your_auth_domain>  
VITE_FIREBASE_PROJECT_ID=<your_project_id>  
VITE_FIREBASE_STORAGE_BUCKET=<your_storage_bucket>  
VITE_FIREBASE_MESSAGING_SENDER_ID=<your_sender_id>  
VITE_FIREBASE_APP_ID=<your_app_id>  
VITE_TMDB_API_KEY=<your_tmdb_api_key>  

### 4  Start the Development Server
```bash  
npm run dev 
``` 
The app will be available at http://localhost:5173.

### 5  Project Structure
src/  
├── components/          # Reusable components  
├── views/               # Main views (pages)  
├── stores/              # State management with Pinia  
├── firebase/            # Firebase configuration  
├── assets/              # Static files (images, styles)  
├── App.vue              # Root component  
├── main.js              # Main entry point  

### 6   🔒 Firebase Setup

- Create a project in Firebase.
- Enable Firestore Database and Authentication (using Email/Password).
- Add the credentials to the .env file

### 7   Contributors

- Xavi --> https://github.com/xpilasi
- Mary --> https://github.com/MaryQuiroz
- Carlos --> https://github.com/carlos-full-stack
- Gonzalo --> https://github.com/GontxiPino

### 8   📧 Contact
If you have questions or want to collaborate, feel free to contact us:
Email: pilasiha@gmail.com
