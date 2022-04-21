<template>
  <div class="ph4 pv3 bg-washed-red vh-100">
    <header class="tc pv3 pv3-ns">
      <h1 class="mt2 mb0 baskerville fw1 f2-ns f3">Contact Card Barcode Generator</h1>
    </header>

    <div class="flex flex-wrap justify-between">
      <form class="tc w-50-ns w-100 mt4 ph5-ns" @submit.prevent="upload">
        <label for="contact-name" class="db tl">Contact name</label>
        <input id="contact-name" v-model="contactName" type="text" name="contact-name" class="db pa2 w-40-l w-80-m w-100 mt2 br2 ba b--black-50">

        <label for="contact-number" class="db tl mt4">Contact number</label>
        <input id="contact-number" v-model="contactNumber" type="number" number="contact-number" class="db pa2 w-40-l w-80-m w-100 mt2 br2 ba b--black-50">

        <button class="f6 link dim br2 ph3 pv2 db white bg-dark-green ba b--green pointer tl mv3" type="submit">Generate Barcode</button>
      </form>

      <section class="w-50-ns w-100 tl pa3">
        <section>
          <ul v-if="errors.length > 0">
            <li v-for="(error,index) in errors" :key="index">{{error}}</li>
          </ul>
        </section>
        <VueBarcode ref="barcode" :value="contactName + ' ' + contactNumber" tag="img" name="barcode.png" class="dn"></VueBarcode>
        <section v-if="results && results.secure_url">
          <img :src="results.secure_url" :alt="results.public_id" class="mv3" />
          <input disabled type="text" class="db w-90 pv3 ph2 br2 ba b--black-40 f7" :value="results.url" />
          <button class="f6 link dim br2 ph3 pv2 db white bg-dark-green ba b--green mt2">Share link</button>
        </section>
      </section>
    </div>
  </div>
</template>

<script>
  import VueBarcode from '@chenfengyuan/vue-barcode';

export default {
  components: {
    VueBarcode
  },
  data(){
    return {
      contactName: '',
      contactNumber: 0,
      results: null,
      errors: [],
      file: null,
      cloudName: "***",
      preset: "lan9vkdw",
      tags: "browser-upload",
      fileContents: null,
      formData: null,
    }
  }, 
  mounted(){
    this.handleImageAttachment()
  },
  methods: {
    handleImageAttachment() {
      this.file = this.$refs.barcode.$attrs
    },
    prepareFormData() {
      this.formData = new FormData();
      this.formData.append("upload_preset", this.preset);
      this.formData.append("tags", this.tags); // Optional - add tag for image admin in Cloudinary
      this.formData.append("file", this.fileContents);
    },

    // function to upload image and return it from Cloudinary
    upload () {
      if (this.contactName < 1 || this.contactNumber < 1) {
        this.errors.push("You must fill the empty fileds to generate the barcode");
        return;
      }
      // clear errors
      else {
        this.errors = [];
      }
      this.fileContents = this.$refs.barcode.$el.currentSrc;
      this.prepareFormData();
      const cloudinaryUploadURL = `https://api.cloudinary.com/v1_1/${this.cloudName}/upload`;
      const requestObj = {
        url: cloudinaryUploadURL,
        method: "POST",
        data: this.formData,
      };
      this.$axios(requestObj)
        .then(response => {
          this.results = response.data;
          navigator.clipboard
          .writeText(this.results.url)
        })
        .catch(error => {
          this.errors.push(error);
        })
      },
    }
  }
</script>

<style>
/* chrome: removes the number type inner anchor  */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox: removes the number type inner anchor */
input[type="number"] {
  -moz-appearance: textfield;
}
</style>