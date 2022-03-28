<template>
  <div id="alt-name">
    <b-container fluid>
      <b-row class="my-1">
        {{ altNames.msg }}
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  name: "AltName",
  components: {},
  props: ["altNames"],
};
</script>

<style>
#alt-ame {
  margin: 60px 30px 0 30px;
}
</style>
