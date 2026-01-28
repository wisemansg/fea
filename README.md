# Globe Insights Explorer

A clean, modern single-page web app to explore country information: flag, coat of arms, capital, population, currency, languages, driving side, time zones, Gini index, and more — powered by the free REST Countries API.

![Globe Insights Explorer Screenshot](https://via.placeholder.com/800x500/4f46e5/ffffff?text=App+Screenshot+Here)  
*(Replace this placeholder with a real screenshot of your running app — upload it to the repo or use imgur)*

## Live Demo (No account/login needed)

Just click this link — it renders your HTML file directly in the browser:

**[➜ Open Live Demo](https://htmlpreview.github.io/?https://github.com/YOUR_USERNAME/YOUR_REPO/raw/main/globe-insights.html)**

(Replace `YOUR_USERNAME` / `YOUR_REPO` and make sure the file is named `globe-insights.html` or adjust the path.)

## How to run locally (zero setup)

1. Download or copy the file [`globe-insights.html`](#full-source-code-below)
2. Save it to your computer
3. Double-click the file — it opens in your browser and works offline (after first load, since it fetches data from the internet)

## Features

- Search any country (e.g. "Norway", "Japan", "Brazil")
- Shows flag + coat of arms (when available)
- Displays key facts in elegant cards
- Google Maps link
- Responsive design (mobile friendly)
- Loading & error states
- Press Enter to search

## Full Source Code (globe-insights.html)

Copy the entire code below into a file named `globe-insights.html` and open it in any browser.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Globe Insights Explorer</title>
  
  <!-- Google Fonts: Inter -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet" />
  
  <style>
    :root {
      --primary: #4f46e5;       /* Indigo accent */
      --primary-dark: #4338ca;
      --bg: linear-gradient(135deg, #1e3a8a, #3b82f6);
      --card-bg: rgba(255, 255, 255, 0.98);
      --text-dark: #0f172a;
      --text-light: #64748b;
      --shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Inter', system-ui, sans-serif;
      background: var(--bg);
      min-height: 100vh;
      color: var(--text-dark);
      padding: 2rem 1rem;
    }

    .app-container {
      max-width: 900px;
      margin: 0 auto;
    }

    .header {
      text-align: center;
      margin-bottom: 2.5rem;
      color: white;
    }

    .header h1 {
      font-size: 2.8rem;
      font-weight: 700;
      letter-spacing: -0.5px;
    }

    .header p {
      font-size: 1.1rem;
      opacity: 0.9;
      margin-top: 0.5rem;
    }

    .search-section {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      background: var(--card-bg);
      padding: 1.8rem;
      border-radius: 1rem;
      box-shadow: var(--shadow);
      margin-bottom: 2rem;
    }

    #country-input {
      padding: 1rem 1.2rem;
      font-size: 1.1rem;
      border: 2px solid #e2e8f0;
      border-radius: 0.75rem;
      outline: none;
      transition: border 0.3s;
    }

    #country-input:focus {
      border-color: var(--primary);
    }

    .buttons {
      display: flex;
      gap: 1rem;
    }

    button {
      flex: 1;
      padding: 1rem;
      font-size: 1.05rem;
      font-weight: 600;
      border: none;
      border-radius: 0.75rem;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    #search-button {
      background: var(--primary);
      color: white;
    }

    #search-button:hover {
      background: var(--primary-dark);
      transform: translateY(-2px);
    }

    #clear-button {
      background: #ef4444;
      color: white;
    }

    #clear-button:hover {
      background: #dc2626;
      transform: translateY(-2px);
    }

    .loading, .error {
      text-align: center;
      padding: 2rem;
      font-size: 1.2rem;
      color: white;
      background: rgba(0,0,0,0.3);
      border-radius: 1rem;
    }

    .hidden {
      display: none;
    }

    .result-container {
      background: var(--card-bg);
      border-radius: 1rem;
      padding: 2rem;
      box-shadow: var(--shadow);
      display: grid;
      gap: 2rem;
    }

    .country-header {
      text-align: center;
    }

    .country-header h2 {
      font-size: 2.2rem;
      margin-bottom: 0.5rem;
      color: var(--text-dark);
    }

    .emblems {
      display: flex;
      justify-content: center;
      gap: 3rem;
      flex-wrap: wrap;
      margin: 1.5rem 0;
    }

    .flag-img, .coat-img {
      max-width: 220px;
      height: auto;
      border-radius: 0.8rem;
      box-shadow: 0 8px 20px rgba(0,0,0,0.2);
      transition: transform 0.3s;
    }

    .flag-img:hover, .coat-img:hover {
      transform: scale(1.05);
    }

    .info-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 1.5rem;
    }

    .info-card {
      background: #f8fafc;
      padding: 1.2rem;
      border-radius: 0.8rem;
      border-left: 5px solid var(--primary);
    }

    .info-card h4 {
      font-size: 1rem;
      color: var(--text-light);
      margin-bottom: 0.6rem;
    }

    .info-card span {
      font-size: 1.15rem;
      font-weight: 600;
      color: var(--text-dark);
    }

    .map-link {
      display: inline-block;
      margin-top: 1rem;
      color: var(--primary);
      font-weight: 600;
      text-decoration: none;
    }

    .map-link:hover {
      text-decoration: underline;
    }

    @media (max-width: 600px) {
      .emblems { gap: 1.5rem; }
      .flag-img, .coat-img { max-width: 180px; }
    }
  </style>
</head>
<body>
  <div class="app-container">
    <header class="header">
      <h1>Globe Insights Explorer</h1>
      <p>Discover detailed profiles of countries worldwide</p>
    </header>

    <div class="search-section">
      <input type="text" id="country-input" placeholder="Type a country name (e.g., Norway, Japan)" autocomplete="off" />
      <div class="buttons">
        <button id="search-button">Explore</button>
        <button id="clear-button">Clear</button>
      </div>
    </div>

    <div id="loading" class="loading hidden">Loading insights...</div>
    <div id="result-container" class="result-container hidden"></div>
    <div id="error-message" class="error hidden"></div>
  </div>

  <script>
    const searchBtn = document.getElementById('search-button');
    const clearBtn = document.getElementById('clear-button');
    const countryInput = document.getElementById('country-input');
    const resultDiv = document.getElementById('result-container');
    const loadingDiv = document.getElementById('loading');
    const errorDiv = document.getElementById('error-message');

    async function fetchCountryInfo(name) {
      if (!name.trim()) {
        showError('Please enter a country name to explore.');
        return;
      }

      showLoading(true);
      clearError();
      resultDiv.innerHTML = '';
      resultDiv.classList.add('hidden');

      try {
        const res = await fetch(`https://restcountries.com/v3.1/name/${name}`);
        if (!res.ok) throw new Error('Country not found');

        const data = await res.json();
        if (data.length === 0) throw new Error('No results');

        // Use the first match
        displayCountry(data[0]);
      } catch (err) {
        if (err.message.includes('not found') || err.message.includes('No results')) {
          showError('Country not found. Try a different name or check spelling.');
        } else {
          showError('An error occurred. Please try again later.');
        }
      } finally {
        showLoading(false);
      }
    }

    function displayCountry(country) {
      const {
        name: { common: name },
        flags: { svg: flag },
        coatOfArms: { svg: coat } = { svg: null },
        capital = ['N/A'],
        region,
        subregion = 'N/A',
        population,
        area,
        gini,
        car: { side: drivingSide = 'N/A' } = {},
        timezones = ['N/A'],
        maps: { googleMaps } = {},
        landlocked = false,
        currencies = {},
        languages = {}
      } = country;

      const currencyKey = Object.keys(currencies)[0];
      const currency = currencyKey ? `${currencies[currencyKey].name} (${currencyKey})` : 'N/A';

      const langList = Object.values(languages).join(', ') || 'N/A';

      const giniText = gini ? `${gini?.[Object.keys(gini)[0]]} (year ${Object.keys(gini)[0]})` : 'N/A';

      resultDiv.innerHTML = `
        <div class="country-header">
          <h2>${name}</h2>
        </div>

        <div class="emblems">
          <img src="${flag}" alt="Flag of ${name}" class="flag-img" />
          ${coat ? `<img src="${coat}" alt="Coat of Arms of ${name}" class="coat-img" />` : ''}
        </div>

        <div class="info-grid">
          <div class="info-card">
            <h4>Capital</h4>
            <span>${capital[0] || 'N/A'}</span>
          </div>
          <div class="info-card">
            <h4>Region / Subregion</h4>
            <span>${region} / ${subregion}</span>
          </div>
          <div class="info-card">
            <h4>Population</h4>
            <span>${population.toLocaleString()}</span>
          </div>
          <div class="info-card">
            <h4>Area</h4>
            <span>${area ? area.toLocaleString() + ' km²' : 'N/A'}</span>
          </div>
          <div class="info-card">
            <h4>Currency</h4>
            <span>${currency}</span>
          </div>
          <div class="info-card">
            <h4>Languages</h4>
            <span>${langList}</span>
          </div>
          <div class="info-card">
            <h4>Driving Side</h4>
            <span>${drivingSide}</span>
          </div>
          <div class="info-card">
            <h4>Time Zone (main)</h4>
            <span>${timezones[0]}</span>
          </div>
          <div class="info-card">
            <h4>Landlocked?</h4>
            <span>${landlocked ? 'Yes' : 'No'}</span>
          </div>
          <div class="info-card">
            <h4>Gini Index (Inequality)</h4>
            <span>${giniText}</span>
          </div>
        </div>

        ${googleMaps ? `<a href="${googleMaps}" target="_blank" class="map-link">View on Google Maps →</a>` : ''}
      `;

      resultDiv.classList.remove('hidden');
    }

    function showLoading(show) {
      loadingDiv.classList.toggle('hidden', !show);
    }

    function showError(msg) {
      errorDiv.textContent = msg;
      errorDiv.classList.remove('hidden');
    }

    function clearError() {
      errorDiv.textContent = '';
      errorDiv.classList.add('hidden');
    }

    // Event listeners
    searchBtn.addEventListener('click', () => fetchCountryInfo(countryInput.value));

    clearBtn.addEventListener('click', () => {
      countryInput.value = '';
      resultDiv.innerHTML = '';
      resultDiv.classList.add('hidden');
      clearError();
      showLoading(false);
    });

    // Bonus: Search when pressing Enter
    countryInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        fetchCountryInfo(countryInput.value);
      }
    });
  </script>
</body>
</html>
