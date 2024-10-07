## Class 11: Integrating APIs

### Introduction

In this class, we will learn how to integrate APIs into our React applications. We will explore how to fetch data using `fetch` and `axios`, handle responses, manage loading states and errors, and use `.env` files to keep API keys private.

### 1. Fetching Data

#### Using `fetch`

The `fetch` API is a built-in JavaScript function that allows you to make network requests. It is simple to use and works well for most use cases.

**Example**: Fetching random dog images from a public API that does not require an API key.

1. **Fetch Data**:
   ```jsx
   import React, { useState, useEffect } from 'react';

   function DogImage() {
     const [imageUrl, setImageUrl] = useState('');
     const [loading, setLoading] = useState(true);
     const [error, setError] = useState(null);

     useEffect(() => {
       fetch('https://dog.ceo/api/breeds/image/random')
         .then((response) => {
           if (!response.ok) {
             throw new Error('Network response was not ok');
           }
           return response.json();
         })
         .then((data) => {
           setImageUrl(data.message);
           setLoading(false);
         })
         .catch((error) => {
           setError(error);
           setLoading(false);
         });
     }, []);

     if (loading) return <p>Loading...</p>;
     if (error) return <p>Error: {error.message}</p>;

     return <img src={imageUrl} alt="Random Dog" />;
   }

   export default DogImage;
   ```

**Explanation**:
- **useState**: Manages the state for the image URL, loading status, and error.
- **useEffect**: Fetches the data when the component mounts.
- **fetch**: Makes a network request to the API.
- **Handling Responses**: Checks if the response is okay, parses the JSON, and updates the state.
- **Error Handling**: Catches any errors and updates the error state.

#### Using `axios`

`axios` is a popular library for making HTTP requests. It provides a more powerful and flexible API compared to `fetch`.

**Example**: Fetching random cat facts from a public API that does not require an API key.

1. **Install `axios`**:
   ```bash
   npm install axios
   ```

2. **Fetch Data**:
   ```jsx
   import React, { useState, useEffect } from 'react';
   import axios from 'axios';

   function CatFact() {
     const [fact, setFact] = useState('');
     const [loading, setLoading] = useState(true);
     const [error, setError] = useState(null);

     useEffect(() => {
       axios.get('https://catfact.ninja/fact')
         .then((response) => {
           setFact(response.data.fact);
           setLoading(false);
         })
         .catch((error) => {
           setError(error);
           setLoading(false);
         });
     }, []);

     if (loading) return <p>Loading...</p>;
     if (error) return <p>Error: {error.message}</p>;

     return <p>{fact}</p>;
   }

   export default CatFact;
   ```

**Explanation**:
- **axios.get**: Makes a GET request to the API.
- **Handling Responses**: Updates the state with the fetched data.
- **Error Handling**: Catches any errors and updates the error state.

### 2. Handling Responses

When fetching data from an API, it's important to handle different states such as loading, success, and error.

**Example**: Managing loading states and errors in the `DogImage` component.

1. **Loading State**:
   ```jsx
   if (loading) return <p>Loading...</p>;
   ```

2. **Error State**:
   ```jsx
   if (error) return <p>Error: {error.message}</p>;
   ```

3. **Success State**:
   ```jsx
   return <img src={imageUrl} alt="Random Dog" />;
   ```

### 3. Using `.env` Files for API Keys

When working with APIs that require an API key, it's important to keep the key private and not expose it to the end user. This can be achieved using `.env` files.

#### Purpose of `.env` Files
- **.env Files**: These files store environment variables, such as API keys, in a secure and organized manner. They help keep sensitive information out of your source code.

**Example**: Fetching weather data from an API that requires an API key.

1. **Create a `.env` File**:
   - Create a file named `.env` in the root of your project.
   - Add your API key to the file:
     ```
     REACT_APP_WEATHER_API_KEY=your_api_key_here
     ```

2. **Access the API Key in Your Code**:
   ```jsx
   import React, { useState, useEffect } from 'react';
   import axios from 'axios';

   function Weather() {
     const [weather, setWeather] = useState(null);
     const [loading, setLoading] = useState(true);
     const [error, setError] = useState(null);

     useEffect(() => {
       const apiKey = process.env.REACT_APP_WEATHER_API_KEY;
       axios.get(`https://api.openweathermap.org/data/2.5/weather?q=London&appid=${apiKey}`)
         .then((response) => {
           setWeather(response.data);
           setLoading(false);
         })
         .catch((error) => {
           setError(error);
           setLoading(false);
         });
     }, []);

     if (loading) return <p>Loading...</p>;
     if (error) return <p>Error: {error.message}</p>;

     return (
       <div>
         <h1>Weather in {weather.name}</h1>
         <p>Temperature: {weather.main.temp}°C</p>
         <p>Weather: {weather.weather[0].description}</p>
       </div>
     );
   }

   export default Weather;
   ```

**Explanation**:
- **.env File**: Stores the API key securely.
- **process.env**: Accesses the environment variable in your code.
- **axios.get**: Makes a GET request to the weather API using the API key.

### Summary

In this class, we covered how to integrate APIs into React applications. We learned how to fetch data using `fetch` and `axios`, handle responses, manage loading states and errors, and use `.env` files to keep API keys private. By the end of this class, you should be able to fetch data from APIs securely and efficiently in your React applications.
