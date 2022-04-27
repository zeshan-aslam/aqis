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
                    <form method="post" @submit.prevent="saveItem()" id="addOutcomeForm" @keydown="form.errors.clear($event.target.name)">

                        <div class="md-layout">
                        <label class="md-layout-item md-size-15 md-form-label">
                          Outcome
                        </label>
                        <div class="md-layout-item">
                          <md-field>
                            <md-input name="outcome" v-model="form.outcome" type="text"></md-input>
                          </md-field>
                          <span class="help is-danger" v-if="form.errors.has('outcome')"
                            v-text="form.errors.get('outcome')"></span>
                        </div>
                      </div>

                        <div class="md-layout">
                          <label class="md-layout-item md-size-15 md-form-label">
                          Status
                        </label>
                          <div class="md-layout-item">
                              <md-radio name="status" v-model="form.status" :value=1 @change="form.errors.clear('status')">Active</md-radio>
                              <md-radio name="status" v-model="form.status" :value=0 @change="form.errors.clear('status')">Inactive</md-radio>
                              <span class="help is-danger" v-if="form.errors.has('status')"
                            v-text="form.errors.get('status')"></span>
                          </div>
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

export default {
  name: "AddOutcomeModal",

  created() {
  },

  computed: {
  },

  data() {
    return {
      users: [],
      form: new Form({
        category_id: this.$store.state.category_id,
        outcome: '',
        status: '',
      }),
    };
  },
  components: {},

  methods: {
    close() {
      this.$emit("close");
    },
    saveItem() {
      return this.form.post('/api/category')
      .then (response => {
        this.close()
      })
      .catch (error => {
        console.log(error)
      })
    }
  }
};
</script>