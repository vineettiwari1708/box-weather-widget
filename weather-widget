document.addEventListener("DOMContentLoaded", function () {
  // Find the script tag that loaded this file
  const currentScript = document.currentScript || document.getElementById("uf-weather-script");
  const latAttr = currentScript?.getAttribute("data-lat");
  const lonAttr = currentScript?.getAttribute("data-lon");

  // === Floating Weather Button ===
  const weatherBtn = document.createElement("button");
  weatherBtn.id = "open-weather-widget";
  weatherBtn.innerHTML = `<i class="fas fa-cloud-sun"></i>`;
  Object.assign(weatherBtn.style, {
    position: "fixed",
    bottom: "100px",
    right: "20px",
    width: "60px",
    height: "60px",
    borderRadius: "50%",
    backgroundColor: "#17a2b8",
    color: "#fff",
    border: "none",
    fontSize: "24px",
    zIndex: "99999",
    boxShadow: "0 4px 12px rgba(0,0,0,0.3)",
    cursor: "pointer"
  });
  document.body.appendChild(weatherBtn);

  // === Weather Panel ===
  const weatherPanel = document.createElement("div");
  weatherPanel.id = "weather-widget";
  Object.assign(weatherPanel.style, {
    position: "fixed",
    bottom: "100px",
    right: "20px",
    width: "280px",
    height: "150px",
    backgroundColor: "#fff",
    boxShadow: "0 6px 20px rgba(0,0,0,0.2)",
    borderRadius: "12px",
    display: "none",
    flexDirection: "column",
    overflow: "hidden",
    zIndex: "99998",
    fontFamily: "Arial, sans-serif"
  });

  weatherPanel.innerHTML = `
    <div style="background: #e9ecef; padding: 8px 12px; display: flex; justify-content: space-between; align-items: center;">
      <strong>Weather</strong>
      <button id="close-weather-widget" style="
        background: rgba(0,0,0,0.6);
        color: white;
        border: none;
        border-radius: 50%;
        width: 28px;
        height: 28px;
        font-size: 16px;
        cursor: pointer;
      ">&times;</button>
    </div>
    <div id="weather-info" style="padding: 10px; font-size: 14px; color: #333;">
      Loading weather...
    </div>
  `;
  document.body.appendChild(weatherPanel);

  const weatherInfo = weatherPanel.querySelector("#weather-info");

  async function fetchWeather(lat, lon) {
    try {
      const url = `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current_weather=true`;
      const res = await fetch(url);
      const data = await res.json();
      const weather = data.current_weather;

      const weatherCodes = {
        0: "Clear sky ☀️",
        1: "Mainly clear 🌤",
        2: "Partly cloudy ⛅",
        3: "Overcast ☁️",
        45: "Fog 🌫",
        48: "Rime fog ❄️",
        51: "Light drizzle 🌦",
        61: "Light rain 🌧",
        71: "Light snow 🌨",
        95: "Thunderstorm ⛈️"
      };
      const description = weatherCodes[weather.weathercode] || "Unknown";

      weatherInfo.innerHTML = `
        <div>
          <strong>Your Location</strong><br>
          Temperature: ${weather.temperature}°C<br>
          Wind: ${weather.windspeed} km/h<br>
          Condition: ${description}
        </div>
      `;
    } catch (err) {
      weatherInfo.innerHTML = "Failed to load weather data.";
    }
  }

  function getLocationAndFetchWeather() {
    if (latAttr && lonAttr) {
      fetchWeather(parseFloat(latAttr), parseFloat(lonAttr));
    } else if ("geolocation" in navigator) {
      weatherInfo.innerHTML = "Getting your location...";
      navigator.geolocation.getCurrentPosition(
        (pos) => {
          fetchWeather(pos.coords.latitude, pos.coords.longitude);
        },
        (err) => {
          weatherInfo.innerHTML = `Could not get location: ${err.message}`;
        }
      );
    } else {
      weatherInfo.innerHTML = "Geolocation not supported by your browser.";
    }
  }

  // === Event Listeners ===
  weatherBtn.addEventListener("click", () => {
    weatherPanel.style.display = "flex";
    weatherBtn.style.display = "none";
    getLocationAndFetchWeather();
  });

  const closeWeatherBtn = weatherPanel.querySelector("#close-weather-widget");
  closeWeatherBtn.addEventListener("click", () => {
    weatherPanel.style.display = "none";
    weatherBtn.style.display = "block";
  });
});
