<template>
  <div>
    <h3>{{ details.udf.name }}</h3>

    <b-tabs content-class="mt-3">
      <b-tab title="Details" active>
        <Details :details="details" />
      </b-tab>

      <b-tab title="Nutrition">
        <Nutrition
          :fieldss="[
            'nutrient',
            { key: 'quantity', label: 'Value per 50 g' },
            { key: 'quantityDouble', label: 'Value per 100 g' },
            'unit',
          ]"
          :itemss="nutritions"
        />
      </b-tab>

      <b-tab title="Alt. Names">
        <AltName :altNames="altNames" />
      </b-tab>

      <b-tab title="Measures">
        <Measure
          :fieldss="[
            { key: '$id', label: 'Default' },
            'id',
            'description',
            'grams',
            'each_to_grams_factor',
            'grams_to_cups_factor',
          ]"
          :itemss="measures"
        />
        <Table :table="nutritions"/>
      </b-tab>
    </b-tabs>
  </div>
</template>

<script>
// @ is an alias to /src
import Details from "@/views/Details.vue";
import Nutrition from "@/views/Nutrition.vue";
import Measure from "@/views/Measure.vue";
import AltName from "@/views/AltName.vue";

const apiUrl =
  "https://graph-test.meredithcorp.io/v3/ingredient-manager/ingredient_";
const ingredientId = 16421;
const followAll = "$i_udf.ingredient,udf/measures/measure";
const follow = "udf/default_each";
const followDeep = "udf,parent";

export default {
  name: "Ingredient",
  components: {
    Details,
    Nutrition,
    AltName,
    Measure,
  },
  data() {
    return {
      details: {},
      nutritions: {},
      altNames: {},
      measures: {},
      errorMsg: "",
    };
  },
  methods: {
    async getData() {
      try {
        const finalApiUrl =
          apiUrl + parseInt(this.$route.params.id) ?? ingredientId;
        const response = await this.$http.get(finalApiUrl, {
          params: {
            followAll: followAll,
            follow: follow,
            followDeep: followDeep,
          },
        });

        this.errorMsg = "No Result";

        if (response.data.udf) {
          this.details = response.data;

          // Nutrition Details

          const nutritions = response.data.udf.nutrition.nutrition;

          const returnNutritions = [];

          for (const key in nutritions) {
            let objNurtition = {};

            objNurtition.nutrient = nutritions[key].nutrient;
            objNurtition.quantity = nutritions[key].quantity.toFixed(3);
            objNurtition.quantityDouble = (
              nutritions[key].quantity * 2
            ).toFixed(3);
            objNurtition.unit = nutritions[key].unit;

            returnNutritions.push(objNurtition);
          }
          this.nutritions = returnNutritions;

          // Alt Names Details

          this.altNames = { msg: "Alt. Names" };

          // Measure Details

          const measures = response.data.udf.measures;

          const returnMeasures = [];

          for (const key in measures) {
            let objMeasure = {};

            objMeasure.$id = measures[key].measure.$id;
            objMeasure.id = measures[key].measure.udf.cms_id;
            objMeasure.description = measures[key].measure.udf.name;
            objMeasure.grams = measures[key].grams;
            objMeasure.each_to_grams_factor =
              measures[key].each_to_grams_factor;
            objMeasure.grams_to_cups_factor = measures[key].grams_to_cups_factor;

            console.log(typeof measures[key].grams_to_cups_factor);
            
            returnMeasures.push(objMeasure);
          }
          this.measures = returnMeasures;

          this.errorMsg = '';
        }
      } catch (error) {
        console.log(error);
      }
    },
  },

  created() {
    this.getData();
  },
};
</script>

