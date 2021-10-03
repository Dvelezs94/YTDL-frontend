<template>
    <button type="button" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded inline-flex items-center" disabled>
        <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
        </svg>
        <span>Processing video...{{ progress_percent }}%</span>
    </button>
</template>

<script>
export default {
    name: 'LoadingButton',
    data() {
        return {
            progress_percent: 0,
            loading_interval: null
        }
    },
    created() {
        this.loading_interval = setInterval(() => {
            this.progress_percent = this.progress_percent + this.randomIntFromInterval(1, 3)
            if (this.progress_percent >= 99) {
                this.progress_percent = 99;   
            }
        }, 100);
    },
    unmounted() {
        clearInterval(this.loading_interval);
    },
    methods: {
        randomIntFromInterval(min, max) { // min and max included 
          return Math.floor(Math.random() * (max - min + 1) + min)
        }
    }
}
</script>