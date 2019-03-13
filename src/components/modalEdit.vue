<template>
  <div>
    <form action="addWork">
      <div>
        <label for="name">Nombre de la tarea</label>
        <input
          type="text"
          placeholder="Nombre de la tarea"
          v-model="work.title"
          class="form-control mb-3">
      </div>
      <hr>
      <p><b>Duración</b></p>
      <div class="row justify-content-around my-2">
        <b-button variant="outline-info" @click="setTimeShort" >Corta</b-button>
        <b-button variant="outline-info" @click="setTimeMedium" >Mediana</b-button>
        <b-button variant="outline-info" @click="setTimeLarge">Larga</b-button>
        <b-button variant="outline-info" @click="setTimeOther" >Otro</b-button>
      </div>
      <div class="row">
        <div class="col-12">
          <p>{{textTime}}</p>
        </div>
        <div class="col-md-6 col-12">
          <label for="">Minutos</label>
          <input
            type="number"
            placeholder="Minutos"
            v-model.number='work.min'
            class="form-control"
            min="0" max="60">
        </div>
        <div class="col-md-6 col-12">
          <label for="">Segundos</label>
          <input
            type="number"
            placeholder="Segundos"
            v-model.number='work.sec'
            class="form-control"

            min="0" max="60">
        </div>
      </div>
      <hr>
      <div class="col-12 text-center my-3">
        <button type="button" @click="addWork" class="btn btn-primary">Editar Tarea</button>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  props:[
    'work'
  ],
  methods: {
    updateWork: function(){
      objIndex = myArray.findIndex((obj => obj.id == 1));
    }
  }
}
</script>
