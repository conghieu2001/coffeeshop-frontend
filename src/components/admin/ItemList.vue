<script>
import ItemService from '../../services/item.service';
export default {
  props: {
    items: { type: Array, default: [] },
    activeIndex: { type: Number, default: -1 },
  },
  data() {},
  emits: ["update:activeIndex"],
  methods: {
    updateActiveIndex(index) {
      this.$emit("update:activeIndex", index);
    },
    async deleteItem(id) {
      if(confirm("Bạn muốn xóa Sản phẩm này?")) {
        try {
          await ItemService.delete(id);
          // this.$router.push({ name: "ShowAllPost" });
          location.reload();
        } catch(error) {
          console.log(error);
        }
      }
    }
  },
};
</script>
<template>
  <table class="table">
    <thead class="table-info">
      <tr>
        <th class="pl-2">STT</th>
        <!-- <th>ID</th> -->
        <th class="item-name-headd">Name</th>
        <th class="item-description-headd">Description</th>
        <th>Image</th>
        <th>Favorite</th>
        <th>Price</th>
        <!-- <th>Loại</th> -->
        <th class="text-center">Option</th>
      </tr>
    </thead>
    <tbody>
      <tr
      class="row-info-item"
        v-for="(item, index) in items"
        :key="item._id"
        :class="{ active: index === activeIndex }"
        @click="updateActiveIndex(index)"
      >
        <td class="pl-3">{{ index+1 }}</td>
        <td>{{ item.name }}</td>
        <td class="td-descriptionn">{{ item.description }}</td>
        <td class="td-imagee"><img :src="item.image" alt=""></td>
        <td v-if="item.favorite === true"><i class="fas fa-heart" style="color: #ee2035;"></i></td>
        <td v-else><i class="far fa-heart" style="color: #000000;"></i></td>
        <td>{{ item.price }}</td>
        <td class="option-click">
          <router-link
            :to="{
              name: 'item.edit',
              params: { id: item._id },
            }"
          >
            <button class="btn btn-warning">
              <i class="fas fa-edit"></i>
            </button>
          </router-link>
          <button
            type="button"
            class="btn btn-danger ml-2"
            @click="deleteItem(item._id)"
          >
            <i class="fas fa-trash"></i>
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style>
table {
  width: 80%;
}
.option-click {
  padding-right: 0px !important;
  text-align: center;
}
.fa-edit {
  color: #6d6c6c;
}
.td-imagee {
  width: 50px;
  height: 70px;
}
.td-imagee img {
  width: 100%;
  height: 100%;
}
.td-descriptionn{
  width: 300px !important;
  display: -webkit-box;
  max-height: 4rem;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: normal;
  -webkit-line-clamp: 2;
  line-height: 1.5rem;
}
.item-name-headd {
  width: 150px;
}
.item-description-headd {
  width: 300px;
}
.fa-heart {
  font-size: 22px;
  padding-top: 10px;
  padding-left: 15px;
}
.row-info-item {
  height: 73px;
}
</style>
