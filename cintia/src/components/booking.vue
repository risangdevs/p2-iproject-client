<template>
  <!-- Booking Card -->
  <div class="container vh-90">
    <div
      class="d-flex justify-content-center"
      style="margin: 20px"
      v-for="booking in bookings"
      :key="booking.id"
    >
      <div class="card" style="width: 60rem; margin: 10px; padding: 20px">
        <div class="d-flex">
          <img :src="booking.movie.poster" alt="Card image cap" />
          <div class="text-justify pl-4 align-self-center">
            <a :href="'//' + booking.movie.imdbUrl" target="_blank">
              <h5 class="card-title">{{ booking.movie.title }}</h5></a
            >
            <p class="card-text">Genre: {{ booking.movie.genre }}</p>
            <p>Actors: {{ booking.movie.actors }}</p>
            <p>Plot: {{ booking.movie.plot }}</p>
            <a
              href="#"
              class="btn btn-danger"
              @click="deleteBooking(booking.movie.id)"
              >Cancel Booking</a
            ><br /><br />
            <a
              href="#"
              class="btn btn-primary"
              @click="payNow(booking.movie.id)"
              v-if="booking.isPaid === false"
              >Pay Now</a
            >
            <a
              href="#"
              class="btn btn-success"
              v-if="booking.isPaid === true"
              >Paid</a
            ><br />
            <a
              v-if="redirect&&!booking.isPaid"
              :href="redirect"
              target="_blank"
            >
              <h6 style="margin-top: 10px">
                Click this to continue payment
              </h6></a
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bookings: [],
      redirect: "",
    };
  },
  methods: {
    fetchBookings() {
      this.$store
        .dispatch("aFetchBookings")
        .then((res) => {
          this.bookings = res;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    deleteBooking(id) {
      this.$store
        .dispatch("aDelete", id)
        .then(() => {
          this.$router.push("/movies");
        })
        .catch((err) => {
          console.log(err);
        });
    },
    payNow(id) {
      this.$store
        .dispatch("aPayNow",id)
        .then((res) => {
          console.log(res);
          this.redirect = res.redirect_url;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    paidSuccess(id) {
      this.$store
        .dispatch("aPaidSuccess", id)
        .then(() => {
          this.redirect=""
          this.fetchBookings();
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  created() {
    this.fetchBookings();
  },
};
</script>

<style></style>
