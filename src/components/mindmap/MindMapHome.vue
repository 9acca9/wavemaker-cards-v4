<template>
  <div id="ToolHome" v-if="!$root.session.mindmap.selected">
    <div class="wavemaker_info_box">
      <table style="width: 100%">
        <tr>
          <td><h1>{{this.$root.setlang.tools.mindmap}}</h1></td>
          <td style="width: 100px">
            <button @click="showaddform = !showaddform" class="interfaceBtn">
              <svg  version="1.1"  viewBox="0 0 24 24"><path d="M21 16V13C21 11.89 20.11 11 19 11H13V8H15V2H9V8H11V11H5C3.89 11 3 11.89 3 13V16H1V22H7V16H5V13H11V16H9V22H15V16H13V13H19V16H17V22H23V16H21M11 4H13V6H11V4M5 20H3V18H5V20M13 20H11V18H13V20M21 20H19V18H21V20Z" /></svg>
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
    <path  d="M17,13H13V17H11V13H7V11H11V7H13V11H17M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2Z" />
</svg>
          </button>
        </div>
      </div>
      <hr />

      <table style="width: 100%"  v-if="!showaddform">
        <tr v-for="(item, index) in $root.shadowDB.Mindmap" :key="index">
          <td>
            <div class="title">{{ item.title }}</div>
          
             <div class="author">{{ item.description }}</div>
                 <i> <br/>  {{$root.niceDate(item.lastupdated)}} </i>
          </td>
          <td style="width: 100px">
            <button
              @click="$root.session.mindmap.selected = item.uuid"
              class="interfaceBtn"
            >
           <svg  viewBox="0 0 24 24">
    <path  d="M20.71,7.04C21.1,6.65 21.1,6 20.71,5.63L18.37,3.29C18,2.9 17.35,2.9 16.96,3.29L15.12,5.12L18.87,8.87M3,17.25V21H6.75L17.81,9.93L14.06,6.18L3,17.25Z" />
</svg>
            </button>
          </td>
        </tr>
      </table>
    </div>
  </div>
  <div v-if="$root.session.mindmap.selected">
     <WorkSpace />
 <ToolBox />

  </div>
</template>

<script>
import ToolBox from './ToolBox.vue'
import WorkSpace from "./WorkSpace.vue"
export default {
  name: "MindMapHome",
  components: {
WorkSpace,
ToolBox
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
      if (!this.title ) {
        alert(this.$root.setlang.tools.warn);
        return false;
      }
      let obj = {};
      obj.title = this.title;
      obj.description = this.description;
      obj.uuid = this.$root.uuid(); // genertate your own UUID so thta you can update the shadow db
      this.$root.shadowDB.Mindmap[obj.uuid] = obj;
      this.$root.AddRecord("Mindmap", obj);
      this.title = "";
      this.description = "";
      this.showaddform = false;
    },
  },
  created() {
    if (!this.$root.session.mindmap) {
      this.$root.session.mindmap = {};
        this.$root.session.mindmap.linkstyle="solid";
        this.$root.session.mindmap.currentnode = null      }
  },
};
</script>

<style scoped>



</style>
