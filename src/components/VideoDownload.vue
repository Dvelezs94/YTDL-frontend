<template>
  <div class="flex justify-center">
    <div>
      <img class="w-2/3 ml-auto mr-auto mb-6" :src="require('../assets/logomain.png')" alt="Youtube converter to Mp3">
      <form name="postVideoForm" @submit="postVideoForDownload" method="post">
        <div class="flex border border-gray-200 rounded-full p-4 shadow text-xl">
            <input class="w-full outline-none px-3" type="text" placeholder="Paste Youtube link here" required="true" v-model="video_src" />
        </div>
        <div class="mt-8 text-center">
          <button id="submitSource" type="submit" class="bg-gray-200 border border-gray-300 py-3 px-4 rounded hover:bg-gray-400 hover:border-gray-500 disabled:opacity-50">Convert to MP3</button>
          <div class="mt-4">
            <component :is="download_component" :download_url="mp3_download"></component>
            <component :is="loading_component"></component>
            <component :is="error_component" :error_msg="error_response"></component>
          </div>
          
        </div>
      </form>
    </div>
  </div>
  <ToolDescription/>
</template>

<script>
import DownloadButton from "./DownloadButton.vue";
import LoadingButton from "./LoadingButton.vue";
import ErrorAlert from "./ErrorAlert.vue";
import ToolDescription from "./ToolDescription.vue";

export default {
  name: 'VideoDownload',
  components: {
    DownloadButton, 
    LoadingButton,
    ErrorAlert,
    ToolDescription
  },
  data() {
    return {
      video_src: null,
      mp3_download: null,
      download_component: null,
      loading_component: null,
      error_component: null,
      error_response: null
    }
  },
  methods: {
    postVideoForDownload(e) {
      e.preventDefault();

      const axiosInstance = this.axios.create({
        headers: {
          "Access-Control-Allow-Origin": "*"
        }
      });

      axiosInstance.interceptors.request.use(
        req => {
          this.loading_component = LoadingButton
          document.getElementById("submitSource").disabled = true
          this.download_component = null
          this.error_component = null
          return req
        }, 
        err => {
          console.log('Error req: ', err.message);
          return Promise.reject(err);
        }
      );

      axiosInstance.interceptors.response.use(
        res => {
          this.loading_component = null
          document.getElementById("submitSource").disabled = false
          this.mp3_download = res.data.download_url
          this.download_component = DownloadButton
          return res
        }, 
        err => {
          this.loading_component = null
          document.getElementById("submitSource").disabled = false
          this.error_response = err.response.data.detail
          this.error_component = ErrorAlert
          return Promise.reject(err);
        }
      );


      axiosInstance.post("https://api.y2dl.co/video", { 'video_src': this.video_src });
    }
  }
}
</script>

