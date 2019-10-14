<template>
  <section>
    <div class="container">
      <h1>Product List</h1>
      <router-link style="float: right" :to="'/admin/product/insert/'"><button class="btn btn-primary mb-5">Insert</button></router-link>
      <table class="table">
        <tr>
          <th scope="col">Nama Produk</th>
          <th scope="col">Brand</th>
          <th scope="col">Image</th>
          <th scope="col">Price</th>
          <th scope="col" colspan="2">Action</th>
        </tr>
        <tr v-for="(productRow, index) in products" :key="index">
          <td v-text="productRow.name"></td>
          <td v-text="productRow.brand"></td>
          <td><img :src="productRow.image" alt="" style="width: 300px;"></td>
          <td v-text="productRow.price"></td>
          <td><router-link :to="'/admin/product/update/'+productRow.id"><button class="btn btn-info">Edit</button></router-link></td>
          <td><button class="btn btn-danger" @click="deleteData(productRow.id)">Delete</button></td>
        </tr>
      </table>
    </div>
  </section>
</template>

<script>
export default {
  data () {
    return {
      products: []
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      var self = this
      this.$axios.get('http://localhost:3000/product/findAll').then(function (response) {
        self.products = response.data
      }).catch(function (error) {
        console.log(error)
      })
    },
    deleteData (id) {
      var self = this
      if (confirm('Are you sure want to delete data?')) {
        this.$axios.delete('http://localhost:3000/product/deleteOne/' + id).then(function (response) {
          if (response.status === 200) {
            alert('Berhasil hapus')
            self.getData()
          }
        }).catch(function (error) {
          console.log(error)
        })
      }
    }
  }
}
</script>