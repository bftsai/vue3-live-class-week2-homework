<template>
    <div class="container">
      <div class="row py-3">
        <div class="col-md-6">
          <h2>產品列表</h2>
          <table class="table table-hover mt-4">
            <thead>
              <tr>
                <th width="150">產品名稱</th>
                <th width="120">原價</th>
                <th width="120">售價</th>
                <th width="150">是否啟用</th>
                <th width="120">查看細節</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="product in products" :key="product.id">
                <td width="150">{{ product.title }}</td>
                <td width="120">{{ product.origin_price }}</td>
                <td width="120">{{ product.price }}</td>
                <td width="150">
                  <span :class="{ 'text-success': product.is_enabled }">{{
                    product.is_enabled ? '啟用' : '未啟用'
                  }}</span>
                </td>
                <td width="120">
                  <button
                    type="button"
                    class="btn btn-primary-500 text-light fw-bold"
                    @click="showProductDetails"
                  >
                    查看細節
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
          <p>
            目前有
            <span>{{ products ? 0 : products.length }}</span> 項產品
          </p>
        </div>
        <div class="col-md-6">
          <h2>單一產品細節</h2>
          <template>
            <div class="card mb-3">
              <img src="" class="card-img-top primary-image" alt="主圖" />
              <div class="card-body">
                <h5 class="card-title">
                  {{}}
                  <span class="badge bg-primary ms-2">{{}}</span>
                </h5>
                <p class="card-text">商品描述：{{}}</p>
                <p class="card-text">商品內容：{{}}</p>
                <div class="d-flex">
                  <p class="card-text me-2">{{}}</p>
                  <p class="card-text text-secondary">
                    <del>{{}}</del>
                  </p>
                  元 / {{}}
                </div>
              </div>
            </div>
            <template>
              <img src="" alt="" class="images m-2" />
            </template>
          </template>
          <p class="text-secondary">請選擇一個商品查看</p>
        </div>
      </div>
    </div>
  </template>
  <script>
  import axios from 'axios';
  const { VITE_API, VITE_PATH } = import.meta.env;
  
  export default {
    data() {
      return {
        text: '測試',
        products: [],
      };
    },
    methods: {
      getProducts() {
        console.log('this.getProducts');
        const url = `${VITE_API}v2/api/${VITE_PATH}/admin/products`;
        const token=document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/,"$1",);
        axios.defaults.headers.common['Authorization'] = token;
        axios
          .get(url)
          .then((res) => {
            console.log(res.data.products);
            this.products = res.data.products;
            console.log('成功取得', this.products);
          })
          .catch((err) => {
            console.log(err);
          });
      },
      showProductDetails() {
        console.log('查看細節');
      },
    },
    mounted() {
      this.getProducts();
    },
  };
  </script>