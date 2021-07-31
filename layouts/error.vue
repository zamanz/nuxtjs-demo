<template>
    <div class="error-page">
        <div class="container">
            <div class="wrapper">
                <ErrorCode :code="error.statusCode"/>
                <h3>Oops! Something went wrong.</h3>
                <p>{{ error.message }}</p>
                <div class="d-flex justify-content-center align-items-center">
                    <a href="/" class="btn btn-success mx-2">Go to Home</a>
                    <button type="button" class="btn btn-info mx-2 text-light" @click="reload">Reload</button>
                    <button type="button" class="btn btn-secondary mx-2 text-light" @click="goBack">Go Back</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: "Error",
    props:{
        error:{
            default: null,
            type: [Object]
        },
    },
    created(){
        console.log("[Error Details: ]",this.error);
    },
    methods:{
        reload(){
            window.location.reload()
        },
        goBack(){
            console.log('Go Back')
        }
    }

}
</script>
<style lang="scss" scoped>

.error-page{
    padding: 60px 0;
    .wrapper{
        max-width: 500px;
        margin: 0 auto;
        text-align: center;
        svg{
            max-width: 150px;
            margin: 0px auto 30px auto;
        }
        h3{
            font-weight: 600;
            margin-bottom: 12px;
        }
        p{
            font-size: 18px;
            margin-bottom: 15px;
            line-height: 1.5;
        }
    }
}

</style>
