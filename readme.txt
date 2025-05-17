<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>NBA Central Hub</title>
  <style>
    :root {
      --bg-dark: #121212;
      --card-bg: #1e1e1e;
      --highlight: #1cfd49;
      --accent: #FFD700;
      --text-light: #ffffff;
      --gray: #2a2a2a;
      --gray-border: #444;
    }
    * {
      margin: 0; padding: 0; box-sizing: border-box;
    }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--bg-dark);
      color: var(--text-light);
      padding: 20px;
      text-align: center;
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
    }
    h1 { font-size: 2.5rem; margin-bottom: 15px; color: var(--accent); }
    h2 { color: var(--accent); margin: 20px 0 10px; }
    .marquee {
      background: var(--card-bg);
      color: var(--highlight);
      font-weight: 600;
      overflow: hidden;
      width: 100%;
      max-width: 900px;
      margin: 0 auto 30px;
      border-radius: 8px;
      height: 40px;
      display: flex;
      align-items: center;
    }
    .marquee span {
      display: inline-block;
      white-space: nowrap;
      padding-left: 100%;
      animation: marquee 20s linear infinite;
    }
    @keyframes marquee {
      0% { transform: translateX(0); }
      100% { transform: translateX(-100%); }
    }
    .team-list {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: center;
      max-width: 1000px;
      margin-bottom: 40px;
    }
    .team-link {
      background: var(--card-bg);
      padding: 15px;
      border-radius: 10px;
      text-align: center;
      width: 180px;
      color: #fff;
      text-decoration: none;
      transition: transform 0.3s ease;
    }
    .team-link:hover {
      transform: scale(1.05);
    }
    .team-link img {
      width: 60px;
      height: 60px;
      object-fit: contain;
      display: block;
      margin: 0 auto 10px auto;
    }
    #news, #scores, #standings, #chat {
      max-width: 900px;
      width: 100%;
      margin-bottom: 40px;
    }
    .score-box {
      background: var(--gray);
      margin: 10px auto;
      border-radius: 10px;
      padding: 14px 20px;
      font-weight: 600;
      font-size: 1.1rem;
    }
    .standings-table {
      width: 100%;
      border-collapse: collapse;
    }
    .standings-table th,
    .standings-table td {
      border: 1px solid var(--gray-border);
      padding: 12px 15px;
      text-align: center;
    }
    .standings-table th {
      background-color: #333;
    }
    .standings-table tr:nth-child(even) {
      background-color: var(--gray);
    }
    ul#news-list {
      list-style-type: none;
      padding-left: 0;
      font-size: 1.05rem;
      color: var(--highlight);
    }
    ul#news-list li {
      padding: 8px 0;
      border-bottom: 1px solid var(--gray-border);
    }
  </style>
</head>
<body>
  <h1>NBA Central Hub</h1>
  <div class="marquee"><span>🏀 LeBron James retires • Draymond Green stops fouling • Steph Curry grows to 6'7 • Giannis gets traded to Lakers! 🏀</span></div>
  <div class="team-list" id="team-list"></div>

  <div id="scores">
    <h2>Live Scores</h2>
    <div class="score-box">Warriors 115 - Suns 108 (Final)</div>
    <div class="score-box">Lakers 122 - Nuggets 117 (OT)</div>
    <div class="score-box">Celtics 109 - Heat 101 (Final)</div>
  </div>

  <div id="standings">
    <h2>Eastern Conference Standings</h2>
    <table class="standings-table">
      <thead>
        <tr><th>Team</th><th>W</th><th>L</th><th>GB</th></tr>
      </thead>
      <tbody>
        <tr><td>Celtics</td><td>55</td><td>27</td><td>-</td></tr>
        <tr><td>Bucks</td><td>52</td><td>30</td><td>3</td></tr>
        <tr><td>76ers</td><td>50</td><td>32</td><td>5</td></tr>
      </tbody>
    </table>
  </div>

  <div id="news">
    <h2>Recent Basketball News</h2>
    <ul id="news-list">
      <li><a href="#">Victor Wembanyama dominates in debut game</a></li>
      <li><a href="#">Trade rumors swirl around Damian Lillard</a></li>
      <li><a href="#">Luka Doncic records historic triple-double</a></li>
    </ul>
  </div>

  <div id="chat">
    <h2>Chat</h2>
    <!-- Chat interface goes here -->
  </div>

  <script>
    const teamNameMap = {
      1: "Atlanta Hawks", 2: "Boston Celtics", 3: "Brooklyn Nets", 4: "Charlotte Hornets",
      5: "Chicago Bulls", 6: "Cleveland Cavaliers", 7: "Dallas Mavericks", 8: "Denver Nuggets",
      9: "Detroit Pistons", 10: "Golden State Warriors", 11: "Houston Rockets", 12: "Indiana Pacers",
      13: "LA Clippers", 14: "Los Angeles Lakers", 15: "Memphis Grizzlies", 16: "Miami Heat",
      17: "Milwaukee Bucks", 18: "Minnesota Timberwolves", 19: "New Orleans Pelicans", 20: "New York Knicks",
      21: "Oklahoma City Thunder", 22: "Orlando Magic", 23: "Philadelphia 76ers", 24: "Phoenix Suns",
      25: "Portland Trail Blazers", 26: "Sacramento Kings", 27: "San Antonio Spurs", 28: "Toronto Raptors",
      29: "Utah Jazz", 30: "Washington Wizards"
    };

    const teamLogos = {
      "Atlanta Hawks": "https://loodibee.com/wp-content/uploads/nba-atlanta-hawks-logo.png",
      "Boston Celtics": "https://loodibee.com/wp-content/uploads/nba-boston-celtics-logo.png",
      "Brooklyn Nets": "https://loodibee.com/wp-content/uploads/nba-brooklyn-nets-logo.png",
      "Charlotte Hornets": "https://loodibee.com/wp-content/uploads/nba-charlotte-hornets-logo.png",
      "Chicago Bulls": "https://loodibee.com/wp-content/uploads/nba-chicago-bulls-logo.png",
      "Cleveland Cavaliers": "https://loodibee.com/wp-content/uploads/nba-cleveland-cavaliers-logo.png",
      "Dallas Mavericks": "https://loodibee.com/wp-content/uploads/nba-dallas-mavericks-logo.png",
      "Denver Nuggets": "https://upload.wikimedia.org/wikipedia/en/7/76/Denver_Nuggets.svg",
      "Detroit Pistons": "https://loodibee.com/wp-content/uploads/nba-detroit-pistons-logo.png",
      "Golden State Warriors": "https://loodibee.com/wp-content/uploads/nba-golden-state-warriors-logo.png",
      "Houston Rockets": "https://loodibee.com/wp-content/uploads/nba-houston-rockets-logo.png",
      "Indiana Pacers": "https://loodibee.com/wp-content/uploads/nba-indiana-pacers-logo.png",
      "LA Clippers": "https://loodibee.com/wp-content/uploads/nba-la-clippers-logo.png",
      "Los Angeles Lakers": "https://loodibee.com/wp-content/uploads/nba-los-angeles-lakers-logo.png",
      "Memphis Grizzlies": "https://loodibee.com/wp-content/uploads/nba-memphis-grizzlies-logo.png",
      "Miami Heat": "https://loodibee.com/wp-content/uploads/nba-miami-heat-logo.png",
      "Milwaukee Bucks": "https://loodibee.com/wp-content/uploads/nba-milwaukee-bucks-logo.png",
      "Minnesota Timberwolves": "https://loodibee.com/wp-content/uploads/nba-minnesota-timberwolves-logo.png",
      "New Orleans Pelicans": "https://loodibee.com/wp-content/uploads/nba-new-orleans-pelicans-logo.png",
      "New York Knicks": "https://loodibee.com/wp-content/uploads/nba-new-york-knicks-logo.png",
      "Oklahoma City Thunder": "https://loodibee.com/wp-content/uploads/nba-oklahoma-city-thunder-logo.png",
      "Orlando Magic": "https://loodibee.com/wp-content/uploads/nba-orlando-magic-logo.png",
      "Philadelphia 76ers": "https://loodibee.com/wp-content/uploads/nba-philadelphia-76ers-logo.png",
      "Phoenix Suns": "https://loodibee.com/wp-content/uploads/nba-phoenix-suns-logo.png",
      "Portland Trail Blazers": "https://loodibee.com/wp-content/uploads/nba-portland-trail-blazers-logo.png",
      "Sacramento Kings": "https://loodibee.com/wp-content/uploads/nba-sacramento-kings-logo.png",
      "San Antonio Spurs": "https://loodibee.com/wp-content/uploads/nba-san-antonio-spurs-logo.png",
      "Toronto Raptors": "https://loodibee.com/wp-content/uploads/nba-toronto-raptors-logo.png",
      "Utah Jazz": "https://loodibee.com/wp-content/uploads/nba-utah-jazz-logo.png",
      "Washington Wizards": "https://loodibee.com/wp-content/uploads/nba-washington-wizards-logo.png"
    };

    function populateTeamList() {
      const container = document.getElementById("team-list");
      for (const [id, name] of Object.entries(teamNameMap)) {
        const logo = teamLogos[name] || "";
        const logoLink = document.createElement("a");
        logoLink.href = `https://www.espn.com/nba/team/roster/_/name/${name.split(' ').pop().toLowerCase()}`;
        logoLink.target = "_blank";
        logoLink.rel = "noopener noreferrer";

        const link = document.createElement("a");
        link.href = `team.html?id=${id}`;
        link.className = "team-link";
        logoLink.innerHTML = `<img src="${logo}" alt="${name} logo">`;
        link.innerHTML = `${logoLink.outerHTML}<div>${name}</div>`;
        container.appendChild(link);
      }
    }

    populateTeamList();
  </script>
</body>
</html>
