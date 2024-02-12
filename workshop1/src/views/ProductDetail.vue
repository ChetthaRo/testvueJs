<template>
  <div class="mt-10 ml-10 mr-10">
    <v-card height="300px" width="500px">
      <!-- <v-col cols="6">
        <v-img max-height="200" max-width="250" src="../assets/logo.png" />
      </v-col> -->
      <v-row v-for="form in items" :key="form.title" dense >
        <v-col
          style="color: #274472"
          cols="12"
          md="3"
          :class="
            form.isPrimary && 'primary--text font-weight-font-weight-regular mt-5 ml-5'
          "
        >
          {{ form.title }}
        </v-col>
        <v-col cols="12" md="7">
          <div v-if="form.type === 'image'">
            <v-img :src="form.description" height="200" width="200" cover />
          </div>
          <div v-else class="mt-5 ml-5">
            {{ form.description }}
          </div>
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      form: {
        nameproduct: "",
        price: "",
        amount: "",
        status: false,
      },
    };
  },
  computed: {
    items() {
      const { nameproduct, price, amount, status } = this.form;

      return [
        {
          title: "ชื่อสินค้า",
          description: nameproduct,
          isPrimary: true,
        },

        {
          title: "ราคา",
          description: `${price} บาท`,
          isPrimary: true,
        },

        {
          title: "จำนวนสต๊อกที่มีในร้าน",
          description: amount,
          isPrimary: true,
        },

        {
          title: "การใช้งาน",
          description: status ? "เปิดให้บริการ" : "ปิดบริการ",
          align: "center",
          isPrimary: true,
        },
        
      ];
    },
    productId() {
      return this.$route?.params?.productId;
    },
  },
  async mounted() {
    console.log("productId :", this.productId);

    await this.axios
      .get(`http://localhost:3000/api/product/${this.productId}`)
      .then((res) => {
        console.log("resProduct", res);
        this.form = {
          ...res.data.data,
        };
      })
      .catch((error) => {
        console.log("error:", error);
        this.Swal.fire({
          icon: " error",
          title: "เกิดข้อผิดพลาด",
          text: error.response?.data?.message,
        });
      });
  },
};
</script>

<style>
</style>