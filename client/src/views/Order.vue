<template>
  <div class="main d-flex flex-column mb-5">
    <div class="content-title">
      <h1 class="mt-3">Orders</h1>
      <hr style="width: 80%;" />
      <div class="d-flex flex-column">
        <div class="d-flex justify-content-around align-items-center mb-4">
          <h5 class="m-0">Receipt</h5>
          <h5 class="m-0">Status</h5>
          <h5 class="m-0">Option</h5>
        </div>
        <div>
          <OrderCard v-for="list in lists" :key="list._id" :list="list" @pay="getTransaction()"></OrderCard>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
const url = `http://34.87.3.189`;
import OrderCard from "../components/OrderCart";
export default {
  data() {
    return {
      lists: []
    };
  },
  components: {
    OrderCard
  },
  methods: {
    getTransaction() {
      const token = localStorage.getItem("token");
      axios({
        url: `${url}/transactions`,
        method: "get",
        headers: {
          token
        }
      })
        .then(({ data }) => {
          this.lists = data;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  created() {
    this.getTransaction();
  }
};
</script>

<style scoped>
</style>