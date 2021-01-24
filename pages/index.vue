<template>
  <div class="container">
    <div class="tables">
      <h3 class="page-title">Current Summary</h3>
      <p><em>Click on column names to sort data</em></p>

      <table class="table">
        <thead>
          <tr class="table-header">
            <th scope="col" role="columnheader">
              <button class="label" @click="sortBy('Name')">Name</button>
            </th>
            <th scope="col" role="columnheader">
              <button class="label" @click="sortBy('Domain')">Domain</button>
            </th>
            <th scope="col" role="columnheader">
              <button class="label" @click="sortBy('BreachDate')">
                Date of Breach
              </button>
            </th>
            <th scope="col" role="columnheader">
              <button class="label" @click="sortBy('PwnCount')">
                Comprimised
              </button>
            </th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="breach of sortedList" :key="breach.Name">
            <td>{{ breach.Name }}</td>
            <td>{{ breach.Domain }}</td>
            <td>{{ breach.BreachDate }}</td>
            <td>{{ breach.PwnCount }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
/**
 * BreachModel from https://haveibeenpwned.com/API/v3#BreachModel
 * @typedef {Object} BreachDto
 * @property {string} Name - A Pascal-cased name representing the breach which is unique across all other breaches
 * @property {string} Title - A descriptive title for the breach suitable for displaying to end users.
 * @property {string} Domain - The domain of the primary website the breach occurred on.
 * @property {string} BreachDate - The date (with no time) the breach originally occurred on in ISO 8601 format.
 * @property {string} AddedDate - The date and time (precision to the minute) the breach was added to the system in ISO 8601 format.
 * @property {string} ModifiedDate - The date and time (precision to the minute) the breach was modified in ISO 8601 format.
 * @property {number} PwnCount - The total number of accounts loaded into the system.
 * @property {string} Description - Contains an overview of the breach represented in HTML markup.
 * @property {string[]} DataClasses - This attribute describes the nature of the data compromised in the breach and contains an alphabetically ordered string array of impacted data classes.
 * @property {boolean} IsVerified - Indicates that the breach is considered unverified.
 * @property {boolean} IsFabricated - Indicates that the breach is considered fabricated.
 * @property {boolean} IsSensitive - Indicates if the breach is considered sensitive.
 * @property {boolean} IsRetired - Indicates if the breach has been retired.
 * @property {boolean} IsSpamList - Indicates if the breach is considered a spam list.
 * @property {boolean} LogoPath - A URI that specifies where a logo for the breached service can be found. Logos are always in PNG format.
 */
export default {
  data() {
    return {
      /** @type {BreachDto[]} */
      breaches: [],
      sortDirection: 'ascending',
      sortKey: 'Name',
    }
  },
  async fetch() {
    this.breaches = await fetch(
      'https://haveibeenpwned.com/api/v3/breaches'
    ).then((res) => res.json())
  },
  methods: {
    sortBy: function (sortKey) {
      if (this.sortKey === sortKey) {
        this.sortDirection =
          this.sortDirection === 'ascending' ? 'descending' : 'ascending'
      }

      this.sortKey = sortKey
    },
  },
  computed: {
    /** @returns {BreachDto[]} sorted list of Breaches */
    sortedList: function () {
      return this.breaches
        .slice(0)
        .sort((a, b) =>
          this.sortDirection === 'ascending'
            ? a[this.sortKey] > b[this.sortKey]
            : a[this.sortKey] < b[this.sortKey]
        )
    },
  },
}
</script>

<style>
body {
  margin: 0%;
  padding: 0%;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.page-title {
  margin-bottom: 3.5%;
}

.table-header {
  background-color: #526488;
  color: aliceblue;
}

/*For table header labels*/
.label {
  color: aliceblue;
  background-color: #526488;
  border: none;
  font: inherit;
}

.label:hover {
  color: #18a8a8;
  cursor: pointer;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
