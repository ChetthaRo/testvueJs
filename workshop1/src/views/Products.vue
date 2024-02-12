<template>
  <div>
    <v-row class="mt-3 ml-3">
      <!-- <v-col cols="12">
        <v-btn color="success" @click="newItem()">newItem</v-btn>
    </v-col> -->
      <v-col cols="4" v-for="(item, index) in dataProducts" :key="index">
        <v-card @click="addProductToOrder(item)">
          <!-- <v-img height="250" src="../assets/rooney.jpg" /> -->
          <v-card-title>
            <span>Name Product : </span>{{ item.nameproduct }}
          </v-card-title>
          <v-card-subtitle>
            <span>Amount : </span>{{ item.amount }}
          </v-card-subtitle>
          <v-card-title> <span>Price : </span> {{ item.price }}</v-card-title>
          <v-spacer></v-spacer>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="success" @click="editItem(item)">Edit</v-btn>
            <v-btn color="error" @click="deleteData(item)">Delete</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import Swal from "sweetalert2";
export default {
  data: () => {
    return {
      dataProducts: [],
    };
  },
  async mounted() {
    await this.fetchProduct();
  },
  methods: {
    async fetchProduct() {
      this.loading = true;

      await this.axios
        .get("http://localhost:3000/api/product")
        .then((res) => {
          console.log("res:", res);
          this.dataProducts = res.data.data;
          console.log("dataProducts", this.dataProducts);
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => {
          this.loading = false;
        });
    },
    addProductToOrder(item) {
      console.log(item);
      this.$router.push({
        name: "productdetal",
        params: {
          productId: item._id,
        },
      });
    },
    editItem(item) {
      console.log("item:", item);
      this.$router.push({
        name: "editproduct",
        params: {
          productId: item._id,
        },
      });
    },
    async deleteData(item) {
      if (confirm("ยืนยันการลบ")) {
        try {
          await this.axios
            .delete("http://localhost:3000/api/delete/product/" + item._id)
            .then((res) => {
              console.log(res);

              Swal.fire({
                title: "Delete Product Success !",
                icon: "success",
              }).then((result) => {
                if (result.isConfirmed) {
                  this.fetchProduct();
                }
              });
            });
        } catch (error) {
          alert("พัง");
        }
      } else console.log("cancel");
    },
  },
};
</script>

<style>
</style>