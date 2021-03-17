<template>
   <form @submit.prevent="store" class="q-gutter-md">
    <q-list>
      <q-item>
        <q-item-section>
          <p class="text-h2">Guardar imagen</p>
        </q-item-section>
      </q-item>
      <q-item>
        <q-item-section>
          <q-item-label class="q-pb-xs">Nombre:</q-item-label>
          <q-input dense outlined v-model="name" type="text" :rules="[val => !!val || 'Campo Obligatorio']"/>
        </q-item-section>
      </q-item>
      <q-item>
      <q-item-section>
        <q-item-label class="q-pb-xs">Imagen:</q-item-label>
        <q-file outlined v-model="photo" hint="Inserta una imagen">
        <template v-slot:prepend>
          <q-icon name="attach_file" />
        </template>
        </q-file>
      </q-item-section>
      </q-item>
      <q-item>
      <q-item-section>
        <q-btn type="submit" label="Guardar Foto" color="primary"/>
      </q-item-section>
      </q-item>
    </q-list>
   </form>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Store',
  data () {
    return {
      name: '',
      photo: null
    }
  },
  methods: {
    store () {
      var color = 'negative'
      var icon = 'report_problem'
      var msg = 'Error al guardar la foto'

      const formData = new FormData()

      formData.append('title', this.name)
      formData.append('image', this.photo)

      axios.post('/api/photos', formData)
      .then(response => {
        if (response !== null) {
          color = 'positive'
          icon = 'check_circle'
          msg = 'Se ha guardado la foto'

          this.name = ''
          this.photo = null
        }

        this.$q.notify({
          color: color,
          position: 'top',
          message: msg,
          icon: icon
        })

        this.$router.push('/all')
      })
      .catch(e => {
        color = 'negative'
        icon = 'report_problem'
        msg = 'Error al guardar la foto'

        this.$q.notify({
          color: color,
          position: 'top',
          message: msg,
          icon: icon
        })
      })
    }
  }
}
</script>

<style>

</style>
