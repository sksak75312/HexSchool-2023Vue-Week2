<template>
  <div
    class="modal fade"
    id="adminProduct"
    data-bs-backdrop="static"
    data-bs-keyboard="false"
    tabindex="-1"
    aria-labelledby="staticBackdropLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="staticBackdropLabel">{{ curProduct.title }}</h1>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <img :src="curProduct.imageUrl" class="card-img-top mb-3" :alt="curProduct.title" />
          <p>產品類別: {{ curProduct.category }}</p>
          <p>產品描述: {{ curProduct.description }}</p>
          <p>
            售價: <del>{{ curProduct.origin_price }}</del> {{ curProduct.price }} /
            {{ curProduct.unit }}
          </p>
          <div class="row g-3">
            <div class="col-4" v-for="image in curProduct.imagesUrl" :key="image">
              <img :src="image" class="img-fluid object-fit-cover" :alt="image" />
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">關閉</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['curProduct']
}
</script>
