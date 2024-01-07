<template>
  <div class="container mt-5">
    <h2>產品列表</h2>
    <table class="table table-bordered text-center align-middle">
      <thead>
        <tr>
          <th scope="col">產品名稱</th>
          <th scope="col">產品類別</th>
          <th scope="col">原價</th>
          <th scope="col">售價</th>
          <th scope="col">是否啟用</th>
          <th scope="col">查看細節</th>
          <th scope="col">刪除</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in productsList" :key="product.id">
          <th>{{ product.title }}</th>
          <td>{{ product.category }}</td>
          <td>{{ product.origin_price }}</td>
          <td>{{ product.price }}</td>
          <td>
            <span class="text-primary" v-if="product.is_enabled">啟用</span>
            <span class="text-danger" v-else>未啟用</span>
          </td>
          <td>
            <button
              type="button"
              class="btn btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#adminProduct"
              @click="curProduct = product"
            >
              查看詳情
            </button>
          </td>
          <td>
            <button type="button" class="btn btn-danger" @click="deleteProduct(product.id)">
              刪除
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <ProductModal :curProduct="curProduct"></ProductModal>
  </div>
</template>

<script>
import ProductModal from '../components/ProductModal.vue'
import axios from 'axios'
import Swel from 'sweetalert2'

const delCheckSwel2 = {
  title: '是否要刪除產品',
  icon: 'warning',
  showCancelButton: true,
  confirmButtonText: '確定',
  cancelButtonText: '取消',
  reverseButtons: true
}

const { VITE_API, VITE_PATH } = import.meta.env
export default {
  data() {
    return {
      productsList: [],
      curProduct: {}
    }
  },
  methods: {
    // 檢查使用者
    postCheckUser() {
      // 取出 token
      const token = document.cookie
        .split('; ')
        .find((row) => row.startsWith('week2homework'))
        ?.split('=')[1]
      axios.defaults.headers.common['Authorization'] = token
      this.$http
        .post(`${VITE_API}v2/api/user/check`, null)
        .then(() => {
          this.getProducts()
        })
        .catch(() => {
          this.$router.push('/')
        })
    },

    // 取得產品列表
    getProducts() {
      this.$http
        .get(`${VITE_API}v2/api/${VITE_PATH}/admin/products/all`)
        .then((res) => {
          this.productsList = res.data.products

        })
        .catch((err) => {
          console.log(err)
        })
    },

    // 產品刪除
    deleteProduct(id) {
      Swel.fire(delCheckSwel2).then((result) => {
        if (result.isConfirmed) {
          this.$http
            .delete(
              `${VITE_API}v2/api/${VITE_PATH}/admin/product/${id}`
            )
            .then((res) => {
              const { message } = res.data;
              this.getProducts()
              Swel.fire({
                title: message,
                icon: 'success'
              })
            })
            .catch((err) => {
              const { message } = err.response.data
              Swel.fire({
                title: message,
                icon: 'error'
              })
            })
        }
      })
    }
  },

  mounted() {
    this.postCheckUser()
  },

  components: {
    ProductModal
  }
}
</script>
