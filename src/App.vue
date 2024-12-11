<template>
  <div class="container">
    <img src="/fomo3d-logo.jpeg" alt="Logo" class="logo-image" />

    <h1>Revenue Share Calculator</h1>

    <!-- Gobbler Revenue -->
    <div class="card revenue-card">
      <label for="daily_revenue">Gobbler Daily Revenue:</label>
      <input v-model.number="dailyRevenue" type="number" id="daily_revenue" min="0" step="0.01"
        placeholder="Please enter daily revenue" required />
    </div>

    <!-- Tokens -->
    <div class="holdings">
      <!-- Token 1 -->
      <div class="card holdings-card">
        <img src="/fomo3d.jpg" alt="Token 1 Logo" />

        <!-- Total staked supply -->
        <label for="token1_total_staked">Total Staked $FOMO3D Tokens:</label>
        <input type="number" v-model.number="tokens.token1.totalStaked" id="token1_total_staked" min="0" step="0.01"
          placeholder="Total staked tokens" required />

        <!-- User staked supply -->
        <label for="token1_user_staked">Your Staked $FOMO3D Tokens:</label>
        <input v-model.number="tokens.token1.userStaked" type="number" id="token1_user_staked" min="0" step="0.01"
          placeholder="Enter your staked amount" />
      </div>

      <!-- Token 2 -->
      <div class="card holdings-card">
        <img src="/pet.jpg" alt="Token 2 Logo" />

        <!-- Total staked supply -->
        <label for="token2_total_staked">Total Staked $PET Tokens:</label>
        <input v-model.number="tokens.token2.totalStaked" type="number" id="token2_total_staked" min="0" step="0.01"
          placeholder="Total staked tokens" required />

        <!-- User staked supply -->
        <label for="token2_user_staked">Your Staked $PET Tokens:</label>
        <input v-model.number="tokens.token2.userStaked" type="number" id="token2_user_staked" min="0" step="0.01"
          placeholder="Enter your staked amount" />
      </div>

      <!-- Token 3 -->
      <div class="card holdings-card">
        <img src="/mage.jpg" alt="Token 3 Logo" />

        <!-- Total staked supply -->
        <label for="token3_total_staked">Total Staked NFTs:</label>
        <input v-model.number="tokens.token3.totalStaked" type="number" id="token3_total_staked" min="0" step="1"
          placeholder="Enter total staked NFTs" required />

        <!-- User staked supply -->
        <label for="token3_user_staked">Your Staked NFTs:</label>
        <input v-model.number="tokens.token3.userStaked" type="number" id="token3_user_staked" min="0" step="1"
          placeholder="Enter your staked NFTs" />
      </div>
    </div>

    <!-- Calculate Button -->
    <button @click="calculateRevenue">Calculate</button>

    <!-- Result Section -->
    <div v-if="calculated" class="result">
      <h2>Calculated Revenue Share</h2>
      <table class="results-table">
        <thead>
          <tr>
            <th>Token</th>
            <th>Your Share ($)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>$FOMO3D</td>
            <td>${{ results.token1Revenue }}</td>
          </tr>
          <tr>
            <td>$PET</td>
            <td>${{ results.token2Revenue }}</td>
          </tr>
          <tr>
            <td>NFTs</td>
            <td>${{ results.token3Revenue }}</td>
          </tr>
          <tr class="total-row">
            <td><strong>Total Daily Revenue</strong></td>
            <td><strong>${{ results.totalRevenue }}</strong></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dailyRevenue: 1000000,
      tokens: {
        token1: {
          totalStaked: 1,
          userStaked: 0,
        },
        token2: {
          totalStaked: 1,
          userStaked: 0,
        },
        token3: {
          totalStaked: 400,
          userStaked: 0,
        },
      },
      results: {
        token1Revenue: 0,
        token2Revenue: 0,
        token3Revenue: 0,
        totalRevenue: 0,
      },
      calculated: false,
    };
  },
  methods: {
    calculateRevenue() {
      const dailyRevenue = this.dailyRevenue;

      // Define revenue shares
      const token1Share = 0.69; // 69%
      const token2Share = 0.015; // 1.5%
      const token3Share = 0.25; // 25%

      // Step 1: Calculate the revenue allocated for each token
      const token1RevenuePool = dailyRevenue * token1Share;
      const token2RevenuePool = dailyRevenue * token2Share;
      const token3RevenuePool = dailyRevenue * token3Share;

      // Step 2: Calculate the user's share percentage
      const userSharePercentageToken1 = this.tokens.token1.userStaked / this.tokens.token1.totalStaked;
      const userSharePercentageToken2 = this.tokens.token2.userStaked / this.tokens.token2.totalStaked;
      const userSharePercentageToken3 = this.tokens.token3.userStaked / this.tokens.token3.totalStaked;

      // Step 3: Calculate the user's daily revenue
      const userDailyRevenueToken1 = token1RevenuePool * userSharePercentageToken1;
      const userDailyRevenueToken2 = token2RevenuePool * userSharePercentageToken2;
      const userDailyRevenueToken3 = token3RevenuePool * userSharePercentageToken3;

      // Total revenue for all tokens
      const totalRevenue = userDailyRevenueToken1 + userDailyRevenueToken2 + userDailyRevenueToken3;

      // Update results
      this.results.token1Revenue = this.formatNumber(userDailyRevenueToken1);
      this.results.token2Revenue = this.formatNumber(userDailyRevenueToken2);
      this.results.token3Revenue = this.formatNumber(userDailyRevenueToken3);
      this.results.totalRevenue = this.formatNumber(totalRevenue);

      this.calculated = true;
    },
    formatNumber(value) {
      return new Intl.NumberFormat('en-US', {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(value);
    }
  },
};
</script>

<style scoped>
body {
  font-family: 'Arial', sans-serif;
  color: #ffffff;
  background-color: #0d0d0f;
  margin: 0;
  padding: 20px;
}

.container {
  min-width: 320px;
  max-width: 70%;
  margin: 0 auto;
  background-color: #1a1a1d;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.5);
}

.revenue-card {
  width: 100%;
  max-width: 500px;
  margin: 20px auto;
  padding: 20px;
  background-color: #25252a;
  border-radius: 8px;
  text-align: center;
  box-sizing: border-box;
}

.revenue-card input[type="number"] {
  width: calc(100% - 20px);
  margin: 10px auto;
  display: block;
}

.card {
  background-color: #25252a;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  box-sizing: border-box;
}

.holdings {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 20px;
  flex-wrap: wrap;
}

.holdings-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 30%;
  height: auto;
  overflow: hidden;
}

.holdings-card img {
  max-width: 90%;
  height: auto;
  margin: 0 auto 20px auto;
  display: block;
  border-radius: 8px;
  object-fit: contain;
}

input[type="number"] {
  width: 90%;
  padding: 10px;
  border: 1px solid #444;
  background-color: #2d2d33;
  color: #ffffff;
  border-radius: 6px;
  margin-bottom: 20px;
}

input::placeholder {
  color: #a3a3a3;
}

button {
  width: 60%;
  display: block;
  margin: 30px auto 10px auto;
  padding: 12px;
  background: linear-gradient(90deg, #6a00ff, #8900ff);
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 18px;
  cursor: pointer;
}

button:hover {
  background: linear-gradient(90deg, #5a00e6, #7a00e6);
}

h1,
h2 {
  text-align: center;
  color: #e3e3e3;
}

label {
  font-weight: bold;
  color: #bfbfbf;
  display: block;
  margin-bottom: 10px;
}

.result {
  margin-top: 30px;
  padding: 20px;
  background-color: #25252a;
  border-radius: 8px;
  border: 2px solid #6a00ff;
}

.logo-image {
  display: block;
  margin: 0 auto 20px auto;
  max-width: 80%;
  height: auto;
}

.results-table {
  width: 100%;
  margin-top: 20px;
  border-collapse: collapse;
  font-size: 1rem;
  text-align: left;
  color: #e3e3e3;
}

.results-table thead {
  background-color: #6a00ff;
  color: white;
}

.results-table th,
.results-table td {
  padding: 12px;
  border: 1px solid #444;
}

.results-table tbody tr:nth-child(odd) {
  background-color: #2d2d33;
}

.results-table tbody tr:nth-child(even) {
  background-color: #25252a;
}

.results-table .total-row {
  background-color: #1a1a1d;
  font-weight: bold;
}

.results-table .total-row td {
  border-top: 2px solid #6a00ff;
}

.results-table tbody tr:hover {
  background-color: #3a3a42;
}

/* Media Query for Responsiveness */
@media screen and (max-width: 768px) {
  .container {
    max-width: 90%;
    padding: 20px;
  }

  .holdings {
    flex-direction: column;
    gap: 20px;
  }

  .holdings-card {
    width: 100%;
  }

  .holdings-card img {
    max-width: 60%;
  }

  button {
    width: 80%;
  }
}

@media screen and (max-width: 480px) {
  h1 {
    font-size: 1.5rem;
  }

  label {
    font-size: 0.9rem;
  }

  input[type="number"] {
    padding: 8px;
  }
}
</style>
