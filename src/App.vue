<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg bg-light">
      <div class="container-fluid">
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <select name="team" v-model="selectedTeam">
              <option value="ALL">ALL</option>
              <option value="IND">IND</option>
              <option value="AUS">AUS</option>
              <option value="PAK">PAK</option>
              <option value="ENG">ENG</option>
            </select>
          </ul>
          <form class="d-flex" role="search">
            <input class="form-control me-2" type="search" placeholder="Search" v-model="searchQuery">
            <button class="btn btn-outline-success" type="button" @click="searchPlayers">Search</button>
          </form>
        </div>
      </div>
    </nav>

    <div class="container text-center">
      <div class="row batsman-row">
        <h1>Batsmen</h1>
        <div v-for="player in filteredPlayers('batsman')" :key="player.name" class="col">
          <div class="card">
            <img :src="player.image" class="card-img-top" alt="player image">
            <div class="card-body">
              <h5 class="card-title">{{ player.name }}</h5>
              <p class="card-text">Matches: {{ player.matches }}</p>
              <p class="card-text">Runs: {{ player.runs }}</p>
              <p class="card-text">50/100s: {{ player['50s'] }}/{{ player['100s'] }}</p>
              <p class="card-text">Highest Score: {{ player.highest_score }}</p>
              <p class="card-text">Team Name: {{ player.team_name }}</p>
              <p class="card-text">Best Bowling Figures: {{ player.best_bowling_figures }}</p>
            </div>
          </div>
        </div>
      </div>

      <div class="row bowler-row">
        <h1>Bowlers</h1>
        <div v-for="player in filteredPlayers('bowler')" :key="player.name" class="col">
          <div class="card">
            <img :src="player.image" class="card-img-top" alt="player image">
            <div class="card-body">
              <h5 class="card-title">{{ player.name }}</h5>
              <p class="card-text">Matches: {{ player.matches }}</p>
              <p class="card-text">Runs: {{ player.runs }}</p>
              <p class="card-text">50/100s: {{ player['50s'] }}/{{ player['100s'] }}</p>
              <p class="card-text">Highest Score: {{ player.highest_score }}</p>
              <p class="card-text">Team Name: {{ player.team_name }}</p>
              <p class="card-text">Best Bowling Figures: {{ player.best_bowling_figures }}</p>
            </div>
          </div>
        </div>
      </div>

      <div class="row all-rounder-row">
        <h1>All-rounders</h1>
        <div v-for="player in filteredPlayers('allrounder')" :key="player.name" class="col">
          <div class="card">
            <img :src="player.image" class="card-img-top" alt="player image">
            <div class="card-body">
              <h5 class="card-title">{{ player.name }}</h5>
              <p class="card-text">Matches: {{ player.matches }}</p>
              <p class="card-text">Runs: {{ player.runs }}</p>
              <p class="card-text">50/100s: {{ player['50s'] }}/{{ player['100s'] }}</p>
              <p class="card-text">Highest Score: {{ player.highest_score }}</p>
              <p class="card-text">Team Name: {{ player.team_name }}</p>
              <p class="card-text">Best Bowling Figures: {{ player.best_bowling_figures }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '',
      selectedTeam: 'ALL',
      originalPlayers: [
        { name: "Virat Kohli", matches: "101", role: 2, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: 0, team_name: "IND", image: "https://resources.pulse.icc-cricket.com/players/130x150/164.png" },
        { name: "Hardik Pandya", matches: "100", role: 3, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: "1-23", team_name: "IND", image: "https://resources.pulse.icc-cricket.com/players/130x150/2740.png" },
        { name: "Jasprit Bumrah", matches: "100", role: 4, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: "5-23", team_name: "IND", image: "https://resources.pulse.icc-cricket.com/players/130x150/1124.png" },
        { name: "David Warner", matches: "100", role: 2, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: 0, team_name: "AUS", image: "https://resources.pulse.icc-cricket.com/players/210/170.png" },
        { name: "Glen Maxwell", matches: "100", role: 3, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: "1-23", team_name: "AUS", image: "https://resources.pulse.icc-cricket.com/players/130x150/282.png" },
        { name: "Pat Cummins", matches: "100", role: 4, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: "5-23", team_name: "AUS", image: "https://resources.pulse.icc-cricket.com/players/130x150/488.png" },
        { name: "Babar Azam", matches: "100", role: 2, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: 0, team_name: "PAK", image: "https://resources.pulse.icc-cricket.com/players/130x150/2759.png" },
        { name: "Shaheen Afridi", matches: "100", role: 3, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: "1-23", team_name: "PAK", image: "https://resources.pulse.icc-cricket.com/players/130x150/4530.png" },
        { name: "Haris Rauf", matches: "100", role: 4, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: "5-23", team_name: "PAK", image: "" },
        { name: "Jonny Bairstow", matches: "100", role: 2, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: 0, team_name: "ENG", image: "https://resources.pulse.icc-cricket.com/players/130x150/506.png" },
        { name: "Ben Stokes", matches: "100", role: 3, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: "1-23", team_name: "ENG", image: "https://resources.pulse.icc-cricket.com/players/130x150/158.png" },
        { name: "Jofra Archer", matches: "100", role: 4, runs: "10234", "50s": "23", "100s": "46", highest_score: "183", best_bowling_figures: "5-23", team_name: "ENG", image: "https://resources.pulse.icc-cricket.com/players/130x150/4183.png" },
      ],
    };
  },
  methods: {
  searchPlayers() {
    this.searchQuery = this.searchQuery.trim().toLowerCase();
    console.log("Searching for:", this.searchQuery);
  },
  filteredPlayers(roleType) {
    const roleMap = { batsman: 2, bowler: 4, allrounder: 3 };
    return this.originalPlayers.filter((player) => {
      return (
        player.role === roleMap[roleType] &&
        (this.selectedTeam === 'ALL' || player.team_name === this.selectedTeam) &&
        player.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    });
  },
}

};
</script>

<style scoped>
body {
  background-color: #f8f9fa;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.navbar {
  margin-bottom: 30px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
.navbar-brand {
  font-weight: bold;
  font-size: 24px;
}
.navbar select, .navbar .form-control {
  margin-right: 15px;
}

form.d-flex {
  margin-left: auto;
  display: flex;
  align-items: center;
}

.form-control {
  width: 250px;
  font-size: 16px;
  padding: 5px;
  border-radius: 5px;
}

.btn-outline-success {
  font-size: 16px;
  padding: 6px 12px;
  border-radius: 5px;
}

h1 {
  margin-top: 30px;
  font-size: 32px;
  font-weight: bold;
  color: #343a40;
  border-bottom: 2px solid #007bff;
  padding-bottom: 10px;
  display: inline-block;
  text-align: center;
  width: 100%;
  margin-bottom: 30px;
}

.card {
  width: 100%;
  max-width: 300px;
  background-color: rgb(236, 210, 160);
  margin: 15px auto;
  border-radius: 10px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.card:hover {
  transform: translateY(-5px);
  background-color: rgb(118, 221, 235);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.card-img-top {
  border-radius: 10px 10px 0 0;
}

.card-body {
  padding: 20px;
  text-align: left;
}

.card-title {
  font-size: 22px;
  font-weight: 600;
  margin-bottom: 15px;
  color: #bc2929;
}

.card-text {
  font-size: 16px;
  color: #495057;
  margin-bottom: 8px;
}

.batsman-row, .bowler-row, .all-rounder-row {
  margin-bottom: 60px;
}

.container .row {
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
}

#team {
  font-size: 16px;
  padding: 5px;
  border-radius: 5px;
}

@media (min-width: 768px) {
  .card {
    width: 260px;
  }
}

@media (min-width: 1200px) {
  .card {
    width: 300px;
  }
}
</style>
