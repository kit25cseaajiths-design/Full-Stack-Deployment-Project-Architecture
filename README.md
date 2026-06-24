<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Weather Dashboard</title>
  <style>
    body {
      font-family: Inter, sans-serif;
      background: #0D1B2A;
      color: #F0EDE8;
      margin: 0;
      padding: 0;
    }
    .container {
      max-width: 600px;
      margin: 50px auto;
      text-align: center;
    }
    h1 {
      color: #00C9A7;
    }
    input {
      padding: 10px;
      width: 70%;
      border: 1px solid #00C9A7;
      border-radius: 4px;
      background: #F0EDE8;
      color: #0D1B2A;
    }
    button {
      padding: 10px 20px;
      background: #00C9A7;
      border: none;
      color: #0D1B2A;
      cursor: pointer;
      border-radius: 4px;
      margin-left: 10px;
    }
    button:hover {
      background: #00a387;
    }
    .weather {
      margin-top: 20px;
      padding: 20px;
      border-radius: 8px;
      background: rgba(255,255,255,0.05);
    }
    .error {
      color: red;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Weather Dashboard</h1>
    <input type="text" id="cityInput" placeholder="Enter city name">
    <button onclick="getWeather()">Search</button>
    <div class="weather" id="weather"></div>
  </div>

  <script>
    const apiKey = "YOUR_API_KEY"; // Replace with your OpenWeatherMap API key

    async function getWeather() {
      const city = document.getElementById("cityInput").value.trim();
      const weatherDiv = document.getElementById("weather");

      if (!city) {
        weatherDiv.innerHTML = "<p class='error'>Please enter a city name.</p>";
        return;
      }

      try {
        const response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`
        );

        if (!response.ok) {
          throw new Error(`City not found (Status: ${response.status})`);
        }

        const data = await response.json();
        renderWeather(data);
      } catch (error) {
        weatherDiv.innerHTML = `<p class='error'>Error: ${error.message}</p>`;
      }
    }

    function renderWeather(data) {
      const { name } = data;
      const { temp, humidity } = data.main;
      const { speed } = data.wind;
      const { description } = data.weather[0];

      document.getElementById("weather").innerHTML = `
        <h2>${name}</h2>
        <p>🌡️ Temperature: ${temp} °C</p>
        <p>💧 Humidity: ${humidity}%</p>
        <p>🌬️ Wind Speed: ${speed} m/s</p>
        <p>☁️ Condition: ${description}</p>
      `;
    }
  </script>
</body>
</html>
