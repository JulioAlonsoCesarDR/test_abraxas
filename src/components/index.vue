<template>
  <div>
    <div class="container">
      <div class="jumbotron">
        <h1 class="text-center">Lista de tareas</h1>
        <div class="input-group mb-3">
          <span class="input-group-btn">
            <button type="button" v-b-modal.modalCreate class="btn btn-primary">Crear nueva Tarea</button>
          </span>
        </div>

        <div>
          <b-modal  ref="modalCreate" id="modalCreate" title="Crear nueva tarea">
            <form action="createWork">
              <div>
                <label for="name">Nombre de la tarea</label>
                <input
                  type="text"
                  placeholder="Nombre de la tarea"
                  v-model="title"
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
                    v-model.number='min'
                    class="form-control"
                    min="0" max="60">
                </div>
                <div class="col-md-6 col-12">
                  <label for="">Segundos</label>
                  <input
                    type="number"
                    placeholder="Segundos"
                    v-model.number='sec'
                    class="form-control"

                    min="0" max="60">
                </div>
              </div>
              <hr>
              <div class="col-12 text-center my-3">
                <button type="button" @click="createWork" class="btn btn-primary">Crear nueva Tarea</button>
              </div>
            </form>
          </b-modal>
        </div>

        <ul class="list-group">
          <li
            v-for="(work, index) in works"
            :key="index"
            class="list-group-item d-flex justify-content-between">
            <div>
              <span>{{ work.title }}</span>
              <b-collapse :id="work.id+1">
                <b-card>
                  <div class="">
                    <div>
                      {{work.min}}:{{work.sec}}
                    </div>
                      <b-button @click="startTimer" variant="outline-primary">Comenzar</b-button>
                    <div>

                    </div>
                  </div>
                </b-card>
              </b-collapse>
            </div>
            <div>
              <b-button variant="success" v-b-toggle="work.id+1"> <i class="fas fa-play"></i></b-button>
              <b-button variant="info" @click="updateModal(work)"><i class="fas fa-edit"></i></b-button>
              <b-button variant="danger" v-on:click="deleteWork(index)"> <i class="fas fa-times"></i></b-button>
            </div>

          </li>
        </ul>
      </div>
      <b-modal ref="modalEdit" id="modalEdit" title="Crear nueva tarea">
        <div v-if="workUpdate">
          <form>
            <div>
              <label for="name">Nombre de la tarea</label>
              <input
                type="text"
                placeholder="Nombre de la tarea"
                v-model="workUpdate.title"
                class="form-control mb-3">
            </div>
            <hr>
            <p><b>Duración</b></p>
            <!-- <div class="row justify-content-around my-2">
              <b-button variant="outline-info" @click="setTimeShort" >Corta</b-button>
              <b-button variant="outline-info" @click="setTimeMedium" >Mediana</b-button>
              <b-button variant="outline-info" @click="setTimeLarge">Larga</b-button>
              <b-button variant="outline-info" @click="setTimeOther" >Otro</b-button>
            </div> -->
            <div class="row">
              <div class="col-12">
                <p>{{textTime}}</p>
              </div>
              <div class="col-md-6 col-12">
                <label for="">Minutos</label>
                <input
                  type="number"
                  placeholder="Minutos"
                  v-model.number='workUpdate.min'
                  class="form-control"
                  min="0" max="60">
              </div>
              <div class="col-md-6 col-12">
                <label for="">Segundos</label>
                <input
                  type="number"
                  placeholder="Segundos"
                  v-model.number='workUpdate.sec'
                  class="form-control"

                  min="0" max="60">
              </div>
            </div>
            <hr>
            <div class="col-12 text-center my-3">
              <button type="button" @click="updateWork" class="btn btn-primary">Editar Tarea</button>
            </div>
          </form>
        </div>
      </b-modal>
    </div>
  </div>
</template>

<script>
// import ModalEdit from './modalEdit'
export default {
  name: "index",
  components: {
    // ModalEdit
  },
  data() {
    return {
      works: [
        {
          id:0,
          title: "Aprender Vue.js",
          finish: false,
          min:10,
          sec:10,
          realTime: 0,
          totalTime: 0,
        },
        {
          id:1,
          title: "Aprender Angular",
          finish: false,
          min:120,
          sec:0,
          realTime: 0,
          totalTime: 0,
        },
        {
          id:2,
          title: "Aprender React",
          finish: false,
          min:2,
          sec:10,
          realTime: 0,
          totalTime: 0,
        }
      ],
      title: "",
      finish: false,
      timer: null,
      min:null,
      sec:null,
      realTime:0,
      totalTime: 0,
      textTime: '',
      workUpdate: null,
      timer: null,
      // min: null,
      // sec: null,
      // totalTime:0,
    };
  },
  methods: {
    setTimeShort: function() {
      this.min = 30
      this.sec = 0
      this.textTime = 'Duracion menos de 30 min'

    },
    setTimeMedium: function(){
      this.min = 60
      this.sec = 0
      this.textTime = 'Duracion de 30 min a 60 min'

    },
    setTimeLarge: function(){
      this.min = 60
      this.sec = 0
      this.textTime = 'Duracion mas de 60 min y menos de 120'

    },
    setTimeOther: function(){
      this.min = 120
      this.sec = 0
      this.textTime = 'Duracion hasta 120 min'

    },
    createWork: function() {
      if (this.title && this.min <= 120 && this.sec <= 60) {
        this.works.push({
          id: this.works.length+1,
          title : this.title,
          min : this.min,
          sec : this.sec,
          finish: false,
          timer: null,
          realTime:0,
          totalTime: 0,
        })
        this.$refs.modalCreate.hide()
        this.title =""
        this.min = null
        this.sec = null
        this.textTime = ""
      }
    },
    updateWork: function(){
      var objIndex = this.works.findIndex((obj => obj.id == this.workUpdate.id));
      this.works[objIndex].title = this.workUpdate.title
      this.works[objIndex].min = this.workUpdate.min
      this.works[objIndex].sec = this.workUpdate.sec
      this.$refs.modalEdit.hide()
},
    updateModal: function(item) {
      this.workUpdate = item
      this.$refs.modalEdit.show()
    },
    deleteWork: function(item) {
      this.works.splice(item, 1);
    },

    startTimer: function(min, sec) {
      let totalTime=(min * 60) + sec
      this.timer = setInterval(() => this.countdown(totalTime), 1000);
      console.log(this.timer)
    },
    padTime: function(time) {
      return (time < 10 ? '0' : '') + time;
    },
    countdown: function(totalTime) {
      if(this.totalTime >= 1){
        this.totalTime--;
      } else{
        this.totalTime = 0;
      }
    }

  },

 computed: {
    minutes: function(totalTime) {
      const minutes = Math.floor(totalTime / 60);
      return this.padTime(minutes);
    },
    seconds: function(totalTime,minutes) {
      const seconds = this.totalTime - (minutes * 60);
      return this.padTime(seconds);
    }
  },
};
</script>
<style lang="">
  .modal-footer {
    display: none;
  }
</style>
