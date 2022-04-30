<template>
  <transition name="modal">
    <div class="modal is-active">
      <div class="modal-background"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Add Outcome</p>
          <button class="delete" aria-label="close" @click="close"></button>
        </header>
        <section class="modal-card-body">
          <form method="post" @submit.prevent="saveItem()" id="addOutcomeForm">
            <div class="md-layout">
              <label class="md-layout-item md-size-15 md-form-label">
                Outcome
              </label>
              <div class="md-layout-item">
                <md-field>
                  <md-input
                    name="outcome"

                    type="text"
                  ></md-input>
                </md-field>
                <span
                  class="help is-danger"
                  v-if="form.errors.has('outcome')"
                  v-text="form.errors.get('outcome')"
                ></span>
              </div>
            </div>
            <div class="md-layout">
              <label class="md-layout-item md-size-15 md-form-label">
                Status
              </label>
              <div class="md-layout-item">
                <md-field>
                  <select
                    class="select md-menu md-select"

                    name="status"
                    id="status"
                    @change="form.errors.clear('status')"
                  >
                    <option
                      v-for="(outcomeStatus, index) in outcomeStatuses"
                      :key="index"
                      :value="outcomeStatus.item"
                    >
                      {{ outcomeStatus.item }}
                    </option>
                  </select>
                </md-field>
                <span
                  class="help is-danger"
                  v-if="form.errors.has('status')"
                  v-text="form.errors.get('status')"
                ></span>
              </div>
            </div>

            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field>
                <label>Notes</label>
                <md-textarea v-model="form.notes"></md-textarea>
              </md-field>
            </div>
          </form>
        </section>
        <footer class="modal-card-foot">
          <button class="button is-success" type="submit" form="addOutcomeForm">
            <span>Save</span>
            <span class="icon is-small">
              <i class="far fa-save"></i>
            </span>
          </button>
          <button class="button" @click="close">Cancel</button>
        </footer>
      </div>
    </div>
  </transition>
</template>

<script>
import Form from "../../Forms.js";
import { mapFields } from "vuex-map-fields";

export default {
  name: "AddOutcomeModal",

  props: {
    client_id: "",
    editing: "",
  },

  created() {},

  mounted() {
    this.fetchData();
  },

  computed: {
    ...mapFields(["dropdowns"]),
  },

  data() {
    return {
      users: [],
      outcomeStatuses: [],

        client_id: this.props.client_id,
        category_id: this.$store.state.category_id,
        outcome: "",
        status: "",
        notes: "",
 
    };
  },
  components: {},

  methods: {
    close() {
      this.$emit("close");
    },
    saveItem() {
      return this.form
        .post("/api/outcomes")
        .then((response) => {
          this.close();
          console.log(response);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getOutcomeStatuses() {
      return axios.get("/api/getdropdown/14");
    },
    fetchData() {
      axios.all([this.getOutcomeStatuses()]).then((response) => {
        console.log("Data ", response);
        this.outcomeStatuses = response[0].data.items;
      });
    },
  },
};
</script>
