<template>
  <div id="ToolHome" v-if="!$root.$data.session.writer.selected">
    <div class="wavemaker_info_box">
      <table style="width: 100%">
        <tr>
          <td><h1>{{this.$root.setlang.tools.writer}}</h1></td>
          <td style="width: 100px">
            <button @click="showaddform = !showaddform" class="interfaceBtn">
              <svg  viewBox="0 0 24 24">
                <path
                  d="M13 19C13 20.1 13.3 21.12 13.81 22H6C4.89 22 4 21.11 4 20V4C4 2.9 4.89 2 6 2H7V9L9.5 7.5L12 9V2H18C19.1 2 20 2.89 20 4V13.09C19.67 13.04 19.34 13 19 13C15.69 13 13 15.69 13 19M20 18V15H18V18H15V20H18V23H20V20H23V18H20Z"
                />
              </svg>
            </button>
          </td>
        </tr>
      </table>

      <div v-if="showaddform">
        <hr />
        <input
          type="text"
          v-model="title"
          :placeholder="this.$root.setlang.tools.name"
          class="formInput"
        />
        <input
          type="text"
          v-model="description"
          :placeholder="this.$root.setlang.tools.description"
          class="formInput"
        />
        <div style="text-align: right">
          <button
            @click="addItem"
            class="interfaceBtn"
            
          >
            <svg  viewBox="0 0 24 24">
              <path
                d="M13 19C13 20.1 13.3 21.12 13.81 22H6C4.89 22 4 21.11 4 20V4C4 2.9 4.89 2 6 2H7V9L9.5 7.5L12 9V2H18C19.1 2 20 2.89 20 4V13.09C19.67 13.04 19.34 13 19 13C15.69 13 13 15.69 13 19M20 18V15H18V18H15V20H18V23H20V20H23V18H20Z"
              />
            </svg>
          </button>
        </div>
      </div>
      <hr />

      <table style="width: 100%"  v-if="!showaddform">
        <tr v-for="(item, index) in $root.$data.shadowDB.Writer" :key="index">
          <td>
            <div class="title">{{ item.title }}</div>
          
             <div class="author">{{ item.description }}</div>
                 <i> <br/>  {{$root.niceDate(item.lastupdated)}} </i>
          </td>
          <td style="width: 100px">
            <button
              @click="$root.$data.session.writer.selected = item.uuid"
              class="interfaceBtn"
            >
              <svg  viewBox="0 0 24 24">
                <path
                  d="M19.39 10.74L11 19.13V22H6C4.89 22 4 21.11 4 20V4C4 2.9 4.89 2 6 2H7V9L9.5 7.5L12 9V2H18C19.1 2 20 2.89 20 4V10.3C19.78 10.42 19.57 10.56 19.39 10.74M13 19.96V22H15.04L21.17 15.88L19.13 13.83L13 19.96M22.85 13.47L21.53 12.15C21.33 11.95 21 11.95 20.81 12.15L19.83 13.13L21.87 15.17L22.85 14.19C23.05 14 23.05 13.67 22.85 13.47Z"
                />
              </svg>
            </button>
          </td>
        </tr>
      </table>
    </div>
  </div>

  <WriterTool v-if="$root.$data.session.writer.selected" /> 
</template>

<script>
import WriterTool from "./WriterTool.vue";
export default {
  name: "WriterHome",
  components: {
    WriterTool,
  },
  data() {
    return {
      title: null,
      description: null,
      showaddform: false,
    };
  },
  methods: {
    addItem() {
      if (!this.title) {
        alert(this.$root.setlang.tools.warn);
        return false;
      }
      let obj = {};
      obj.title = this.title;
      obj.description = this.description;
      obj.files = [];
      obj.uuid = this.$root.uuid(); // genertate your own UUID so thta you can update the shadow db
      this.$root.$data.shadowDB.Writer[obj.uuid] = obj;
      this.$root.AddRecord("Writer", obj);

      this.title = "";
      this.description = "";
      this.showaddform = false;
    },
  },
  created() {
    if (!this.$root.$data.session.writer) {
      this.$root.$data.session.writer = {};
    }
  },
};
</script>

<style scoped>



</style>
