<template>
  <div class="container">
    <div class="tables">
      <h3 class="page-title">Current Summary</h3>
      <p><em>Click on column names to sort data</em></p>

      <table class="table">
        <thead>
          <tr class="table-header">
            <th><a class="label" href="#">Name</a></th>
            <th>Domain</th>
            <th>Date of Breach</th>
            <th>Accounts Comprimised</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="breach of breaches">
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
export default {
  data() {
    return {
      breaches: [],
    }
  },
  async fetch() {
    this.breaches = await fetch(
      'https://haveibeenpwned.com/api/v3/breaches'
    ).then((res) => res.json())
  },
  sortBy: function (sortKey) {
    this.reverse = this.sortKey == sortKey ? !this.reverse : false

    this.sortKey = sortKey
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
}

.label:hover {
  color: #18a8a8;
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
