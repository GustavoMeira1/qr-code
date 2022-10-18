<template>
    <v-app>
        <div class="card-box">
            <v-card class="card">
                <v-card class="img">
                    <v-img max-width="300px" src="../../public/image-qr-code.png"></v-img>
                </v-card>
                <div class="text">
                    <h2 style="color:hsl(218, 44%, 22%)">Improve your front-end skills by building projects</h2>
                    <p style="color:hsl(220, 15%, 55%)">Scan the QR code to visit FrontEnd Mentor and take your coding skills to the next level</p>
                </div>
            </v-card>
        </div>
    </v-app>
</template>

<script>
export default {
    name: 'qrCode',
    data(){
        return{
        };
    },
    mounted(){
    },
    methods:{
    }

}
</script>

<style>
.card-box{
    display: flex;
    align-items: center;
    justify-content: center;
    margin: auto;
   
}
.card{
    height: 480px;
    width: 330px;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center; 
    border-radius: 20px !important;
}
.img{
    margin-top: 10px !important;
    margin: auto;
    width: 90%;
    border-radius: 20px !important;

}
.text{
    margin-top: 10px !important;
    margin: 20px;
}
</style>