<template>
  <div id="app">
    <div class="container-fluid">
      <div class="card">
        <div class="card-body">
          <div class="table-responsive">
            <table class="table table-borderless table-hover">
              <thead class="thead-dark">
                <tr>
                  <th @click="sort('number')">
                    <i v-show="show('number', 1)" class="fas fa-chevron-up"></i>
                    <i v-show="show('number', -1)" class="fas fa-chevron-down"></i> Number
                  </th>
                  <th @click="sort('player')">
                    <i v-show="show('player', 1)" class="fas fa-chevron-up"></i>
                    <i v-show="show('player', -1)" class="fas fa-chevron-down"></i> Player
                  </th>
                  <th @click="sort('position')">
                    <i v-show="show('position', 1)" class="fas fa-chevron-up"></i>
                    <i v-show="show('position', -1)" class="fas fa-chevron-down"></i> Position
                  </th>
                  <th @click="sort('height')">
                    <i v-show="show('height', 1)" class="fas fa-chevron-up"></i>
                    <i v-show="show('height', -1)" class="fas fa-chevron-down"></i> Height
                  </th>
                  <th @click="sort('weight')">
                    <i v-show="show('weight', 1)" class="fas fa-chevron-up"></i>
                    <i v-show="show('weight', -1)" class="fas fa-chevron-down"></i> Weight
                  </th>
                  <th @click="sort('dob')">
                    <i v-show="show('dob', 1)" class="fas fa-chevron-up"></i>
                    <i v-show="show('dob', -1)" class="fas fa-chevron-down"></i> Date of Birth
                  </th>
                  <th @click="sort('nationality')">
                    <i v-show="show('nationality', 1)" class="fas fa-chevron-up"></i>
                    <i v-show="show('nationality', -1)" class="fas fa-chevron-down"></i> Nationality
                  </th>
                  <th @click="sort('years')">
                    <i v-show="show('years', 1)" class="fas fa-chevron-up"></i>
                    <i v-show="show('years', -1)" class="fas fa-chevron-down"></i> Years Experience
                  </th>
                  <th @click="sort('college')">
                    <i v-show="show('college', 1)" class="fas fa-chevron-up"></i>
                    <i v-show="show('college', -1)" class="fas fa-chevron-down"></i> College
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="athlete in sortedAthletes" :key="athlete.player">
                  <td>{{athlete.number}}</td>
                  <td>{{athlete.player}}</td>
                  <td>{{athlete.position}}</td>
                  <td>{{athlete.height}}</td>
                  <td>{{athlete.weight}}</td>
                  <td>{{athlete.dob}}</td>
                  <td>{{athlete.nationality}}</td>
                  <td>{{athlete.years}}</td>
                  <td>{{athlete.college}}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import sixers from "./sixers.json";
import moment from 'moment';

export default {
  name: "App",
  data() {
    return {
      athletes: sixers,
      sortParam: "number",
      sortDirection: 1
    };
  },
  methods: {
    sort(sortParam) {
      if (sortParam === this.sortParam) {
        this.sortDirection = this.sortDirection * -1;
      }
      this.sortParam = sortParam;
    },
    show(sortParam, sortDirection) {
      return (
        sortParam === this.sortParam && sortDirection === this.sortDirection
      );
    }
  },
  computed: {
    sortedAthletes() {
      return this.athletes.sort((a, b) => {
        let sortValueA = a[this.sortParam];
        let sortValueB = b[this.sortParam];
        // handle rookie as 0 years
        if (this.sortParam === 'years') {
          if (sortValueA === 'R') {
            sortValueA = 0;
          }
          if (b[this.sortParam] === 'R') {
            sortValueB = 0;
          }
        }
        // covert dates
        if (this.sortParam === 'dob') {
          sortValueA = moment(sortValueA);
          sortValueB = moment(sortValueB);
        }
        if (sortValueA < sortValueB) {
          return -1 * this.sortDirection;
        }
        if (sortValueA > sortValueB) {
          return 1 * this.sortDirection;
        }
        return 0;
      });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 4rem
}
.card {
  border-radius: 1rem;
}
</style>
