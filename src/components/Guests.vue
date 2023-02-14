<script>
export default {
  props: ["adults", "rooms", "children"],
  methods: {
    incrementAdults() {
      this.$emit("incrementAdults");
    },
    decrementAdults() {
      this.$emit("decrementAdults");
    },
    incrementChildren() {
      this.$emit("incrementChildren");
    },
    decrementChildren() {
      this.$emit("decrementChildren");
    },
    incrementRooms() {
      this.$emit("incrementRooms");
    },
    decrementRooms() {
      this.$emit("decrementRooms");
    },
  },
};
</script>
<template>
  <div class="guests-box" id="guests">
    <ul>
      <li class="">
        <p>Adults {{ adults }}</p>
        <button @click="decrementAdults" :disabled="adults <= 1">-</button>
        <button @click="incrementAdults" :disabled="adults + children >= 9">
          +
        </button>
      </li>
      <li>
        <p>Children {{ children }}</p>
        <button @click="decrementChildren" :disabled="children <= 0">-</button>
        <button
          @click="incrementChildren"
          :disabled="
            this.children >= this.adults || this.children + this.adults >= 9
          "
        >
          +
        </button>
      </li>
      <li>
        <p>Rooms {{ rooms }}</p>
        <button @click="decrementRooms" :disabled="rooms <= 1">-</button>
        <button @click="incrementRooms" :disabled="rooms >= 4">+</button>
      </li>
    </ul>
  </div>
</template>
