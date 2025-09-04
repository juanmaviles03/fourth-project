<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>API Dashboard 8 Mini Apps</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <link rel="stylesheet" href="styles.css" />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
</head>
<body>
  <h1>API Dashboard 8 Mini Apps</h1>

  <div class="grid">
    <!-- Dog Image -->
    <section class="card" id="dog-card">
      <h2>Dog Image</h2>
      <div class="row">
        <button class="btn" id="dog-btn">New Dog</button>
        <span class="muted" id="dog-status"></span>
      </div>
      <div class="img-wrap"><img id="dog-img" alt="Random dog"></div>
    </section>

    <!-- Cat Image -->
    <section class="card" id="cat-card">
      <h2>Cat Image</h2>
      <div class="row">
        <button class="btn" id="cat-btn">New Cat</button>
        <span class="muted" id="cat-status"></span>
      </div>
      <div class="img-wrap"><img id="cat-img" alt="Random cat"></div>
    </section>

    <!-- Weather (Open-Meteo) -->
    <section class="card" id="weather-card">
      <h2>Weather (Open-Meteo)</h2>
      <div class="row">
        <input id="city-input" placeholder="City (NYC, SF, Miami, London…)" />
        <button class="btn" id="weather-btn">Get Weather</button>
        <span class="muted" id="weather-status"></span>
      </div>
      <div id="weather-out">
        <div class="kpi" id="weather-temp">--°</div>
        <div class="muted" id="weather-meta">Lat: --, Lon: --</div>
      </div>
    </section>

    <!-- USD -> EUR -->
    <section class="card" id="fx-card">
      <h2>Currency (USD → EUR)</h2>
      <div class="row">
        <button class="btn" id="fx-btn">Refresh</button>
        <span class="muted" id="fx-status"></span>
      </div>
      <div class="kpi" id="fx-rate">--.--</div>
      <div class="muted" id="fx-updated"></div>
    </section>

    <!-- TMDB Trending (needs API key) -->
    <section class="card" id="tmdb-card">
      <h2>Trending Movies (TMDB)</h2>
      <div class="row">
        <button class="btn" id="tmdb-btn">Load Trending</button>
        <span class="muted" id="tmdb-status"></span>
      </div>
      <div class="list" id="tmdb-list"></div>
      <div class="muted">Add your TMDB key in <code>TMDB_KEY</code>.</div>
    </section>

    <!-- GitHub User -->
    <section class="card" id="gh-card">
      <h2>GitHub User</h2>
      <div class="row">
        <input id="gh-user" placeholder="e.g. torvalds" />
        <button class="btn" id="gh-btn">Fetch</button>
        <span class="muted" id="gh-status"></span>
      </div>
      <div class="row">
        <div class="img-wrap gh-avatar-wrap">
          <img id="gh-avatar" alt="Avatar">
        </div>
        <div class="grow">
          <div id="gh-name" class="pill">—</div>
          <div class="muted" id="gh-bio">—</div>
          <div class="muted" id="gh-stats">—</div>
        </div>
      </div>
    </section>
