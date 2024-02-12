<template>
  <div>
    <v-app-bar app color="primary" dark clipped-left :elevation="1">
      <v-app-bar-nav-icon></v-app-bar-nav-icon>
      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>
    </v-app-bar>
    <v-navigation-drawer permanent app clipped flat color="primary">
      <v-list-item-group>
        <div v-for="(menuList, i) in items" :key="i">
          <v-list-item :to="menuList.to" router exact>
            <v-list-item-content>
              <v-list-item-title class="nav-title-text">{{
                menuList.title
              }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </div>
      </v-list-item-group>
    </v-navigation-drawer>
    <v-main>
      <router-view />
    </v-main>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      items: [
        {
          title: "สิ้นค้าทั้งหมด",
          to: "/products",
        },
        {
          title: "คำสั่งซื้อ",
          to: "/order-list",
        },

        {
          title: "ตั้งค่า",
          submenu: [
            { name: "ร้านค้า", path: "/setting-profile" },
            { name: "LALAMOVE เครดิต", path: "/credit" },
            { name: "เปลี่ยนรหัสผ่าน", path: "/setting-password" },
          ],
        },
      ],
    };
  },
};
</script>

<style></style>