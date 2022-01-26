<template>
  <div class="grid">
    <div
      class="grid grid-cols-2 divide-x divide-gray-400 shadow-md bg-gray-200 p-5 text-center rounded-full"
    >
      <div class="text-4xl">
        <span class="font-bold"
          >{{ hours }}:{{ minutes }}:{{ seconds }}</span
        >
        <button
          class="ml-10 pt-2 pb-2 shadow-md text-white pl-20 pr-20 text-center rounded-full"
          v-bind:class="[color]"
          @click="toggleTimer"
        >
          {{ text }}
        </button>
      </div>
      <div class="text-4xl">
        <img
          class="inline-block h-20 w-20 rounded-full ring-4 ring-white"
          v-bind:class="[radius]"
          src="https://images.unsplash.com/photo-1567532939604-b6b5b0db2604?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
          alt=""
        />
        <div class="ml-5 inline-block">
          <div class="dropdown ">
            <button
              class=" text-gray-700 font-semibold  px-4 rounded inline-flex items-center"
            >
              <span class="mr-1"
                >{{ lastItem.employee.firstName }}
                {{ lastItem.employee.lastName }}</span
              >
          <svg class="w-5 h-5 text-gray-800 dark:text-white" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 15 15"
            fill="currentColor">
            <path fill-rule="evenodd"
                d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                clip-rule="evenodd" />
        </svg>
            </button>
            <ul class="dropdown-menu absolute hidden text-gray-700 pt-1 shadow-xl rounded divide-y divide-gray-200 text-2xl">
              <li class="">
                <a
                  class="rounded-t bg-white hover:bg-gray-100 py-5 px-36 block whitespace-no-wrap"
                  href="#"
                  >Mis cuentas</a
                >
              </li>
              <li class="">
                <a
                  class="bg-white hover:bg-gray-100 py-5 block whitespace-no-wrap"
                  href="#"
                  >Vista empleado</a
                >
              </li>
              <li class="">
                <a
                  class="bg-white hover:bg-gray-100 py-5  block whitespace-no-wrap"
                  href="#"
                  >Mi perfil</a
                >
              </li>
              <li class="">
                <a
                  class="rounded-b bg-white hover:bg-gray-100 py-5  block whitespace-no-wrap"
                  href="#"
                  >Cerrar sesión</a
                >
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//He tratado de usar el timer en local pero carecía de sentido sin la capacidad de hacer POST.
import response from "../assets/response.json";
export default {
  name: "Timetracker",
  props: ["stats, order"],
  data() {
    return {
      hours: 0,
      minutes: 0,
      seconds: 0,
      isRunning: false,
      users: response.data,
      color: "bg-green-300",
      radius: "ring-red-500",
      text: "Entrar",
    };
  },
  computed: {
    lastItem() {
      return this.users.slice(-1)[0];
    },
  },
  methods: {
    //Función timer simple
    toggleTimer() {
      if (this.isRunning) {
        clearInterval(this.interval);
        this.color = "bg-green-300";
        this.radius = "ring-red-500";
        this.text = "Entrar";

      } else {
        this.interval = setInterval(() => {
          this.seconds = this.seconds + 1;
          if (this.seconds > 59) {
            this.minutes = this.minutes + 1;
            this.seconds = 0;
            if (this.minutes > 59) {
              this.hours = this.hours + 1;
              this.minutes = 0;
            }
          }
        }, 1000);
        this.color = "bg-red-300";
        this.radius = "ring-green-500";
        this.text = "Pausar";
      }
      this.isRunning = !this.isRunning;
    },
  },
  //Filtro para formatear los "0". No he conseguido aplicarlo.
  filters: {
    formattedTime: function() {
      var sec_num = parseInt(value, 10);
      var hours = Math.floor(sec_num / 3600);
      var minutes = Math.floor((sec_num - hours * 3600) / 60);
      var seconds = sec_num - hours * 3600 - minutes * 60;
      if (hours < 10) {
        hours = "0" + hours;
      }
      if (minutes < 10) {
        minutes = "0" + minutes;
      }
      if (seconds < 10) {
        seconds = "0" + seconds;
      }
      return +hours + ":" + minutes + ":" + seconds;
    }
  },
};
</script>

<style scoped>
.dropdown:hover .dropdown-menu {
  display: block;
}
</style>
