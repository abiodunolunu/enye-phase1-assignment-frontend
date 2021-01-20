<template>
<div class="table__wrapper">
  <table cellspacing="0">
    <thead>
      <tr>
        <td>S/N</td>
        <td>Full name</td>
        <td>Email</td>
        <td>Gender</td>
        <td>Credit Card No</td>
        <td>Payment Method</td>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(profile, i) in filteredProfiles" :key="i">
        <td>{{ profile["S/N"] }}</td>
        <td>{{ profile.fullName }}</td>
        <td>
          <a :href="`mailto:${profile.Email}`">{{
            profile.Email.toLowerCase()
          }}</a>
        </td>
        <td>{{ profile.Gender }}</td>
        <td>{{ profile.CreditCardNumber }}</td>
        <td>{{ profile.PaymentMethod }}</td>
      </tr>
      <tr v-if="filteredProfiles.length <= 0">
        <td>n/A</td>
        <td>n/A</td>
        <td>n/A</td>
        <td>n/A</td>
        <td>n/A</td>
        <td>n/A</td>
      </tr>
    </tbody>
  </table>
  </div>
</template>

<script>
export default {
  props: ["filteredProfiles"],
};
</script>

<style>
.table__wrapper {
    width: 100%;
    overflow-x: auto;
}

table {
  width: 100%;
  font-size: 14px;
  font-weight: 400;
  text-align: center;
}

table thead {
  background: #eaeeee;
  font-weight: 500;
}
thead {
  text-transform: uppercase;
  color: #737373;
}

table td {
  padding: 10px 5px;
  width: fit-content;
}


table td a {
    text-decoration: none;
    color: #5661CF;
}

tbody tr:nth-of-type(even) {
  background: #f7fdfd;
}

</style>