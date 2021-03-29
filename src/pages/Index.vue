<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input v-model="search" placeholder="Search" dark borderless>
        <template v-slot:before>
          <q-icon name="my_location" />
        </template>

        <template v-slot:append>
          <q-btn round dense flat icon="search" />
        </template>
      </q-input>
    </div>
    <div class="col text-white text-center">
      <div class="text-h4 text-weight-light">Virar</div>
      <div class="text-h6 text-weight-light">Rain</div>
      <div class="text-h1 text-weight-thin q-my-lg relative-position">
        <span>40</span>
        <span class="text-h4 relative-position degree">&deg;</span>
      </div>
    </div>
    <div class="col text-center">
      <img src="https://www.fillmurray.com/100/100" alt="Bill" />
    </div>
    <div class="cole skyline"></div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      search: "",
    };
  },
};
</script>

<style lang="scss">
.q-page {
  background: #00416a; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to left,
    #ffe000,
    #799f0c,
    #00416a
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to left,
    #ffe000,
    #799f0c,
    #00416a
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */ /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
.degree {
  top: -44px;
}
.skyline {
  flex: 0 0 100px;
  background: url(../../public/skyline.png);
  background-position: center bottom;
  background-size: contain;
}
</style>