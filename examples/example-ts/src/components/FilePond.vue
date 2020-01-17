<template>
  <div id="component">
    <file-pond
      name="test"
      ref="pond"
      label-idle="Drop files here..."
      allow-multiple="true"
      v-bind:server="myServer"
      v-bind:files="myFiles"
      v-on:init="handleFilePondInit"
    />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

// Import Vue FilePond
import vueFilePond, { VueFilePondComponent } from "vue-filepond";

// Import FilePond styles
import "filepond/dist/filepond.min.css";

// Import FilePond plugins
// Please note that you need to install these plugins separately
// `npm i filepond-plugin-image-preview filepond-plugin-image-exif-orientation --save`
// @ts-ignore No types yet
import FilePondPluginImageExifOrientation from "filepond-plugin-image-exif-orientation";
// @ts-ignore No types yet
import FilePondPluginImagePreview from "filepond-plugin-image-preview";
import "filepond-plugin-image-preview/dist/filepond-plugin-image-preview.min.css";
import { FilePondOptionProps } from "filepond";

const FilePond = vueFilePond(FilePondPluginImagePreview, FilePondPluginImagePreview);



@Component({ components: { FilePond } })
export default class HelloWorld extends Vue {
  $refs!: {
    filepond: VueFilePondComponent;
  };
  //If you want to strongly type your options you can access the prop models like this

  myServer: FilePondOptionProps["server"] = {
    process: (fieldName, file, metadata, load) => {
      // simulates uploading a file
      setTimeout(() => {
        load(Date.now().toString());
      }, 1500);
    },
    load: (source, load) => {
      // simulates loading a file from the server
      fetch(source)
        .then(res => res.blob())
        .then(load);
    }
  };

  myFiles: FilePondOptionProps["files"] = [
    {
      source: "photo.jpeg",
      options: {
        type: "local"
      }
    }
  ];

  handleFilePondInit() {
    console.log("FilePond has initialized");
  }

  doSomethingWithFilepond() {}
}
</script>