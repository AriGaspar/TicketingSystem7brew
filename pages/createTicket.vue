<template>
    <div class="px-40 milkstore-text text-xl">
<!-- TITLE -->
        <MainTitleComp :title="'Create Ticket'"/>
<!-- FORM -->
        <form action="">
            <div class="grid grid-cols-3 w-full px-32 justify-center gap-4 items-start">
                <label for="subject" class="text-xxl col-span-1 pt-2">SUBJECT</label>
                <input type="text" id="subject" class="milkstore04-text text-sm pl-3 col-span-2 pr-4 py-2 border border-red-700 focus:outline-none focus:border-blue-500  text-xl w-full" placeholder="Hygiene issues" required />

                <label for="department" class="text-xxl col-span-1 pt-2">DEPARTMENT</label>
                <div class="col-span-2">
                  <DropboxComp id="department" :text_type="'milkstore04-text text-sm'" :options="['IT' , 'HR' ,'CS']" @input="handleSelectedOption" />
                </div>

                <label for="priority" class="text-xxl col-span-1 pt-2">PRIORITY</label>
                <div class="col-span-2">
                  <DropboxComp id="priority" :text_type="'milkstore04-text text-sm'" :options="['HIGH' , 'MEDIUM' ,'LOW']" @input="handleSelectedOption" />
                </div>

                <div class="flex flex-col">
                    <label for="desc" class="text-xxl col-span-1 pt-2">DESCRIPTION</label>
                    <span class="milkstore04-text text-sm">Please be detailed</span>
                </div>
                <textarea id="desc" rows="4" class="milkstore04-text text-sm pl-3 col-span-2 pr-4 py-2 border border-red-700 focus:outline-none focus:border-blue-500  text-xl w-full" placeholder="Write your thoughts here..."></textarea>
                
                <div></div> <!-- DO NOT REMOVE! -->
                <!-- SUBMIT BUTTON -->
                <div class="flex justify-between w-full col-span-2 items-center">
                  <button class="bg-custom-red-wine milkstore-text text-xl w-36 h-15 hover:bg-red-500 text-white font-bold py-2 px-4 items-center justify-center">
                    <span class="pl-2">SUBMIT</span>
                  </button>
                  <div>
                    <input type="file" ref="fileInput" style="display: none" @change="handleFileUpload">
                    <button @click="openFileExplorer" class="flex flex-row items-center milkstore04-text pr-4">
                      <svg v-if="!isFileObtained"  fill="#000000" width="20px" height="20px" viewBox="0 0 24 24" id="paper-clip-top-right" data-name="Flat Line" xmlns="http://www.w3.org/2000/svg" class="icon flat-line"><path id="primary" d="M5.23,8.73,9.18,4.78a6.1,6.1,0,0,1,8.61,0h0a6.09,6.09,0,0,1,0,8.6l-6.43,6.43a4,4,0,0,1-5.74,0h0a4.06,4.06,0,0,1,0-5.73l6.74-6.74a2,2,0,0,1,2.87,0h0a2,2,0,0,1,0,2.87l-7,7" style="fill: none; stroke: rgb(0, 0, 0); stroke-linecap: round; stroke-linejoin: round; stroke-width: 2;"></path></svg>
                      <p>{{ isFileObtained? selectedFileName:"NO FILE" }}</p>
                    </button>
                  </div>
                  <nuxt-link to="/tickettable">About</nuxt-link>
                  <!-- <a href="/path/to/your/file.pdf" target="_blank">Open File</a> -->
                </div>
            </div>
        </form>
    </div>
</template>


<style lang="css">
  @import "../src/assets/css/general-styles.css";
</style>
<script>
import MainTitleComp from '../src/components/MainTitleComp.vue';
import DropboxComp from '../src/components/DropboxComp.vue';


export default {
  name:'TicketCreation',
  components: {
    MainTitleComp,
    DropboxComp
  },
  data() {
    return {
      selectedOption: null,
      selectedFile: null,
      isFileObtained: false,
      selectedFileName: '',

    };
  },
  methods: {
    handleSelectedOption(option) {
      this.selectedOption = option;
      console.log('Selected option in parent component:', this.selectedOption);
    },
    openFileExplorer() {
      this.$refs.fileInput.click();
    },
    handleFileUpload(event) {
      const selectedFile = event.target.files[0];
      if (selectedFile) {
        this.selectedFileName = (selectedFile.name.length < 20)? selectedFile.name :selectedFile.name.substring(0,15)+'... '+selectedFile.name.substring(selectedFile.name.length-6,selectedFile.name.length);
        this.selectedFile = selectedFile;
        
        this.isFileObtained = true;
      }
    },
    goBack() {
      this.$router.go(-1); // Go back one step in the history
    }
  }
};
</script>