<template>
  <div class="container" v-if="ALL_PROFILES.length > 0">
    <Filters
      v-model="patientName"
      :genders="genders"
      :paymentMethods="paymentMethods"
      @selectedGender="setGender"
      @selectedPaymentMethod="setMethod"
    />

    <div class="profiles-container">
      <div class="pagination">
        <button @click="goToPreviousPage">
          <i class="fas fa-angle-left"></i>
        </button>
        <!-- Show the first and last numbers on the page -->
        {{ profilesToDisplay[0]["S/N"] }} -
        {{ profilesToDisplay[profilesToDisplay.length - 1]["S/N"] }} of
        {{ ALL_PROFILES.length }}
        <button @click="goToNextPage">
          <i class="fas fa-angle-right"></i>
        </button>
      </div>

      <ProfileTable :filteredProfiles="filteredProfiles" />
    </div>
  </div>
</template>
<script>
import { computed, onMounted, ref, watch } from "vue";
import Filters from "./components/Filters.vue";
import ProfileTable from "./components/ProfileTable.vue";

export default {
  name: "App",
  components: { Filters, ProfileTable },
  setup() {
    let ALL_PROFILES = ref([]);
    let profilesToDisplay = ref([]);
    let genders = ref([]);
    let paymentMethods = ref([]);
    let totalNumberOfPages = ref(Number);
    let currentPage = ref(Number);
    let itemsPerPage = 20;
    let selectedGender = ref("");
    let selectedPaymentMethod = ref("");
    let patientName = ref("");
    let setGender = function (data) {
      selectedGender.value = data;
    };
    let setMethod = function (data) {
      selectedPaymentMethod.value = data;
    };

    onMounted(() => {
      fetch("https://api.enye.tech/v1/challenge/records")
        .then((res) => res.json())
        .then((data) => {
          ALL_PROFILES.value = data.records.profiles;
          ALL_PROFILES.value = ALL_PROFILES.value.map((pro, i) => {
            return {
              ...pro,
              "S/N": i + 1,
              fullName: pro.FirstName + " " + pro.LastName,
            };
          });
          genders.value = new Set(
            ALL_PROFILES.value.map((profile) => profile.Gender)
          );
          paymentMethods.value = new Set(
            ALL_PROFILES.value.map((profile) => profile.PaymentMethod)
          );
          totalNumberOfPages.value = Math.ceil(
            ALL_PROFILES.value.length / itemsPerPage
          );
          currentPage.value = 1;
        });
    });

    watch(currentPage, () => {
      profilesToDisplay.value = ALL_PROFILES.value.slice(
        (currentPage.value - 1) * itemsPerPage,
        (currentPage.value - 1) * itemsPerPage + itemsPerPage
      );
    });

    const filterByGender = (profiles) => {
      return profiles.filter(
        (pro) => !pro.Gender.indexOf(selectedGender.value)
      );
    };
    const filterByPaymentMethod = (profiles) => {
      return profiles.filter(
        (pro) => !pro.PaymentMethod.indexOf(selectedPaymentMethod.value)
      );
    };

    const filterByName = (profiles) => {
      return profiles.filter((pro) =>
        pro.fullName.toLowerCase().includes(patientName.value.toLowerCase())
      );
    };

    const filteredProfiles = computed(() => {
      return filterByName(
        filterByGender(filterByPaymentMethod(profilesToDisplay.value))
      );
    });

    const goToNextPage = () => {
      if (currentPage.value < totalNumberOfPages.value) {
        currentPage.value++;
      }
    };

    const goToPreviousPage = () => {
      if (currentPage.value >= 2) {
        currentPage.value--;
      }
    };

    return {
      ALL_PROFILES,
      profilesToDisplay,
      genders,
      paymentMethods,
      goToNextPage,
      goToPreviousPage,
      patientName,
      filteredProfiles,
      setGender,
      setMethod,
    };
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@200;300;400;500&display=swap');
@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.2/css/all.min.css");

* {
  box-sizing: border-box;
}
#app {
 font-family: 'Montserrat', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #081414;
  margin-top: 0px;
}

button,
input {
  font-family: inherit;
}

.container {
  width: 100%;
  display: grid;
  grid-template-columns: auto 1fr;
  overflow: hidden;
}

.pagination {
  text-align: right;
}

.pagination button {
  height: 32px;
  width: 32px;
  border-radius: 50%;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  background: transparent;
  font-size: 16px;
  border: 0;
  color: #737373;
}

.pagination button:hover {
  background: rgba(50, 50, 50, 0.1);
  cursor: pointer;
}

.bold {
  font-weight: 500;
}

@media (max-width: 1100px) {
  .container {
  display: block;
  }
}

@media (max-width: 768px) {
  .pagination {
    text-align: center;
    margin-bottom: 5px;
  }
}
</style>
