<template>
  <q-page padding>
  <div class="row q-mb-md col-gutter-md">
    <div class="col-md-12 col-xs-12 col-lg-12">
      <div class="row">
        <div class="col-auto">
          <div class="left green"></div>
          </div>
        <div class="col">
          <q-banner inline-actions class="text-green-grey-14">
            <div class="text-h6">Input DVD</div>
            <div>Input Data DVD Film Baru</div>
          </q-banner>
        </div>
      </div>
    </div>
  </div>
  <q-card flat>
    <q-card-section class="row">
    <q-form
      @submit="onSubmit()"
      class="q-col-gutter-md q-col-lg-6 col-md-6 col-xs-12"
      >
      <q-input
          filled
          v-model="form.judulFilm"
          label="Judul Film"
          :rules="[ val => val && val.length > 0 || 'Mohon Isi Judul']"
      />
      <q-input
          filled
          type="number"
          v-model="form.harga"
          label="Harga Film"
          :rules="[ val => val && val.length > 0 || 'Mohon Isi Harga']"
      />
      <q-input
          filled
          v-model="form.tahun"
          label="Tahun"
          :rules="[ val => val && val.length > 0 || 'Mohon Isi Tahun']"
      />
      <q-select
        filled
        v-model="form.genre"
        :options="optionGenre"
        label="Pilih Genre"
      />

      <div class="flex">
        Pilih Rating
        <q-rating
          class="q-ml-md"
          v-model="form.rating"
          size="2em"
          :max="5"
          color="orange"
        />
      </div>

      <q-input
      v-model="form.deskripsi"
      filled
      label="Deskripsi"
      type="textarea"
    />

      <q-file accept=".jpg, image/*"  color="primary" filled v-model="image" label="Upload Cover Film">
        <template v-slot:prepend>
          <q-icon name="cloud_upload" />
        </template>
      </q-file>

      <div>
          <q-btn label="Submit" type="submit" color="primary"/>
          <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
      </div>
     </q-form>
    </q-card-section>
  </q-card>
  </q-page>
</template>
<script>
export default {
  data () {
    return {
      form: {
        judulFilm: null,
        harga: null,
        tahun: null,
        genre: null,
        rating: 0,
        deskripsi: null
      },
      optionGenre: [
        'Action',
        'Adventure',
        'Comedy',
        'Drama',
        'Fantasy'
      ],
      image: null
    }
  },
  methods: {
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.post('movie/insert', formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.$router.push({ name: 'dataDVD' })
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    }
  }
}
</script>
<style scoped>
.left {
  width: 5px;
  height: 100%;
  background-color: forestgreen;
}
</style>
