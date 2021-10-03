<template>
    <h2>File is ready to download. Enjoy!</h2>
    <a id="s3Link" v-bind:href="''+download_url+''" class=" bg-red-500 shadow-md hover:bg-white hover:text-red-500 text-white font-bold py-2 px-4 rounded inline-flex items-center">
        <svg class="fill-current w-4 h-4 mr-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M13 8V2H7v6H2l8 8 8-8h-5zM0 18h20v2H0v-2z"/></svg>
        <span>Download MP3</span>
    </a>
</template>

<script>
export default {
    name: 'DownloadButton',
    props: {
        download_url: { 
            type: String,
            required: true
        }
    }
}
</script>