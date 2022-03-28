<template>
  <div id="details">
    <b-container fluid>
      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="cms-id">CMS ID</label>
        </b-col>
        <b-col sm="9" class="d-flex">
          <span> {{ details.udf.cms_id }} </span>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="name">Ingredient Name</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="name"
            size="sm"
            placeholder="Enter ingredient name"
            v-model="details.udf.name"
          ></b-form-input>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="shopping-list-name">Shopping List Name</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="lshopping-ist-name"
            placeholder="Enter shopping list name"
            v-model="details.udf.shopping_list_name"
          ></b-form-input>
        </b-col>
      </b-row>

      <hr />

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="directory">Directory</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="directory"
            placeholder="Enter Directory"
            v-model="details.udf.name"
          ></b-form-input>
        </b-col>
      </b-row>

      <b-row class="my-1" v-if="details.udf.grams_to_cups">
        <b-col sm="3" class="d-flex">
          <label for="grams-to-cups">Grams to Cups</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="grams-to-cups"
            placeholder="Enter Grams to Cups"
            type="number"
            :value="details.udf.grams_to_cups.toFixed(3)"
          ></b-form-input>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="grams-to-each">Grams to Each</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="grams-to-each"
            placeholder="Enter Grams to Each"
            v-model="details.udf.grams_to_each"
          ></b-form-input>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="hits">Hits</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="hits"
            placeholder="Enter Hits"
            v-model="details.udf.hits"
          ></b-form-input>
        </b-col>
      </b-row>

      <hr />

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="store-location">Store Location</label>
        </b-col>
        <b-col sm="9">
          <b-form-select
            id="store-location"
            class="form-control"
            v-model="details.udf.store_location"
            :options="storeOptions"
          ></b-form-select>
        </b-col>
      </b-row>

      <b-row class="my-1" v-if="details.udf.default_each">
        <b-col sm="3" class="d-flex">
          <label for="default-each">Default Each</label>
        </b-col>
        <b-col sm="9">
          <b-form-select
            id="default-each"
            class="form-control"
            v-model="details.udf.default_each.udf.name"
            :options="defaultEachOptions"
          ></b-form-select>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="unit-lower">Unit Lower</label>
        </b-col>
        <b-col sm="9">
          <b-form-select
            id="unit-lower"
            class="form-control"
            v-model="details.udf.unit_lower"
            :options="unitLowerOptions"
          ></b-form-select>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="unit-upper">Unit Upper</label>
        </b-col>
        <b-col sm="9">
          <b-form-select
            id="unit-upper"
            class="form-control"
            v-model="details.udf.unit_lowerr"
            :options="unitUpperOptions"
          ></b-form-select>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="flop">Flop</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="flop"
            size="sm"
            placeholder="Enter Flop"
            v-model="details.udf.flop"
          ></b-form-input>
        </b-col>
      </b-row>

      <b-row class="my-1" v-if="details.udf.usda">
        <b-col sm="3" class="d-flex">
          <label for="usda.$id.replace('data-','')">USDA</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="usda.$id.replace('data-','')"
            size="sm"
            placeholder="Enter USDA"
            :value="details.udf.usda.$id.replace('usda/', '')"
          ></b-form-input>
        </b-col>
      </b-row>

      <b-row class="my-1" v-if="details.udf.esha">
        <b-col sm="3" class="d-flex">
          <label for="esha">ESHA</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="esha"
            size="sm"
            placeholder="Enter ESHA"
            :value="details.udf.esha.$id.replace('usda/', '')"
          ></b-form-input>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="product-code">Product Code</label>
        </b-col>
        <b-col sm="9">
          <b-form-input
            id="product-code"
            size="sm"
            placeholder="Enter Product Code"
            v-model="details.udf.product_code"
          ></b-form-input>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="state">State of Matter Type</label>
        </b-col>
        <b-col sm="9">
          <div class="d-inline-flex">
            <b-form-radio value="liquid"> Liquid</b-form-radio>
            <b-form-radio value="solid"> Solid</b-form-radio>
            <b-form-radio value="null"> N/A</b-form-radio>
          </div>
        </b-col>
      </b-row>

      <b-row class="my-1">
        <b-col sm="3" class="d-flex">
          <label for="cooked">Cooked Type</label>
        </b-col>
        <b-col sm="9">
          <div class="d-inline-flex">
            <b-form-radio value="raw"> Raw</b-form-radio>
            <b-form-radio value="cooked"> Cooked</b-form-radio>
            <b-form-radio value="null"> Neither</b-form-radio>
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  name: "Details",
  components: {},
  props: ["details"],
  data() {
    return {
      storeOptions: [
        { value: null, text: "Select Store Location" },
        { value: "Additional Recipes", text: "Additional Recipes" },
        { value: "Baby Care", text: "Baby Care" },
        { value: "Herbs and Spices", text: "Herbs and Spices" },
      ],
      defaultEachOptions: [
        { value: null, text: "Select Default Each" },
        { value: "each", text: "each" },
        { value: "cup (g)", text: "cup (g)" },
        { value: "pinch", text: "pinch" },
      ],
      unitLowerOptions: [
        { value: null, text: "Select Unit Lower" },
        { value: "Imperial Volume", text: "Imperial Volume" },
        { value: "Imperial Weight", text: "Imperial Weight" },
        { value: "Each / Other", text: "Each / Other" },
      ],
      unitUpperOptions: [
        { value: null, text: "Select Unit Upper" },
        { value: "Imperial Volume", text: "Imperial Volume" },
        { value: "Imperial Weight", text: "Imperial Weight" },
        { value: "Each / Other", text: "Each / Other" },
      ],
    };
  },
};
</script>

<style>
#details {
  margin: 60px 30px 0 30px;
}
</style>