<template>
<div class="wrapper">

  <div class="filters" v-if="genders && paymentMethods && showFilters">
    <div class="heading">Filters</div>
    <label for="search bar">
      <span><i class="fas fa-search"></i></span>
      <input
        id="search"
        type="text"
        name=""
        @input="$emit('update:modelValue', $event.target.value)"
        placeholder="Search By Name"
      />
    </label>

    <div class="filter__type">
      <p class="title">Gender</p>
      <label for="removegender" @click="$emit('selectedGender', '')">
        <input id="removegender" type="radio" name="Gender" value="" />
        <span class="remove-filter"
          ><i class="fas fa-ban"></i> Remove Gender filter.</span
        >
      </label>

      <label
        :for="gender"
        v-for="(gender, i) in genders"
        :key="gender + i"
        @click="$emit('selectedGender', gender)"
      >
        <input :id="gender" type="radio" name="Gender" :value="gender" />
        <span>{{ gender }}</span>
      </label>
    </div>
    <div class="filter__type">
      <p class="title">Payment Method</p>
      <label
        for="removePaymentMethod"
        @click="$emit('selectedPaymentMethod', '')"
      >
        <input
          id="removePaymentMethod"
          type="radio"
          name="method Type"
          value=""
        />
        <span class="remove-filter"
          ><i class="fas fa-ban"></i> Remove PaymentMethod filter.</span
        >
      </label>
      <label
        :for="method"
        v-for="(method, i) in paymentMethods"
        :key="method + i"
        @click="$emit('selectedPaymentMethod', method)"
      >
        <input :id="method" type="radio" name="method Type" :value="method" />
        <span>{{ method }}</span>
      </label>
    </div>
  </div>
  <button @click="showFilters = !showFilters" class="toggleFilters">
    {{ showFilters ? "Hide" : "Show" }} filters
  </button>
</div>

</template>

<script>
import { ref } from "vue";
export default {
  props: ["genders", "paymentMethods"],
  setup() {
    const showFilters = ref(true);

    window.addEventListener('resize', (e) => {
        if(e.currentTarget.innerWidth >= 767) {
          showFilters.value = true
        }
    })  
    return { showFilters };
  },
};
</script>

<style>
.toggleFilters {
  background: #444;
  color: #fff;
  border: 0;
  padding: 8px;
  width: 150px;
  cursor: pointer;
  border: 2px solid transparent;
  text-transform: uppercase;
  margin-top: 20px;
  margin-bottom: 20px;
  display: none;
}

.toggleFilters:hover {
  border: 2px solid #444;
  background: transparent;
  color: #444;
}

.filters {
  padding: 0 10px;
}

.filters {
  display: flex;
  flex-direction: column;
}

.filters .heading {
  font-size: 26px;
  font-weight: 500;
}

.filter__type {
  width: fit-content;
  min-width: 280px;
  padding: 0 10px;
}

.filter__type .title {
  font-size: 12px;
  text-transform: uppercase;
  font-weight: 500;
}

.filter__type label {
  margin: 0 1.5em;
}

.filter__type label {
  position: relative;
  display: block;
  height: 32px;
  width: 100%;
  cursor: pointer;
}

.filter__type label input {
  all: unset;
}

label[for="search bar"] {
  background: #eaeeee;
  display: flex;
  align-items: center;
  padding-left: 10px;
  border-radius: 5px;
}

label input[type="text"] {
  background: transparent;
  border: 0;
  outline: 0;
  height: 42px;
  flex: 1;
  padding: 0 10px;
}

label[for="search bar"] span {
  padding: 4px 0;
}

.filter__type label span {
  position: absolute;
  display: flex;
  align-items: center;
  padding: 0 10px;
  top: 0;
  left: 0;
  height: 100%;
  width: 150px;
  border-radius: 5px;
}

label span.remove-filter {
  width: fit-content;
  color: red;
  font-size: 12px;
}

span.remove-filter:hover {
  background: red;
  color: #fff;
}

span.remove-filter i {
  margin-right: 4px;
}

label input:checked ~ span {
  background: rgb(223, 217, 217);
  color: #444;
}

label input:checked ~ span.remove-filter {
  background: transparent;
  color: #444;
}

@media (max-width: 1100px) {
  .filters {
    flex-direction: row;
    flex-wrap: wrap;
    align-items: baseline;
    justify-content: space-between;
  }

  .filters .heading {
    width: 100%;
  }

  .filters label[for="search bar"] {
    width: 100%;
  }
}

@media (max-width: 768px) {
  .filters {
    margin-bottom: 30px;
  }

  .filter__type {
    width: 100%;
  }

  .toggleFilters {
    display: inline-block;
  }
}
</style>