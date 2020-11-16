<template>
    <div class="file">
        <form @submit.prevent="onSubmit" enctype="multipart/form-data">
            <div class="fields">
                <label>Upload File</label><br/>
                <input 
                type="file"
                ref="file"
                @change="onSelect"/>
            </div>
            <div class="fields">
                <button>Submit</button>
            </div>
            <div class="message">
                <h5>{{message}}</h5>
            </div>
        </form>
    </div>
    
</template>
    
<script>
import axios from 'axios'
export default {  
    name: 'FileUpload',
    data(){
        return {
            file :"",
            message : ""
        }
    },
    methods : {
        onSelect() {
            const allowedTypes = ['image/jpeg', 'image/jpg', 'image/png'];
            const file = this.$refs.file.files[0];
            this.file = file
            if(!allowedTypes.includes(file.type)){
                this.message = 'Only images are allowed'
            }
            if(file.size > 500000){
                this.message = 'File too large'
            }
        },
        async onSubmit(){
            const formData = new FormData();
            formData.append('file', this.file)
            try {
                await axios.post('http://localhost:5000/upload', formData)
                this.message = 'FIle Uploaded!'
            } catch (err) {
                console.log(err)
                this.message ='Something went wronmg'
            }
        }
    }
    }
</script>