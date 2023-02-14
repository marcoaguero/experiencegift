<script lang="ts">
import { ref, computed } from "vue";
import destinations from "./data/destinations.js";
import Datepicker from "@vuepic/vue-datepicker";
import Guests from "./components/Guests.vue";
import "@vuepic/vue-datepicker/dist/main.css";

export default {
  components: { Datepicker, Guests },
  setup() {
    let searchTerm = ref("");
    const searchDestinations = computed(() => {
      if (searchTerm.value === "") {
        return [];
      }
      let matches = 0;
      return destinations.filter((destination) => {
        if (
          destination.name
            .toLowerCase()
            .startsWith(searchTerm.value.toLowerCase()) &&
          matches < 10
        ) {
          matches++;
          return destination;
        }
      });
    });
    const selectDestination = (a, b) => {
      selectedDestination.value = a;
      searchTerm.value = a + " - " + b;
      // console.log(a + " - " + b);
    };
    let selectedDestination = ref("");
    const date = ref();
    const dateRange = new Date();
    const minDate = new Date(dateRange.setDate(dateRange.getDate() + 2));
    const maxDate = new Date(dateRange.setDate(dateRange.getDate() + 365));
    const adults = ref(1);
    const children = ref(0);
    const rooms = ref(1);

    const incrementAdults = () => {
      return adults.value++;
    };
    const decrementAdults = () => {
      return adults.value--;
    };
    const incrementChildren = () => {
      return children.value++;
    };
    const decrementChildren = () => {
      return children.value--;
    };
    const incrementRooms = () => {
      return rooms.value++;
    };
    const decrementRooms = () => {
      return rooms.value--;
    };
    const showInfo = ref(false);
    const toggleShowInfo = () => {
      showInfo.value = !showInfo.value;
      // console.log(showInfo.value);
    };

    const bookingInfo = {
      destinationInfo: String,
      dateCheckin: String,
      dateCheckout: String,
      adultsInfo: Number,
      childrenInfo: Number,
      roomInfo: Number,
    };
    const info = Object.create(bookingInfo);
    const generateObject = () => {
      info.destinationInfo = selectedDestination.value;
      info.dateCheckin = date._rawValue[0];
      info.dateCheckout = date._rawValue[1];
      info.adultsInfo = adults.value;
      info.childrenInfo = children.value;
      info.roomInfo = rooms.value;
      console.log(info);
    };

    return {
      destinations,
      searchTerm,
      searchDestinations,
      selectDestination,
      selectedDestination,
      dateRange,
      date,
      minDate,
      maxDate,
      adults,
      children,
      rooms,
      showInfo,
      bookingInfo,
      info,
      incrementAdults,
      decrementAdults,
      incrementChildren,
      decrementChildren,
      incrementRooms,
      decrementRooms,
      toggleShowInfo,
      generateObject,
    };
  },
};
</script>

<template>
  <div class="main-container">
    <div class="container" id="destination">
      <svg
        class="logo"
        xmlns="http://www.w3.org/2000/svg"
        width="21.234"
        height="18.344"
        viewBox="0 0 21.234 18.344"
      >
        <path
          id="icon-bed"
          d="M18.82,42.924v-5.9a2.18,2.18,0,0,0-2.177-2.177H4.592A2.18,2.18,0,0,0,2.415,37.02v5.9A3.157,3.157,0,0,0,0,45.988v4.4a.633.633,0,0,0,.633.633H2.415v1.53a.633.633,0,1,0,1.265,0v-1.53H17.554v1.53a.633.633,0,1,0,1.265,0v-1.53H20.6a.633.633,0,0,0,.633-.633v-4.4A3.156,3.156,0,0,0,18.82,42.924ZM3.68,37.02a.913.913,0,0,1,.912-.912h12.05a.913.913,0,0,1,.912.912v5.816h-2.09v-1a3,3,0,0,0-3-3h-3.7a3,3,0,0,0-3,3v1H3.68ZM14.2,41.839v1H7.036v-1a1.736,1.736,0,0,1,1.734-1.734h3.7A1.736,1.736,0,0,1,14.2,41.839ZM1.265,49.759V45.988A1.889,1.889,0,0,1,3.152,44.1H18.083a1.889,1.889,0,0,1,1.886,1.886v3.771Z"
          transform="translate(0 -34.843)"
          fill="#d78d00"
        />
      </svg>
      <input
        type="text"
        id="search"
        placeholder="Destination"
        v-model="searchTerm"
        class="box"
        style="flex: 1 1 auto; padding-left: 50px"
      />
    </div>
    <div class="container" id="datepicker">
      <Datepicker
        v-model="date"
        range
        max-range="30"
        :partial-range="false"
        :enable-time-picker="false"
        :min-date="minDate"
        :max-date="maxDate"
        placeholder="Select Date"
        class="box"
        style="flex: 0 0 auto"
      />
    </div>
    <div class="container" style="cursor: pointer" @click="toggleShowInfo">
      <svg
        class="logo"
        xmlns="http://www.w3.org/2000/svg"
        width="19"
        height="18.839"
        viewBox="0 0 19 18.839"
      >
        <path
          id="icon-passengers"
          d="M16.354,12.426a9.436,9.436,0,0,0-3.608-2.245,5.459,5.459,0,1,0-8.578-4.57A5.435,5.435,0,0,0,6.574,10.1,9.5,9.5,0,0,0,.16,19.08H1.6a8.017,8.017,0,1,1,16.034,0H19.16a9.837,9.837,0,0,0-2.806-6.654ZM9.62,9.62a4.008,4.008,0,1,1,4.008-4.008A4.008,4.008,0,0,1,9.62,9.62Z"
          transform="translate(-0.16 -0.241)"
          fill="#d78d00"
        />
      </svg>
      <div
        class="box"
        style="padding-left: 50px; display: flex; align-items: center"
      >
        {{ adults }} Adults - {{ children }} Children - {{ rooms }} Room
      </div>
    </div>
    <button
      type="button"
      style="cursor: pointer"
      class="searchbutton"
      @click="generateObject"
    >
      Search
    </button>
    <!-- <button>toggle</button> -->
  </div>

  <ul v-if="searchDestinations.length" class="destination-box">
    <li
      v-for="destination in searchDestinations"
      :key="destination.name"
      @click="selectDestination(destination.name, destination.countryName)"
    >
      {{ destination.name }} - {{ destination.countryName }}
    </li>
  </ul>
  <div>
    <Guests
      :adults="adults"
      :children="children"
      :rooms="rooms"
      @incrementAdults="incrementAdults"
      @decrementAdults="decrementAdults"
      @incrementChildren="incrementChildren"
      @decrementChildren="decrementChildren"
      @incrementRooms="incrementRooms"
      @decrementRooms="decrementRooms"
      v-show="showInfo == true"
    />
  </div>
</template>

<style scoped></style>
