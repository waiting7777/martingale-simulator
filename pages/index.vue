<template>
  <div class="container mt-3">
    <h4>Martingale Testing</h4>
    <br/>
    <h4>Conditions</h4>
    <div id="condition-contain">
      <div class="input-group mt-3">
        <div class="input-group-prepend">
          <label class="input-group-text">Total Assets</label>
        </div>
        <input type="number" class="form-control" v-model="total">
      </div>
      <div class="input-group mt-3">
        <div class="input-group-prepend">
          <label class="input-group-text">Bet Amount</label>
        </div>
        <input type="number" class="form-control" v-model="betAmount" step="10">
      </div>
      <div class="input-group mt-3">
        <div class="input-group-prepend">
          <label class="input-group-text">Win Rate</label>
        </div>
        <input type="number" class="form-control" v-model="winRate">
        <div class="input-group-append">
          <span class="input-group-text">%</span>
        </div>
      </div>
      <div class="input-group mt-3">
        <div class="input-group-prepend">
          <label class="input-group-text">Reture Factor</label>
        </div>
        <input type="number" class="form-control" v-model="returnFactor">
      </div>
      <!-- <div class="input-group mt-3">
        <div class="input-group-prepend">
          <label class="input-group-text">Leave Threshold</label>
        </div>
        <input type="number" class="form-control" v-model="leaveThreshold">
      </div> -->
      <div class="input-group mt-3">
        <div class="input-group-prepend">
          <label class="input-group-text">Play Times</label>
        </div>
        <input type="number" class="form-control" v-model="playTimes">
      </div>
      <div class="mt-3">
        <button class="btn btn-info" @click="simulate(1)">Martingale Simulate</button>
      </div>
      <div class="mt-3">
        <button class="btn btn-info" @click="simulate(2)">Another Simulate</button>
      </div>
    </div>
    <br/>
    <h4>Result</h4>
    <div>
      <div class="mb-3">
        Win: {{winTimes}}, Lose: {{loseTimes}}, Result Assets: {{final}}
      </div>
      <b-table striped hover :items="result"></b-table>
    </div>
  </div>
</template>

<script>

export default {
  methods: {
    simulate(type) {
      console.log(6);
      let total = this.total;
      this.result = [];
      this.winTimes = 0;
      this.loseTimes = 0;
      
      for (let i = 0; i < this.playTimes; i++) {
        let betAmount;
        if (type == 1) {
          betAmount = this.betAmount * Math.pow(2, this.conter);
        } else {
          betAmount = this.betAmount * (1 + this.conter);
        }
        if (total < betAmount) {
          this.conter = 0;
          break;
        }
        const random = Math.floor(Math.random() * 100) + 1;
        if (random <= this.winRate) {
          total +=  betAmount * this.returnFactor;
          this.result.push({
            result: 'win',
            betAmount: betAmount,
            total: total
          });
          this.conter = 0;
          this.winTimes += 1;
        } else {
          total -= betAmount;
          this.result.push({
            result: 'lose',
            betAmount: betAmount,
            total: total
          });
          this.conter += 1;
          this.loseTimes += 1;
        }
      }
      this.final = total;
      console.log(this.result);
    }
  },
  data: () => {
    return {
      total: 10000,
      betAmount: 10,
      winRate: 49,
      returnFactor: 2,
      leaveThreshold: 0,
      playTimes: 1000,
      conter: 0,
      result: [],
      winTimes: 0,
      loseTimes: 0,
      final: 0,
    }
  }
}
</script>

<style>
  #condition-contain {
    width: 200px;
  }
</style>
