<template>
  <div id="index">
    <b-container fluid>
      <b-row class="my-1">
        <h1>{{ msg }}</h1>
      </b-row>
      <b-row class="my-1">
        <form>
          <b-col sm="10" class="d-inline-flex">
            <input
              class="form-control"
              type="integer"
              required="required"
              placeholder="Enter Ingredient ID"
              aria-label="Search"
              ref="ingredient_id"
            />
          </b-col>
          <b-col sm="2" class="d-inline-flex">
            <b-button
              class="form-button"
              variant="primary"
              @click.prevent="searchIngredient()"
              :disabled="loading"
              ><b-icon icon="search"></b-icon> Search</b-button
            >

            <b-button class="form-button" type="reset" variant="danger"
              ><b-icon icon="bootstrap-reboot" variant="info"></b-icon>
              Reset</b-button
            >
          </b-col>
        </form>
      </b-row>
      <b-row class="my-1 search">
        <b-col sm="12">
          <div class="text-center" v-if="loading">
            <b-spinner variant="success"></b-spinner>
          </div>
          <div v-if="errorMsg" class="text-center alert alert-danger error-msg">
            {{ errorMsg }}
          </div>
          <div v-if="items.length > 0">
            <b-table
              striped
              hover
              :items="items"
              :fields="[{ key: 'id', lable: 'Ingredient ID' }, 'name']"
            >
              <template #cell(id)="data">
                <router-link :to="'/ingredient/' + data.value">{{
                  data.value
                }}</router-link>
              </template>
            </b-table>
          </div>
        </b-col>
      </b-row>
      <b-row class="my-1 ess-links">
        <h3>Essential Links</h3>
        <ul>
          <li>
            <a href="https://github.com" target="_blank" rel="noopener"
              >Github</a
            >
          </li>
        </ul>
      </b-row>
    </b-container>
  </div>
</template>

<script>
const apiUrl =
  "https://graph-test.meredithcorp.io/v3/ingredient-manager/ingredient_";
const followAll = "$i_udf.ingredient,udf/measures/measure";
const follow = "udf/default_each";
const followDeep = "udf,parent";

export default {
  name: "Index",
  props: {
    msg: String,
  },
  data: function () {
    return {
      loading: false,
      items: [],
      errorMsg: "",
    };
  },
  methods: {
    async searchIngredient() {
      const returnNutrition = [];
      const objNurtition = {};

      const searchKeyword = this.$refs.ingredient_id.value.trim();

      console.log("search keyword" + searchKeyword);

      if (!searchKeyword) {
        this.errorMsg = "Please Enter Ingredient ID";
      }

      if (searchKeyword) {
        const finalApiUrl = apiUrl.concat("", searchKeyword);

        try {
          this.loading = true;
          const response = await this.$http.get(finalApiUrl, {
            params: {
              followAll: followAll,
              follow: follow,
              followDeep: followDeep,
            },
          });

          this.loading = false;
          this.errorMsg = "No Result";

          if (response.data.udf) {
            objNurtition.id = response.data.udf.cms_id;
            objNurtition.name = response.data.udf.name;

            this.errorMsg = "";
          }
        } catch (error) {
          objNurtition.id = "";
          objNurtition.directory = "";

          this.loading = false;
          console.log(error);
          this.errorMsg = "Please Enter Correct Ingredient ID";
        }
      }

      returnNutrition.push(objNurtition);

      this.items = returnNutrition;
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.items = {};
      this.errorMsg = {};
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#index {
  margin: 60px 30px 0 0;
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
.search {
  margin-top: 30px;
}
.form-button {
  margin-left: 10px;
}
.error-msg {
  margin-top: 30px;
}
table {
  margin-top: 30px;
}
</style>
