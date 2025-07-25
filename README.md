# SkyCast - Find My Weather
## Date: 25/7/25
## Objective:
To build a responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API. This project demonstrates the use of Axios for API calls, React Router for navigation, React Hooks for state management, controlled components with validation, and basic styling with CSS.
## Tasks:

#### 1. Project Setup
Initialize React app.

Install necessary dependencies: npm install axios react-router-dom

#### 2. Routing
Set up BrowserRouter in App.js.

Create two routes:

/ – Home page with input form.

/weather – Page to display weather results.

#### 3. Home Page (City Input)
Create a controlled input field for the city name.

Add validation to ensure the input is not empty.

On valid form submission, navigate to /weather and store the city name.

#### 4. Weather Page (API Integration)
Use Axios to fetch data from the OpenWeatherMap API using the city name.

Show temperature, humidity, wind speed, and weather condition.

Convert and display temperature in both Celsius and Fahrenheit using useMemo.

#### 5. React Hooks
Use useState for managing city, weather data, and loading state.

Use useEffect to trigger the Axios call on page load.

Use useCallback to optimize form submit handler.

Use useMemo for temperature conversion logic.

#### 6. UI Styling (CSS)
Create a responsive and clean layout using CSS.

Style form, buttons, weather display cards, and navigation links.

## Programs:
weather.js
```
import React, { useEffect, useState, useMemo } from 'react';
import { useLocation, useNavigate } from 'react-router-dom';
import axios from 'axios';
import './Weather.css';

function Weather() {
  const location = useLocation();
  const navigate = useNavigate();
  const queryParams = new URLSearchParams(location.search);
  const city = queryParams.get('city');

  const [weatherData, setWeatherData] = useState(null);
  const [loading, setLoading] = useState(true);

  const apiKey = '3d2ad89ee7244f568d861727252507';

  useEffect(() => {
    if (!city) {
      navigate('/');
      return;
    }

    const fetchWeather = async () => {
      try {
        const response = await axios.get(
          `https://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${city}`
        );
        setWeatherData(response.data);
      } catch (err) {
        alert('City not found!');
        navigate('/');
      } finally {
        setLoading(false);
      }
    };

    fetchWeather();
  }, [city, navigate, apiKey]);

  const tempFahrenheit = useMemo(() => {
    if (!weatherData) return null;
    return weatherData.current.temp_f;
  }, [weatherData]);

  if (loading) return <div className="weather-container">Loading...</div>;

  return (
    <div className="weather-container">
      <h2>Weather in {weatherData.location.name}</h2>
      <div className="weather-card">
        <p><strong>Temperature:</strong> {weatherData.current.temp_c} °C / {tempFahrenheit} °F</p>
        <p><strong>Humidity:</strong> {weatherData.current.humidity} %</p>
        <p><strong>Wind Speed:</strong> {weatherData.current.wind_kph} kph</p>
        <p><strong>Condition:</strong> {weatherData.current.condition.text}</p>
      </div>
      <button onClick={() => navigate('/')}>Back</button>
    </div>
  );
}

export default Weather;
```
Home.js
```
import React, { useState, useCallback } from 'react';
import { useNavigate } from 'react-router-dom';
import './Home.css';

function Home() {
  const [city, setCity] = useState('');
  const [error, setError] = useState('');
  const navigate = useNavigate();

  const handleSubmit = useCallback((e) => {
    e.preventDefault();
    if (city.trim() === '') {
      setError('City name is required');
    } else {
      setError('');
      navigate(`/weather?city=${city}`);
    }
  }, [city, navigate]);

  return (
    <div className="home-container">
      <h2>Enter City Name</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={city}
          onChange={(e) => setCity(e.target.value)}
          placeholder="city name"
        />
        <button type="submit">Get Weather</button>
        {error && <p className="error">{error}</p>}
      </form>
    </div>
  );
}

export default Home;
```


## Output:
<img width="1910" height="717" alt="image" src="https://github.com/user-attachments/assets/cf080b5d-6360-4f21-b3e9-878cbb0d9157" />
<img width="1907" height="765" alt="image" src="https://github.com/user-attachments/assets/5e99d1d1-44b0-40ff-a4c7-73b8a5df66f7" />


## Result:
A responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API has been built successfully. 
