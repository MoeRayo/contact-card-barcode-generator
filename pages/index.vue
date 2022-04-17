<template>
  <div class="ph4 pv3 bg-washed-red vh-100">
    <header class="tc pv3 pv3-ns">
      <h1 class="mt2 mb0 baskerville fw1 f2-ns f3">Contact Card Barcode Generator</h1>
    </header>

    <div class="flex flex-wrap justify-between">
      <form class="tc w-50-ns w-50 mt4">
        <label for="contact-name" class="db tl">Contact name</label>
        <input id="contact-name" v-model="contactName" type="text" name="contact-name" class="db pa2 w-30-l w-70-m w-100 mt2 br2 ba b--black-50">

        <label for="contact-number" class="db tl mt4">Contact number</label>
        <input id="contact-number" v-model="contactNumber" type="text" number="contact-number" class="db pa2 w-30-l w-70-m w-100 mt2 br2 ba b--black-50">
      </form>

      <section>
        <VueBarcode ref="barcode" :value="contactName + contactNumber" tag="img" format="CODE128" :options="options" class="db center tc mv3"></VueBarcode>
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
      contactNumber: ' '
    }
  }, 
}
</script>
