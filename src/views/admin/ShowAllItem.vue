<template>
  <main>
    <div class="pt-3 containner">
      <div class="row">
        <div class="col-3 left-content">
          <div class="info-admin d-flex align-items-baseline">
            <div class="pr-2">
              <i class="far fa-grin-hearts"></i>
            </div>
            <div>
              <h6>{{ this.UserName }}</h6>
            </div>
          </div>
          <hr />
          <ul>
            <li>
              <router-link to="/admin">
                <i class="fas fa-home"></i>
                <span>Dashboard</span>
              </router-link>
            </li>
            <li>
              <router-link to="/items">
                <i class="fas fa-list"></i>
                <span>Danh Mục Sản Phẩm</span>
              </router-link>
            </li>
            <li>
              <router-link to="/orders">
                <i class="fas fa-money-bill-wave"></i>
                <span>Đơn Hàng</span>
              </router-link>
            </li>
            <li>
              <router-link to="/posts">
                <i class="fas fa-folder-open"></i>
                <span>Bài Viết</span>
              </router-link>
            </li>
            <li>
              <router-link to="/recruitments">
                <i class="fas fa-user-plus"></i>
                <span>Tuyển Dụng</span>
              </router-link>
            </li>
            <li>
              <router-link to="/stores">
                <i class="fas fa-map"></i>
                <span>Cửa Hàng</span>
              </router-link>
            </li>
            <li>
              <router-link to="/comments">
                <i class="fa-solid fa-comment"></i>
                <span>Bình Luận</span>
              </router-link>
            </li>
          </ul>
        </div>
        <div class="col-8 right-contentt pt-4">
          <div class="content-right-items">
            <div class="homepage-titlee d-flex align-items-baseline mb-1">
              <h5>Sản Phẩm</h5>
              <InputSearch v-model="searchText" />
            </div>
            <hr />
            <div>
              <ProductCategory
                @select:coffee="getAllItemByCoffee"
                @select:all="getAllProduct"
                @select:hitea="getAllItemByHitea"
                @select:tea="getAllItemByTea"
                @select:favorite="getAllItemFavorite"
              ></ProductCategory>
              <ItemList
                class="mt-3"
                v-if="filteredItemsCount > 0"
                :items="filteredItems"
                v-model:activeIndex="activeIndex"
              />
              <p v-else>Không có sản phẩm nào.</p>
            </div>
          </div>
          <hr />
          <div class="footer text-center pb-4">
            2022 - 2023 &copy; Simple theme by <a href="/">CongHieu</a>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import InputSearch from "@/components/admin/InputSearch.vue";
import ItemList from "@/components/admin/ItemList.vue";
import ItemService from "@/services/item.service";
import ProductCategory from "../../components/admin/ProductCategory.vue";
export default {
  components: {
    InputSearch,
    ItemList,
    ProductCategory
  },
  data() {
    return {
      UserName: "",
      items: [],
      activeIndex: -1,
      searchText: "",
    };
  },
  watch: {
    searchText() {
      this.activeIndex = -1;
    },
  },
  computed: {
    itemStrings() {
      return this.items.map((item) => {
        const { name, description, image, price } = item;
        return [name, description, image, price].join("");
      });
    },
    filteredItems() {
      if (!this.searchText) return this.items;
      return this.items.filter((_item, index) =>
        this.itemStrings[index].includes(this.searchText)
      );
    },
    activeItem() {
      if (this.activeIndex < 0) return null;
      return this.filteredItems[this.activeIndex];
    },
    filteredItemsCount() {
      return this.filteredItems.length;
    },
  },
  methods: {
    getUserName() {
      const user = JSON.parse(localStorage.getItem("users"));
      if (localStorage.getItem("users") != null) {
        this.UserName = user.name;
      }
    },
    async retrieveItems() {
      try {
        this.items = await ItemService.getAll();
      } catch (error) {
        console.log(error);
      }
    },
    refreshList() {
      this.retrieveItems();
      this.activeIndex = -1;
    },
    async getAllProduct() {
      this.items = await ItemService.getAll();
    },
    async getAllItemByCoffee() {
      this.items = await ItemService.getAllItemByCoffee();
    },
    async getAllItemByHitea() {
      this.items = await ItemService.getAllItemByHitea();
    },
    async getAllItemByTea() {
      this.items = await ItemService.getAllItemByTea();
    },
    async getAllItemFavorite() {
      this.items = await ItemService.getFavorite();
    }
  },
  mounted() {
    this.refreshList();
    this.getUserName();
  },
  created() {
    this.getAllProduct();
  }
};
</script>
<style>
main {
  background-color: #f5f5f5;
  min-height: 640px;
}
.containner {
  padding-left: 100px;
}
.left-content {
  background-color: #fff;
  height: 590px;
  box-shadow: 20px -20px 60px #d9d9d9, -20px 20px 60px #ffffff;
}
.content-right {
  background-color: #fff;
  min-height: 590px;
  box-shadow: 20px -20px 60px #d9d9d9, -20px 20px 60px #ffffff;
}
.content-right {
  margin-left: 10px;
}
li {
  list-style: none;
  padding-top: 30px;
}
.info-admin {
  padding-top: 40px;
  /* padding-left: 60px; */
  justify-content: center;
  text-align: center;
}
li > a > i {
  margin-right: 15px;
}
li > a > span {
  color: #6e768e;
}
.fa-home,
.fa-list,
.fa-money-bill-wave,
.fa-folder-open,
.fa-user-plus,
.fa-comment,
.fa-map {
  color: #6e768e;
}
.homepage-title h5 {
  color: #6e768e;
}
.contentDoanhThu {
  display: flex;
  justify-content: space-around;
  height: 150px;
  /* text-align: center; */
  margin-top: 50px;
}
a {
  list-style: none;
  color: #000;
}

.showItem:hover,
.showOrders:hover,
.showSales:hover,
.showPosts:hover {
  color: #000;
  list-style: none;
  text-decoration: none;
  /* border-bottom: 1px solid #000;
      transition: width 2s ease-in-out; */
}
.showItem {
  border-left: 7px solid rgb(9, 9, 206);
  padding-left: 10px;
  padding-top: 10px;
}
.showItem span {
  font-size: 11px;
  color: rgb(9, 9, 206);
}
.showItem p {
  font-weight: bold;
  padding-left: 15px;
}
.showPosts {
  border-left: 7px solid rgb(6, 163, 56);
  padding-left: 10px;
  padding-top: 10px;
}
.showPosts span {
  font-size: 11px;
  color: rgb(6, 163, 56);
}
.showPosts p {
  font-weight: bold;
  padding-left: 15px;
}
.showOrders {
  border-left: 7px solid rgb(201, 182, 13);
  padding-left: 10px;
  padding-top: 10px;
}
.showOrders span {
  font-size: 11px;
  color: rgb(201, 182, 13);
}
.showOrders p {
  font-weight: bold;
  padding-left: 15px;
}
.showSales {
  border-left: 7px solid rgb(225, 19, 115);
  padding-left: 10px;
  padding-top: 10px;
}
.showSales span {
  font-size: 11px;
  color: rgb(225, 19, 115);
}
.showSales p {
  font-weight: bold;
  padding-left: 15px;
}
.kqSearch {
  height: 250px;
}
.textF {
  font-family: Tahoma, sans-serif;
  color: #4c4e54;
  font-size: 16px;
  padding-left: 5px;
}
.btn-2 {
  background: rgb(251, 33, 117);
  background: linear-gradient(
    0deg,
    rgba(251, 33, 117, 1) 0%,
    rgba(234, 76, 137, 1) 100%
  );
  border: none;
}
.btn-2:before {
  height: 0%;
  width: 2px;
}
.btn-2:hover {
  box-shadow: 4px 4px 6px 0 rgba(255, 255, 255, 0.5),
    -4px -4px 6px 0 rgba(236, 238, 241, 0.5),
    inset -4px -4px 6px 0 rgba(244, 218, 246, 0.2),
    inset 4px 4px 6px 0 rgba(233, 187, 233, 0.4);
}
.right-contentt {
  max-height: auto;
  background-color: #fff;
  /* height: 590px; */
  box-shadow: 20px -20px 60px #d9d9d9, -20px 20px 60px #ffffff;
  margin-left: 10px;
}
.content-right-items {
  min-height: 500px;
}
</style>

