# NodeProjectSteps

Below is a step-by-step guide to building a secure OTP-based authentication system in **React.js**, as per the provided problem statement. It includes the file structure, installation instructions, and detailed code for each step.

---

## **Step 1: Project Setup**

### **1. Install Node.js**
Ensure you have Node.js and npm installed. Check by running:
```bash
node -v
npm -v
```

If not installed, download from [Node.js official website](https://nodejs.org/).

---

### **2. Create a React App**
Run the following commands:
```bash
npx create-react-app otp-auth-app
cd otp-auth-app
```

---

### **3. Install Required Packages**
Install the necessary dependencies:
```bash
npm install react-router-dom@6 apexcharts react-apexcharts
```

---

## **Step 2: Project Structure**

Organize your project as follows:
```
otp-auth-app/
├── public/
│   └── index.html
├── src/
│   ├── components/
│   │   ├── OTPInput.js
│   │   ├── Dashboard.js
│   │   ├── ResendOTP.js
│   ├── utils/
│   │   └── generateOTP.js
│   ├── App.js
│   ├── index.js
│   ├── App.css
├── package.json
```

---

## **Step 3: Implementing Code**

### **1. `index.js`**
This is the entry point for your application.
```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './App.css';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

### **2. `App.js`**
This sets up routing and navigation between screens.
```javascript
import React from 'react';
import { HashRouter, Routes, Route } from 'react-router-dom';
import OTPInput from './components/OTPInput';
import Dashboard from './components/Dashboard';
import ResendOTP from './components/ResendOTP';

function App() {
  return (
    <HashRouter>
      <Routes>
        <Route path="/" element={<OTPInput />} />
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/resend-otp" element={<ResendOTP />} />
      </Routes>
    </HashRouter>
  );
}

export default App;
```

---

### **3. `utils/generateOTP.js`**
A utility function to generate a 6-digit OTP.
```javascript
export function generateOTP() {
  return Math.floor(100000 + Math.random() * 900000).toString();
}
```

---

### **4. `components/OTPInput.js`**
This handles OTP generation, input, and validation.
```javascript
import React, { useState, useEffect } from 'react';
import { useNavigate } from 'react-router-dom';
import { generateOTP } from '../utils/generateOTP';

function OTPInput() {
  const [otp, setOtp] = useState('');
  const [generatedOtp, setGeneratedOtp] = useState('');
  const [timer, setTimer] = useState(30);
  const navigate = useNavigate();

  useEffect(() => {
    const newOtp = generateOTP();
    setGeneratedOtp(newOtp);
    alert(`Your OTP is: ${newOtp}`); // Display OTP
    const countdown = setInterval(() => {
      setTimer((prev) => {
        if (prev === 1) {
          clearInterval(countdown);
          navigate('/resend-otp');
        }
        return prev - 1;
      });
    }, 1000);

    return () => clearInterval(countdown);
  }, [navigate]);

  const handleSubmit = (e) => {
    e.preventDefault();
    if (otp === generatedOtp) {
      navigate('/dashboard');
    } else {
      alert('Incorrect OTP!');
    }
  };

  return (
    <div className="otp-container">
      <h1>OTP Authentication</h1>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={otp}
          onChange={(e) => setOtp(e.target.value)}
          placeholder="Enter OTP"
          maxLength={6}
        />
        <button type="submit">Verify</button>
      </form>
      <p>Time remaining: {timer}s</p>
    </div>
  );
}

export default OTPInput;
```

---

### **5. `components/Dashboard.js`**
A simple dashboard screen.
```javascript
import React from 'react';

function Dashboard() {
  return (
    <div className="dashboard-container">
      <h1>Welcome to the Dashboard</h1>
    </div>
  );
}

export default Dashboard;
```

---

### **6. `components/ResendOTP.js`**
Screen for resending OTP.
```javascript
import React from 'react';
import { useNavigate } from 'react-router-dom';

function ResendOTP() {
  const navigate = useNavigate();

  const handleResend = () => {
    navigate('/');
  };

  return (
    <div className="resend-container">
      <h1>OTP Expired</h1>
      <button onClick={handleResend}>Resend OTP</button>
    </div>
  );
}

export default ResendOTP;
```

---

### **7. `App.css`**
Basic styling for the application.
```css
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f4f4f9;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.otp-container,
.dashboard-container,
.resend-container {
  text-align: center;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

h1 {
  color: #333;
}

input {
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
}

button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

p {
  color: #555;
}
```

---

## **Step 4: Deployment**

### **1. Build the App**
Run the following command to build your app:
```bash
npm run build
```

---

### **2. Deploy to Netlify**
- Go to [Netlify](https://www.netlify.com/).
- Drag and drop the `build` folder into Netlify.

---

### **Step 5: Testing**
1. Navigate to the root URL.
2. Enter the OTP sent via alert.
3. Test scenarios like:
   - Correct OTP within 30 seconds.
   - Incorrect OTP.
   - Expired OTP and resend functionality.

Let me know if you need help with deployment or further modifications!
