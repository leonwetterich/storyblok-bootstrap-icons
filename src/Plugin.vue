<template>
  <div>
    <div class="uk-form-row">
      <label>Search for a Bootstrap icon</label>
      <input type="text" placeholder="alarm-fill" v-model="search" class="uk-width-1-1">
    </div>
    <div class="uk-form-row">
      <p v-if="this.model.icon" class="success">You selected the <b>{{this.model.icon}}</b>.</p>
      <p v-if="!this.model.icon" class="error">Please select an icon</p>
    </div>
    <div class="icon-container">
      <p><span v-for="(result,index) in results" :key="index" @click="select(result)" :class="{'selected': model.icon && model.icon === result}"><i :class="'bi-'+result"></i></span></p>
    </div>
  </div>
</template>

<script>
var link = document.createElement("link");
link.href =
  "https://cdn.jsdelivr.net/npm/bootstrap-icons@1.4.1/font/bootstrap-icons.css";
link.type = "text/css";
link.rel = "stylesheet";
link.media = "screen,print";

document.getElementsByTagName("head")[0].appendChild(link);
import JSON from "./icons.json";
export default {
  mixins: [window.Storyblok.plugin],
  data() {
    return {
      search: "",
      icons: JSON.icons,
      errors: [],
    };
  },
  computed: {
    results() {
      return this.icons.filter((e) => {
        return e.includes(this.search);
      });
    },
  },
  methods: {
    select(icon) {
      this.errors = [];
      this.search = icon;
      this.model.icon = icon;
      this.search = "";
    },
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: "bootstrap-icons",
        icon: "",
      };
    },
    pluginCreated() {
      // eslint-disable-next-line
      console.log(
        "View source and customize: https://github.com/storyblok/storyblok-fieldtype"
      );
    },
  },
  watch: {
    model: {
      handler: function(value) {
        this.$emit("changed-model", value);
      },
      deep: true,
    },
  },
};
</script>

<style>
.p-metatags__google-title {
  color: blue;
  text-decoration: underline;
}

.p-metatags__google-link {
  color: green;
}

.p-metatags__preview {
  margin: 5px 0 15px;
  padding: 10px;
  color: #000;
  background: #fff;
}
span {
  padding: 8px 10px;
}
p {
  line-height: 33px;
}
.icon-container {
  max-height: 300px;
  overflow: scroll;
}
.selected {
  background-color: #1b253e;
  color: #09b3af;
  border-radius: 5px;
  transition: 0.3s all;
}
.icon-container p span:hover {
  transition: 0.3s all;
  cursor: pointer;
  background-color: #09b3af;
  color: #1b253e;
  border-radius: 5px;
}
.error {
  background-color: #e135354d;
  color: #e13535;
  border-radius: 4px;
  padding: 0px 10px;
}
.success {
  background-color: #2db9504d;
  color: #1a9439;
  border-radius: 4px;
  padding: 0px 10px;
}
</style>
