<template>
  <q-page class="flex column" :class="bgClass">
    <div class="col q-pt-lg q-px-md">
      <q-input
        v-model="search"
        placeholder="Search"
        dark
        borderless
        @keyup.enter="getWetherBySearch"
      >
        <template v-slot:before>
          <q-icon name="my_location" @click="getLocation" />
        </template>

        <template v-slot:append>
          <q-btn round dense flat icon="search" @click="getWetherBySearch" />
        </template>
      </q-input>
    </div>

    <template v-if="wetherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">{{ wetherData.name }}</div>
        <div class="text-h6 text-weight-light">
          {{ wetherData.weather[0].main }}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{ Math.round(wetherData.main.temp) }}</span>
          <span class="text-h4 relative-position degree">&deg;C</span>
        </div>
      </div>

      <div class="col text-center">
        <img
          :src="`http://openweathermap.org/img/wn/${wetherData.weather[0].icon}@2x.png`"
        />
      </div>
    </template>

    <template v-else>
      <div class="col column text-center tex-whhite">
        <div class="col text-h2 text-weight-thin">
          Quasar <br />
          Wether
        </div>
        <q-btn class="col" flat @click="getLocation">
          <q-icon left size="3em" name="my_location" />
          <div>Find My Location</div>
        </q-btn>
      </div>
    </template>

    <div class="cole skyline"></div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      search: "",
      wetherData: null,
      lat: null,
      long: null,
      apiUrl: "https://api.openweathermap.org/data/2.5/weather",
      apiKey: "b038c215e2a6373388935bfa6d7c3a76",
    };
  },
  computed: {
    bgClass() {
      if (this.wetherData) {
        if (this.wetherData.weather[0].icon.endsWith("n")) {
          return "bg-night";
        } else {
          return "bg-day";
        }
      }
    },
  },
  methods: {
    getLocation() {
      this.$q.loading.show();
      if (this.$q.platform.is.electron) {
        console.log('this.$q.platform.is.electron', this.$q.platform.is.electron)
        this.$axios(`https://freegeoip.app/json/`).then((response)=>{
          console.log('Response :', response);
          this.lat = response.data.latitude;
          this.long = response.data.longitude;
          this.getWetherByCoords();
        })
      } else {
        navigator.geolocation.getCurrentPosition((position) => {
          console.log("postion :", position);
          this.lat = position.coords.latitude;
          this.long = position.coords.longitude;
          this.getWetherByCoords();
        });
      }
    },
    getWetherByCoords() {
      this.$q.loading.show();
      this.$axios(
        `${this.apiUrl}?lat=${this.lat}&lon=${this.long}&appid=${this.apiKey}&units=Metric`
      ).then((response) => {
        console.log("response :", response);
        this.wetherData = response.data;
        this.$q.loading.hide();
      });
    },
    getWetherBySearch() {
      this.$q.loading.show();
      this.$axios(
        `${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=Metric`
      ).then((response) => {
        console.log("response :", response);
        this.wetherData = response.data;
        this.$q.loading.hide();
      });
    },
  },
};
</script>

<style lang="scss">
.q-page {
  background: linear-gradient(to bottom, #136a8a, #267871);
  &.bg-day {
    background: linear-gradient(to bottom, #00b4db, #0083b0);
  }
  &.bg-night {
    background: linear-gradient(to bottom, #232526, #414385);
  }
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