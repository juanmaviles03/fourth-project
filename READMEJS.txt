// ---------- Helpers ----------
const set = (id, text, cls) => {
  const el = document.getElementById(id);
  if (!el) return;
  el.textContent = text;
  if (cls === "loading") { el.classList.add("loading"); el.classList.remove("error"); }
  else if (cls === "error") { el.classList.add("error"); el.classList.remove("loading"); }
  else { el.classList.remove("loading","error"); }
};

async function fetchJSON(url, opts) {
  const res = await fetch(url, opts);
  if (!res.ok) throw new Error(res.status + " " + res.statusText);
  return res.json();
}

// ----------Dogs ----------
async function loadDog(){
  set("dog-status","Loading…","loading");
  try{
    const data = await fetchJSON("https://dog.ceo/api/breeds/image/random");
    document.getElementById("dog-img").src = data.message;
    set("dog-status","Done","");
  }catch(e){
    set("dog-status","Error loading dog","error");
  }
}
document.getElementById("dog-btn").addEventListener("click", loadDog);
loadDog();

// ----------Cats ----------
async function loadCat(){
  set("cat-status","Loading…","loading");
  try{
    const data = await fetchJSON("https://api.thecatapi.com/v1/images/search");
    document.getElementById("cat-img").src = data[0].url;
    set("cat-status","Done","");
  }catch(e){
    set("cat-status","Error loading cat","error");
  }
}
document.getElementById("cat-btn").addEventListener("click", loadCat);
loadCat();

// ----------Weather (Open-Meteo) ----------
const CITY_COORDS = {
  nyc: { lat:40.7128, lon:-74.0060, label:"New York" },
  sf: { lat:37.7749, lon:-122.4194, label:"San Francisco" },
  london: { lat:51.5072, lon:-0.1276, label:"London" },
  tokyo: { lat:35.6762, lon:139.6503, label:"Tokyo" },
  miami: { lat:25.7617, lon:-80.1918, label:"Miami" }
};

async function loadWeather(){
  set("weather-status","Loading…","loading");
  try{
    const raw = (document.getElementById("city-input").value || "").trim().toLowerCase();
    const key = raw || "miami";
    const city = CITY_COORDS[key] || CITY_COORDS["miami"];
    const url = `https://api.open-meteo.com/v1/forecast?latitude=${city.lat}&longitude=${city.lon}&current_weather=true`;
    const data = await fetchJSON(url);
    const { temperature } = data.current_weather || { temperature: "--" };
    document.getElementById("weather-temp").textContent = `${temperature}°C`;
    document.getElementById("weather-meta").textContent = `${city.label || key.toUpperCase()} – Lat: ${city.lat}, Lon: ${city.lon}`;
    set("weather-status","Done","");
  }catch(e){
    set("weather-status","Error","error");
  }
}
document.getElementById("weather-btn").addEventListener("click", loadWeather);
loadWeather();

// ----------FX (exchangerate.host) ----------
async function loadFX() {
  set("fx-status", "Loading…", "loading");
  try {
    // First try exchangerate.host
    const url1 = "https://api.exchangerate.host/latest?base=USD&symbols=EUR";
    const data1 = await fetchJSON(url1);
    console.log("exchangerate.host response:", data1);

    let rate = data1?.rates?.EUR;
    let date = data1?.date;

    // Fallback to Frankfurter if no rate found
    if (!rate) {
      console.warn("Falling back to Frankfurter API...");
      const url2 = "https://api.frankfurter.app/latest?from=USD&to=EUR";
      const data2 = await fetchJSON(url2);
      console.log("Frankfurter response:", data2);

      rate = data2?.rates?.EUR;
      date = data2?.date;
    }

    if (rate) {
      document.getElementById("fx-rate").textContent = rate.toFixed(4);
      document.getElementById("fx-updated").textContent = `Updated: ${date}`;
      set("fx-status", "Done", "");
    } else {
      throw new Error("No EUR rate found in either API");
    }
  } catch (e) {
    console.error("FX error:", e);
    set("fx-status", "Error", "error");
    document.getElementById("fx-rate").textContent = "--";
    document.getElementById("fx-updated").textContent = "";
  }
}

document.getElementById("fx-btn").addEventListener("click", loadFX);
loadFX();

// ----------TMDB (requires key) ----------
const TMDB_KEY = "690c95d3eec5686a7332868b0eb47b64";
async function loadTMDB(){
  set("tmdb-status","Loading…","loading");
  const list = document.getElementById("tmdb-list");
  list.innerHTML = "";
  try{
    if (TMDB_KEY.startsWith("REPLACE")) throw new Error("Missing TMDB key");
    const data = await fetchJSON(`https://api.themoviedb.org/3/trending/movie/week?api_key=${TMDB_KEY}`);
    (data.results || []).slice(0,5).forEach(m=>{
      const div = document.createElement("div");
      div.className = "pill";
      div.textContent = `${m.title} (${String(m.release_date||"").slice(0,4)}) ★${(m.vote_average||0).toFixed(1)}`;
      list.appendChild(div);
    });
    set("tmdb-status","Done","");
  }catch(e){
    set("tmdb-status","Add your TMDB key to load trending","error");
  }
}
document.getElementById("tmdb-btn").addEventListener("click", loadTMDB);

// ----------GitHub ----------
async function loadGitHub(){
  const u = (document.getElementById("gh-user").value || "torvalds").trim();
  set("gh-status","Loading…","loading");
  try{
    const data = await fetchJSON(`https://api.github.com/users/${encodeURIComponent(u)}`);
    document.getElementById("gh-avatar").src = data.avatar_url;
    document.getElementById("gh-name").textContent = data.name || data.login;
    document.getElementById("gh-bio").textContent = data.bio || "—";
    document.getElementById("gh-stats").textContent = `Repos: ${data.public_repos} • Followers: ${data.followers}`;
    set("gh-status","Done","");
  }catch(e){
    set("gh-status","User not found","error");
    document.getElementById("gh-avatar").src = "";
    document.getElementById("gh-name").textContent = "—";
    document.getElementById("gh-bio").textContent = "—";
    document.getElementById("gh-stats").textContent = "—";
  }
}
document.getElementById("gh-btn").addEventListener("click", loadGitHub);
loadGitHub();

// ----------Joke ----------
async function loadJoke(){
  set("joke-status","Loading…","loading");
  try{
    const data = await fetchJSON("https://v2.jokeapi.dev/joke/Any?type=single");
    document.getElementById("joke-text").textContent = data.joke || "No joke found.";
    set("joke-status","Done","");
  }catch(e){
    set("joke-status","Error","error");
  }
}
document.getElementById("joke-btn").addEventListener("click", loadJoke);
loadJoke();

// ----------Public APIs (using Marketstack and Key) ----------
const MARKETSTACK_KEY = "a21f47a7c9628675862302eea01e31be"; // key for demo (replace if needed)

async function loadPublicAPI() {
  set("apis-status", "Loading…", "loading");
  const out = document.getElementById("apis-out");

  try {
    // Example: search Apple (you can change query or randomize)
    const query = "apple";
    const url = `http://api.marketstack.com/v1/tickers?access_key=${MARKETSTACK_KEY}&search=${encodeURIComponent(query)}`;

    const data = await fetchJSON(url);
    console.log("Marketstack response:", data);

    const entries = Array.isArray(data?.data) ? data.data : [];
    if (!entries.length) throw new Error("No tickers found");

    // Just pick the first result
    const pick = entries[0,2];
    const name = pick?.name || "Unknown";
    const symbol = pick?.symbol || "";
    const exch = pick?.stock_exchange?.name || "";

    out.innerHTML = `
      <div>
        <strong>${name}</strong> (${symbol})<br>
        Exchange: ${exch}<br>
        <a href="https://marketstack.com/" target="_blank" rel="noreferrer">Visit Marketstack</a>
      </div>
    `;
    set("apis-status", "Done", "");
  } catch (e) {
    console.error("Marketstack error:", e);
    set("apis-status", "Error", "error");
    out.textContent = "Could not load Marketstack data.";
  }
}

document.getElementById("apis-btn").addEventListener("click", loadPublicAPI);
loadPublicAPI();

/*document.getElementById("apis-btn").addEventListener("click", () => {
  const q = (document.getElementById("apis-query").value || "apple").trim();
  loadPublicAPI(q);
});*/