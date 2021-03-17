<template>
  <q-page class="q-pa-md row items-start q-gutter-md">
    <p class="text-h2 col-xs-12">Imagenes</p>
    <q-card v-for="photo in listPhotos" flat :key="photo.id" class="bordered">
      <q-card-section>
        <img width="150px" height="auto" :src="`data:image/gif;base64,${photo.image.data}`" />
      </q-card-section>
       <q-separator inset />
      <q-card-section>
        <div class="absolute-bottom text-subtitle2 text-center">
            {{photo.title}}
          </div>
      </q-card-section>

      <q-separator dark />
      <q-card-actions>
        <q-btn outline rounded color="red-9" label="Eliminar fotografía" @click="confirm = true, photoId = photo.id" />
      </q-card-actions>
    </q-card>
    <q-dialog v-model="confirm" persistent>
        <q-card>
          <q-card-section class="row items-center">
            <q-avatar icon="announcement" color="red-9" text-color="white" />
            <span class="q-ml-sm">¿Deseas eliminar la fotografía?</span>
          </q-card-section>

          <q-card-actions align="right">
            <q-btn flat label="Cancelar" color="primary" v-close-popup />
            <q-btn flat label="Eliminar" color="red-9" @click="dropPhoto(photoId)" v-close-popup />
          </q-card-actions>
        </q-card>
      </q-dialog>
  </q-page>
</template>

<script>
import axios from 'axios'

export default {
  name: 'All',
  data () {
    return {
      listPhotos: [],
      confirm: false,
      photoId: 0
    }
  },
  created () {
    this.all()
  },
  methods: {
    all () {
      var color = 'negative'
      var icon = 'report_problem'
      var msg = 'Error al consultar las fotos'

      axios.get('/api/photos')
      .then(response => {
        if (response !== null) {
          color = 'positive'
          icon = 'check_circle'
          msg = 'Se han consultado todas las fotos'

          this.listPhotos = response.data
          // console.log('L', this.listPhotos)
        }

        this.$q.notify({
          color: color,
          position: 'top',
          message: msg,
          icon: icon
        })
      })
    },
    dropPhoto (id) {
      this.confirm = false
      var color = 'negative'
      var icon = 'report_problem'
      var msg = 'Error al eliminar la foto'

      axios.delete('/api/photos/'+id)
      .then(response => {
        if (response.data !== null) {
          color = 'positive'
          icon = 'check_circle'
          msg = 'Se ha eliminado la foto'
          this.all()
        }

        this.$q.notify({
          color: color,
          position: 'top',
          message: msg,
          icon: icon
        })
      })
      .catch(e => {
        color = 'negative'
        icon = 'report_problem'
        msg = 'Error al eliminar la foto'

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
