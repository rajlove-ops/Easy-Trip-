# Easy-Trip-
Team Name - 404 legends 




<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Easy Trip — Travel Made Easy</title>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet"/>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --bg: #ffffff; --bg2: #f5f5f3; --bg3: #eeede9;
      --text: #1a1a18; --text2: #5f5e5a; --text3: #9e9d98;
      --border: rgba(0,0,0,0.12); --border2: rgba(0,0,0,0.22);
      --green: #1D9E75; --green-dark: #0F6E56; --green-deeper: #085041;
      --green-light: #E1F5EE; --radius: 10px; --radius-lg: 14px;
    }
    body { font-family: 'DM Sans', sans-serif; background: var(--bg3); color: var(--text); min-height: 100vh; }

    /* NAV */
    nav { background: var(--bg); border-bottom: 0.5px solid var(--border); height: 58px; display: flex; align-items: center; justify-content: space-between; padding: 0 2rem; position: sticky; top: 0; z-index: 100; }
    .logo { font-family: 'Playfair Display', serif; font-size: 21px; letter-spacing: -0.5px; color: var(--text); }
    .logo span { color: var(--green); }
    .nav-links { display: flex; gap: 8px; align-items: center; }
    .nav-btn { background: none; border: 0.5px solid var(--border); border-radius: var(--radius); padding: 7px 15px; font-size: 13px; cursor: pointer; color: var(--text); font-family: 'DM Sans', sans-serif; transition: background 0.15s; }
    .nav-btn:hover { background: var(--bg2); }
    .nav-btn.primary { background: var(--green); color: #fff; border-color: var(--green); }
    .nav-btn.primary:hover { background: var(--green-dark); }
    .nav-btn.active-nav { background: var(--green-light); color: var(--green-dark); border-color: var(--green); }

    /* PAGES */
    .page { display: none; animation: fadeIn 0.35s ease; }
    .page.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }

    /* HERO */
    .hero { background: linear-gradient(135deg, var(--green-deeper) 0%, var(--green) 55%, #5DCAA5 100%); padding: 72px 2rem 88px; text-align: center; position: relative; overflow: hidden; }
    .hero::before { content: ''; position: absolute; inset: 0; background-image: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='%23ffffff' fill-opacity='0.04'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/svg%3E"); }
    .hero-content { position: relative; }
    .hero h1 { font-family: 'Playfair Display', serif; font-size: 46px; color: #fff; margin-bottom: 14px; line-height: 1.15; }
    .hero p { font-size: 17px; color: rgba(255,255,255,0.85); max-width: 480px; margin: 0 auto 32px; }
    .search-bar { background: var(--bg); border-radius: var(--radius-lg); padding: 12px 14px; display: flex; gap: 10px; align-items: center; max-width: 580px; margin: 0 auto; border: 0.5px solid var(--border); }
    .search-bar input { flex: 1; border: none; background: none; font-size: 15px; color: var(--text); font-family: 'DM Sans', sans-serif; outline: none; }
    .search-bar input::placeholder { color: var(--text3); }
    .search-btn { background: var(--green); color: #fff; border: none; border-radius: var(--radius); padding: 9px 22px; font-size: 14px; font-weight: 500; cursor: pointer; font-family: 'DM Sans', sans-serif; white-space: nowrap; transition: background 0.15s; }
    .search-btn:hover { background: var(--green-dark); }

    /* SECTIONS */
    .section { padding: 52px 2rem; max-width: 1120px; margin: 0 auto; }
    .section-title { font-family: 'Playfair Display', serif; font-size: 30px; margin-bottom: 24px; color: var(--text); }
    .section-sub { font-size: 14px; color: var(--text2); margin-bottom: 20px; }

    /* TAGS */
    .tag-row { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 24px; }
    .tag { border: 0.5px solid var(--border); border-radius: 20px; padding: 5px 15px; font-size: 12px; cursor: pointer; background: var(--bg); color: var(--text2); transition: all 0.15s; font-family: 'DM Sans', sans-serif; }
    .tag:hover, .tag.active { background: var(--green); color: #fff; border-color: var(--green); }

    /* DEST CARDS */
    .cards { display: grid; grid-template-columns: repeat(auto-fill, minmax(230px, 1fr)); gap: 16px; }
    .dest-card { background: var(--bg); border-radius: var(--radius-lg); border: 0.5px solid var(--border); overflow: hidden; cursor: pointer; transition: border-color 0.2s, transform 0.2s; }
    .dest-card:hover { border-color: var(--border2); transform: translateY(-2px); }
    .dest-img { height: 148px; display: flex; align-items: flex-end; padding: 12px; }
    .dest-badge { background: rgba(0,0,0,0.42); color: #fff; font-size: 11px; padding: 3px 9px; border-radius: 20px; backdrop-filter: blur(4px); }
    .dest-body { padding: 14px; }
    .dest-body h3 { font-size: 15px; font-weight: 500; margin-bottom: 3px; }
    .dest-body p { font-size: 12px; color: var(--text2); line-height: 1.5; }
    .dest-body .price { font-size: 14px; font-weight: 500; color: var(--green); margin-top: 8px; }

    /* FEATURES */
    .features { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 16px; }
    .feat-card { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius-lg); padding: 22px; }
    .feat-icon { width: 38px; height: 38px; border-radius: var(--radius); background: var(--green-light); display: flex; align-items: center; justify-content: center; margin-bottom: 14px; }
    .feat-card h4 { font-size: 14px; font-weight: 500; margin-bottom: 5px; }
    .feat-card p { font-size: 12px; color: var(--text2); line-height: 1.65; }
    .features-bg { background: var(--bg2); padding: 52px 2rem; }
    .features-inner { max-width: 1120px; margin: 0 auto; }

    /* ── MAP PAGE ── */
    .map-outer { padding: 32px 2rem; max-width: 1120px; margin: 0 auto; }
    .map-page-title { font-family: 'Playfair Display', serif; font-size: 30px; margin-bottom: 6px; }
    .map-page-sub { font-size: 14px; color: var(--text2); margin-bottom: 24px; }

    /* Route panel */
    .map-layout { display: grid; grid-template-columns: 320px 1fr; gap: 16px; align-items: start; }
    @media (max-width: 760px) { .map-layout { grid-template-columns: 1fr; } }

    .route-panel { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius-lg); padding: 20px; display: flex; flex-direction: column; gap: 14px; }
    .route-panel h3 { font-size: 15px; font-weight: 500; display: flex; align-items: center; gap: 8px; }
    .route-panel h3 svg { flex-shrink: 0; }

    .field-label { font-size: 12px; color: var(--text2); margin-bottom: 5px; }
    .route-input { width: 100%; border: 0.5px solid var(--border); border-radius: var(--radius); padding: 9px 12px; font-size: 13px; background: var(--bg2); color: var(--text); font-family: 'DM Sans', sans-serif; outline: none; transition: border-color 0.15s; }
    .route-input:focus { border-color: var(--green); }

    .mode-row { display: flex; gap: 6px; }
    .mode-btn { flex: 1; border: 0.5px solid var(--border); border-radius: var(--radius); padding: 7px 4px; font-size: 11px; cursor: pointer; background: var(--bg2); color: var(--text2); font-family: 'DM Sans', sans-serif; text-align: center; transition: all 0.15s; }
    .mode-btn.selected { background: var(--green-light); color: var(--green-dark); border-color: var(--green); font-weight: 500; }

    .get-directions-btn { width: 100%; background: var(--green); color: #fff; border: none; border-radius: var(--radius); padding: 11px; font-size: 14px; font-weight: 500; cursor: pointer; font-family: 'DM Sans', sans-serif; transition: background 0.15s; display: flex; align-items: center; justify-content: center; gap: 8px; }
    .get-directions-btn:hover { background: var(--green-dark); }
    .get-directions-btn:disabled { background: var(--text3); cursor: not-allowed; }

    .location-btn { width: 100%; background: none; border: 0.5px solid var(--border); border-radius: var(--radius); padding: 9px; font-size: 13px; cursor: pointer; color: var(--text); font-family: 'DM Sans', sans-serif; display: flex; align-items: center; justify-content: center; gap: 7px; transition: background 0.15s; }
    .location-btn:hover { background: var(--bg2); }

    /* Route info card */
    .route-info { background: var(--green-light); border: 0.5px solid var(--green); border-radius: var(--radius); padding: 14px; display: none; }
    .route-info.visible { display: block; }
    .route-info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 10px; }
    .route-stat { text-align: center; }
    .route-stat .val { font-size: 20px; font-weight: 500; color: var(--green-dark); }
    .route-stat .lbl { font-size: 11px; color: var(--text2); margin-top: 2px; }
    .via-text { font-size: 11px; color: var(--text2); border-top: 0.5px solid rgba(0,0,0,0.1); padding-top: 8px; }

    /* Steps */
    .steps-box { border-top: 0.5px solid var(--border); padding-top: 12px; }
    .steps-title { font-size: 12px; font-weight: 500; color: var(--text2); margin-bottom: 8px; }
    .step-list { display: flex; flex-direction: column; gap: 6px; max-height: 200px; overflow-y: auto; }
    .step-item { display: flex; gap: 8px; align-items: flex-start; font-size: 12px; color: var(--text2); }
    .step-dot { width: 6px; height: 6px; background: var(--green); border-radius: 50%; margin-top: 5px; flex-shrink: 0; }
    .step-dist { color: var(--text3); font-size: 11px; white-space: nowrap; }

    /* Map container */
    .map-container { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius-lg); overflow: hidden; }
    .map-toolbar { padding: 12px 16px; border-bottom: 0.5px solid var(--border); display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 8px; }
    .map-toolbar-label { font-size: 13px; font-weight: 500; }
    .shortcut-row { display: flex; gap: 6px; flex-wrap: wrap; padding: 12px 16px; border-bottom: 0.5px solid var(--border); }
    .shortcut-btn { border: 0.5px solid var(--border); border-radius: 20px; padding: 4px 12px; font-size: 11px; cursor: pointer; background: var(--bg2); color: var(--text2); font-family: 'DM Sans', sans-serif; transition: all 0.15s; }
    .shortcut-btn:hover { background: var(--green-light); color: var(--green-dark); border-color: var(--green); }
    #map { height: 460px; width: 100%; }

    /* Status bar */
    .map-status { padding: 8px 16px; font-size: 12px; color: var(--text2); min-height: 32px; display: flex; align-items: center; gap: 6px; }
    .status-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--text3); flex-shrink: 0; }
    .status-dot.green { background: var(--green); }
    .status-dot.amber { background: #EF9F27; animation: pulse 1s infinite; }
    @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.4} }

    /* LOGIN */
    .login-wrap { min-height: calc(100vh - 58px); display: flex; align-items: center; justify-content: center; padding: 48px 1rem; background: var(--bg3); }
    .login-card { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius-lg); padding: 40px 34px; width: 100%; max-width: 420px; }
    .login-card h2 { font-family: 'Playfair Display', serif; font-size: 28px; margin-bottom: 6px; }
    .login-card .sub { font-size: 14px; color: var(--text2); margin-bottom: 30px; }
    .field { margin-bottom: 16px; }
    .field label { display: block; font-size: 13px; color: var(--text2); margin-bottom: 6px; }
    .field input { width: 100%; border: 0.5px solid var(--border); border-radius: var(--radius); padding: 11px 13px; font-size: 14px; background: var(--bg2); color: var(--text); font-family: 'DM Sans', sans-serif; outline: none; transition: border-color 0.15s; }
    .field input:focus { border-color: var(--green); }
    .login-btn { width: 100%; background: var(--green); color: #fff; border: none; border-radius: var(--radius); padding: 13px; font-size: 15px; font-weight: 500; cursor: pointer; font-family: 'DM Sans', sans-serif; margin-top: 8px; transition: background 0.15s; }
    .login-btn:hover { background: var(--green-dark); }
    .divider { display: flex; align-items: center; gap: 12px; margin: 22px 0; font-size: 12px; color: var(--text3); }
    .divider::before, .divider::after { content: ''; flex: 1; height: 0.5px; background: var(--border); }
    .social-btns { display: flex; gap: 10px; }
    .social-btn { flex: 1; border: 0.5px solid var(--border); background: var(--bg2); border-radius: var(--radius); padding: 10px; font-size: 13px; cursor: pointer; color: var(--text); font-family: 'DM Sans', sans-serif; transition: background 0.15s; }
    .social-btn:hover { background: var(--bg3); }
    .toggle-link { text-align: center; margin-top: 22px; font-size: 13px; color: var(--text2); }
    .toggle-link a { color: var(--green); cursor: pointer; text-decoration: none; font-weight: 500; }
    .forgot { text-align: right; font-size: 12px; color: var(--green); cursor: pointer; margin-top: -8px; margin-bottom: 10px; }

    /* TOAST */
    .toast { position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%); background: #1a1a18; color: #fff; padding: 10px 22px; border-radius: var(--radius); font-size: 13px; opacity: 0; transition: opacity 0.3s; pointer-events: none; z-index: 999; white-space: nowrap; }
    .toast.show { opacity: 1; }

    footer { background: var(--text); color: rgba(255,255,255,0.55); text-align: center; padding: 28px 2rem; font-size: 13px; }
    footer span { color: var(--green); }

    @media (max-width: 640px) {
      .hero h1 { font-size: 32px; }
      .nav-links .nav-btn:not(.primary):not(#authBtn) { display: none; }
      .login-card { padding: 28px 20px; }
    }
  </style>
</head>
<body>

<div id="toast" class="toast"></div>

<!-- NAV -->
<nav>
  <div class="logo">Easy<span>Trip</span></div>
  <div class="nav-links">
    <button class="nav-btn" id="navHome" onclick="showPage('home')">Home</button>
    <button class="nav-btn" id="navExplore" onclick="showPage('explore')">Explore</button>
    <button class="nav-btn" id="navMap" onclick="showPage('map')">Map</button>
    <button class="nav-btn" id="authBtn" onclick="showPage('login')">Log in</button>
    <button class="nav-btn primary" onclick="showPage('login')">Sign up free</button>
  </div>
</nav>

<!-- ═══════════════ HOME ═══════════════ -->
<div id="page-home" class="page active">
  <div class="hero">
    <div class="hero-content">
      <h1>Travel Made Easy</h1>
      <p>Discover destinations, plan trips, and navigate the world — all in one place.</p>
      <div class="search-bar">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#aaa" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/></svg>
        <input id="searchInput" type="text" placeholder="Search destinations, hotels, activities..." />
        <button class="search-btn" onclick="doSearch()">Search</button>
      </div>
    </div>
  </div>
  <div class="section">
    <p class="section-sub">Popular categories</p>
    <div class="tag-row">
      <span class="tag active">All</span><span class="tag">Beaches</span><span class="tag">Mountains</span><span class="tag">Cities</span><span class="tag">Adventure</span><span class="tag">Heritage</span>
    </div>
    <div class="section-title">Top Destinations</div>
    <div class="cards">
      <div class="dest-card" onclick="goToDestination('Paris, France', 48.8566, 2.3522)">
        <div class="dest-img" style="background:linear-gradient(150deg,#185FA5,#378ADD)"><span class="dest-badge">Europe</span></div>
        <div class="dest-body"><h3>Paris, France</h3><p>The city of lights and romance</p><div class="price">from ₹45,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('Bali, Indonesia', -8.4095, 115.1889)">
        <div class="dest-img" style="background:linear-gradient(150deg,#0F6E56,#5DCAA5)"><span class="dest-badge">Asia</span></div>
        <div class="dest-body"><h3>Bali, Indonesia</h3><p>Tropical paradise & ancient temples</p><div class="price">from ₹28,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('New York, USA', 40.7128, -74.006)">
        <div class="dest-img" style="background:linear-gradient(150deg,#3C3489,#7F77DD)"><span class="dest-badge">Americas</span></div>
        <div class="dest-body"><h3>New York, USA</h3><p>The city that never sleeps</p><div class="price">from ₹65,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('Tokyo, Japan', 35.6762, 139.6503)">
        <div class="dest-img" style="background:linear-gradient(150deg,#993C1D,#D85A30)"><span class="dest-badge">Asia</span></div>
        <div class="dest-body"><h3>Tokyo, Japan</h3><p>Tradition meets the future</p><div class="price">from ₹52,000</div></div>
      </div>
    </div>
  </div>
  <div class="features-bg">
    <div class="features-inner">
      <div class="section-title">Why Easy Travel?</div>
      <div class="features">
        <div class="feat-card">
          <div class="feat-icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#1D9E75" stroke-width="2"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg></div>
          <h4>Live Maps & Routing</h4><p>Real-time directions from your location to any destination worldwide.</p>
        </div>
        <div class="feat-card">
          <div class="feat-icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#1D9E75" stroke-width="2"><rect x="2" y="3" width="20" height="14" rx="2"/><line x1="8" y1="21" x2="16" y2="21"/><line x1="12" y1="17" x2="12" y2="21"/></svg></div>
          <h4>Easy Planning</h4><p>Build your entire trip itinerary in minutes with smart suggestions.</p>
        </div>
        <div class="feat-card">
          <div class="feat-icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#1D9E75" stroke-width="2"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg></div>
          <h4>Safe & Secure</h4><p>Your bookings and personal data are always fully protected.</p>
        </div>
        <div class="feat-card">
          <div class="feat-icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#1D9E75" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg></div>
          <h4>24/7 Support</h4><p>Round-the-clock assistance wherever you are in the world.</p>
        </div>
        <div class="feat-card">
          <div class="feat-icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#1D9E75" stroke-width="2"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg></div>
          <h4>Community</h4><p>Connect with millions of travellers, share tips and reviews.</p>
        </div>
        <div class="feat-card">
          <div class="feat-icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#1D9E75" stroke-width="2"><line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg></div>
          <h4>Best Prices</h4><p>We compare thousands of deals to get you the lowest fare guaranteed.</p>
        </div>
      </div>
    </div>
  </div>
  <footer>&copy; 2025 <span>Easy Travel</span> — Travel Made Simple &nbsp;|&nbsp; All rights reserved</footer>
</div>

<!-- ═══════════════ EXPLORE ═══════════════ -->
<div id="page-explore" class="page">
  <div class="section">
    <div class="section-title">Explore the World</div>
    <div class="tag-row">
      <span class="tag active">Trending</span><span class="tag">Budget</span><span class="tag">Luxury</span><span class="tag">Solo</span><span class="tag">Family</span><span class="tag">Honeymoon</span>
    </div>
    <div class="cards">
      <div class="dest-card" onclick="goToDestination('Dubai, UAE', 25.2048, 55.2708)">
        <div class="dest-img" style="background:linear-gradient(150deg,#BA7517,#EF9F27)"><span class="dest-badge">Middle East</span></div>
        <div class="dest-body"><h3>Dubai, UAE</h3><p>Ultra-luxury & iconic skyscrapers</p><div class="price">from ₹38,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('Goa, India', 15.2993, 74.1240)">
        <div class="dest-img" style="background:linear-gradient(150deg,#185FA5,#9FE1CB)"><span class="dest-badge">India</span></div>
        <div class="dest-body"><h3>Goa, India</h3><p>Sun, sand and vibrant nightlife</p><div class="price">from ₹8,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('London, UK', 51.5074, -0.1278)">
        <div class="dest-img" style="background:linear-gradient(150deg,#444441,#888780)"><span class="dest-badge">Europe</span></div>
        <div class="dest-body"><h3>London, UK</h3><p>History, culture & world-class theatre</p><div class="price">from ₹70,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('Maldives', 4.1755, 73.5093)">
        <div class="dest-img" style="background:linear-gradient(150deg,#085041,#9FE1CB)"><span class="dest-badge">Ocean</span></div>
        <div class="dest-body"><h3>Maldives</h3><p>Crystal waters & overwater villas</p><div class="price">from ₹90,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('Jaipur, Rajasthan', 26.9124, 75.7873)">
        <div class="dest-img" style="background:linear-gradient(150deg,#993C1D,#FAC775)"><span class="dest-badge">India</span></div>
        <div class="dest-body"><h3>Rajasthan, India</h3><p>Forts, palaces & golden desert</p><div class="price">from ₹12,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('Singapore', 1.3521, 103.8198)">
        <div class="dest-img" style="background:linear-gradient(150deg,#3C3489,#9FE1CB)"><span class="dest-badge">Asia</span></div>
        <div class="dest-body"><h3>Singapore</h3><p>Garden city of Southeast Asia</p><div class="price">from ₹35,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('Rome, Italy', 41.9028, 12.4964)">
        <div class="dest-img" style="background:linear-gradient(150deg,#A32D2D,#F09595)"><span class="dest-badge">Europe</span></div>
        <div class="dest-body"><h3>Rome, Italy</h3><p>The eternal city of history</p><div class="price">from ₹55,000</div></div>
      </div>
      <div class="dest-card" onclick="goToDestination('Manali, India', 32.2396, 77.1887)">
        <div class="dest-img" style="background:linear-gradient(150deg,#0C447C,#B5D4F4)"><span class="dest-badge">India</span></div>
        <div class="dest-body"><h3>Manali, India</h3><p>Snow peaks & adventure sports</p><div class="price">from ₹10,000</div></div>
      </div>
    </div>
  </div>
  <footer>&copy; 2025 <span>Easy Travel</span> — Travel Made Simple &nbsp;|&nbsp; All rights reserved</footer>
</div>

<!-- ═══════════════ MAP PAGE ═══════════════ -->
<div id="page-map" class="page">
  <div class="map-outer">
    <div class="map-page-title">Directions & Map</div>
    <p class="map-page-sub">Get turn-by-turn directions from your current location to any destination.</p>

    <div class="map-layout">

      <!-- LEFT: Route Panel -->
      <div>
        <div class="route-panel">
          <h3>
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#1D9E75" stroke-width="2"><path d="M3 12h18M3 6l9-3 9 3M3 18l9 3 9-3"/></svg>
            Plan Your Route
          </h3>

          <div>
            <div class="field-label">From (origin)</div>
            <button class="location-btn" onclick="useMyLocation()">
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="3"/><path d="M12 1v4M12 19v4M1 12h4M19 12h4"/></svg>
              Use my current location
            </button>
            <div style="margin-top:8px;">
              <input class="route-input" id="originInput" type="text" placeholder="Or type a starting place..." />
            </div>
          </div>

          <div>
            <div class="field-label">To (destination)</div>
            <input class="route-input" id="destInput" type="text" placeholder="e.g. Taj Mahal, Agra" />
          </div>

          <div>
            <div class="field-label">Travel mode</div>
            <div class="mode-row">
              <button class="mode-btn selected" id="mode-driving" onclick="selectMode('driving')">🚗 Drive</button>
              <button class="mode-btn" id="mode-cycling" onclick="selectMode('cycling')">🚴 Cycle</button>
              <button class="mode-btn" id="mode-walking" onclick="selectMode('walking')">🚶 Walk</button>
            </div>
          </div>

          <button class="get-directions-btn" onclick="getDirections()">
            <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polygon points="3 11 22 2 13 21 11 13 3 11"/></svg>
            Get Directions
          </button>

          <!-- Route info -->
          <div class="route-info" id="routeInfo">
            <div class="route-info-grid">
              <div class="route-stat"><div class="val" id="routeDist">—</div><div class="lbl">Distance</div></div>
              <div class="route-stat"><div class="val" id="routeTime">—</div><div class="lbl">Est. time</div></div>
            </div>
            <div class="via-text" id="routeVia"></div>
          </div>

          <!-- Turn steps -->
          <div class="steps-box" id="stepsBox" style="display:none;">
            <div class="steps-title">Turn-by-turn directions</div>
            <div class="step-list" id="stepList"></div>
          </div>
        </div>
      </div>

      <!-- RIGHT: Map -->
      <div class="map-container">
        <div class="map-toolbar">
          <span class="map-toolbar-label">World Explorer</span>
          <div style="display:flex;gap:6px;flex-wrap:wrap;">
            <button class="nav-btn" onclick="searchNearby('hotel')">Hotels</button>
            <button class="nav-btn" onclick="searchNearby('tourist_attraction')">Attractions</button>
            <button class="nav-btn" onclick="searchNearby('restaurant')">Restaurants</button>
          </div>
        </div>
        <div class="shortcut-row">
          <span class="shortcut-btn" onclick="flyToCity('Paris', 48.8566, 2.3522)">Paris</span>
          <span class="shortcut-btn" onclick="flyToCity('Bali', -8.4095, 115.1889)">Bali</span>
          <span class="shortcut-btn" onclick="flyToCity('New York', 40.7128, -74.006)">New York</span>
          <span class="shortcut-btn" onclick="flyToCity('Tokyo', 35.6762, 139.6503)">Tokyo</span>
          <span class="shortcut-btn" onclick="flyToCity('Dubai', 25.2048, 55.2708)">Dubai</span>
          <span class="shortcut-btn" onclick="flyToCity('Goa', 15.2993, 74.124)">Goa</span>
          <span class="shortcut-btn" onclick="flyToCity('London', 51.5074, -0.1278)">London</span>
          <span class="shortcut-btn" onclick="flyToCity('Singapore', 1.3521, 103.8198)">Singapore</span>
          <span class="shortcut-btn" onclick="flyToCity('Maldives', 4.1755, 73.5093)">Maldives</span>
          <span class="shortcut-btn" onclick="flyToCity('Jaipur', 26.9124, 75.7873)">Jaipur</span>
          <span class="shortcut-btn" onclick="flyToCity('Manali', 32.2396, 77.1887)">Manali</span>
        </div>
        <div id="map"></div>
        <div class="map-status">
          <div class="status-dot" id="statusDot"></div>
          <span id="statusText">Click "Get Directions" to find a route</span>
        </div>
      </div>
    </div>
  </div>
  <footer>&copy; 2025 <span>Easy Trip</span> — Travel Made Easy &nbsp;|&nbsp; All rights reserved</footer>
</div>

<!-- ═══════════════ LOGIN ═══════════════ -->
<div id="page-login" class="page">
  <div class="login-wrap">
    <div class="login-card">
      <h2 id="formTitle">Welcome back</h2>
      <p class="sub" id="formSub">Log in to your Easy Travel account</p>
      <div id="nameField" class="field" style="display:none;"><label>Full name</label><input type="text" id="nameInput" placeholder="Your full name"/></div>
      <div class="field"><label>Email address</label><input type="email" id="emailInput" placeholder="you@example.com"/></div>
      <div class="field"><label>Password</label><input type="password" id="passInput" placeholder="••••••••"/></div>
      <div id="confirmField" class="field" style="display:none;"><label>Confirm password</label><input type="password" id="confirmInput" placeholder="••••••••"/></div>
      <p class="forgot" id="forgotLink" onclick="toast('Password reset email sent!')">Forgot password?</p>
      <button class="login-btn" id="loginBtn" onclick="doLogin()">Log in</button>
      <div class="divider">or continue with</div>
      <div class="social-btns">
        <button class="social-btn" onclick="socialLogin('Google')">
          <svg width="14" height="14" viewBox="0 0 24 24" style="vertical-align:middle;margin-right:6px;" fill="none"><path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4"/><path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853"/><path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" fill="#FBBC05"/><path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335"/></svg>
          Google
        </button>
        <button class="social-btn" onclick="socialLogin('Facebook')">
          <svg width="14" height="14" viewBox="0 0 24 24" style="vertical-align:middle;margin-right:6px;" fill="#1877F2"><path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/></svg>
          Facebook
        </button>
      </div>
      <div class="toggle-link" id="toggleLink">Don't have an account? <a onclick="toggleForm()">Sign up free</a></div>
    </div>
  </div>
</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
/* ── STATE ── */
let map, userMarker, destMarker, routeLayer;
let userLatLng = null;
let destLatLng = null;
let travelMode = 'driving';
let isLogin = true;
let mapInitialized = false;
let poiMarkers = []; // Array to hold nearby location markers

/* ── NAV ── */
function showPage(p) {
  document.querySelectorAll('.page').forEach(x => x.classList.remove('active'));
  document.getElementById('page-' + p).classList.add('active');
  ['home','explore','map'].forEach(n => {
    const btn = document.getElementById('nav' + n.charAt(0).toUpperCase() + n.slice(1));
    if (btn) btn.classList.toggle('active-nav', n === p);
  });
  window.scrollTo({ top: 0, behavior: 'smooth' });
  if (p === 'map' && !mapInitialized) initMap();
}

function toast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(t._timer);
  t._timer = setTimeout(() => t.classList.remove('show'), 2800);
}

/* ── TAG FILTER ── */
document.querySelectorAll('.tag').forEach(tag => {
  tag.addEventListener('click', function() {
    this.parentElement.querySelectorAll('.tag').forEach(t => t.classList.remove('active'));
    this.classList.add('active');
    toast('Filtered by ' + this.innerText);
  });
});

/* ── MAP & ROUTING ── */
function initMap() {
  if (mapInitialized) return;
  // Default to central India
  map = L.map('map').setView([20.5937, 78.9629], 5); 
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
  }).addTo(map);
  mapInitialized = true;
}

// Geocoding using free Nominatim API
async function geocode(query) {
  try {
    const res = await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(query)}`);
    const data = await res.json();
    if (data && data.length > 0) return [parseFloat(data[0].lat), parseFloat(data[0].lon)];
  } catch (e) {
    console.error("Geocoding failed", e);
  }
  return null;
}

// Calculate route and find nearby locations
async function getDirections() {
  const originVal = document.getElementById('originInput').value;
  const destVal = document.getElementById('destInput').value;
  
  if (!originVal || !destVal) {
    toast("Please enter both origin and destination!");
    return;
  }
  
  document.getElementById('statusText').innerText = "Calculating route...";
  document.getElementById('statusDot').className = "status-dot amber";

  const startCoord = (originVal.toLowerCase() === "my current location" && userLatLng) 
                     ? userLatLng 
                     : await geocode(originVal);

  const endCoord = await geocode(destVal);

  if (!startCoord || !endCoord) {
    toast("Could not find locations.");
    document.getElementById('statusText').innerText = "Location not found.";
    document.getElementById('statusDot').className = "status-dot";
    return;
  }

  // ✅ FIXED OSRM PROFILE
  let profile = "driving";
  if (travelMode === "walking") profile = "foot";
  if (travelMode === "cycling") profile = "cycling";

  const url = `https://router.project-osrm.org/route/v1/${profile}/${startCoord[1]},${startCoord[0]};${endCoord[1]},${endCoord[0]}?overview=full&geometries=geojson&steps=true`;

  try {
    const res = await fetch(url);
    const data = await res.json();

    if (!data.routes || data.routes.length === 0) {
      throw new Error("No route found");
    }

    const route = data.routes[0];
    const coords = route.geometry.coordinates;

    drawRoute(coords, startCoord, endCoord);

    // ✅ Distance & Time
    document.getElementById('routeInfo').classList.add('visible');
    document.getElementById('routeDist').innerText = (route.distance / 1000).toFixed(1) + " km";
    document.getElementById('routeTime').innerText = Math.round(route.duration / 60) + " mins";

    // ✅ TURN-BY-TURN STEPS (FIXED)
    const stepsBox = document.getElementById('stepsBox');
    const stepList = document.getElementById('stepList');
    stepList.innerHTML = "";

    if (route.legs && route.legs.length > 0) {
      route.legs[0].steps.forEach(step => {
        const div = document.createElement("div");
        div.className = "step-item";

        div.innerHTML = `
          <div class="step-dot"></div>
          <div>
            ${step.maneuver.instruction || "Continue"}
            <div class="step-dist">${(step.distance / 1000).toFixed(2)} km</div>
          </div>
        `;

        stepList.appendChild(div);
      });

      stepsBox.style.display = "block";
    }

    // Nearby fake POIs (your logic kept)
    showLocationsOnRoute(coords);

    document.getElementById('statusText').innerText = "Route ready with directions!";
    document.getElementById('statusDot').className = "status-dot green";

  } catch (error) {
    console.error(error);
    toast("Route calculation failed!");
    document.getElementById('statusText').innerText = "Routing failed.";
    document.getElementById('statusDot').className = "status-dot";
  }
}

function drawRoute(coords, start, end) {
  if (routeLayer) map.removeLayer(routeLayer);
  if (userMarker) map.removeLayer(userMarker);
  if (destMarker) map.removeLayer(destMarker);
  
  // Clear old POIs
  poiMarkers.forEach(m => map.removeLayer(m));
  poiMarkers = [];

  // Convert [lng, lat] to [lat, lng] for Leaflet
  const latLngs = coords.map(c => [c[1], c[0]]);
  
  routeLayer = L.polyline(latLngs, {color: '#1D9E75', weight: 5}).addTo(map);
  userMarker = L.marker(start).addTo(map).bindPopup("Origin");
  destMarker = L.marker(end).addTo(map).bindPopup("Destination");
  
  map.fitBounds(routeLayer.getBounds(), { padding: [40, 40] });
}

// Calculate and show nearby locations along the specific route
function showLocationsOnRoute(routeCoords) {
  // To simulate finding places on a route without a paid backend API,
  // we divide the route into segments and place markers near the path.
  // In a full-stack app, you would send these coordinates to Google Places or Foursquare API.
  
  const totalWaypoints = routeCoords.length;
  const stopsToFind = 15; // Number of locations to show along the route
  const step = Math.floor(totalWaypoints / (stopsToFind + 1));
  
  const placeTypes = ['🏨 Traditional Hotels', '☕ Tradition food', '⛽ Gas Station', '📸 Traditional Tempel', 'Traditional area'];

  for(let i = 1; i <= stopsToFind; i++) {
    if (routeCoords[i * step]) {
      const pt = routeCoords[i * step];
      const lng = pt[0];
      const lat = pt[1];
      
      // Offset slightly so markers sit next to the road line, not exactly on it
      const offsetLat = lat + (Math.random() - 0.5) * 0.005;
      const offsetLng = lng + (Math.random() - 0.5) * 0.005;
      
      const randomPlace = placeTypes[Math.floor(Math.random() * placeTypes.length)];
      
      // Create a distinct marker for nearby locations
      const marker = L.circleMarker([offsetLat, offsetLng], {
        color: '#EF9F27',
        fillColor: '#fff',
        fillOpacity: 1,
        radius: 7,
        weight: 2
      }).addTo(map).bindPopup(`<b>${randomPlace}</b><br>Located directly on your route.`);
      
      poiMarkers.push(marker);
    }
  }
  
  setTimeout(() => toast("Found " + stopsToFind + " places along your route!"), 1000);
}

/* ── UTILS & INTERACTIONS ── */
function useMyLocation() {
  if (navigator.geolocation) {
    toast("Locating you...");
    navigator.geolocation.getCurrentPosition(pos => {
      userLatLng = [pos.coords.latitude, pos.coords.longitude];
      document.getElementById('originInput').value = "My Current Location";
      toast("Location acquired!");
      if(mapInitialized) map.flyTo(userLatLng, 13);
    }, () => toast("Location access denied."));
  } else {
    toast("Geolocation not supported by this browser.");
  }
}

function selectMode(mode) {
  travelMode = mode;
  document.querySelectorAll('.mode-btn').forEach(b => b.classList.remove('selected'));
  document.getElementById('mode-' + mode).classList.add('selected');
}

function searchNearby(type) {
  toast("Searching for " + type.replace('_', ' ') + "s nearby...");
}

function flyToCity(name, lat, lng) {
  document.getElementById('destInput').value = name;
  if(mapInitialized) map.flyTo([lat, lng], 11);
  toast("Selected " + name);
}

function goToDestination(name, lat, lng) {
  showPage('map');
  flyToCity(name, lat, lng);
}

async function doSearch() {
  const query = document.getElementById('searchInput').value.trim();
  
  if (!query) {
    toast("Please enter a location");
    return;
  }

  toast("Searching location...");
  
  const coords = await geocode(query);

  if (!coords) {
    toast("Location not found!");
    return;
  }

  showPage('map');

  setTimeout(() => {
    if (!mapInitialized) initMap();

    map.flyTo(coords, 12);

    // Remove old marker if exists
    if (destMarker) map.removeLayer(destMarker);

    destMarker = L.marker(coords).addTo(map)
      .bindPopup(`<b>${query}</b>`)
      .openPopup();

    document.getElementById('destInput').value = query;

    toast("Location found!");
  }, 300);
}

/* ── AUTH STUBS ── */
function toggleForm() {
  isLogin = !isLogin;
  document.getElementById('formTitle').innerText = isLogin ? 'Welcome back' : 'Create an account';
  document.getElementById('formSub').innerText = isLogin ? 'Log in to your account' : 'Join for free and start planning';
  document.getElementById('nameField').style.display = isLogin ? 'none' : 'block';
  document.getElementById('confirmField').style.display = isLogin ? 'none' : 'block';
  document.getElementById('loginBtn').innerText = isLogin ? 'Log in' : 'Sign up';
  document.getElementById('forgotLink').style.display = isLogin ? 'block' : 'none';
  document.getElementById('toggleLink').innerHTML = isLogin 
    ? 'Don\'t have an account? <a onclick="toggleForm()">Sign up free</a>' 
    : 'Already have an account? <a onclick="toggleForm()">Log in</a>';
}

function doLogin() {
  const action = isLogin ? 'Logged in' : 'Account created';
  toast(action + " successfully!");
  setTimeout(() => showPage('home'), 1000);
}

function socialLogin(provider) { 
  toast("Connecting to " + provider + "..."); 
}

// Init startup
window.onload = () => showPage('home');
</script>
</body>
</html>



   
