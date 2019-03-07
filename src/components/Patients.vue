<template>
  <div class="hello">
    <p>teodoraaaa ftw!</p>
    <h3>Our patients</h3>
    <b-table
      hover
      :items="patients"
      :fields="fields"
      :current-page="currentPage"
      :per-page="perPage"
      @row-selected="rowSelected"
    >
      <template slot="show_details" slot-scope="row">
        <b-button size="sm" @click="row.toggleDetails" class="mr-2">
          {{ row.detailsShowing ? "Hide" : "Show" }} Details
        </b-button>
      </template>

      <template slot="row-details" slot-scope="row">
        <b-card>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"><b>Full name:</b></b-col>
            <b-col>{{ row.item.first_name }} {{ row.item.last_name }}</b-col>
          </b-row>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"><b>Email:</b></b-col>
            <b-col>{{ row.item.email }}</b-col>
          </b-row>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"><b>Gender:</b></b-col>
            <b-col>{{ row.item.gender }}</b-col>
          </b-row>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"><b>Diagnosis Code:</b></b-col>
            <b-col>{{ row.item.diagnosis_code }}</b-col>
          </b-row>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"
              ><b>Diagnosis Description:</b></b-col
            >
            <b-col>{{ row.item.diagnosis_description }}</b-col>
          </b-row>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"
              ><b>Diagnosis Detailed:</b></b-col
            >
            <b-col>{{ row.item.diagnosis_description_detailed }}</b-col>
          </b-row>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"
              ><b>Administered Treatment:</b></b-col
            >
            <b-col>{{ row.item.administered_drug_treatment }}</b-col>
          </b-row>

          <b-button size="sm" @click="row.toggleDetails">Hide Details</b-button>
        </b-card>
      </template>
    </b-table>
    <b-row>
      <b-col md="6" class="my-1">
        <b-pagination
          :total-rows="totalRows"
          :per-page="perPage"
          v-model="currentPage"
          class="my-0"
        />
      </b-col>
    </b-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Patients",
  props: {
    msg: String
  },
  data() {
    return {
      currentPage: 1,
      perPage: 5,
      selected: [],
      fields: [
        { key: "id", label: "ID", sortable: true },
        { key: "first_name", label: "First Name", sortable: true },
        { key: "last_name", label: "Last Name", sortable: true },
        { key: "email", label: "Email", sortable: true },
        "show_details"
      ],
      patients: null,
      totalRows: 6
    };
  },
  created() {
    axios
      .get(`https://alexgr.ro/ehealth/patients.json`)
      .then(response => {
        // JSON responses are automatically parsed.
        this.patients = response.data;
        this.totalRows = response.data.length;
      })
      .catch(e => {
        this.errors.push(e);
      });
  },
  methods: {
    rowSelected(items) {
      this.selected = items;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
