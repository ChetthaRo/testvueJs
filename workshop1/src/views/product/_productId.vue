<template>
  <v-row>
    <v-container>
      <v-form ref="form" @submit.prevent="formValidate">
        <div class="pl-6 cs-title-page mb-4">{{ Nani }}</div>
        <v-col cols="12">
          <v-card class="pa-8" style="color: #274472">
            <v-row>
              <v-col cs-title cols="2" class="align-self-center"
                >ชื่อสินค้า</v-col
              >
              <v-col cols="5">
                <v-text-field
                  id="formName"
                  v-model="form.nameproduct"
                  :rules="nameRules"
                  outlined
                  hide-details
                  dense
                >
                </v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="2" class="align-self-center">ราคา</v-col>
              <v-col cols="5">
                <v-text-field
                  id="formPrice"
                  v-model="form.price"
                  :rules="priceRules"
                  outlined
                  hide-details
                  dense
                  suffix="บาท"
                >
                </v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="2" class="align-self-center"
                >จำนวนสต๊อกที่มีในร้าน</v-col
              >
              <v-col cols="5">
                <v-text-field
                  id="formAmount"
                  v-model="form.amount"
                  :rules="amountRules"
                  outlined
                  hide-details
                  dense
                >
                </v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col cols="2" class="align-self-center">เปิดใช้งาน</v-col>
              <v-col>
                <v-switch
                  id="formStatus"
                  v-model="form.status"
                  inset
                  :label="`${form.status ? 'เปิดการใช้งาน' : 'ปิดการใช้งาน'}`"
                >
                </v-switch>
              </v-col>
            </v-row>
            <v-row justify="center">
              <v-col cols="1">
                <v-btn class="pd-4" block @click="changePage">ยกเลิก</v-btn>
              </v-col>
              <v-col cols="1">
                <v-btn color="#274472" dark block type="submit">บันทึก</v-btn>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-form>
    </v-container>
  </v-row>
</template>
  
  <script>
import Swal from "sweetalert2";
export default {
  data() {
    return {
      Nani: "แก้ไขสินค้า",
      switch1: true,
      form: {
        nameproduct: "",
        price: "",
        amount: "",
        status: false,
      },
      nameRules: [(v) => !!v || "กรุณากรอกข้อมูล"],
      priceRules: [
        (v) => !!v || "กรุณากรอกข้อมูล",
        (v) => /^[0-9]*$/.test(v) || "กรุณากรอกข้อมูลให้ถูกต้อง",
      ],
      amountRules: [
        (v) => !!v || "กรุณากรอกข้อมูล",
        (v) => /^[0-9]*$/.test(v) || "กรุณากรอกข้อมูลให้ถูกต้อง",
      ],
      productWeightRules: [
        (v) => !!v || "กรุณากรอกข้อมูล",
        (v) => /^[0-9.]*$/.test(v) || "กรุณากรอกข้อมูลให้ถูกต้อง",
      ],
    };
  },

  computed: {
    productId() {
      return this.$route?.params?.productId;
    },
  },

  async mounted() {
    console.log("productId :", this.productId);
    if (this.productId === "create") {
      this.Nani = `สร้างสินค้าใหม่`;
      return;
    }

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
        Swal.fire({
          icon: " error",
          title: "เกิดข้อผิดพลาด",
          text: error.response?.data?.message,
        });
      });
  },

  methods: {
    changePage() {
      this.$router.push({ name: "products" });
    },

    async formValidate() {
      console.log("productId", this.productId);

      if (this.$refs.form.validate()) {
        console.log("uptodate");
        try {
          await this.axios.put("http://localhost:3000/api/update/product/" + this.productId, this.form)
            .then((res) => {
              console.log(res);
              Swal.fire({
                title: "Update Product Success !",
                icon: "success",
              }).then((result) => {
                if (result.isConfirmed) {
                  this.$router.push("products");
                }
              });
            });
        } catch (error) {
          console.log(error);
          alert("พัง");
        }
      }
    },
  },
};
</script>
  
  <style lang="scss" scoped>
//   @import url('~/assets/main.scss');

.button-border-wrap {
  border: 1px dashed black;
  border-radius: 12px;
  flex-direction: column;
  cursor: pointer;
}

.preview-image {
  width: 210px !important;
  height: 155px !important;
  padding: 80px !important;
}

.test {
  border: 2px dashed #979595;
  border-radius: 5px;
}
</style>
  