/* eslint-disable unicorn/prefer-includes */
<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-12">
                <div class="card mt-3">
                    <div class="card-header d-flex justify-content-between align-items-center">
                        <h3>Profile</h3>
                        <div>
                            <NuxtLink v-if="$i18n.locale == 'bn'" :to="switchLocalePath('en')">English</NuxtLink>
                            <NuxtLink v-if="$i18n.locale == 'en'" :to="switchLocalePath('bn')">বাংলা</NuxtLink>
                        </div>
                    </div>
                    <div class="card-body">
                        <div>
                            <button type="button" class="text-gray-500 hover:text-white font-bold rounded border-b-2 border-green-500 hover:bg-green-500 active:bg-green-700 shadow-md py-2 px-6 inline-flex items-center focus:outline-none" @click="$refs['upload-modal'].show()">
                                <span class="mr-2">Upload Image</span>
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-cloud-upload" viewBox="0 0 16 16">
                                    <path fill-rule="evenodd" d="M4.406 1.342A5.53 5.53 0 0 1 8 0c2.69 0 4.923 2 5.166 4.579C14.758 4.804 16 6.137 16 7.773 16 9.569 14.502 11 12.687 11H10a.5.5 0 0 1 0-1h2.688C13.979 10 15 8.988 15 7.773c0-1.216-1.02-2.228-2.313-2.228h-.5v-.5C12.188 2.825 10.328 1 8 1a4.53 4.53 0 0 0-2.941 1.1c-.757.652-1.153 1.438-1.153 2.055v.448l-.445.049C2.064 4.805 1 5.952 1 7.318 1 8.785 2.23 10 3.781 10H6a.5.5 0 0 1 0 1H3.781C1.708 11 0 9.366 0 7.318c0-1.763 1.266-3.223 2.942-3.593.143-.863.698-1.723 1.464-2.383z"/>
                                    <path fill-rule="evenodd" d="M7.646 4.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1-.708.708L8.5 5.707V14.5a.5.5 0 0 1-1 0V5.707L5.354 7.854a.5.5 0 1 1-.708-.708l3-3z"/>
                                </svg>
                            </button>
                            <button type="button" class="text-gray-500 hover:text-white font-bold rounded border-b-2 border-green-500 hover:bg-green-500 active:bg-green-700 shadow-md py-2 px-6 inline-flex items-center focus:outline-none"  @click="openAttachment">
                                <span class="mr-2">Upload File</span>
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-cloud-upload" viewBox="0 0 16 16">
                                    <path fill-rule="evenodd" d="M4.406 1.342A5.53 5.53 0 0 1 8 0c2.69 0 4.923 2 5.166 4.579C14.758 4.804 16 6.137 16 7.773 16 9.569 14.502 11 12.687 11H10a.5.5 0 0 1 0-1h2.688C13.979 10 15 8.988 15 7.773c0-1.216-1.02-2.228-2.313-2.228h-.5v-.5C12.188 2.825 10.328 1 8 1a4.53 4.53 0 0 0-2.941 1.1c-.757.652-1.153 1.438-1.153 2.055v.448l-.445.049C2.064 4.805 1 5.952 1 7.318 1 8.785 2.23 10 3.781 10H6a.5.5 0 0 1 0 1H3.781C1.708 11 0 9.366 0 7.318c0-1.763 1.266-3.223 2.942-3.593.143-.863.698-1.723 1.464-2.383z"/>
                                    <path fill-rule="evenodd" d="M7.646 4.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1-.708.708L8.5 5.707V14.5a.5.5 0 0 1-1 0V5.707L5.354 7.854a.5.5 0 1 1-.708-.708l3-3z"/>
                                </svg>
                            </button>
                        </div>
                        <p class="my-3">{{ user }}</p>
                        <p>{{ $t('profile.content') }}</p>
                    </div>
                    <b-modal ref="upload-modal" title="Using Croperjs for image upload Methods" hide-footer scrollable  size="lg">
                        <div v-if="imgSrc" class="row">
                            <div class="col-lg-8">
                                <div class="image-container mb-2">
                                    <vue-cropper
                                        v-if="imgSrc"
                                        ref="cropper"
                                        :aspect-ratio="1"
                                        :src="imgSrc"
                                        preview=".preview"
                                    />
                                </div>
                                <div class="action-button">
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="cropImage">Crop</button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="reset">Reset</button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="saveImage">Save Image</button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="showFileChooser">Choose a picture</button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="zoom(0.2)"> Zoom In </button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="zoom(-0.2)"> Zoom Out </button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="move(-10, 0)">Move Left</button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="move(10, 0)">Move Right</button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="move(0, -10)">Move Up</button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="move(0, 10)">Move Down</button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="rotate(90)">Rotate +90deg</button>
                                    <button type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="rotate(-90)">Rotate -90deg</button>
                                    <button ref="flipX" type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="flipX">Flip X</button>
                                    <button  ref="flipY" type="button" class="bg-white text-gray-800 font-bold rounded border-b-2 border-green-500 shadow-md py-2 px-6 inline-flex items-center focus:outline-none mt-2" @click.prevent="flipY">Flip Y</button>
                                </div>
                            </div>
                            <div class="col-lg-4">
                                <div class="image-preview-container">
                                    <div class="preview w-100"></div>
                                    <div v-if="cropImg">
                                        <h3 class="text-xl mb-1">Cropped Image</h3>
                                        <img v-if="cropImg" :src="cropImg" alt="Cropped Image" />
                                    </div>
                                </div>
                            </div>
                        </div>
                        <input ref="input" type="file" name="image" accept="image/*" @change="setImage"/>
                        <button v-if="!imgSrc" type="button" @click.prevent="showFileChooser">Choose a picture</button>
                    </b-modal>

                    <b-modal ref="upload-file-modal" title="Upload docs file" hide-footer>
                        <form class="p-0" enctype="multipart/form-data" method="POST" @submit.prevent="submittedAttachmentWithFile">
                            <div class="form-group">
                                <label for="memo-list" class="mb-2">Attachment</label>
                                <input type="file" class="form-control d-block" multiple @change="uploadAttachment">
                            </div>

                            <div class="my-3">
                                <h6 v-for="(attachment_file, index) in selected_attachment" :key="index" class="d-flex justify-content-between align-items-center attachment-file mb-2">
                                    <span>{{ attachment_file.name }}</span>
                                    <span class="btn btn-sm btn-danger" @click="removeAttachment(index)">
                                        Remove
                                    </span>
                                </h6>
                            </div>
                            <div class="modal-footer mt-3 px-0">
                                <button type="submit" class="btn btn-success text-light w-100 m-0">Upload</button>
                            </div>
                        </form>
                    </b-modal>
                </div>
            </div>
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
    data(){
        return {
            user: null,
            imgSrc: '',
            cropImg: '',
            selected_attachment:[]
        }
    },
    
    mounted(){
        this.imgSrc = require('@/assets/images/default.jpg')
    },
    methods:{
        getUser() {
            this.$axios.$get('/user?username=' + this.$route.params.username).then(response => {
                this.user = response
            }).catch(error =>{
                console.log(error);
            });
        },
        cropImage() {
            // get image data for post processing, e.g. upload or setting image src
            this.cropImg = this.$refs.cropper.getCroppedCanvas({
                // manage max width and height
                // minWidth: 256,
                // minHeight: 256,
                // maxWidth: 1000,
                // maxHeight: 1000,
                width: 1000,
                height: 1000,
            }).toDataURL('image/jpeg', 0.6);
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
                    alert('Image successfully upload')
                    console.log(response)
                }).catch(error =>{
                    console.log(error)
                })
            }
        },
        openAttachment(){
            this.$refs['upload-file-modal'].show()
            this.attachment_files = []
            this.selected_attachment = []
        },
        uploadAttachment(event){
            for(let i = 0; i < event.target.files.length; i++){
                if (typeof FileReader === "function") {
                    const reader = new FileReader();

                    reader.onload = (e) => {
                        this.selected_attachment.push({
                            name: event.target.files[i].name,
                            base64: e.target.result
                        })
                    };

                    reader.readAsDataURL(event.target.files[i]);
                }
                else {
                    alert("Sorry, FileReader API not supported");
                }
            }
            console.log(this.selected_attachment)
        },
        removeAttachment(index){
            this.attachment_files.splice(index, 1)
        },
        submittedAttachmentWithFile(){
            this.$axios.$post('upload-docs-file', {
                attachments: this.selected_attachment
            }).then(response =>{
                // this.$refs['upload-file-modal'].hide()
                this.$notifier.snackBar(response, 'bg-success')
                console.log(response)
            }).catch(error =>{
                console.log(error)
            })
            console.log('submittedAttachmentWithFile', this.selected_attachment)
        }
    }
}
</script>

<style lang="scss" scoped>
    input[type="file"] {
        display: none;
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