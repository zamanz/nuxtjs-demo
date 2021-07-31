<template>
    <div class="container">
        <div class="header">
            <h2>Vue CropperJS</h2>
            <a href="https://github.com/Agontuk/vue-cropperjs">Github</a>
        </div>
        <hr />

        <input ref="input" type="file" name="image" accept="image/*" @change="setImage"/>

        <div class="content">
            <section class="cropper-area">
                <div class="img-cropper">
                    <vue-cropper
                        v-if="imgSrc"
                        ref="cropper"
                        :aspect-ratio="1"
                        :src="imgSrc"
                        preview=".preview"
                    />
                </div>
                <div v-if="imgSrc" class="actions">
                    <a href="#" role="button" @click.prevent="zoom(0.2)"> Zoom In </a>
                    <a href="#" role="button" @click.prevent="zoom(-0.2)"> Zoom Out </a>
                    <a href="#" role="button" @click.prevent="move(-10, 0)">Move Left</a>
                    <a href="#" role="button" @click.prevent="move(10, 0)">Move Right</a>
                    <a href="#" role="button" @click.prevent="move(0, -10)">Move Up</a>
                    <a href="#" role="button" @click.prevent="move(0, 10)">Move Down</a>
                    <a href="#" role="button" @click.prevent="rotate(90)">Rotate +90deg</a>
                    <a href="#" role="button" @click.prevent="rotate(-90)">Rotate -90deg</a>
                    <a ref="flipX" href="#" role="button" @click.prevent="flipX">Flip X</a>
                    <a ref="flipY" href="#" role="button" @click.prevent="flipY">Flip Y</a>
                    <a href="#" role="button" @click.prevent="cropImage">Crop</a>
                    <a href="#" role="button" @click.prevent="reset">Reset</a>
                    <a href="#" role="button" @click.prevent="saveImage">Save Image</a>
                </div>
                <div class="actions">
                    <a href="#" role="button" @click.prevent="showFileChooser">Upload Image</a>
                </div>

            </section>
            <section v-if="imgSrc" class="preview-area">
                <p>Preview</p>
                <div class="preview m-auto"></div>
                <p>Cropped Image</p>
                <div class="cropped-image">
                    <img v-if="cropImg" :src="cropImg" alt="Cropped Image" />
                    <div v-else class="crop-placeholder" />
                </div>
            </section>
        </div>
    </div>
</template>

<script>
import VueCropper from "vue-cropperjs";
import "cropperjs/dist/cropper.css";
export default {
    components: {
        VueCropper,
    },
    data() {
        return {
            imgSrc: '',
            cropImg: "",
        };
    },
    methods: {
        cropImage() {
            // get image data for post processing, e.g. upload or setting image src
            this.cropImg = this.$refs.cropper.getCroppedCanvas().toDataURL();
        },
        flipX() {
            const dom = this.$refs.flipX;
            let scale = dom.getAttribute("data-scale");
            scale = scale ? -scale : -1;
            this.$refs.cropper.scaleX(scale);
            dom.setAttribute("data-scale", scale);
        },
        flipY() {
            const dom = this.$refs.flipY;
            let scale = dom.getAttribute("data-scale");
            scale = scale ? -scale : -1;
            this.$refs.cropper.scaleY(scale);
            dom.setAttribute("data-scale", scale);
        },
        move(offsetX, offsetY) {
            this.$refs.cropper.move(offsetX, offsetY);
        },
        reset() {
            this.$refs.cropper.reset();
        },
        rotate(deg) {
            this.$refs.cropper.rotate(deg);
        },
        setImage(e) {
            const file = e.target.files[0];

            if (!file.type.includes("image/")) {
                alert("Please select an image file");
                return;
            }

            if (typeof FileReader === "function") {
                const reader = new FileReader();

                reader.onload = (event) => {
                    this.imgSrc = event.target.result;
                    // rebuild cropper js with the updated source
                    this.$refs.cropper.replace(event.target.result);
                };

                reader.readAsDataURL(file);
            } else {
                alert("Sorry, FileReader API not supported");
            }
        },
        showFileChooser() {
            this.$refs.input.click();
        },
        zoom(percent) {
            this.$refs.cropper.relativeZoom(percent);
        },
        saveImage(){
            if (this.cropImg === ''){
                alert('Please crop this image, then save')
            }
            else{
                this.$axios.$post('/upload-image-by-croperjs', {
                    image: this.cropImg
                }).then(response =>{
                    if(response.status === 200){
                        alert('Image successfully upload')
                    }
                    console.log(response)
                }).catch(error =>{
                    console.log(error)
                })
            }
        }
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    input[type="file"] {
        display: none;
    }

    .header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 10px 0 5px 0;
    }

    .header h2 {
        margin: 0;
    }

    .header a {
        text-decoration: none;
        color: black;
    }

    .content {
        display: flex;
        justify-content: space-between;
    }

    .cropper-area {
        width: 614px;
    }

    .actions {
        margin-top: 1rem;
    }

    .actions a {
        display: inline-block;
        padding: 5px 15px;
        background: #0062cc;
        color: white;
        text-decoration: none;
        border-radius: 3px;
        margin-right: 1rem;
        margin-bottom: 1rem;
    }

    textarea {
        width: 100%;
        height: 100px;
    }

    .preview-area {
        width: 307px;
    }

    .preview-area p {
        font-size: 1.25rem;
        margin: 0;
        margin-bottom: 1rem;
    }

    .preview-area p:last-of-type {
        margin-top: 1rem;
    }

    .preview {
        width: 100%;
        height: calc(372px * (9 / 16));
        overflow: hidden;
    }

    .crop-placeholder {
        width: 100%;
        height: 200px;
        background: #ccc;
    }

    .cropped-image img {
        max-width: 100%;
    }
</style>
