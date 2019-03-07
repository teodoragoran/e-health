<template>
  <div class="hello">
    <h3 class="appname">Medical records</h3>
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
          <b-row class="text-center">
            <b-col><img src="../assets/user.png" width="100px"/></b-col>
            <b-col cols="8">
              <b-col>
                <h4 class="username">
                  {{ row.item.first_name }} {{ row.item.last_name }}
                </h4></b-col
              >
              <b-col>{{ row.item.email }}</b-col>
              <b-col
                ><i>{{ row.item.gender }}</i></b-col
              >
            </b-col>
          </b-row>
          <b-col cols="12">
            <b-row> <h4>Diagnosis</h4> </b-row>
            <b-row class="mb-2">
              <b-col sm="3" class="text-sm-right"><b> Code:</b></b-col>
              <b-col class="text-sm-left">{{ row.item.diagnosis_code }}</b-col>
            </b-row>
            <b-row class="mb-2">
              <b-col sm="3" class="text-sm-right"><b>Description:</b></b-col>
              <b-col class="text-sm-left">{{
                row.item.diagnosis_description
              }}</b-col>
            </b-row>
            <b-row class="mb-2">
              <b-col sm="3" class="text-sm-right"><b>Detailes:</b></b-col>
              <b-col class="text-sm-left">{{
                row.item.diagnosis_description_detailed
              }}</b-col>
            </b-row>
            <b-row class="mb-2"> <h4>Treatment</h4> </b-row>
            <b-row>
              <b-col sm="3" class="text-sm-right"><b> Administered:</b></b-col>
              <b-col class="text-sm-left">{{
                row.item.administered_drug_treatment
              }}</b-col>
            </b-row>
          </b-col>

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
.username {
  color: #7e191b;
  margin-top: 20px;
}
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
.appname {
  margin-bottom: 5%;
  color: #7e191b;
  text-transform: uppercase;
}
</style>
