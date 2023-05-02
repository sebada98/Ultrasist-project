
<template>
  <div>
    <div>
      <main class="main-container">
        <div class="balance">
          <div>
            <p class="balance__title">My Balance</p>
            <p class="balance__total">$921.48</p>
          </div>
          <div>
            <img class="balance__logo" src="/images/logo.svg" alt="" />
          </div>
        </div>
        <div class="chart">
          <h2 class="chart__title">Spending - Last 7 days</h2>
          <div class="chart__bars-container">
          </div>
          <div class="chart__analytics">
            <div>
              <p class="chart__total">Total this month</p>
              <p class="chart__total--value">478.33</p>
            </div>
            <div>
              <p class="chart__percent">+2.4%</p>
              <p class="chart__period">from last month</p>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>

</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "DM Sans", sans-serif;
  background-color: #222;
  font-size: 18px;
}

.main-container {
  width: 344px;
  margin: 68px auto;
}
@media (min-width: 600px) {
  .main-container {
    width: 540px;
  }
}

.balance {
  background-color: #444;
  height: 100px;
  border-radius: 16px;
  padding: 24px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #eee;
  margin-bottom: 16px;
}
.balance__title {
  font-size: 0.7em;
  margin-bottom: 6px;
}
.balance__total {
  font-weight: 700;
  font-size: 1.4em;
}
.balance__logo {
  width: 60px;
}
@media (min-width: 600px) {
  .balance {
    height: 126px;
  }
  .balance__title {
    font-size: 1em;
    margin-bottom: 12px;
  }
  .balance__total {
    font-size: 1.8em;
  }
  .balance__logo {
    width: 75px;
  }
}

.chart {
  background-color: #333;
  color: #eee;
  border-radius: 16px;
  height: 424px;
  padding: 28px 20px;
}
.chart__title {
  color: #fff;
  font-size: 1.25em;
  margin-bottom: 10px;
}
.chart__bars-container {
  display: flex;
  justify-content: space-around;
  align-items: flex-end;
  margin-bottom: 30px;
  height: 220px;
}
.chart__bar {
  background-color: #888;
  height: 200px;
  width: 32px;
  position: relative;
  border-radius: 4px;
}
.chart__bar--label {
  background-color: #eee;
  width: 38px;
  padding: 4px;
  font-size: 0.5em;
  color: #333;
  border-radius: 4px;
  font-weight: 700;
  position: absolute;
  top: -24px;
  left: -4px;
  display: none;
}
.chart__bar--day {
  position: absolute;
  bottom: -16px;
  font-size: 0.6em;
  width: 32px;
  text-align: center;
  color: #eee;
}
.chart__bar:hover {
  cursor: pointer;
  filter: brightness(1.2);
}
.chart__analytics {
  display: flex;
  justify-content: space-between;
  padding: 24px;
  font-size: 0.8em;
  border-top: 3px solid #444;
}
.chart__total {
  margin-bottom: 8px;
}
.chart__total--value {
  color: #fff;
  font-size: 1.4em;
  font-weight: 700;
}
.chart__percent {
  color: #fff;
  font-weight: 700;
  text-align: right;
  margin-top: 18px;
}
@media (min-width: 600px) {
  .chart {
    height: 512px;
    padding: 44px;
  }
  .chart__title {
    font-size: 2em;
  }
  .chart__bars-container {
    margin-bottom: 50px;
  }
  .chart__bar {
    width: 50px;
  }
  .chart__bar--label {
    width: 54px;
    font-size: 0.7em;
    top: -32px;
    left: 2px;
  }
  .chart__bar--day {
    width: 54px;
    bottom: -24px;
    font-size: 0.8em;
  }
  .chart__analytics {
    padding-top: 32px;
    font-size: 1em;
  }
  .chart__total-value {
    font-size: 2.4em;
  }
  .chart__percent {
    margin-top: 30px;
  }
}

.attribution {
  font-size: 11px;
  text-align: center;
  color: #eee;
}

.attribution a {
  color: #888;
}
</style>

