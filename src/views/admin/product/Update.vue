<template>
  <div class="container">
    <h1>Product Update</h1>
    <form method="post" @submit.prevent="submitData()">
      <div class="form-group row">
        <label for="name" class="col-sm-2 col-form-label">Name</label>
        <div class="col-sm-10">
          <input type="text" class="form-control" id="name" v-model="formSubmitData.name">
        </div>
      </div>
      <div class="form-group row">
        <label for="brand" class="col-sm-2 col-form-label">Brand</label>
        <div class="col-sm-10">
          <input type="text" class="form-control" id="brand" v-model="formSubmitData.brand">
        </div>
      </div>
      <div class="form-group row">
        <label for="desc" class="col-sm-2 col-form-label">Description</label>
        <div class="col-sm-10">
          <textarea class="form-control" id="desc" rows="3" v-model="formSubmitData.desc"></textarea>
        </div>
      </div>
      <div class="form-group row">
        <label for="price" class="col-sm-2 col-form-label">Price</label>
        <div class="col-sm-10">
          <input type="number" class="form-control" id="price" v-model="formSubmitData.price">
        </div>
      </div>
      <div class="form-group row">
        <label for="image" class="col-sm-2 col-form-label">Image</label>
        <div class="col-sm-10">
          <input type="file" id="image">
          <img :src="formSubmitData.image" alt="">
        </div>
      </div>
      <div class="form-group row">
        <label for="default" class="col-sm-2 col-form-label">Default</label>
        <div class="col-sm-10">
          <div v-for="(default_row, index_default) in default_list" :key="index_default">
            <input type="radio" :value="default_row.id" v-model="formSubmitData.default" name="default"/><label for="default" class="item-radio" v-text="default_row.title"></label>
          </div>
        </div>
      </div>
      <div class="form-group row">
        <router-link to="/admin/product">
          <button type="button" class="btn btn-danger">Cancel</button>
        </router-link>
        <button type="submit" class="btn btn-primary">Submit</button>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  data () {
    return {
      formSubmitData: {
        name: '',
        brand: '',
        price: '',
        default: '1',
        desc: ''
      },
      id: this.$route.params.id,
      default_list: [
        {
          id: 1,
          title: 'Default'
        },
        {
          id: 0,
          title: 'Not Default'
        }
      ],
    }
  },
  created () {
    this.getDetails()
  },
  methods: {
    getDetails () {
      var self = this
      this.$axios.get('http://localhost:3000/product/findOne/' + this.id).then(function (response) {
        if (response.status === 200) {
          self.formSubmitData.name = response.data.name
          self.formSubmitData.brand = response.data.brand
          self.formSubmitData.price = response.data.price
          self.formSubmitData.desc = response.data.desc
          self.formSubmitData.default = response.data.default
          self.formSubmitData.image = response.data.image
        }
      }).catch(function (error) {
        console.log(error)
      })
    },
    submitData () {
      var self = this
      var post = {
        name: this.formSubmitData.name,
        brand: this.formSubmitData.brand,
        price: this.formSubmitData.price,
        default: this.formSubmitData.default,
        desc: this.formSubmitData.desc,
        image: document.querySelector('#image').files[0]
      }
      var formData = new FormData()
      for (var key in post) {
        formData.append(key, post[key])
      }
      this.$axios.patch('http://localhost:3000/product/updateOne/' + this.id, formData)
      .then(function (response) {
        if (response.status === 200) {
          alert('Berhasil Update')
          self.$router.push('/admin/product')
        }
      }).catch(function (error) {
        console.log(error)
      })
    }
  }
}
</script>