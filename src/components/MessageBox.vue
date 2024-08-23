<template>
    <div id="loading-overlay" class="fixed inset-0 bg-white bg-opacity-75 flex items-center justify-center z-50">
        <div class="flex flex-col items-center">
            <p>{{ msg }}</p>
            <button @click="close" class="mx-4 bg-[#f0f1f9] py-2 px-6 rounded-md font-bold">Close</button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        msg:String
    },
    emits:["close-msg-box"],
    methods:{
        close(){
            this.$emit('close-msg-box', true)
        }
    }
}

</script>