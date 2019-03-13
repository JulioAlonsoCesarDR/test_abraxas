<template>
  <div>
    <div class="container">
      <div class="jumbotron">
        <h1 class="text-center">Lista de tareas</h1>
        <div class="input-group mb-3">
          <input type="text"
                  placeholder="Escribe una nueva tarea"
                  v-model="newWork"
                  class="form-control"
                  v-on:keyup.enter="addWork">
          <span class="input-group-btn">
            <button type="button"
                    v-on:click="addWork"
                    class="btn btn-primary">Agregar</button>
          </span>
        </div>

        <ul class="list-group">
          <li v-for="(work, index) in works" :key="index" class="list-group-item d-flex justify-content-between">
            <div>
              <span>
                {{ work.title }}
              </span>
            </div>
            <div>
              <button type="button" class="btn btn-success btn-xs glyphicon glyphicon-ok"
                      v-on:click="work.finish = !work.finish"></button>
              <button type="button" class="btn btn-danger btn-xs glyphicon glyphicon-remove"
                      v-on:click="deleteWork(index)"></button>
            </div>
          </li>
        </ul>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'index',
  data () {
    return {
      works: [
      {
        title: 'Aprender Vue.js',
        finish: false
      },
      {
        title: 'Aprender Angular',
        finish: false
      },
      {
        title: 'Aprender React',
        finish: false
      },
    ],
    newWork: ''
    }
  },
  methods: {
    addWork: function() {
      var texto = this.newWork.trim();
      if(texto){
        this.works.push({
          title: texto,
          finish: false
        })
      }
      this.newWork = '';
    },
    deleteWork: function(item){
      this.works.splice(item,1)
    }
  },
}
</script>
